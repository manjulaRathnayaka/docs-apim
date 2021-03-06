# Getting Started - Developer Portal API v1

WSO2 API Manager is a complete solution for publishing APIs, creating and managing a developer community, and for scalably routing API traffic. It leverages proven, production-ready, integration, security and governance components from WSO2 Enterprise Service Bus, WSO2 Identity Server, and WSO2 Governance Registry. Moreover, it is powered by API Manager Analytics, thereby making WSO2 API Manager ready for any large-scale deployments right away.

The REST API is implemented based on REST best practices and specifications as a CXF REST web application running on WSO2 API Manager. API development is started with a Swagger specification with a contract-first approach. Please see full [Swagger definition](https://raw.githubusercontent.com/wso2/carbon-apimgt/v6.6.163/components/apimgt/org.wso2.carbon.apimgt.rest.api.store/src/main/resources/store-api.yaml) which is written using Swagger 2.0. This can be also retrieved from the Web app itself using the URL `https://<host-name[:port]>/api/am/store/v1/swagger.yaml`.

The API comes with a pluggable security mechanism. Since API security is implemented as a CXF handler, if you need to plug a custom security mechanism, you can write your own handler and add it to the web service.

Before invoking the API with the access token, obtain the consumer key/secret key pair by calling the dynamic client registration endpoint. You can request an access token with the preferred grant type. An example is shown below,

```
curl -X POST -H "Authorization: Basic YWRtaW46YWRtaW4=" -H "Content-Type: application/json" -d @payload.json https://localhost:9443/client-registration/v0.16/register
```

Sample request:

```
{
   "callbackUrl":"www.google.lk",
   "clientName":"rest_api_store",
   "owner":"admin",
   "grantType":"password refresh_token",
   "saasApp":true
}
```

Sample response:

```
{
   "callBackURL":"www.google.lk",
   "jsonString":"{ \"username\":\"admin\", \"redirect_uris\":\"www.google.lk\", \"client_name\":\"admin_rest_api_store\", \"grant_types\":\"authorization_code password refresh_token iwa:ntlm urn:ietf:params:oauth:grant-type:saml2-bearer client_credentialsimplicit\" }",
   "clientName":null,
   "clientId":"HfEl1jJPdg5tbtrxhAwybN05QGoa",
   "clientSecret":"l6c0aoLcWR3fwezHhc7XoGOht5Aa"
}
```

During the API invocation process request, click the CXF handler first, which calls an introspection API to validate the token. Generate the access token using the already created OAuth application. A sample call to generate the access token is shown below.

Note: Access token must be generated using correct scope for the resource. Scope for each resource is given in resource documentation.

```
curl -k -d "grant_type=password&username=admin&password=admin&scope=apim:subscribe" -H "Authorization: Basic SGZFbDFqSlBkZzV0YnRyeGhBd3liTjA1UUdvYTpsNmMwYW9MY1dSM2Z3ZXpIaGM3WG9HT2h0NUFh" https://localhost:8243/token
```

Token response:

```
{
   "scope":"apim:subscribe",
   "token_type":"Bearer",
   "expires_in":3600,
   "refresh_token":"33c3be152ebf0030b3fb76f2c1f80bf8",
   "access_token":"292ff0fd256814536baca0926f483c8d"
}
```

Now you have a valid access token, which you can use to invoke an API. Navigate through the API descriptions to find the required API, obtain an access token as described above and invoke the API with the authentication header. If you use a different authentication mechanism, this process may change.
