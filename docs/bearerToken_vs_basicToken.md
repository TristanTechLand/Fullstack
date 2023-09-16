# [Backend]: HTTP Token - Basic Token and Bearer Token

## Outline

- [Intro](#intro)
- [How to use Tokens?](#how-to-use-tokens)
- [Basic Token](#basic-token)
- [Bearer Token](#bearer-token)
- [Reponse Error](#response-error)
- [Conclusion](#conclusion)
- [Reference](#reference)

## Intro

When authenticating user's identity and other properties, you would often see tokens in multiple forms. This article will be talking about basic tokens and bearer tokens, two very common tokens applied in HTTP scenerios.

To give you a heads up, basic tokens are used to access a domain or a site, and bearer tokens are used to access `Protect Resource`.

## How to use Tokens?

The most common way is to put tokens in the `HEADER` of http requests using `Authorization`. For example:

```
# Unauthorized Response
GET / HTTP/1.1

# Authorized Query
GET / HTTP/1.1
Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==
```

## Basic Token

As the most standard and simple way of HTTP authenticating, we transfer `password`, `user_id` and other sensitive information without encryption.

Most of the time, when accessing backend servers, you would have to pass the token in advance. If the authentication didn't pass, the `Response` would carry relevant information, more specifically a `HEADER` with `WWW-Authenticate`, also carrying `Realm`(protection space) information.

![cshttp](../img/cshttp.png)

## Bearer Token

Bearer tokens are defined under the protocol HTTP 1.1 TLS. Because of that, it is strictly required for bearer tokens to be used with TLS. Basic tokens likewise, `WWW-Authenticate` and `Authorization` headers also apply to bearer tokens scenarios.

Here are a few notes regarding using bearer tokens:

- Using `Authorization HEADER`

  Ex: Authorization: Bearer QWxhZGRpbjpvcGVuIHNlc2FtZQ==

- Using `Form Body`

  It is not recommended to put the bearer token in HTTP request body. Also, use methods other than `GET`(e.g. `PUT`, `POST`, `DELETE`)

## Response Error

When using tokens with HTTP, HTTP response status code `200` indicates a successful request, whereas code `4XX` indicates a failed request.

For basic tokens, when authentication fails, returns status code 401 and reports the protected scope which the error originated.

- Basic token Error example

```
HTTP/1.1 401 Unauthorized
Date: Mon, 04 Feb 2014 16:50:53 GMT
WWW-Authenticate: Basic realm="WallyWorld"
```

For bearer tokens, when authentication fails, returns status code 400, 401, 403 depending on the scenarios listed in the table below.

|   error type    | status code | scenario                                             |
| :-------------: | :---------: | :--------------------------------------------------- |
| invalid_request |     400     | request missing or unexpected parameters             |
|  invalid_token  |     401     | token invalid                                        |
|  invalid_scope  |     403     | token does not have access to the protected resource |

- Bearer token Error example

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer realm="example",
                  error="invalid_token",
                  error_description="The access token expired"
```

## Conclusion

As a conclusion, basic tokens and bearer tokens are actually pretty simple, of course there are other tokens suitable for other protocols, which I will write about it in the future when I encounter.

To wrap it up, basic tokens are used mainly for accessing certain domains, whereas bearer tokens are used mainly to access protected resources, and also requires a TLS HTTP environment to work.

## Reference

| ref                                                       | link                                                             |
| :-------------------------------------------------------- | :--------------------------------------------------------------- |
| The 'Basic' HTTP Authentication Scheme                    | https://datatracker.ietf.org/doc/html/rfc7617                    |
| The OAuth 2.0 Authorization Framework: Bearer Token Usage | https://datatracker.ietf.org/doc/html/rfc6750                    |
| Basic Authentication                                      | https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication |
