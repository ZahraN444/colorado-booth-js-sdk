
# Getting Started with MultiAuth-Sample

## Introduction

API for Markdown Notes app.

## Install the Package

Run the following command from your project directory to install the package from npm:

```bash
npm install colorado-booth-sdk@1.0.5
```

For additional package details, see the [Npm page for the colorado-booth-sdk@1.0.5 npm](https://www.npmjs.com/package/colorado-booth-sdk/v/1.0.5).

## Test the SDK

To validate the functionality of this SDK, you can execute all tests located in the `test` directory. This SDK utilizes `Jest` as both the testing framework and test runner.

To run the tests, navigate to the root directory of the SDK and execute the following command:

```bash
npm run test
```

Or you can also run tests with coverage report:

```bash
npm run test:coverage
```

## Initialize the API Client

**_Note:_** Documentation for the client can be found [here.](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/client.md)

The following parameters are configurable for the API Client:

| Parameter | Type | Description |
|  --- | --- | --- |
| accessToken2 | `string` |  |
| port | `string` | *Default*: `'80'` |
| suites | `SuiteCodeEnum` | *Default*: `SuiteCodeEnum.Hearts` |
| environment | [`Environment`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/README.md#environments) | The API environment. <br> **Default: `Environment.Testing`** |
| timeout | `number` | Timeout for API calls.<br>*Default*: `0` |
| httpClientOptions | [`Partial<HttpClientOptions>`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/http-client-options.md) | Stable configurable http client options. |
| unstableHttpClientOptions | `any` | Unstable configurable http client options. |
| basicAuthCredentials | [`BasicAuthCredentials`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/basic-authentication.md) | The credential object for basicAuth |
| apiKeyCredentials | [`ApiKeyCredentials`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/custom-query-parameter.md) | The credential object for apiKey |
| apiHeaderCredentials | [`ApiHeaderCredentials`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/custom-header-signature.md) | The credential object for apiHeader |
| oAuthCCGCredentials | [`OAuthCCGCredentials`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/oauth-2-client-credentials-grant.md) | The credential object for oAuthCCG |
| oAuthACGCredentials | [`OAuthACGCredentials`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/oauth-2-authorization-code-grant.md) | The credential object for oAuthACG |
| oAuthROPCGCredentials | [`OAuthROPCGCredentials`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/oauth-2-resource-owner-credentials-grant.md) | The credential object for oAuthROPCG |
| oAuthBearerTokenCredentials | [`OAuthBearerTokenCredentials`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/oauth-2-bearer-token.md) | The credential object for oAuthBearerToken |

The API client can be initialized as follows:

### Code-Based Client Initialization

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

### Configuration-Based Client Initialization

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

See the [Configuration-Based Client Initialization](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/configuration-based-client-initialization.md) section for details.

### Environment-Based Client Initialization

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

See the [Environment-Based Client Initialization](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/environment-based-client-initialization.md) section for details.

## Environments

The SDK can be configured to use a different environment for making API calls. Available environments are:

### Fields

| Name | Description |
|  --- | --- |
| Production | - |
| Testing | **Default** |

## Authorization

This API uses the following authentication schemes.

* [`basicAuth (Basic Authentication)`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/basic-authentication.md)
* [`apiKey (Custom Query Parameter)`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/custom-query-parameter.md)
* [`apiHeader (Custom Header Signature)`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/custom-header-signature.md)
* [`OAuthCCG (OAuth 2 Client Credentials Grant)`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/oauth-2-client-credentials-grant.md)
* [`OAuthACG (OAuth 2 Authorization Code Grant)`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/oauth-2-authorization-code-grant.md)
* [`OAuthROPCG (OAuth 2 Resource Owner Credentials Grant)`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/oauth-2-resource-owner-credentials-grant.md)
* [`OAuthBearerToken (OAuth 2 Bearer token)`](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/auth/oauth-2-bearer-token.md)
* `CustomAuth (Custom Authentication)`

## List of APIs

* [Authentication](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/controllers/authentication.md)

## SDK Infrastructure

### Configuration

* [HttpClientOptions](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/http-client-options.md)
* [RetryConfiguration](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/retry-configuration.md)
* [ProxySettings](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/proxy-settings.md)
* [Configuration-Based Client Initialization](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/configuration-based-client-initialization.md)
* [Environment-Based Client Initialization](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/environment-based-client-initialization.md)

### HTTP

* [HttpRequest](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/http-request.md)

### Utilities

* [ApiResponse](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/api-response.md)
* [ApiError](https://www.github.com/ZahraN444/colorado-booth-js-sdk/tree/1.0.5/doc/api-error.md)

