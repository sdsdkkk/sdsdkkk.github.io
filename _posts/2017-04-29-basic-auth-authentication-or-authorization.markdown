---
layout: post
title:  "Basic Auth: Authentication or Authorization?"
date:   2017-04-29 00:00:00
description: Are We Authenticating?
tags: SoftwareEngineering
---

### What is Auth?

The word "auth" in computer security context can refer to authentication and authorization. These two words' meanings are sometimes confused with each other. The detailed explanation for each is available on Wikipedia.

But to simplify the definition in the context of computer security, we can sum it up to a sentence for each.

> _Authentication_ is the act of proving that you're who you claim to be.

> _Authorization_ is the act of deciding whether you're allowed to access a certain resource.

### Basic Auth

Basic auth is the simplest authentication method we can use for HTTP-based systems. To authenticate, we need to pass username and password. Let's say we use the following credentials.

```
Username  : michael
Password  : iZaw3s0m3
```

The credentials will be used to build an username:password string to be encoded using Base64.

```
Plaintext : michael:iZaw3s0m3
Base64    : bWljaGFlbDppWmF3M3MwbTM=
```

The Base64-encoded string will be passed in the `Authorization` header of the request.

```
Authorization: Basic bWljaGFlbDppWmF3M3MwbTM=
```

This string will be decoded in the server to fetch the original username and password values, to be matched with the valid credentials stored in the server.

### Wait...Authorization Header?

Once, someone asked me whether basic auth is for authentication or authorization. The reason is that she found it inconsistent, as basic auth's is actually _basic authentication_ yet the credentials are put in the request's `Authorization` header.

The explanation I gave was pretty much as follows.

Basic auth is used for authentication, as the name _basic authentication_ points out. The user needs to insert the username and password once to the prompt in their web browser one.

Consider this as the authentication flow, parallel to the login sequence in popular web applications such as Facebook.

Regarding the `Authorization` header, it's used to authorize the access to requested resources. After the user entered their username and password through the basic auth prompt, they'll be able to access the requested resources allowed for them to access without the need to re-enter their credentials. Their credentials are stored by the browser and re-sent using the `Authorization` header.

Consider this parallel to the access control logic in popular web applications, where the logged-in user's session is kept by both the browser and the server, allowing them to access their resources without the need to re-enter the credentials.

Yet, the authorization part should actually be defined in the web application itself. The application need to enforce its own access control, as the session information or `Authorization` header provided by the user is only useful to identify the requesting user.

```
-------------------------------------------------
| Resource         | Allowed Users              |
-------------------------------------------------
| Sales Info       | Michael, Jane, Rose        |
| Finance Report   | Michael, Rose              |
| Staff Attendance | Michael, Jane              |
-------------------------------------------------
```

With the rule defined on the server as listed above, those who're using Michael's credentials are able to access all three resources. Jane and Rose's credentials both are able to be used for accessing Sales Info resource, and one other resource for each.

This is not something that should be defined on the client side and passed along with the `Authorization` header. Otherwise, it can be easily tampered. But that would be another thing to discuss, as it's out of basic auth's scope.

### Conclusion

Basic auth is perfectly fine for, well, authentication as long as the risks are considered. The credentials are sent only over a request header, encoded with Base64. Obviously, it's not the most secure method to use.

Sending plaintext password over basic auth is a big no for systems that need proper security for its registered users, as it needs to be processed as plaintext each time the `Authorization` header is received by the server.

Well, concluding the discussion between me and the person asking whether basic auth is actually for authentication or authorization. It is obviously used for authentication as the proper name, _basic authorization_ implies. The `Authorization` header naming is indeed a bit misleading in my opinion, as it's actually used to contain identity the requester claims to be. The claimed identity needs be processed in the server to enforce the actual authorization.

I think the `Authorization` header might be better if given the name `Identity` or `Credentials` header to better reflect the information contained in the context of basic auth.