# RESTful APIs

**Predictable and Consistent**


## REST vs. gRPC vs. GraphQL

- *Designing Web APIs* Table 2-2

## Books

* Designing Web APIs by Brenda Jin, Saurabh Sahni, and Amir Shevat (O'Reilly, 2018) 
* Irresistable APIs by Kirsten Hunter (Manning, 2016)
* The Design of Web APIs by Arnaud Lauret (Manning, 2019 )
* Design and Build Great Web APIs by Mike Amundsen (Pragmatic Bookshelf, 2020)
* API Security in Action by Neil Madden (Manning, 2021)
* Mastering API Architecture by James Gough, Daniel Bryant, and Matthew Auburn (O'Reilly, 2022)
* REST in Practice by Jim Webber, Savas Parastatidis and Ian Robinson (O'Reilly, 2010)
https://pages.apigee.com/rs/apigee/images/api-design-ebook-2012-03.pdf

## Articles / Sites

* [REST APIs must be hypertext-driven](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven) by Roy T. Fielding
* https://restfulapi.net/
* https://jsonapi.org/
* https://apievangelist.com

## RESTful API examples

* [Jive REST API](https://developers.jivesoftware.com/api/v3/cloud/rest/)
* [Stripe API](https://stripe.com/docs/api)
* [STAC API](https://github.com/radiantearth/stac-api-spec)
* [GitHub API](https://docs.github.com/en/rest)
* [Zendesk API](https://developer.zendesk.com/rest_api/docs/support/users#create-or-update-many-users)

## Specification

OpenAPI (fmrly Swagger) is the most popular. 

[ALPS](http://alps.io/) looks cool.

* [APIs.guru](https://github.com/apis-guru/openapi-directory) OpenAPI directory
* Chapter 4 in Laurent
* [OpenAPI Map](https://openapi-map.apihandyman.io/)
* [VS Code Swagger Viewer](https://marketplace.visualstudio.com/items?itemName=Arjun.swagger-viewer)

## Documentation

* [ReDoc](https://github.com/Redocly/redoc) for nice reference documentation
* Add a Getting Started guide! 
* Add a common scenarios guide!
* Documentation should tell a story (Hunter)

## Utility Services

* [Apiary.io -- Mock API to start](https://apiary.io/how-apiary-works)
* Service implementing http://httpbin.org/
* https://httpstatuses.com
* CLI client https://httpie.io/

## Principles

* Think "API First" -- webapp as clients (SPA, mobile app, bots) + API, not traditional backend that renders web.
* Prefer **hypermedia** (a root URI and navigation via link rels) to fixed interface (requires OOB info) (Laurent Ch 6.3) -- Hypermedia As The Engine Of Application State, or HATEOAS -- dynamically discover resources via rels
* Think **entity resources**, not **action resources**
* Fine-grained, composable resources, but it's also okay to have resources that make the client's job easier.
* DARRT stands for data, actions, resources, representations, and transitions (Amundsen)
* Documentation should tell a story. Getting Started, full docs, scenarios.

## Practices

- Consider data wrappers
  - Don't ever return a raw array -- wrap it in a single-field object, or you'll never be able to add any additional fields to the result
  - don't use an array with positional semantics, just create an object -- example: GeoJSON bbox
- RFC 3339 for datetimes.
- gzip compression for responses (may not be enabled by default in your framework)
- Manage API versions independently of software implementation.

## Patch

Patch is actually pretty hard. [JSON Patch](http://jsonpatch.com/) describes the details.

[Gnieh Diffson](https://github.com/gnieh/diffson) is a great Scala library for implementing it.

## Auth

OAuth, please. Header `Authentication` is best. Maybe add query parameter `access_token` too so that it can be
called with simple GET requests or redirects (though this reveals the token in the request).

Auth0 is a good platform for handling OAuth-based authentication.

Make scopes more granular than you think you'll need them.

[JWT.io](https://jwt.io/) does in-browser decoding.

- *Designing Web APIs* Chapter 3 section *OAuth Best Practices*

## Testing

???????????

## Batch/Bulk Updates

* [Google Drive](https://developers.google.com/drive/api/v3/batch)
* Zendesk [blog](https://developerblog.zendesk.com/from-100-requests-to-1-introducing-our-new-bulk-and-batch-apis-a5bb294e2132) and [API docs](https://developer.zendesk.com/rest_api/docs/support/users#create-or-update-many-users)

## Versioning

This is probably the hardest part!

* uri or path
* Content-Type application/vnd.pv.api.v2.3+json

### References

- Design and Build Great Web APIs Ch 12

### Principles and Practices

- Never remove, rename, or redefine resources
- Additions should be backwards-compatible and optional -- never mandatory.
  
- Put a version in the host or path. Host is easy w/ multi-tenant services and a reasonable deployment system. And, API gateways can redirect traffic however need be anyway
  - E.g., http://api.v1.example.org/items
  - E.g., http://api.example.org/v1/items


## Links

- [HTTP Link Hints](https://mnot.github.io/I-D/link-hint/)
- [HAL](https://stateless.group/hal_specification.html), [Siren](https://github.com/kevinswiber/siren), Collection+JSON, JSON API, JSON-LD, Hydra
- [IANA Link Relations](http://www.iana.org/assignments/link-relations/link-relations.xhtml)

## Errors

- [Problem Details for HTTP APIs](https://tools.ietf.org/html/rfc7807)
- "Organize your errors into status codes, headers, machine-readable codes, and human-readable strings" (DWAPIs)

## Other

* Instead of polling, consider thin webhooks (webhook calls with "there was a change" and a thin payload, and then the request is made to get the thick object), or a combination of both

## Design Documentation

- Amundsen's API Project Assets Checklist
- API Technical Specification from DWAPIs: Title, Authors, Problem, Solution, Implementation, Authentication, Other things we considered

## Other Endpoints

- Bulk ingest -- not RESTful, but useful. Difficult to do well.
- Search - filter, date filter, order, fields
- Pagination
  - offset-based -- difficult for non-relational dbs, and always expensive for later pages
  - cursor-based -- still expensive. Elasticsearch has "search after". Usually coupled to datastore.

Pagination Link -- merge means apply these changes on top of your previous request.

```json
{
            "rel": "next",
            "href": "http://api.cool-sat.com/search",
            "method": "POST",
            "body": {
                "page": 2,
                "limit": 10
            },
            "merge": true
        }
```

- Rate-limiting -- DWAPIs has a great overview of the algorithms
- 

## Implementation Considerations

- Caching -- especially challenging when OOB writes are allowed!
- Conditional -- `If-None-Match` and `ETag`

## Leonard Richardson’s Maturity Model

- Level 0: The swamp of POX
- Level 1: Resources
- Level 2: HTTP verbs
- Level 3: Hypermedia controls (HATEOAS, for Hypermedia as the Engine of Application State)
