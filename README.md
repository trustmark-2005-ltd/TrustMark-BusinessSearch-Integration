# TrustMark BusinessSearch Integration

## Introduction

The TrustMark BusinessSearch Integration API allows consumers to search for businesses (business profiles) that exist within the TrustMark Communications Platform.

## Access

API key provided by TrustMark is needed to access the API. It should be added as `x-api-key` header.

## Response Codes

TrustMark uses [HTTP response status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) to indicate the success or failure of your API requests.

### 2xx

Success status codes confirm that your request worked as expected.

### 400 Bad Request

Almost likely that a missing or malformed request has been provided, often due to incorrect data-types being provided.

### 429 Too Many Requests

You have exceeded your usage quota of requests.

### 5xx

Error status codes indicate an error. Whilst in this early stage we would always appreciate feedback and your http request used to cause the error.

## Endpoints

### BusinessProfiles

> GET /BusinessProfiles

Allows searching for businesses (business profiles) that exist within the TrustMark Communications Platform. Provides multiple query string filter parameters which can be used to narrow down the results:

| Field | Information |
| --- | --- |
| location | A location where businesses provide their services or where their headquarters is located (depending on `locationSearchType`). Postcode, town name or local authority (district) can be provided here. The provided value is used as an input string to find location details with use of Google services. It is translated to predefined search areas or specific point on the map + 30 miles radius. If this field is provided it is used to sort the returned businesses on how close they are located to a given point. |
| countries | Multiple predefined countries can be provided in addition to the `location` field. Narrows down the search results to businesses providing their services / located in given countries (depending on `locationSearchType`). Allowed countries are defined in [Dictionary Data](dictionary_data.md). |
| regions | Multiple predefined regions can be provided in addition to the `location` field. Narrows down the search results to businesses providing their services / located in given regions (depending on `locationSearchType`). Allowed regions are defined in [Dictionary Data](dictionary_data.md). |
| districts | Multiple predefined districts can be provided in addition to the `location` field. Narrows down the search results to businesses providing their services / located in given districts (depending on `locationSearchType`). Allowed districts are defined in [Dictionary Data](dictionary_data.md). |
| locationSearchType | Defines how the location parameters (`location`, `countries`, `regions`, `districts`) are interpreted: either where businesses provide their services or where their headquarters is located. Allowed types: `WhereTheBusinessProvidesTheirServices`, `WhereTheBusinessIsLocated`. Default: `WhereTheBusinessProvidesTheirServices`. |
| tradeCodes | Allows to narrow down the search results to businesses registered for the provided trades (trade codes). |
| licenceNumber | Allows to find a business by TrustMark licence number (TMLN). It has to be an exact match. |
| tradeStandards | Allows to narrow down the search results to businesses registered for the provided trade standards. |
| tradingStandardsApproved | Allows to narrow down the search results to businesses with trading standards approved (`true`) or without trading standards approved (`false`). |
| nationalBusinesses | Allows to narrow down the search results to national businesses only (`true`) or local businesses only (`false`). |
| pageIndex | Page index number. Default: 0, which means a first page. |
| pageSize | Number of businesses returned within a single page. Default: 10. Max: 100. |

#### Response

```json
{
    "results": [
        {
            "id": "string",
            "companyName": "string",
            "tradingStandardsApproved": boolean,
            "nationalBusiness": boolean,
            "aboutInformation": "string?",
            "address": {
                "town": "string?",
                "county": "string?",
                "postcode": "string?"
            },
            "trades": [
                {
                    "code": number,
                    "name": "string?",
                    "description": "string?"
                }
            ],
            "phoneNumbers": [
                "string"
            ],
            "websites": [
                "string"
            ],
            "communicationsPlatformLink": "string"
        }
    ],
    "totalItemsCount": number
}
```

#### Example error response

> 400 Bad Request

```json
{
    "title": "One or more validation errors occurred.",
    "errors": {
        "PageSize": [
            "'Page Size' must be less than or equal to '100'."
        ]
    }
}
```
