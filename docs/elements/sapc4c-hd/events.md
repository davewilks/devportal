---
heading: SAP C4C Helpdesk
seo: Events | SAP C4C Helpdesk | Cloud Elements API Docs
title: Events
description: Enable SAP Anywhere events for your application.
layout: sidebarelementdoc
breadcrumbs: /docs/elements.html
elementId: 3522
parent: Back to Element Guides
order: 25
---

# Events

{% include polling_and_webhooks_defined.md %}

Cloud Elements supports polling events for the following {{page.heading}} resources:

* accounts
* contacts
* incidents
* incidents-statuses

{% include callout.html content="<strong>On this page</strong></br><a href=#configure-polling-through-the-ui>Configure Polling Through the UI</a></br><a href=#configure-polling-through-api>Configure Polling Through API</a></br><a href=#parameters>Parameters</a></br><a href=#example-curl>Example cURL</br><a href=#example-response>Example Response</a></a>" type="info" %}

## Configure Polling Through the UI

Use the {{site.console}} to authenticate with SAP C4C and create an element instance with polling enabled.

To authenticate an element instance with polling:

1. Sign in to Cloud Elements, and then search for the element in our Elements Catalog.

    | Latest UI | Earlier UI  |
    | :------------- | :------------- |
    |  ![Search](../img/Element-Search2.png)  |  ![Search](../img/Element-Search.png)  |

3. Create an element instance.

    | Latest UI | Earlier UI  |
    | :------------- | :------------- |
    | Hover over the element card, and then click __Create Instance__.</br> ![Create Instance](../img/Create-Instance.gif)  | Click __Add Instance__.</br> ![Search](../img/Add-Instance.png)  |

5. Enter a name for the element instance.
6. Complete the required parameters in the Configuration section: **Subdomain**, **Username**, and **Password**. See [Parameters](#parameters) for descriptions.
7. Switch **Events Enabled** on.
8. Add an Event Notification Callback URL.
4. Use the __Event poller refresh interval (mins)__ slider or enter a number in minutes to specify how often Cloud Elements should poll for changes.
5. Select and configure the resources to poll.

    | Latest UI | Earlier UI  |
    | :------------- | :------------- |
    | Select the resources to poll. </br>Optionally, click the pencil icon to further configure polling. | Edit the JSON to add or remove resources and optionally change the `datesConfiguration`.  |

7. Click __Create Instance__ (latest UI) or __Next__ (earlier UI).
8. Optionally add tags in the earlier UI:
     1. On the Tag It page, enter any tags that might help further define the instance.
      * To add more than one tag, click __Add__ after each tag.
      ![Add tag](../img/Add-Tag.png)
     1. Click __Done__.
9. Note the **Token** and **ID** and save them for all future requests using the element instance.
8. Take a look at the documentation for the element resources now available to you.

## Configure Polling Through API

Use the `/instances` endpoint to authenticate with SAP C4C and create an element instance with polling enabled.

{% include note.html content="The endpoint returns an Element token upon successful completion. Retain the token for all subsequent requests involving this element instance.  " %}

To authenticate an element instance with polling:

1. Construct a JSON body as shown below (see [Parameters](#parameters)):

    ```json
    {
        "element": {
          "key": "sapc4chd"
        },
        "configuration": {
          "subdomain": "<domain>.crm.ondemand.com",
          "username": "<YOUR_SAP_C4C_USERNAME>",
          "password": "<YOUR_SAP_C4C_PASSWORD>",
          "event.notification.enabled": true,
          "event.notification.callback.url": "http://mycoolapp.com",
          "event.poller.refresh_interval": "15",
          "accounts": {
            "url": "/hubs/helpdesk/accounts?where=ChangedOn>='${date:yyyy-MM-dd'T'HH:mm:ss'Z'}",
           "idField": "ObjectID",
           "datesConfiguration": {
             "updatedDateField": "ChangedOn",
             "updatedDateFormat": "milliseconds",
             "createdDateField": "CreatedOn",
             "createdDateFormat": "milliseconds"
           },
            "contacts": {
              "url": "/hubs/helpdesk/contacts?where=ChangedOn>='${date:yyyy-MM-dd'T'HH:mm:ss'Z'}'",
              "idField": "ObjectID",
              "datesConfiguration": {
                "updatedDateField": "ChangedOn",
                "updatedDateFormat": "milliseconds",
                "createdDateField": "CreatedOn",
                "createdDateFormat": "milliseconds"
              }
            },
            "incidents": {
              "url": "/hubs/helpdesk/incidents?where=ChangedOn>='${date:yyyy-MM-dd'T'HH:mm:ss'Z'}",
              "idField": "ObjectID",
              "datesConfiguration": {
                "updatedDateField": "ChangedOn",
                "updatedDateFormat": "milliseconds",
                "createdDateField": "CreatedOn",
                "createdDateFormat": "milliseconds"
              }
            },
            "incidents-statuses": {
              "url": "/hubs/helpdesk/incidents-statuses?where=ChangedOn>='${date:yyyy-MM-dd'T'HH:mm:ss'Z'}",
              "idField": "ObjectID",
              "datesConfiguration": {
                "updatedDateField": "ChangedOn",
                "updatedDateFormat": "milliseconds",
                "createdDateField": "CreatedOn",
                "createdDateFormat": "milliseconds"
              }
            }
          },
        },
        "tags": [
          "ElementDocs"
          ],
        "name": "<AUTHENTICATED_INSTANCE_NAME>"
    }
    ```
1. Call the following, including the JSON body you constructed in the previous step:

        POST /instances

    {% include note.html content="Make sure that you include the User and Organization keys in the header. See <a href=index.html#authenticating-with-cloud-elements>the Overview</a> for details. " %}

1. Locate the `token` and `id` in the response and save them for all future requests using the element instance.

## Parameters

API parameters not shown in the {{site.console}} are in `code formatting`.

{% include note.html content="Event related parameters are described in <a href=slaesforce-events.html>Events</a>." %}

| Parameter | Description   | Data Type |
| :------------- | :------------- | :------------- |
| 'key' | The element key.<br>sapc4chd  | string  |
|  Name</br>`name` |  The name for the element instance created during authentication.   | string  |
| Subdomain </br>`subdomain`| The url of your {{page.heading}} site. Replace <domain> in the default url with your own information.    |   |string |
| Username | Your user name for {{page.heading}}. | String |
| Password | Your password for {{page.heading}}. | String |
| Events Enabled </br>`event.notification.enabled` | *Optional*. Identifies that events are enabled for the element instance.</br>Default: `false`.  | boolean |
| Event Notification Callback URL</br>`event.notification.callback.url` |  The URL where you want Cloud Elements to send the events. | string |
| Event poller refresh interval (mins)</br>`event.poller.refresh_interval`  | A number in minutes to identify how often the poller should check for changes. |  number|
| Configure Polling </br>`"event.poller.configuration"` | Resource to poll along with its configuration. | JSON Object |
| accounts</br>`"event.poller.configuration": "{\"accounts\"...}"`|*Optional*</br>The {{page.heading}} `accounts` resource available for polling. |JSON object |
| contacts</br>`"event.poller.configuration": "{\"contacts\"...}"`  | *Optional*</br>The {{page.heading}} `contacts` resource available for polling.  |JSON object |
| leads</br>`"event.poller.configuration": "{\"leads\"...}"`  | *Optional*</br>The {{page.heading}} `incidents` resource available for polling.  |JSON object |
| opportunities</br>`"event.poller.configuration": "{\"opportunities\"...}"`  | *Optional*</br>The {{page.heading}} `incidents-statuses` resource available for polling.  |JSON object |
| tags | *Optional*. User-defined tags to further identify the instance. | string |

## Example cURL

```
curl -X POST \
  https://staging.cloud-elements.com/elements/api-v2/instances \
  -H 'Authorization: User <INSERT>, Organization <INSERT>'  \
  -H 'content-type: application/json' \
  -d '{
  "element": {
	"key": "sapc4chd"
  },
  "configuration": {
    "subdomain": "<domain>.crm.ondemand.com",
    "username": "<USERNAME>",
    "password": "<YOUR_SAP_C4C_PASSWORD>",
          "event.notification.enabled": true,
          "event.notification.callback.url": "http://mycoolapp.com",
          "event.poller.refresh_interval": "15",
          "incidents": {
            "url": "/hubs/helpdesk/incidents?where=ChangedOn>='\''${date:yyyy-MM-dd'\''T'\''HH:mm:ss'\''Z'\''}",
           "idField": "ObjectID",
           "datesConfiguration": {
             "updatedDateField": "ChangedOn",
             "updatedDateFormat": "milliseconds",
             "createdDateField": "CreatedOn",
             "createdDateFormat": "milliseconds"
           }
}
  },
  "tags": [
	"ElementDocs"
  ],
  "name": "<AUTHENTICATED_INSTANCE_NAME>"
}
'
```

## Example Response

```json
{
  "id": 50753,
  "name": "SAPC4CAPI1",
  "createdDate": "2017-05-19T14:00:59Z",
  "token": "4RFxtlivv2BW9oAoO64wCnLvwwps4SPCf6LyCUq8Ihg=",
  "element": {
    "id": 5354,
    "name": "SAP C4C Helpdesk",
    "key": "sapc4chd",
    "description": "Add a SAP Cloud for Customer (C4C) Instance to connect your existing SAP Cloud for Customer (C4C) account to the Helpdesk Hub, allowing you to manage accounts, contacts, incidents, etc. across multiple Helpdesk Elements. You will need your SAP Cloud for Customer (C4C) account information to add an instance.",
    "image": "elements/provider_sapc4c.png",
    "active": true,
    "deleted": false,
    "typeOauth": false,
    "trialAccount": false,
    "resources": [],
    "transformationsEnabled": true,
    "bulkDownloadEnabled": true,
    "bulkUploadEnabled": true,
    "cloneable": true,
    "extendable": false,
    "beta": false,
    "authentication": {
      "type": "basic"
    },
    "extended": false,
    "hub": "Helpdesk",
    "protocolType": "odata",
    "parameters": [
      {
        "id": 3990,
        "createdDate": "2017-05-17T09:37:05Z",
        "name": "subdomain",
        "vendorName": "siteUrl",
        "type": "configuration",
        "vendorType": "path",
        "source": "request",
        "elementId": 5353,
        "required": false
      }
    ],
    "private": false
  },
  "elementId": 5353,
  "provisionInteractions": [],
  "valid": true,
  "disabled": false,
  "maxCacheSize": 0,
  "cacheTimeToLive": 0,
  "configuration": {  },
  "eventsEnabled": true,
  "eventsNotificationCallbackUrl": "http://mycoolapp.com",
  "subscriptionId": 1157990,
  "traceLoggingEnabled": false,
  "cachingEnabled": false,
  "externalAuthentication": "none",
  "user": {  }
}
```