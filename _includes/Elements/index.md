## Base URL

The Cloud Element Base URL for all API calls is `https://api.cloud-elements.com/elements/api-v2`.

HTTP requests to the REST API are protected with HTTP Custom authentication with your Organization and User secret and an Element Instance token. We use many standard HTTP features, like HTTP verbs, understood by most HTTP clients. JSON is returned in all responses from the API, including errors. The APIs have predictable, straightforward URLs and use HTTP response codes to indicate API errors.

## Authenticating with Cloud Elements

To authenticate with Cloud Elements, you need to know your Organization Secret and User Secret. When making some calls, you also need to know the Element Instance token.

When you create an account with us, we assign you an Organization Secret and a User Secret. An Organization is a customer of Cloud Elements (`/organizations`). The User and Organization secrets represent your account with Cloud Elements.

To find your Organization and User Secret open the profile menu.
![Search](/assets/img/elements/org-user-secret-C2.png)

Alternatively, use the` /users/{emailOrId}` endpoint:

```bash
curl -X GET \
  https://staging.cloud-elements.com/elements/api-v2/users/4370 \
  -H 'authorization: User <ADMINISTRATOR_USER_SECRET>, Organization <ORGANIZATION_SECRET>' \
  -H 'content-type: application/json' \
  -H 'elements-user-password: <USER_PASSWORD>' \
```

When you create a new connection to an endpoint, you will receive an Element Instance token. After you create an instance, Cloud Elements automatically refreshes the token behind the scenes so that you won't need to connect your application again.

To find your Element instance token:

        GET /instances/<INSTANCE_ID>

An Element Instance token and a User secret are required to execute one of our Hub API calls (e.g. `/hubs/documents/files` or `/hubs/crm/contacts`). For more information about Hubs, see the Hub API docs in the navigation area.

Pass tokens and secrets as basic HTTP Header values.

* To make a Platform or API call, include the following in the header:

        Authorization: User <INSERT_USER_SECRET>, Organization <INSERT_USER_SECRET>

* To make a Hub API call, include the following in the header:

        User <INSERT_USER_SECRET>, Element <INSERT_ELEMENT_INSTANCE_TOKEN>

{% include error-codes.md %}

Get started by [setting up the API provider](setup.html).
