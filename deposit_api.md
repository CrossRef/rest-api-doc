# CrossRef RESTful Deposit API

## Versioning

| Date | Version | Changes |
|------|---------|---------|
| 2014-03-10 | v1 | Initial version |

## Background

CrossRef provides two deposit mechanisms for our members and manuscript system vendors.
The first is an API that can accept automated deposits of CrossRef metadata. This API
has some deficiencies, including an inability to programatically track the status of
a deposit, and an outdated system for

This API attempts to address deficiencies within the current CrossRef XML deposit API.

## Extension to the CrossRef REST API

This document provides an extension to the CrossRef REST API, described
[here](http://github.com/CrossRef/rest-api-doc/funder_api_doc.md). It uses the same
API versioning scheme, JSON response format and concepts of singular and listy
responses. The base URI for this API is:

    http://api.crossref.org

However, the routes described in this documentation must be accessed over HTTPS:

    https://api.crossref.org

## Authentication

All routes described in this document require authentication using CrossRef member
credentials. These must be supplied on each requests using the HTTP basic authentication
scheme. Member credentials are provided in a HTTP header as described in
[RFC2617](https://www.ietf.org/rfc/rfc2617.txt).

## Making a Deposit

    POST /deposits

Requests to `/deposits` must be authenticated and made over HTTPS.

Deposits are made by POSTing the contents of a deposit to `/deposits`. If initial
checks against the deposit (for example, XML validation) succeed, a `303` redirect
will be returned with a `Location` header defining the deposit status query link:

    HTTP/1.1 303 See Other
	Location: /deposits/1234-5678-1234-5678

### Specifying a Deposit Content Type

Deposits POSTed to `/deposits` _must_ specify a deposit content type. This is done
by setting a `Content-Type` header in requests to `/deposits`. It _must_ be set to
one of:

| Content Type | Description |
|--------------|-------------|
| application/vnd.crossref.deposit+xml | [Full deposit](http://doi.crossref.org/schemas/crossref4.3.4.xsd) |
| application/vnd.crossref.partial+xml | [Partial (resource) deposit](http://doi.crossref.org/schemas/doi_resources4.3.2.xsd) |

### Specifying a Ping Back URL

    POST /deposits?pingback={url_encoded_url}

The deposit API can return information on completed or failed deposits to a user by making
a HTTP request to a per-deposit defined URL. Use the `pingback` parameter to specify
a URL encoded ping back URL. The user must specify a URL that will return
a `200` HTTP status response on successfully accepting ping back information. If the deposit
API receives any other HTTP status, or if the URL is unaccessible for any reason, the API will
make repeated requests to the URL, following a pattern of exponential back off. The maximum number
of request attempts the API will make is undefined.

## Listing Previous Deposits

    GET /deposits

## Querying the Status of a Deposit

    GET /deposits/{id}

Requests to `/deposits/{id}` must be authenticated and made over HTTPS.

Retrieve the status of a deposit, including submission status and details of any
errors by making a GET request to the redirect URL returned when making a deposit.

## Retrieving Original Deposit Objects

    GET /deposits/{id}/data

Requests to `/deposits/{id}/data` must be authenticated and made over HTTPS.




