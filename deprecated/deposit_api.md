# CrossRef RESTful Deposit API (Alpha)

## Versioning

| Date | Version | Changes |
|------|---------|---------|
| 2014-03-10 | v1 | Initial version |
| 2015-01-15 | v2 | Mention PDF deposits |

Note: this code is used by the Open Journals System (OJS) integration. It is not intended for use by other Crossref members, who should use the production deposit system at http://doi.crossref.org

## Background

CrossRef provides a deposit mechanisms for our members and manuscript system vendors.
The current deposit mechanism provides a URL end-point that can accept deposits of
CrossRef metadata. However, this mechanism has some deficiencies, including:

- an inability to programmatically track the status of a deposit
- an outdated method of returning deposit results to a user (e-mail responses containing
  success or failure notices)
- no way of programmatically querying for a historic list of deposits
- no way of programmatically retrieveing previously deposited XML.

This document proposes a RESTful deposit API that attempts to address deficiencies
within the current CrossRef XML deposit mechanism.

## Extension to the CrossRef REST API

This document provides an extension to the CrossRef REST API, described
[here](https://github.com/CrossRef/rest-api-doc/blob/master/funder_kpi_api.md). It uses the same
API versioning scheme, JSON response format and concepts of singular and listy
responses. The base URI for this API is:

    http://api.crossref.org

However, the routes described in this documentation must be accessed over HTTPS:

    https://api.crossref.org

## Authorization

All routes described in this document require authentication using CrossRef member
credentials. These must be supplied on each requests using the HTTP basic authentication
scheme. Member credentials are provided in a HTTP header as described in
[RFC2617](https://www.ietf.org/rfc/rfc2617.txt). To create an `Authorization` header:

1. Create a string, "username:password"
2. Base64 encode the username, password string
3. Insert this bas64 string into an `Authorization` header:

        Authorization: Basic {base64 encoded user:password pair}

## Making a Deposit

    POST /deposits

Requests to `/deposits` must be authenticated and made over HTTPS.

Deposits are made by POSTing the contents of a deposit to `/deposits`. If initial
checks against the deposit (for example, XML validation) succeed, a `303` redirect
will be returned with a `Location` header defining the deposit status query link:

    HTTP/1.1 303 See Other
    Location: /deposits/1234-5678-1234-5678

Making a deposit with cURL:

    $ curl -i -H "Content-Type: application/vnd.crossref.deposit+xml" -u username:password --data-binary @my-deposit.xml https://api.crossref.org/deposits

Deposits are modified slightly from the submitted form. This is to faciliate
some of the features of this API. Deposit e-mail addresses are changed to an
e-mail address where deposit notifications can be intercepted by this API.
Deposit `batch ID`s are changed to match the deposit ID given out by the API.

Deposits may be made where the e-mail and `batch ID` are left blank. However, the
elements themselves must be present to pass schema validation.

| XPath | Content change |
|-------|----------------|
| //head/doi_batch_id | Changed to match deposit resource ID (as in `/deposits/{ID}`) |
| //head/email_address | Changed to `labs-notifications@crossref.org` |

### Specifying a Deposit Content Type

Deposits POSTed to `/deposits` _must_ specify a deposit content type. This is done
by setting a `Content-Type` header in requests to `/deposits`. It _must_ be set to
one of:

| Content Type | Description |
|--------------|-------------|
| application/vnd.crossref.deposit+xml | [Full deposit](http://doi.crossref.org/schemas/crossref4.3.4.xsd) |
| application/vnd.crossref.partial+xml | [Partial (resource) deposit](http://doi.crossref.org/schemas/doi_resources4.3.2.xsd) |
| application/pdf | PDF deposit for citation extraction |

### Specifying a Ping Back URL

    POST /deposits?pingback={url_encoded_url}

The deposit API can return information on completed or failed deposits to a
user by making a HTTP request to a per-deposit defined URL. Use the `pingback`
parameter to specify a URL encoded ping back URL. The user must specify a URL
that will return a `200` HTTP status response on successfully accepting ping
back information. If the deposit API receives any other HTTP status, or if the
URL is unaccessible for any reason, the API will make repeated requests to the
URL, following a pattern of exponential back off. The maximum number of request
attempts the API will make is undefined.

### Depositing a Test Deposit

    POST /deposits?test=true

Set the `test` parameter to `true`, `t` or `1` (any other value is considered false)
to make a test deposit. Such a deposit will go through the normal deposit process
but its contents will not be made live. By default, `test` is false.

## Listing Previous Deposits

    GET /deposits

Requests to `/deposits` must be authenticated and made over HTTPS.

List previous deposists. The list of deposits can be paged with the `rows` and
`offset` query parameters (see the
[CrossRef REST API documentation](https://github.com/CrossRef/rest-api-doc/blob/master/funder_kpi_api.md)).

The `/deposits` route also specifies some filters:

| Filter | Possible Values | Description |
|--------|-----------------|-------------|
| status | One of `submitted`, `failed` or `completed` | Return only those deposits with given status |
| from-submitted-time | Date | Return only those deposits that were deposited on or after the given date |
| until-submitted-time | Date | Return only those deposits that were deposited on or before the given date |
| doi | DOI | Return only those deposits that deposited against the given DOI |
| test | One of `true`, `t`, `1`, `false`, `f`, `0` | Return only those deposits that are or are not test deposits. By default all deposits, both test and live, are returned. |
| type| Content Type | Return only those deposits with the given content type (mime type) |

Dates should be of the form `YYYY-MM-DD`, `YYYY-MM` or `YYYY`.

For more information on filters, including how to specify them, see the filters section
in the [CrossRef REST API documentation](https://github.com/CrossRef/rest-api-doc/blob/master/funder_kpi_api.md).

## Querying the Status of a Deposit

    GET /deposits/{id}

Requests to `/deposits/{id}` must be authenticated and made over HTTPS.

Retrieve the status of a deposit, including submission status and details of any
errors by making a GET request to the redirect URL returned when making a deposit.

### Error Types

| Major Type | Minor Type |
|------------|------------|
| submission | added-with-conflict |
|            | version-older-than-last |
|            | title-deleted-by-crossref-admin |
|            | npe |
|            | unique-doi |
| permission | not-your-handle |
|            | not-your-prefix |
|            | not-your-title |
|            | not-your-issn |
| xml-syntax | malformed |
|            | schema-validation-fail |
|            | bad-character-data |
|            | content-in-prolog |
|            | bad-character-encoding |
| xml-content | differing-prefixes |
|             | invalid-year |
|             | submission-version-is-null |

## Querying the Deposit Status of a DOI

    GET /deposits?filter=doi:{url_encoded_doi}

Get the deposit history of a DOI. Lists all deposits for a DOI. Useful to combile with
other filters, such as status. For example, check if a given DOI has had a successful
deposit. If it has, those deposits will be returned, if not, no results will return:

    GET /deposits?filter=doi:{url_encoded_doi},status:completed

## Retrieving Original Deposit Objects

    GET /deposits/{id}/data

Requests to `/deposits/{id}/data` must be authenticated and made over HTTPS.

The original deposit XML may be retrieved using the `/deposits/{id}/data` route.
The response `Content-Type` will match the content type specified when depositing
the XML.

