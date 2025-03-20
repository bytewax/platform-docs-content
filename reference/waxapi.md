This section explains how to use WaxAPI.

## Security

WaxAPI uses bearer authentication (also knows as token authentication) to identify the entity that is requesting the API.

Every call must include the `Authorization` header. The value of the header should be the word `Bearer`, followed by a space, followed by an access token obtained from the Identity Provider configured in the Bytewax Platform.

This is an example of a request to WaxAPI:

```bash
curl https://waxapi.yourcompany.com/dataflows/
-H "Authorization: Bearer <TOKEN>"
```

## Status Codes

WaxAPI uses conventional HTTP status codes to indicate the success or failure of an API request.

| Code Range | Description |
|------------|-------------|
| Codes in the `2xx` range | Indicate success |
| Codes in the `4xx` range | Indicate an error caused by the client request |
| Codes in the `5xx` range | Indicate an error inside WaxAPI |

## OpenAPI

WaxAPI exposes a Swagger UI (OpenAPI v2) on `/swagger/index.html` and a full spec in `/swagger/doc.json`

So, if you have the Bytewax Platform installed, you should check any of those resources to know the details about WaxAPI.

We also expose in the Bytewax Platform documentation the same JSON file mentioned [here](/api/openapi/v0.7.2).
With a tool like Postman, you can check WaxAPI endpoints importing that specification.
