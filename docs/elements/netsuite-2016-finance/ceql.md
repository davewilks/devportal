---
heading: NetSuite 2016 Finance
seo: Query Language | NetSuite 2016 Finance | Cloud Elements API Docs
title: Querying
description: Make database queries using Cloud Elements Query Language.
layout: sidebarelementdoc
breadcrumbs: /docs/elements.html
elementId: 988
parent: Back to Element Guides
order: 45
sitemap: false
---

# Cloud Elements Query Language

The Cloud Elements Query Language (CEQL) is a query language used by
Cloud Elements to standardize searching across all of our different
elements. Many APIs support some form of searching in their APIs but
they’re almost all different, so we have standardized a common way to
search across all of our elements. Cloud Elements translates the CEQL to
the endpoint’s searching syntax, however at times, CEQL supports more
than the endpoint.

## CEQL in {{page.heading}}

* Use CEQL to query the following endpoints:
  * GET /campaigns
  * GET /accounts
  * GET /accounts/{accountId}/activities
  * GET /accounts/{accountId}/notes
  * GET /accounts/{accountId}/tasks
  * GET /users
  * GET /{objectName}
  * GET /{objectName}/{objectId}/attachments
  * GET /{objectName}/{objectId}/{childObjectName}
  * GET /opportunities
  * GET /opportunities/{opportunityId}/activities
  * GET /opportunities/{opportunityId}/notes
  * POST /bulk/query
  * GET /contacts
  * GET /contacts/{contactId}/activities
  * GET /contacts/{contactId}/notes
  * GET /contacts/{contactId}/tasks
  * GET /leads
  * GET /leads/{leadId}/activities
  * GET /leads/{leadId}/notes
  * GET /leads/{leadId}/tasks
  * GET /reports/{id}

  {% include ceql-ref.md%}
