# TrustMark BusinessSearch Integration

## Introduction

The TrustMark BusinessSearch Integration API allows integrators to search for TrustMark Registered Businesses and embed the results into their platform.
Flexible searching is supported permitting the user to combine various geographical options with one or more trade codes. 

Note - this is early preview and may be subject to change.

## Access

Before this API can be used a Data Sharing Agreement must be in place. To request one please draft a summary of your use case for the API and email data@trustmark.org.uk 

If the application is successful and an API key provided by TrustMark it should be added to the `x-api-key` header.

## Service charges
Fees for commercial use of this service (Excluding VAT) are:
    Setup fee: £1,500
    Annual Service Charge: £4,750

Access may be revoked if the service is abused or data shared outside of any agreement.

If your model requires lodgement of work delivered it may be possible to offset some of these fees against lodgement. 

### Embedding search results in a product

The following text must be presented alongside the search results with the TrustMark logo presented.

>Search powered by

_A TrustMark Logo variant that is most suited to your use case will be provided_

>TrustMark is the only UK Government-Endorsed Quality Scheme for home improvements carried out in and around the home. We are passionate about quality and assurance and what that means for homeowners and our Registered Businesses.



## Response Codes

TrustMark uses [HTTP response status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) to indicate the success or failure of your API requests.

### 2xx

Success status codes confirm that your request worked as expected.

### 400 Bad Request

Almost likely that a missing or malformed request has been provided, often due to incorrect data-types being provided.

### 429 Too Many Requests

You have exceeded your usage quota of requests. The usage quota is currently 50k per month. This may be subject to change.  

### 5xx

Error status codes indicate an error. While in this early stage we would always appreciate feedback and your http request used to cause the error.

## Endpoints

URL: https://api.trustmark.org.uk

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
| fundingOptionTypes | Allows to narrow down the search results to the type of funding option (`Grant`, `Incentive`, `Payment Option`). |
| fundingOptionIds | Allows to narrow down the search results to the specific funding option ID. |
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
                    "tradeCode": number,
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
            "communicationsPlatformLink": "string",
            "tmln": "string"
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

#### Example requests

> Basic request with no filters, returning all business profiles (first page, 10 records)

`/business-profiles`

> Request returning business profiles with trading standards approved **and** registered for the provided trades (24 **or** 91), second page of results

`/business-profiles?tradingStandardsApproved=true&tradeCodes=24&tradeCodes=91&pageIndex=1`

> Request returning profiles of businesses located in London

`/business-profiles?location=london`

> Request returning profiles of businesses located in Cardiff offering Loft and Cavity Wall insulation 

`/business-profiles?tradeCodes=93&tradeCodes=91&location=Cardiff`






