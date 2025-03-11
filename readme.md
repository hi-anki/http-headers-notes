# HTTP Headers
+ They help in passing additional information within a message.
+ In HTTP/1.x, a header is a case-insensitive name followed by a colon and its value.
+ In HTTP/2 and above, headers are displayed in lowercase when viewed in developer tools, and prefixed with a colon for a special group of pseudo-headers.
+ Headers can be grouped on the basis of the context they serve in.

## Content Definition Headers
### `Accept` (Req-Res)
+ It indicates which content types the sender is able to understand.
+ Syntax:
   ```
   <!-- A precise media type, like text/plain, image/png -->
   Accept: <media-type>/<MIME_subtype>

   <!-- A media type without a subtype, like, all images (image/*) -->
   Accept: <media-type>/*

   <!-- Anything -->
   Accept: */*
   ```
+ **Note:** Systems that involve API interaction commonly request application/json responses.
+ **Note:** It can't contain a CORS-unsafe request header byte: 0x00-0x1F (except for 0x09 (HT), which is allowed), "():<>?@[\]{}, and 0x7F (DEL).
+ **Note:** It can only have values consisting of 0-9, A-Z, a-z, space or *,-.;=

### `Accept-Encoding` (Req-Res)
+ It indicates the content encoding (usually a compression algorithm) that the sender can understand.
+ Example:
   ```
   Accept-Encoding: gzip
   Accept-Encoding: *
   ```

### `Accept-Patch` (Res)
+ It indicates which media types the server is able to understand in a PATCH request.
+ Syntax:
   ```
   Accept-Patch: <media-type>/<subtype>
   Accept-Patch: <media-type>/*
   Accept-Patch: */*

   // Comma-separated list of media types
   Accept-Patch: <media-type>/<subtype>, <media-type>/<subtype>
   ```
   
### `Accept-Patch` (Res)
+ It indicates which media types are accepted by the server in a POST request.
+ Syntax:
   ```
   Accept-Post: <media-type>/<subtype>
   Accept-Post: <media-type>/*
   Accept-Post: */*

   // Comma-separated list of media types
   Accept-Post: <media-type>/<subtype>, <media-type>/<subtype>
   ```

### `Access-Control-Allow-Credentials` (Res)
+ It tells browsers whether the server allows credentials to be included in cross-origin HTTP requests.
+ Credentials include cookies, TLS client certificates, and authentication headers containing a username and password. 
  + By default, these credentials are not sent in cross-origin requests, as it can make a site vulnerable to CSRF attacks.
+ A client can ask for credentials to be included in cross-site requests in several ways:
  + Using `fetch()`, by setting the `credentials` option to `"include"`.
  + Using `XMLHttpRequest`, by setting the `XMLHttpRequest.withCredentials` property to `true`.
  + Using `EventSource()`, by setting the `EventSource.withCredentials` property to `true`.
+ **Note:** The `true` here is case-sensitive.
+ **Note:** If you don't need credentials, omit this header entirely rather than setting its value to `false`.

+ Example:
  ```js
  Access-Control-Allow-Credentials: true

  fetch(url, {
    credentials: "include",
  });

  const xhr = new XMLHttpRequest();
  xhr.open("GET", "http://example.com/", true);
  xhr.withCredentials = true;
  xhr.send(null);
  ```

## Preflight Communication Headers
| Header | Message | Description |
| - | - | - |
| `Access-Control-Request-Headers` | Request | Used by browsers when making a preflight request to let the server know which HTTP headers the client might send when the actual request is made. |
| `Access-Control-Request-Methods` | Request | Used by browsers when making a preflight request to let the server know which HTTP methods the client might use when the actual request is made. |
| `Access-Control-Allow-Headers` | Response | Returned in response to a preflight request, indicating the headers allowed in an actual request to the resource. |
| `Access-Control-Allow-Methods` | Response | Returned in response to a preflight request, indicating the methods allowed in an actual request to the resource. |

### `Allow` (Res)
+ It lists the set of request methods supported by a resource. 
+ This header must be sent if the server responds with a `405 Method Not Allowed` status code to indicate which request methods can be used instead. An empty Allow value indicates that the resource allows no request methods.

## Authentication
### `Authorization` (Req)
+ It is used to provide credentials that authenticate a user agent with a server, allowing access to protected resources.
+ Syntax:
  ```
  Authorization: <auth-scheme> <authorization-parameters>

  // Basic authentication
  Authorization: Basic <credentials>

  // Digest authentication
  Authorization: Digest username=<username>,
      realm="<realm>",
      uri="<url>",
      algorithm=<algorithm>,
      nonce="<nonce>",
      nc=<nc>,
      cnonce="<cnonce>",
      qop=<qop>,
      response="<response>",
      opaque="<opaque>"
  ```
  > Authentication scheme defines how the credentials are encoded.

## `Cache-Control` (Req-Res)
+ Caching directives are case-insensitive. However, lowercase is recommended because some implementations do not recognize uppercase directives.
+ Multiple directives are permitted and must be comma-separated.

## `Clear-Site-Data` (Res)
It indicates client to remove all the browsing data of certain types (cookies, storage, cache) associated with the requesting website. It allows web developers to have more control over the data stored by browsers for their origins.

| Directive | Description | Syntax |
| - | - | - |
| cache | remove all the locally cached data. | `clear-site-data: "cache"` |
| clientHints | remove all the client hints | `clear-site-data: "clientHints"` |
| cookies | remove all the cookies | `clear-site-data: "cookies"` |
| storage | remove all DOM storage | `clear-site-data: "storage"` |
| executionContext | the client should reload all browsing contexts for the origin of the response | `clear-site-data: "executionContext"` |
| * | remove everything | `clear-site-data: "*"` |