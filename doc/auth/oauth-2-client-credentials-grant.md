
# OAuth 2 Client Credentials Grant



Documentation for accessing and setting credentials for OAuthCCG.

## Auth Credentials

| Name | Type | Description | Setter |
|  --- | --- | --- | --- |
| oAuthClientId | `string` | OAuth 2 Client ID | `oAuthClientId` |
| oAuthClientSecret | `string` | OAuth 2 Client Secret | `oAuthClientSecret` |
| oAuthToken | `OAuthToken` | Object for storing information about the OAuth token | `oAuthToken` |
| oAuthClockSkew | `number` | Clock skew time in seconds applied while checking the OAuth Token expiry. | `clockSkew` |
| oAuthTokenProvider | `(lastOAuthToken: OAuthToken \| undefined, authManager: OAuthCCGManager) => Promise<OAuthToken>` | Registers a callback for oAuth Token Provider used for automatic token fetching/refreshing. | `oAuthTokenProvider` |
| oAuthOnTokenUpdate | `(token: OAuthToken) => void` | Registers a callback for token update event. | `oAuthOnTokenUpdate` |



**Note:** Auth credentials can be set using `oAuthCCGCredentials` object in the client.

## Usage Example

### Client Initialization

You must initialize the client with *OAuth 2.0 Client Credentials Grant* credentials as shown in the following code snippet. This will fetch the OAuth token automatically when any of the endpoints, requiring *OAuth 2.0 Client Credentials Grant* authentication, are called.

```ts
import { Client } from 'colorado-booth-sdk';

const client = new Client({
  oAuthCCGCredentials: {
    oAuthClientId: 'OAuthClientId',
    oAuthClientSecret: 'OAuthClientSecret'
  },
});
```



Your application can also manually provide an OAuthToken using the setter `oAuthToken` in `oAuthCCGCredentials` object. This function takes in an instance of OAuthToken containing information for authorizing client requests and refreshing the token itself.

### Adding OAuth Token Update Callback

Whenever the OAuth Token gets updated, the provided callback implementation will be executed. For instance, you may use it to store your access token whenever it gets updated.

```ts
import { Client, OAuthToken } from 'colorado-booth-sdk';

const client = new Client({
  oAuthCCGCredentials: {
    oAuthClientId: 'OAuthClientId',
    oAuthClientSecret: 'OAuthClientSecret',
    oAuthOnTokenUpdate: (token: OAuthToken) => {
      // Add the callback handler to perform operations like save to DB or file etc.
      // It will be triggered whenever the token gets updated
      saveTokenToDatabase(token);
    }
  },
});
```

### Adding Custom OAuth Token Provider

To authorize a client using a stored access token, set up the `oAuthTokenProvider` in `oAuthCCGCredentials` along with the other auth parameters before creating the client:

```ts
import { Client, OAuthCCGManager, OAuthToken } from 'colorado-booth-sdk';

const client = new Client({
  oAuthCCGCredentials: {
    oAuthClientId: 'OAuthClientId',
    oAuthClientSecret: 'OAuthClientSecret',
    oAuthTokenProvider: (lastOAuthToken: OAuthToken | undefined, authManager: OAuthCCGManager) => {
      // Add the callback handler to provide a new OAuth token
      // It will be triggered whenever the lastOAuthToken is undefined or expired
      return loadTokenFromDatabase() ?? authManager.fetchToken();
    }
  },
});
```


