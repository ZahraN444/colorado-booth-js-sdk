
# OAuth 2 Authorization Code Grant



Documentation for accessing and setting credentials for OAuthACG.

## Auth Credentials

| Name | Type | Description | Setter |
|  --- | --- | --- | --- |
| oAuthClientId | `string` | OAuth 2 Client ID | `oAuthClientId` |
| oAuthClientSecret | `string` | OAuth 2 Client Secret | `oAuthClientSecret` |
| oAuthRedirectUri | `string` | OAuth 2 Redirection endpoint or Callback Uri | `oAuthRedirectUri` |
| oAuthToken | `OAuthToken` | Object for storing information about the OAuth token | `oAuthToken` |
| oAuthScopes | `OAuthScopeOAuthACGEnum[]` | List of scopes that apply to the OAuth token | `oAuthScopes` |
| oAuthClockSkew | `number` | Clock skew time in seconds applied while checking the OAuth Token expiry. | `clockSkew` |
| oAuthTokenProvider | `(lastOAuthToken: OAuthToken \| undefined, authManager: OAuthACGManager) => Promise<OAuthToken>` | Registers a callback for oAuth Token Provider used for automatic token fetching/refreshing. | `oAuthTokenProvider` |
| oAuthOnTokenUpdate | `(token: OAuthToken) => void` | Registers a callback for token update event. | `oAuthOnTokenUpdate` |



**Note:** Auth credentials can be set using `oAuthACGCredentials` object in the client.

## Usage Example

### 1\. Client Initialization

You must initialize the client with *OAuth 2.0 Authorization Code Grant* credentials as shown in the following code snippet.

```ts
import { Client, OAuthScopeOAuthACGEnum } from 'colorado-booth-sdk';

const client = new Client({
  oAuthACGCredentials: {
    oAuthClientId: 'OAuthClientId',
    oAuthClientSecret: 'OAuthClientSecret',
    oAuthRedirectUri: 'OAuthRedirectUri',
    oAuthScopes: [
      OAuthScopeOAuthACGEnum.ReadScope
    ]
  },
});
```



Your application must obtain user authorization before it can execute an endpoint call in case this SDK chooses to use *OAuth 2.0 Authorization Code Grant*. This authorization includes the following steps

### 2\. Obtain user consent

To obtain user's consent, you must redirect the user to the authorization page.The `buildAuthorizationUrl()` method creates the URL to the authorization page. You must have initialized the client with scopes for which you need permission to access.

```ts
const authUrl = client.oAuthACGManager?.buildAuthorizationUrl();
```

### 3\. Handle the OAuth server response

Once the user responds to the consent request, the OAuth 2.0 server responds to your application's access request by redirecting the user to the redirect URI specified set in `Configuration`.

If the user approves the request, the authorization code will be sent as the `code` query string:

```
https://example.com/oauth/callback?code=XXXXXXXXXXXXXXXXXXXXXXXXX
```

If the user does not approve the request, the response contains an `error` query string:

```
https://example.com/oauth/callback?error=access_denied
```

### 4\. Authorize the client using the code

After the server receives the code, it can exchange this for an *access token*. The access token is an object containing information for authorizing client requests and refreshing the token itself.

```ts
try {
  const token = await client.oAuthACGManager?.fetchToken(authorizationCode);
  if (token) {
    client.withConfiguration({
      oAuthACGCredentials: {
        ...config.oAuthACGCredentials,
        oAuthToken: token
      }
    });
  }
} catch(error) {
  // handle ApiError or OAuthProviderError if needed
}
```

### Scopes

Scopes enable your application to only request access to the resources it needs while enabling users to control the amount of access they grant to your application. Available scopes are defined in the [`OAuthScopeOAuthACGEnum`](../../doc/models/o-auth-scope-o-auth-acg-enum.md) enumeration.

| Scope Name | Description |
|  --- | --- |
| `ReadScope` | Read request for files |

### Refreshing the token

An access token may expire after sometime. To extend its lifetime, you must refresh the token.

```ts
try {
  const token = await client.oAuthACGManager?.refreshToken();
} catch(error) {
  // handle error
}
```

If a token expires, an exception will be thrown before the next endpoint call requiring authentication.

### Storing an access token for reuse

It is recommended that you store the access token for reuse.

```ts
Store the token in session storage or local storage.
```

### Creating a client from a stored token

To authorize a client using a stored access token, just set the access token in Configuration along with the other configuration parameters before creating the client:

```ts
const newClient = client.withConfiguration({
  oAuthACGCredentials: {
    ...config.oAuthACGCredentials,
    oAuthToken: token
  }
});
```

### Complete example



```ts
import {
  Client,
  OAuthScopeOAuthACGEnum,
  OAuthToken,
} from 'colorado-booth-sdk';

// function for storing token to database
async function saveTokenToDatabase(token: OAuthToken) {
  // Code to save the token to the database
}

// function for loading token from database
async function loadTokenFromDatabase(): Promise<OAuthToken | undefined> {
  // Load token from the database and return it (return undefined if no token exists)
  return undefined;
}

// create a new client from configuration
const client = new Client({
  oAuthACGCredentials: {
    oAuthClientId: 'OAuthClientId',
    oAuthClientSecret: 'OAuthClientSecret',
    oAuthRedirectUri: 'OAuthRedirectUri',
    oAuthScopes: [
      OAuthScopeOAuthACGEnum.ReadScope
    ]
  },
});

// obtain access token, needed for client to be authorized
const previousToken = await loadTokenFromDatabase();
if (previousToken) {
  // restore previous access token and update the cloned configuration with the token
  client.withConfiguration({
    oAuthACGCredentials: {
      ...config.oAuthACGCredentials,
      oAuthToken: previousToken
    }
  });
}
else {
    // redirect user to a page that handles authorization
}
```


