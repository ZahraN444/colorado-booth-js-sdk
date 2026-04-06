
# Custom Header Signature



Documentation for accessing and setting credentials for apiHeader.

## Auth Credentials

| Name | Type | Description | Setter |
|  --- | --- | --- | --- |
| token | `string` | - | `token` |
| api-key | `string` | - | `apiKey` |



**Note:** Auth credentials can be set using `apiHeaderCredentials` object in the client.

## Usage Example

### Client Initialization

You must provide credentials in the client as shown in the following code snippet.

```ts
import { Client } from 'colorado-booth-sdk';

const client = new Client({
  apiHeaderCredentials: {
    'token': 'token',
    'api-key': 'api-key'
  },
});
```


