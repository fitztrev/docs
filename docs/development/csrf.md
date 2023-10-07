# Preventing unauthorized writes

> **Rule 1:** HTTP endpoints must strictly use the appropriate HTTP methods, i.e. `GET` and other ["safe" methods](https://developer.mozilla.org/en-US/docs/Glossary/safe) should not change any state on the server.

Before even routing, a global request handler [checks](https://github.com/ornicar/lila/blob/master/modules/security/src/main/CSRFRequestHandler.scala) the `Origin` header on all unsafe requests.

# Preventing unauthorized reads

~In absence of CORS, only JSON and XML can be read in cross site requests.~

> ~**Rule 2:** All JSON endpoints that can be queried with [simple requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#Simple_requests) **must** treat requests as unauthenticated/anonymous and **should** live under `/api/`.~

~For requests that cannot be anonymous (e.g. XHR and requests by the mobile app) simply require them to be *not simple*. This can be done by requiring the `X-Requested-With` header (check `isXhr()` in lila).~

~Since `Accept` headers are considered *simple*, it is insufficient to use only `Accept` to request a JSON response.~

The above rule is no longer required in modern browsers (and older browsers cannot load Lichess because we only support modern TLS).

> **Rule 3:** Endpoints under /api/ must not use authentication via Cookie.

This allows `/api/` to safely be whitelisted for CORS from all origins. When authentication is required, OAuth can be used instead.

# WebSockets

WebSockets are special, because they are iniated with a safe request, despite often being used to change server side state. Browsers allow creating WebSockets across origins, even in absence of CORS.

> **Rule 4:** WebSockets from untrusted origins must be treated as unauthenticated/anonymous.

# Trusted origins

* `https://lichess.org` and subdomains
* `ionic://localhost` (mobile app web view on iOS)
* `capacitor://localhost` (mobile app web view on iOS)
* `http://localhost` (android app web view, or controlled by the user)
* `http://localhost:8080` (local development)
* Absense of `Origin` header (mobile app web view, or programmatic access controlled by the user, impossible to forge in cross site requests, note that `null` is rejected)