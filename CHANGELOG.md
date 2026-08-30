# Changelog

## 1.0.8

Ships the JSON login flow so the add-on can authenticate again.

Balboa removed `GET https://iot.controlmyspa.com/idm/tokenEndpoint`, which the
OAuth2 flow called first to discover the mobile client id, client secret and
token endpoint. That call started returning 404 on 29 August 2026 and init
failed on every start.

The replacement is `POST https://iot.controlmyspa.com/auth/login` with a JSON
body, returning an `accessToken` used as a bearer token on the rest of the API.
That code has been in the source since upstream commit 1d5328e on 28 July 2026,
but `version` in `config.yaml` had not moved since December 2022, so the
Supervisor considered 1.0.7 current and never rebuilt the image. Bumping the
version is what actually deploys the fix.

The monthly upstream sync workflow now bumps the patch version whenever it pulls
changes, so a future upstream fix cannot sit unshipped the same way.

Also carries our local fix to `useCelsius()` in `lib/spa.js`, which read the
spa panel's display flag rather than our own conversion setting.
