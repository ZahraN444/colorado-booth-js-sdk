
# Client Class Documentation

The following parameters are configurable for the API Client:

| Parameter | Type | Description |
|  --- | --- | --- |
| accessToken2 | `string` |  |
| port | `string` | *Default*: `'80'` |
| suites | `SuiteCodeEnum` | *Default*: `SuiteCodeEnum.Hearts` |
| environment | [`Environment`](../README.md#environments) | The API environment. <br> **Default: `Environment.Testing`** |
| timeout | `number` | Timeout for API calls.<br>*Default*: `0` |
| httpClientOptions | [`Partial<HttpClientOptions>`](../doc/http-client-options.md) | Stable configurable http client options. |
| unstableHttpClientOptions | `any` | Unstable configurable http client options. |
| basicAuthCredentials | [`BasicAuthCredentials`](auth/basic-authentication.md) | The credential object for basicAuth |
| apiKeyCredentials | [`ApiKeyCredentials`](auth/custom-query-parameter.md) | The credential object for apiKey |
| apiHeaderCredentials | [`ApiHeaderCredentials`](auth/custom-header-signature.md) | The credential object for apiHeader |
| oAuthCCGCredentials | [`OAuthCCGCredentials`](auth/oauth-2-client-credentials-grant.md) | The credential object for oAuthCCG |
| oAuthACGCredentials | [`OAuthACGCredentials`](auth/oauth-2-authorization-code-grant.md) | The credential object for oAuthACG |
| oAuthROPCGCredentials | [`OAuthROPCGCredentials`](auth/oauth-2-resource-owner-credentials-grant.md) | The credential object for oAuthROPCG |
| oAuthBearerTokenCredentials | [`OAuthBearerTokenCredentials`](auth/oauth-2-bearer-token.md) | The credential object for oAuthBearerToken |

The API client can be initialized as follows:

## Code-Based Client Initialization

```ts
import {
  Client,
  Environment,
  OAuthScopeOAuthACGEnum,
  SuiteCodeEnum,
} from 'colorado-booth-sdk';

const client = new Client({
  basicAuthCredentials: {
    username: 'Username',
    password: 'Password'
  },
  apiKeyCredentials: {
    'token': 'token',
    'api-key': 'api-key'
  },
  apiHeaderCredentials: {
    'token': 'token',
    'api-key': 'api-key'
  },
  oAuthCCGCredentials: {
    oAuthClientId: 'OAuthClientId',
    oAuthClientSecret: 'OAuthClientSecret'
  },
  oAuthACGCredentials: {
    oAuthClientId: 'OAuthClientId',
    oAuthClientSecret: 'OAuthClientSecret',
    oAuthRedirectUri: 'OAuthRedirectUri',
    oAuthScopes: [
      OAuthScopeOAuthACGEnum.ReadScope
    ]
  },
  oAuthROPCGCredentials: {
    oAuthClientId: 'OAuthClientId',
    oAuthClientSecret: 'OAuthClientSecret',
    oAuthUsername: 'OAuthUsername',
    oAuthPassword: 'OAuthPassword'
  },
  oAuthBearerTokenCredentials: {
    accessToken: 'AccessToken'
  },
  accessToken2: 'accessToken',
  timeout: 0,
  environment: Environment.Testing,
  port: '80',
  suites: SuiteCodeEnum.Hearts,
});
```

## Configuration-Based Client Initialization

```ts
import * as path from 'path';
import * as fs from 'fs';
import { Client } from 'colorado-booth-sdk';

// Provide absolute path for the configuration file
const absolutePath = path.resolve('./config.json');

// Read the configuration file content
const fileContent = fs.readFileSync(absolutePath, 'utf-8');

// Initialize client from JSON configuration content
const client = Client.fromJsonConfig(fileContent);
```

See the [Configuration-Based Client Initialization](../doc/configuration-based-client-initialization.md) section for details.

## Environment-Based Client Initialization

```ts
import * as dotenv from 'dotenv';
import * as path from 'path';
import * as fs from 'fs';
import { Client } from 'colorado-booth-sdk';

// Optional - Provide absolute path for the .env file
const absolutePath = path.resolve('./.env');

if (fs.existsSync(absolutePath)) {
  // Load environment variables from .env file
  dotenv.config({ path: absolutePath, override: true });
}

// Initialize client using environment variables
const client = Client.fromEnvironment(process.env);
```

See the [Environment-Based Client Initialization](../doc/environment-based-client-initialization.md) section for details.

