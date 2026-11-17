# ADR-011: Hosted on AWS, with the front end and the API on one subdomain

**Date:** 2026-11-16 · **Status:** Accepted

## Context

ADR-003 left hosting open, and the sign-in decision (API spec `sign-in`) is tied to it. The app is for one person, used on an iPhone many times a day, and Today is drawn from the phone's own copy first (ADR-004), so a slow first response from the API is tolerable. The founder wants AWS, wants the setup to be part of the showcase (ADR-003), and is cost-sensitive.

What this app shares with the founder's other projects is decided in the `infra` repo's [ADR-001](https://github.com/jan-mcsparin/infra/blob/main/docs/adr/0001-shared-platform.md): one AWS account, the domain `jan-mcsparin.me` with a subdomain per project, one shared RDS Postgres instance with a database per project, no NAT gateway, Terraform, and deploys from GitHub Actions through OIDC. This ADR covers what is this app's own.

For the API, an always-on container behind a load balancer costs roughly $25 a month more than a function that runs on request, and buys a faster first response that ADR-004 has already made unnecessary.

## Decision

- **The app lives at `goals-to-actions.jan-mcsparin.me`.**
- **The front end and the API share that one origin.** CloudFront serves the built front end from a private S3 bucket at `/` and passes `/api/*` to the API. No CORS, and the session cookie is first-party.
- **The API runs on Lambda as a container image,** with Lambda Web Adapter in front of the same FastAPI app that runs locally. No AWS code in the application.
- **Its database is one database on the shared instance.** The app connects with a role that can't change the schema.
- **Migrations run inside AWS.** The pipeline invokes a function that runs dbmate; nothing outside the network reaches the database.
- **The API makes no calls out to the internet.** Anything that would need one (sending email, a third-party service) is a new decision.
- **Two environments: local and production.**
- **This app's Terraform lives with the app,** not in `infra`. Where exactly is settled when it's written.
- **Hosting waits until the API is done** (API specs, "Not in the map"), **and sign-in is built before the API is first reachable.** No temporary shared secret stands in for it.

## Consequences

- This app adds almost nothing to the shared monthly cost: the function, the bucket and CloudFront are close to free at one person's use.
- The first request after a quiet spell takes a second or two while the function starts. Today doesn't wait for it, and a check-off made in that moment is already optimistic (ADR-004).
- **The subdomain is hard to change.** A new origin means a new passkey (if that is the sign-in method) and reinstalling the app on the phone, which loses its stored plan and queue.
- **What this asks of sign-in:** the session cookie is host-only (the `__Host-` prefix), never set for `.jan-mcsparin.me`. A passkey would be bound to the full subdomain. A method that sends email needs a way out of the private network.
- **Cache headers carry the "new version with no prompt" rule** (front-end issue #8): hashed assets are cached for good; `index.html`, the service worker and the manifest are never cached.
- ADR-009's "no job" stands. A scheduled call to the opening and closing rules can be added later without changing this.
- **To confirm when it's built,** since none of this has been run yet:
  - that the Python 3.15 image starts on Lambda in acceptable time;
  - how CloudFront is made the only way in to the API. A function URL behind CloudFront needs extra request signing for `PUT` and `POST`; an API Gateway HTTP API as the origin avoids that;
  - that the page fallback to `index.html` is done on the front-end path only (a CloudFront Function). CloudFront's error pages apply to the whole distribution and would turn the API's real 404s into the app's page;
  - that `X-Time-Zone` and the cookie reach the API through CloudFront, and that nothing under `/api/` is cached.
- Still to decide: sign-in, in its own ADR.
