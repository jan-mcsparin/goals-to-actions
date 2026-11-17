# ADR-012: Sign-in with a passkey, and a session kept in a cookie

**Date:** 2026-11-16 · **Status:** Accepted

## Context

Until now the API has run as one fixed user. Before it is reachable from a phone it needs to know who is asking ([ADR-011](0011-hosting-on-aws.md)). The API's `sign-in` spec set the limits: no home-made password hash or token format, no secret in the repo, no session a page script can read if a cookie can do the job, and the installed app must stay signed in across iOS restarts. ADR-011 adds two more: the API makes no calls out to the internet, and the front end and the API share one origin.

There is one person, on their own devices, most of them Apple's.

Four methods were compared:

- **A passkey.** Nothing to remember or to leak, can't be phished, and the phone's Face ID is the whole sign-in. Needs nothing outside our own API. Needs a way to add the first one and to recover if all are lost.
- **A sign-in link by email.** Needs an email service and a way out of the private network, and makes the inbox the key.
- **A provider** (Apple, Google). A third party for one user; Apple's needs a paid developer account.
- **A hosted auth service** (Cognito). More moving parts than the app it protects, and its usual set-up keeps tokens where page scripts can read them.

## Decision

**Proving who you are**

- **Sign-in is a passkey** (WebAuthn), bound to `goals-to-actions.jan-mcsparin.me`, and always asks the device to verify the person (Face ID, Touch ID).
- **No password, no email, no third party.**
- **A person can have several passkeys.** One kept in iCloud Keychain covers the iPhone and the Mac.
- **The server side uses a maintained WebAuthn library** (`webauthn`, 3.0.1 at the time of writing), never our own check of the signatures. The browser side uses the browser's own passkey functions.

**Adding a passkey**

- **A passkey is added with a one-time code.** A command run inside AWS, by someone already signed in to the AWS account, makes a code for a user: long, random, used once, gone in ten minutes. Typing it into the app lets that device add a passkey.
- **The same code is the first sign-in, a new device and the way back in** if every passkey is lost. Control of the AWS account is the root of trust.
- **There is no sign-up.** The founder's account is the fixed user, keeping its id and every row.

**Staying signed in**

- **A session is a row in Postgres and a random token in a cookie.** The database keeps only a hash of the token.
- **The cookie is `__Host-`, `HttpOnly`, `Secure` and `SameSite=Lax`,** so no page script can read it and no other subdomain can set or receive it (ADR-011).
- **A session lasts 90 days from its last use,** so an app opened every day never asks again. Signing out ends it on the server.
- **Last use is recorded at most once a day,** in a small write of its own. Reads are read-only, and a write on every request would be the busiest thing the API does.
- **Every route but `health` and the sign-in routes themselves answers 401 without a session.** The passkey steps and the one-time code have to work for someone not yet signed in.
- **A write is refused unless it comes from the app's own origin.** The founder's other projects sit on sibling subdomains, which browsers treat as the same site, so `SameSite` alone doesn't keep them out. The refusal is a 403, which the phone's queue treats as "try again later", never as a refusal of the change (ADR-010).
- **Everything sign-in remembers is in Postgres:** passkeys, sessions, codes, and the challenge of a sign-in under way. The function that runs the API holds nothing between requests.

**When a session ends**

- A 401 is not a refusal of the change. The queue stays as it is ([ADR-010](0010-week-document-and-replayed-queue.md)), sending stops, the app shows what the phone has stored and asks for the passkey. After signing in, the queue is sent.
- The stored plan stays on the phone when a session runs out.
- Signing out on purpose sends the queue first. If changes are waiting and can't be sent, the app says so before going on. Then everything stored on the phone is cleared: the week documents, the other screens' last responses and the queue.

## Consequences

- Nothing secret is ever typed, stored by us, or sent. What the database holds (public keys, token hashes) is of no use to someone who copies it.
- Losing every passkey is not losing the account, but getting back in means the AWS command line. That is fine for one person, and it is what has to change before anyone else uses the app.
- Two new dependencies, to be agreed when the slice is built: the WebAuthn library in the API, which brings `cryptography` with it.
- **The subdomain is now fixed in one more way:** a passkey made for it works nowhere else.
- **Passkeys need HTTPS on a real name, or `localhost`.** The laptop can sign in to a local API. A phone pointed at the laptop's address can't, so the installed app is tested against the hosted one.
- Tests make a session row directly. They don't act out a passkey.
- `data-model.md` gains the tables when the spec is finalised. Whether sessions and codes carry the audit trigger like every other table (API shared decision 3) is settled there; the audit log shouldn't collect token hashes.
- **To confirm when it's built:**
  - that iOS keeps the cookie for the installed app as long as the session lasts, with nothing cutting it to seven days;
  - that the passkey sheet works inside the installed app, not only in Safari;
  - that the library and `cryptography` install on Python 3.15 in the Lambda image;
  - which request header the origin check reads (`Origin` or `Sec-Fetch-Site`), and that CloudFront passes it through;
  - whether the browser's own functions for turning passkey options to and from JSON are on every device in use, or a small helper is needed.
- Still open, and not needed for one person: sign-up, deleting an account, row-level security (ADR-007).
