## Feux Endpoints Guidelines

* Use nouns when constructing paths.  The nouns should represent the resource the endpoint operates on.  For consistency, choose the plural name.
    * `/campaigns` should return a list of campaigns.  `/campaigns/{campaignId}` should return a single  campaign.
* Don't use underscores.  Do use hypens to improve readability if necessary
* Each resource should have a consistent data model across all endpoints's.
    * the list of skus returned from the search sku endpoint `/accounts/{accountId}/skus/search/text` should have the same data model as the list skus returned from the line item products endpoint `/line-items/{lineItemId}/products`
* UI Data models should follow similar rules used when creating a model in a relational database.  Data should be normalized and relationships represented by foreign keys.  When creating a new UI data model, keep in mind it does not and should not have to be coupled with the back end storage system.
    * `GET /accounts/{accountId}/balances` returns balances that each have a list of campaign ids, as opposed to a list of the entire campaigns
* UI Data models should only include properties required
* Forward slash separator (/) must be used to indicate a hierarchical relationship.
    * `/api​/v1​/line-items​/{lineItemId}​/bid` operates on a line item's bid
* Endpoints that change data should return all modified resources.
    * When updating the campaigns linked to a balance using `PUT /balances/{balanceId}/campaigns`, the endpoint should return the updated balance resource.
* Don't return error codes when response should be empty.
    *  If a campaign is created but does not have any line items, this endpoint should return an empty array `/campaigns/{campaignId}/line-items`.

## Proposal

* `expand` query
    * Sometime's we want the entire nested resources to prevent multiple api requests.  This could be accomplished by using a query parameter `expand` that accepts a list of resources that should be included in the response.  e.g. `GET /accounts/{accountId}/balances?expand=campaigns` would return balances with the campaigns in the response