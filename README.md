# WebTrit Adapter OpenAPI
## Overview
The **Adapter** translates API requests from **WebTrit Core** to the target hosted PBX system or BSS, which will be referred to as the **Adaptee**. This translation enables users to authenticate, obtain their SIP credentials, and retrieve other necessary information.

## Terminology
* **Adapter** - the current system
* **Adaptee** - the target hosted PBX system or BSS
* **OTP** (One-Time Password) - a unique, temporary password sent to the user via a predefined delivery method,  such as email, SMS, etc.
* **CDR** (Call Detail Record) - a record of a call, including the caller, callee, duration, etc.

## Session
The `access_token` and `refresh_token` format are implicitly determined by the **Adapter** implementation, adhering to the following protocol:
- the `access_token` is a relatively short-lived, reusable token, typically lasting anywhere from an hour to a day or more
- if during making a protected API call using the current `access_token`, the **Adapter** responds with a `401` status code, the caller will initiate the `updateSession` operation, using the `refresh_token` to obtain new `access_token` and `refresh_token`, and then retry the original API call
- if during the `updateSession` operation, the caller receives a response with a non `200` status code, it indicates that the `refresh_token` has either expired or been invalidated, and the caller must initiate a new session creation using the `createSession` or `createSessionOtp` operations, involving the user
- the `refresh_token` is a long-lived, single-use token, typically with a duration ranging from a week to a month or more

Note: An alternative is to not provide a `refresh_token` and make the `access_token` long-lived. In this case, when the **Adapter** responds with a `401` status code during a protected API call using this `access_token`, the caller can't update the token and must initiate a new session creation using\nthe `createSession` or `createSessionOtp` operations, requiring user involvement. Such usage, however, is not recommended.

## References
[Adapter pattern](https://en.wikipedia.org/wiki/Adapter_pattern).

# Getting Started
The open API description can be used to generate an adapter API method in different languages. For example generation for `Python`.

```bash
fastapi-codegen --input openapi.json --output adapter
```

## License
Licensed under the MIT License, Version X11, which can be found in [LICENSE](./LICENSE).
