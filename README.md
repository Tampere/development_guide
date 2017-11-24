# Web API development guide

Guidelines and best practices for development of APIs and Apps. Work in progress.

## Introduction

This API guideline support API development in finnish public sector. It is
adapted and expanded from [Whitehouse API
guidelines](https://github.com/WhiteHouse/api-standards). Contents of this
guideline are technical and directed for developers. It should be used in
conjunction with API instructions provided by 6Aika, for example their
[recommendations](https://www.avoindata.fi/data/dataset/a3a643f5-7905-4b19-abee-90084b855dc1).

This guide will provide basic instructions on how to build a service focusing on
best practices of API design. The guide is divided into several sections which
are recommended to be read before starting service development.

API development should be design-first driven when ever possible. All APIs must
have machine readable and public specification. Use standard format
specifications such as Swagger, open api definition format, RAML or API
Blueprint.

Although this guide provides relatively clear rules, note that these must be
adjusted to at least the following situations:

* The software product is based on another project, which provides an API and the API differs from the rule-set defined by this guideline. In this case, keep your extensions and modifications consistent with the rest of the product. This should not nonetheless dis-encourage you to propose changes to the base product.

* The security guidelines or similar of your organization bring restrictions. You should respect your organization's rules, yet again trying to improve these rules when needed is encouraged.

* Getting to know other API guidelines is good practice: they might offer instructions not mentioned in this guideline, on the other hand they could give a different yet valid option to doing things.

## Guidelines

This document provides guidelines and examples for Web APIs, encouraging consistency, maintainability, and best practices across applications. Aim is to balance a truly RESTful API interface with a positive developer experience (DX).

This document borrows heavily from:

* [Designing HTTP Interfaces and RESTful Web Services](https://www.youtube.com/watch?v=zEyg0TnieLg)
* [API Facade Pattern](http://apigee.com/about/resources/ebooks/api-fa%C3%A7ade-pattern), by Brian Mulloy, Apigee
* [Web API Design](http://pages.apigee.com/web-api-design-ebook.html), by Brian Mulloy, Apigee
* [Fielding's Dissertation on REST](http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)

This documentation version is a minor modification of [6Aika API Development guide](https://github.com/6aika/development_guide).

## Design-First

API development should be design-first driven when ever possible. All APIs must have machine readable and public specification. Use standard format specifications such as Swagger, open api definition format, RAML or API Blueprint.

## Data models

Use national vocabularies in data modeling. This in long term creates semantic inter-operability. Vocabularies are in Finnish, Swedish and English. If national vocabulary for specific need does not exist, use some international standard vocabulary.

**Resources:**

* [iow.csc.fi](http://iow.csc.fi/#/)
* [Finnish vocabularies and ontologies](http://finto.fi/fi/)

## Documentation

[Good documentation](http://launchany.com/10-questions-your-api-document-must-answer/) answers to following questions:

* What can I do (and not do) With Your API?
* Does Your API Fit my Company’s Needs?
* How Does Your API View my World?
* How do You Secure Your API?
* How Long Will it Take to Get Started?
* Do You Offer SDKs?
* What API Endpoints and Event Integrations Does Your API Offer?
* Why am I Getting This Error Code or Unexpected Response?

Often it is best to offer an interactive documentation such as Swagger, that can create API calls and display results within the documentation.

## Developer eXperience

Try your best to make your API as developer friendly and approachable as possible. Steps to achieve this include:

* Good documentation, preferably with interactive elements, such as is with Swagger
* Consistent and meaningful naming conventions
* Simple usage examples
* Perhaps even specific online community tools, such as Slack channel, Discuss, or even IRC
* Make sure the error messages your API returns are explicit and on the point. The error object might have a link to a documentation of the possible reasons it occurred

## Pragmatic REST

The guidelines aim to support a truly RESTful API. Here are a few exceptions:

* Put the version number of the API in the URL (see examples below). Don’t accept any requests that do not specify a version number.
* Allow users to request formats like JSON or XML like this:

  * [http://example.gov/api/v1/magazines.json](http://example.gov/api/v1/magazines.json)
  * [http://example.gov/api/v1/magazines.xml](http://example.gov/api/v1/magazines.xml)

## RESTful URLs

### General guidelines for RESTful URLs

* A URL identifies a resource.
* URLs should include nouns, not verbs.
* Use plural nouns only for consistency (no singular nouns).
* Use HTTP verbs (GET, POST, PUT, DELETE) to operate on the collections and elements.
* You shouldn’t need to go deeper than resource/identifier/resource.
* Put the version number at the base of your URL, for example [http://example.com/v1/path/to/resource](http://example.com/v1/path/to/resource).
* URL v. header:
  * If it changes the logic you write to handle the response, put it in the URL.
  * If it doesn’t change the logic for each response, like OAuth info, put it in the header.
* Specify optional fields in a comma separated list.
* Formats should be in the form of api/v2/resource/{id}.json

### Good URL examples

* List of magazines:
  * GET [http://www.example.gov/api/v1/magazines.json](http://www.example.gov/api/v1/magazines.json)
* Filtering is a query:
  * GET [http://www.example.gov/api/v1/magazines.json?year=2011&sort=desc](http://www.example.gov/api/v1/magazines.json?year=2011&sort=desc)
  * GET [http://www.example.gov/api/v1/magazines.json?topic=economy&year=2011](http://www.example.gov/api/v1/magazines.json?topic=economy&year=2011)
* A single magazine in JSON format:
  * GET [http://www.example.gov/api/v1/magazines/1234.json](http://www.example.gov/api/v1/magazines/1234.json)
* All articles in (or belonging to) this magazine:
  * GET [http://www.example.gov/api/v1/magazines/1234/articles.json](http://www.example.gov/api/v1/magazines/1234/articles.json)
* All articles in this magazine in XML format:
  * GET [http://example.gov/api/v1/magazines/1234/articles.xml](http://example.gov/api/v1/magazines/1234/articles.xml)
* Specify optional fields in a comma separated list:
  * GET [http://www.example.gov/api/v1/magazines/1234.json?fields=title,subtitle,date](http://www.example.gov/api/v1/magazines/1234.json?fields=title,subtitle,date)
* Add a new article to a particular magazine:
  * POST [http://example.gov/api/v1/magazines/1234/articles](http://example.gov/api/v1/magazines/1234/articles)

### Bad URL examples

* Non-plural noun:
  * [http://www.example.gov/magazine](http://www.example.gov/magazine)
  * [http://www.example.gov/magazine/1234](http://www.example.gov/magazine/1234)
  * [http://www.example.gov/publisher/magazine/1234](http://www.example.gov/publisher/magazine/1234)
* Verb in URL:
  * [http://www.example.gov/magazine/1234/create](http://www.example.gov/magazine/1234/create)
* Filter outside of query string
  * [http://www.example.gov/magazines/2011/desc](http://www.example.gov/magazines/2011/desc)

### Identifiers

It is generally not a good practice to default to using auto-incrementing numeric ids for all resources. An id of 1 usually does not correlate to the actual resource in any ways. Natural ids can give more meaning to the user.

#### Good identifier

* /racetracks/Teivo

#### Bad identifier

* /racetracks/1

Not using auto-incrementing ids can also help you by making database migrations easier maintaining data consistency better.

#### Query filters

Use consistent and meaningful names. A good query filter names can be for example `startDate`, `endDate` and `distance`. Avoid using hyphens, dashes or underscores.

## HTTP Verbs

HTTP verbs, or methods, should be used in compliance with their definitions under the [HTTP/1.1](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) standard. The action taken on the representation will be contextual to the media type being worked on and its current state. Here's an example of how HTTP verbs map to create, read, update, delete operations in a particular context:

| HTTP METHOD | POST            | GET       | PUT         | DELETE |
| ----------- | --------------- | --------- | ----------- | ------ |
| CRUD OP     | CREATE          | READ      | UPDATE      | DELETE |
| /dogs       | Create new dogs | List dogs | Bulk update | Delete all dogs |
| /dogs/1234  | Error           | Show Bo   | If exists, update Bo; If not, error | Delete Bo |

(Example from Web API Design, by Brian Mulloy, Apigee.)

## Responses

* No values in keys
* No internal-specific names (e.g. "node" and "taxonomy term")
* Metadata should only contain direct properties of the response set, not properties of the members of the response set

### Good examples

No values in keys:

    "tags": [
      {"id": "125", "name": "Environment"},
      {"id": "834", "name": "Water Quality"}
    ],

### Bad examples

Values in keys:

    "tags": [
      {"125": "Environment"},
      {"834": "Water Quality"}
    ],

### Metadata and result

Often it is best to divide the response into two main objects: the metadata and the actual results. The metadata object can hold values describing the entire resource and the entire result set, while the result object has the requested resource data.

    {
        "metadata": {
            "resultset": {
                "count": 227,
                "offset": 25,
                "limit": 25
            }
        },
        "results": []
    }

The naming conventions here differ. Some use `meta` and `data`, others `results`, etc. The key point is to use generic naming rather than something directly linked to the resource contents.

#### Good names

* data
* results

#### Bad names

* devices
* articles
* events

Using resource-specific naming makes it difficult to consume the API without any real benefits. The developer must know by now what the results are a representation of.

The metadata could also include the used query filter parameters.

#### Resultsets for human-readable results and transfer minification

If the use-case requires to potentionally return hundreds of rows of data, it is usually best to paginate the results into resultsets or pages. This can lower the amount of data the end users needs to transfer per request. The pagination limit (count of items per page) should
be customizable via request parameters, e.g. `&limit=60`.

HAETOAS should be taken into account. This means hyperlinking any relations for easy human navigation of data as well as
rapid application development. 
    
    {
      "id" : "teivo",
      "name": "Teivo race track",
      "events" : [
          {
            "id": "teivo-thursdays",
            "url": "http://example.com/v1/events/teivo-thursdays"
          },
          {
            "id": "teivo-birthday",
            "url": "http://example.com/v1/events/teivo-birthday"
          }
      ]
    }

## Error handling

Error responses should include a common HTTP status code, message for the developer, message for the end-user (when appropriate), internal error code (corresponding to some specific internally determined ID), links where developers can find more info. For example:

    {
      "status" : 400,
      "developerMessage" : "Verbose, plain language description of the problem. Provide developers suggestions about how to solve their problems here",
      "userMessage" : "This is a message that can be passed along to end-users, if needed.",
      "errorCode" : "444444",
      "moreInfo" : "http://www.example.gov/developer/path/to/help/for/444444, http://drupal.org/node/444444",
    }

Use three simple, common response codes indicating (1) success, (2) failure due to client-side problem, (3) failure due to server-side problem:

* 200 - OK
* 400 - Bad Request
* 500 - Internal Server Error

For some status codes the exact HTTP response code is the perfect explanation to the user as to why the request failed. If the API requires, for example, authentication, a 403 Unauthenticated tells the developer precisely what the issue was. The moreInfo -attribute could then point to a method of acquiring authentication credentials and the developerMessage -attribute could tell the developer to include a Authorization -header on all authenticable methods. Also a 404 Not Found is perfectly clear as a status.

For simple read-only APIs the full error object might be an overkill. Sometimes just a status and message might suffice.

    {
      "status" : 404,
      "message" : "The resource you have requested can not be found."
    }

## Versions

* Never release an API without a version number.
* Versions should be integers, not decimal numbers, prefixed with ‘v’. For example:
  * Good: v1, v2, v3
  * Bad: v-1.1, v1.2, 1.3
* Maintain APIs at least one version back.

## Record limits

* If no limit is specified, return results with a default limit.
* To get records 51 through 75 do this:
  * [http://example.gov/magazines?limit=25&offset=50](http://example.gov/magazines?limit=25&offset=50)
  * offset=50 means, ‘skip the first 50 records’
  * limit=25 means, ‘return a maximum of 25 records’

Information about record limits and total available count should also be included in the response. Example:

    {
        "metadata": {
            "resultset": {
                "count": 227,
                "offset": 25,
                "limit": 25
            }
        },
        "results": []
    }

## Request & Response Examples

### API Resources

* [GET /magazines](#get-magazines)
* [GET /magazines/[id]](#get-magazinesid)
* [POST /magazines/[id]/articles](#post-magazinesidarticles)

### GET /magazines

Example: [http://example.gov/api/v1/magazines.json](http://example.gov/api/v1/magazines.json)

**Response body:**

    {
        "metadata": {
            "resultset": {
                "count": 123,
                "offset": 0,
                "limit": 10
            }
        },
        "results": [
            {
                "id": "1234",
                "type": "magazine",
                "title": "Public Water Systems",
                "tags": [
                    {"id": "125", "name": "Environment"},
                    {"id": "834", "name": "Water Quality"}
                ],
                "created": "1231621302"
            },
            {
                "id": 2351,
                "type": "magazine",
                "title": "Public Schools",
                "tags": [
                    {"id": "125", "name": "Elementary"},
                    {"id": "834", "name": "Charter Schools"}
                ],
                "created": "126251302"
            }
            {
                "id": 2351,
                "type": "magazine",
                "title": "Public Schools",
                "tags": [
                    {"id": "125", "name": "Pre-school"},
                ],
                "created": "126251302"
            }
        ]
    }

### GET /magazines/[id]

Example: [http://example.gov/api/v1/magazines/[id].json](http://example.gov/api/v1/magazines/[id].json)

**Response body:**

    {
        "id": "1234",
        "type": "magazine",
        "title": "Public Water Systems",
        "tags": [
            {"id": "125", "name": "Environment"},
            {"id": "834", "name": "Water Quality"}
        ],
        "created": "1231621302"
    }

### POST /magazines/[id]/articles

Example: Create – POST  [http://example.gov/api/v1/magazines/[id]/articles](http://example.gov/api/v1/magazines/[id]/articles)

**Request body:**

    [
        {
            "title": "Raising Revenue",
            "author_first_name": "Jane",
            "author_last_name": "Smith",
            "author_email": "jane.smith@example.gov",
            "year": "2012",
            "month": "August",
            "day": "18",
            "text": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam eget ante ut augue scelerisque ornare. Aliquam tempus rhoncus quam vel luctus. Sed scelerisque fermentum fringilla. Suspendisse tincidunt nisl a metus feugiat vitae vestibulum enim vulputate. Quisque vehicula dictum elit, vitae cursus libero auctor sed. Vestibulum fermentum elementum nunc. Proin aliquam erat in turpis vehicula sit amet tristique lorem blandit. Nam augue est, bibendum et ultrices non, interdum in est. Quisque gravida orci lobortis... "
        }
    ]

## Mock Responses

It is suggested that each resource accept a 'mock' parameter on the testing server. Passing this parameter should return a mock data response (bypassing the backend).

Implementing this feature early in development ensures that the API will exhibit consistent behavior, supporting a test driven development methodology.

Note: If the mock parameter is included in a request to the production environment, an error should be raised.

### Development testing API

Instead of mocking responses based on a parameter, you may also provide a separate development testing API or a testing API token. These would not change the state of the production database.

## CORS

Alternative to JSONP is CORS - Cross-Origin Resource Sharing. APIs are the threads that let you stitch together a rich web experience. But this experience has a hard time translating to the browser, where the options for cross-domain requests are limited to techniques like JSON-P (which has limited use due to security concerns) or setting up a custom proxy (which can be a pain to set up and maintain).

Cross-Origin Resource Sharing (CORS) is a W3C spec that allows cross-domain communication from the browser. By building on top of the XMLHttpRequest object, CORS allows developers to work with the same idioms as same-domain requests.

Read more from [http://enable-cors.org](http://enable-cors.org)

### JSON-P and CORS

API developer may be tempted to disallow direct script access due to server load concerns. This however greatly limits the client developers options, practically forcing them to setup a proxy server or their own data warehouse and scrape the API contents. This is counter-intuitive to the whole idea of approachable open data.

If the API offers really frequently updating data that takes a lot of processing to deliver, it might be better not serving the API directly from your production database. Perhaps you could have a separate server running just the API and that servers' database could just contain the already processed data.

You may also enforce rate limiting where necessary as long as it doesn't render the whole API obsolete. This needs to be evaluated per each use case.

If the data is not really frequently updated, but the data mass is great, you probably should consider caching the results.

## Authorization

Some API's are fully open and anonymous. This means that the user is not required to authenticate. Some use cases might require per-user authentication with account credentials.

If the API is intended for developer use, per-user authentication might not be viable. Again, if the API has writeable endpoints or it is necessary to limit the amount of requests, developer may be given API tokens for per-application authorization. These could be included in each request as a header `Authrorization: Bearer <token>` or as an `api_key` or `api_token` request field. This allows for rate-limiting, use tracking and application rejection. 

Make sure the api token can be easily acquired (e.g. developer portal, API management service) and easily reset in case the api token leaks (e.g. accidental push to GitHub).

Requesting an api token via email is not good developer experience.