Archive Arrangement Query API
=============================

# Change History

| Date | Changes |
|------|---------|
| 2014-01-13 | Initial version |

# Background

CrossRef metadata can record the archive arrangement for a piece of content. For example,
a publisher may deposit copies of their published content with an archive organisation
such as CLOCKSS or LOCKSS and record this fact in metadata deposited to CrossRef. Each
DOI record can maintain archive arrangement information separately:

    <journal_article publication_type="full_text">
    
        ...
		 
        <archive_locations>
	        <archive name="CLOCKSS"/>
		    <archive name="LOCKSS"/>
        </archive_locations>
    
        ...
    
    </journal_article>

While the archive arrangement information in CrossRef metadata records a publisher's
intent to archive a certain piece of content with a particular organisation, it does
not confirm the existence of an archive copy. A third party wishing to validate the
publisher's claim of an archive arrangement must query the archive organisation
for the archive state of a piece of content.

To faciliate archive arrangement verification, a common query API is proposed that
allows any third party to look up the archive state of a piece of content identified
by a DOI.

# DOI Query Path

A query API implementor must provide a publicly-accessible DOI query path. The path must
either accept a DOI as a query parameter named `doi`:

    http://anarchive.org/status?doi={DOI}

or, alternatively, accept a DOI as part of a HTTP path:

    http://anarchive.org/status/{DOI}

## DOI Syntax

The implementor must handle the issues of embedding DOIs within HTTP paths and query strings.
Refer to the `Numbering` section of the [DOI Handbook](http://www.doi.org/doi_handbook/2_Numbering.html) for a detailed discussion of the syntax of DOIs. However, the important points are:

- DOIs are case insensitive
- DOIs can contain characters that are normally reserved characters within the HTTP specification

# JSON Response

The response to requests to the DOI query path should always be in the JSON representation
mentioned below, regardless of the representation requested by the client. Therefore, the
client's `Accept` header is always ignored, and the only valid `Content-Type` response header
is `application/json`, or `application/json; charset=UTF-8`, where the charset value may
be set to any value appropriate for the response body.

A valid JSON response contains a `message-type` and the `message` itself. Two message types
are specified:

| Type | Meaining |
|------|----------|
| archive | Response to a successful DOI query |
| error | Response to a failed DOI query |

## Archive

An archive message represents the archive state of the content associated with a given
DOI. It can comprise of these properties:

| Name | Optional? | Value | Description |
|------|-----------|-------|-------------|
| doi | No | A DOI without any adornment (`doi:` etc.) e.g. `10.5555/12345678` | The query DOI an archive response refers to. |
| archived_at | Yes | A valid [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) date or date and time string | Date or date and time at which a copy of content was received for archiving. Omission of this element indicates that no archive copy has been received for the query DOI. |
| archived_state | Yes | `dark` or `light` | Whether an archive copy is light or dark. Only valid if `archived_at` is present. |
| archived_location | Yes | Any URL | Publicly accessible download location for the archive copy. Onlu valid if `archived_at` is present. |

## Error

An error response contains a message with two required properties: `error_message` and `error_code`. `error_message` may contain a human readable explanation for any error (or, if that is not available, any additional information about the error). `error_code` must be set to the same value as the HTTP response code, which itself must be an HTTP error code in the 4xx or 5xx range.

## Response Headers

A HTTP response to the DOI query path may contain any headers, but must include a `Content-Type`
header, and in the case of a redirect must include a `Location` header.

## Response Examples

Response for a DOI whose content has been received by the archive, but is currently dark:

    {
	  "message-type": "archive",
	  "message": {
	    "doi": "10.5555/12345678"
	    "archived_at": "2014-01-13T12:24Z",
		"archived_state": "dark"
      }
	}

Response for a DOI whose content has not been received by the archive:

    {
      "message-type": "archive",
	  "message": {
	    "doi": "10.5555/12345678"
	  }
	}

Response for a DOI whose content has been received by the archive and for which there
has been a trigger event, making the content become light:

    {
	  "message-type": "archive",
	  "message": {
        "doi": "10.5555/12345678",
		"archived_at": "2014-01-13T12:24Z",
		"archived_state": "light",
		"archived_location": "http://anarchive.org/archive/10.5555/12345678"
	  }
	}

An error response:

    {
	  "message-type": "error",
	  "message": {
	    "error_message": "Upstream service is currently unavailable.",
		"error_code": 506
	  }
	}

# Response Codes

Responses to the DOI query path should incorporate all appropriate HTTP error codes.
Clients should expect to receive any valid HTTP response code, including redirects
and errors. In the case of redirects, clients should follow the `Location` header.
In the case of error codes, clients should implement a reasonable decaying retry
mechanism.

Clients are not expected to deal with any form of rate-limiting HTTP headers.
