
# OAuth 2 Resource Owner Credentials Grant



Documentation for accessing and setting credentials for OAuthROPCG.

## Auth Credentials

| Name | Type | Description | Setter |
|  --- | --- | --- | --- |
| oAuthClientId | `string` | OAuth 2 Client ID | `oAuthClientId` |
| oAuthClientSecret | `string` | OAuth 2 Client Secret | `oAuthClientSecret` |
| oAuthUsername | `string` | OAuth 2 Resource Owner Username | `oAuthUsername` |
| oAuthPassword | `string` | OAuth 2 Resource Owner Password | `oAuthPassword` |
| oAuthToken | `OAuthToken` | Object for storing information about the OAuth token | `oAuthToken` |
| oAuthClockSkew | `number` | Clock skew time in seconds applied while checking the OAuth Token expiry. | `clockSkew` |
| oAuthTokenProvider | `(lastOAuthToken: OAuthToken \| undefined, authManager: OAuthROPCGManager) => Promise<OAuthToken>` | Registers a callback for oAuth Token Provider used for automatic token fetching/refreshing. | `oAuthTokenProvider` |
| oAuthOnTokenUpdate | `(token: OAuthToken) => void` | Registers a callback for token update event. | `oAuthOnTokenUpdate` |



**Note:** Auth credentials can be set using `oAuthROPCGCredentials` object in the client.

## Usage Example

### Client Initialization

You must initialize the client with *OAuth 2.0 Resource Owner Password Credentials Grant* credentials as shown in the following code snippet.

```ts
import { Client } from 'colorado-booth-sdk';

const client = new Client({
  oAuthROPCGCredentials: {
    oAuthClientId: 'OAuthClientId',
    oAuthClientSecret: 'OAuthClientSecret',
    oAuthUsername: 'OAuthUsername',
    oAuthPassword: 'OAuthPassword'
  },
});
```



Your application must obtain user authorization before it can execute an endpoint call in case this SDK chooses to use *OAuth 2.0 Resource Owner Password Credentials Grant*. This authorization includes the following steps

The `fetchToken` method will exchange the user's credentials for an *access token*. The access token is an object containing information for authorizing client requests and refreshing the token itself.

```ts
try {
  const token = await client.oAuthROPCGManager?.fetchToken();
  if (token) {
    client.withConfiguration({
      oAuthROPCGCredentials: {
        ...config.oAuthROPCGCredentials,
        oAuthToken: token
      }
    });
  }
} catch(error) {
  // handle ApiError or OAuthProviderError if needed
}
```

The client can now make authorized endpoint calls.

### Refreshing the token

An access token may expire after sometime. To extend its lifetime, you must refresh the token.

```ts
try {
  const token = await client.oAuthROPCGManager?.refreshToken();
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
  oAuthROPCGCredentials: {
    ...config.oAuthROPCGCredentials,
    oAuthToken: token
  }
});
```

### Complete example



```ts
import {
  Client,
  OAuthProviderError,
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
  oAuthROPCGCredentials: {
    oAuthClientId: 'OAuthClientId',
    oAuthClientSecret: 'OAuthClientSecret',
    oAuthUsername: 'OAuthUsername',
    oAuthPassword: 'OAuthPassword'
  },
});

// obtain access token, needed for client to be authorized
const previousToken = await loadTokenFromDatabase();
if (previousToken) {
  // restore previous access token and update the cloned configuration with the token
  client.withConfiguration({
    oAuthROPCGCredentials: {
      ...config.oAuthROPCGCredentials,
      oAuthToken: previousToken
    }
  });
} else {
  // obtain a new access token
  try {
    const token = await client.oAuthROPCGManager?.fetchToken();
    if (token) {
        await saveTokenToDatabase(token);
        client.withConfiguration({
          oAuthROPCGCredentials: {
            ...config.oAuthROPCGCredentials,
            oAuthToken: token
          }
        });
    }
  } catch (error) {
      if (error instanceof OAuthProviderError) {
          // handle OAuthProviderError if needed
      }
    }
}
```


