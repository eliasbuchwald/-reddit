Use a POST to `/api/login/USERNAME` to authenticate with reddit, where `USERNAME` is the name to authenticate as. Several parameters must be sent along with the post:

| **field** | **sample value** | **explanation** |
|:----------|:-----------------|:----------------|
| `user`    | `example`        | The username to authenticate as. This is redundant, but required. |
| `passwd`  | `hunter2`        | The plain-text password for the account. |
| `api_type`| `json`           | Must be `json` for the style of auth used in this documentation. |

## POST Example

```
POST /api/login/example HTTP/1.1
Host: www.reddit.com
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Content-Length: 41

api_type=json&user=example&passwd=hunter2
```

## Response

The HTTP status code of the response will always be a 200 (OK), and the contents of the body will be a JSON object, regardless of authentication success. To determine the result of the operation, look at the contents of the JSON payload.

The top level object has a single attribute, `json`, which maps to an object with all the relevant data in it. To determine success, check the size of the `json.errors` array.

### Success

A successful authentication attempt will have a zero-length `json.errors` array, and a `data` attribute containing the user's cookie and modhash ((see **glossary** on the [[API]] page).

```
{"json": {"errors": [], "data": {"modhash": "u4abc21302316ad40013feb16cfccb0b11b786596e5194de14", "cookie": "1234567,2011-07-12T14:53:59,0200b365fa02c61f9532ab244b214bd481941492"}}}
```

### Failure

After an invalid login attempt, the `json.errors` array will have entries, and no other attributes will be present.

```
{"json": {"errors": [["WRONG_PASSWORD", "invalid password"]]}}
```