Archive Arrangement Query API
=============================

## Change History

| Date | Changes |
|------|---------|
| 2014-01-13 | Initial version |

## Background

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

## DOI Query Path

A query API implementor must provide a publicly-accessible DOI query path. The path must
either accept a DOI as a query parameter named `doi` to a HTTP path `/doi/status`:

    http://anarchive.org/doi/status?doi={DOI}

### DOI Syntax

The implementor must handle the issues of embedding DOIs within HTTP paths and query strings.
Refer to the `Numbering` section of the [DOI Handbook](http://www.doi.org/doi_handbook/2_Numbering.html) for a detailed discussion of the syntax of DOIs. However, the important points are:

- DOIs are case insensitive
- DOIs can contain characters that are normally reserved characters within the HTTP specification

## JSON Response

The response to requests to the DOI query path should always be in the JSON representation
mentioned below, regardless of the representation requested by the client. Therefore, the
client's `Accept` header is always ignored, and the only valid `Content-Type` response header
is `application/json`, or `application/json; charset=UTF-8`, where the charset value may
be set to any value appropriate for the response body.

A valid JSON response contains a `status`, `message` and `doi`:

| Name | Optional? | Value | Description |
|------|-----------|-------|-------------|
| status | No | HTTP status code | Must match the HTTP response status code. |
| message | No | Any string | A human readable error or status message. May be empty. |
| doi | No | A DOI without any adornment (`doi:` etc.) e.g. `10.5555/12345678` | The query DOI an archive response refers to. |

### Archived Copies

A JSON response may also contain the archive state one or more archive copies. No
`copies` element, or an empty `copies` element, indicates that no archive copy has been
received for the query DOI.

Each `copies` entry may contain:

| Name | Optional? | Value | Description |
|------|-----------|-------|-------------|
| received_at | No | A valid [ISO 8601](http://en.wikipedia.org/wiki/ISO_8601) date or date and time string | Date or date and time at which a copy of content was received for archiving. |
| state | No | `dark` or `light` | Whether an archive copy is light or dark. |
| location | Yes | Any URL | Publicly accessible download location for the archive copy. |
| content_version | Yes | `am` or `vor` | Indicates that the archive copy is specifically a copy of the author accepted manuscript (`am`) or publisher version of record (`vor`). |
| content_type | Yes | Any valid MIME type | The content type of the archive copy. |

### Response Headers

A HTTP response to the DOI query path may contain any headers, but must include a `Content-Type`
header, and in the case of a redirect must include a `Location` header.

### Response Examples

Response for a DOI whose content has been received by the archive, but is currently dark:

    {
      "status": 200,
	  "message": "",
      "doi": "10.5555/12345678",
	  "copies": [
	    {
	      "received_at": "2014-01-13T12:24Z",
		  "state": "dark",
		  "content_version": "am",
		  "content_type": "application/pdf"
	    },
		{
		  "received_at": "2014-01-13T12:24Z",
		  "state": "dark",
		  "content_version": "vor",
		  "content_type": "text/xml"
	    }
	  ]
	}

Response for a DOI whose content has not been received by the archive:

    {
      "status": 200,
	  "message": ""
	  "doi": "10.5555/12345678",
	  "copies": []
	}

Response for a DOI whose content has been received by the archive and for which there
has been a trigger event, making the content become light:

    {
      "status": 200,
	  "message": "",
      "doi": "10.5555/12345678",
	  "copies": [
	    {
	      "received_at": "2014-01-13T12:24Z",
		  "state": "dark",
		  "content_version": "am",
		  "content_type": "application/pdf",
		  "location": "http://anarchive.org/content/am/10.5555/12345678.pdf"
	    },
		{
		  "received_at": "2014-01-13T12:24Z",
		  "state": "dark",
		  "content_version": "vor",
		  "content_type": "text/xml",
          "location": "http://anarchive.org/content/vor/10.5555/12345678.xml"
	    }
	  ]
  }

An error response:

    {
	  "status": 506,
	  "message": "Upstream service is currently unavailable."
	}

### Response Codes

Responses to the DOI query path should incorporate all appropriate HTTP error codes.
Clients should expect to receive any valid HTTP response code, including redirects
and errors. In the case of redirects, clients should follow the `Location` header.
In the case of error codes, clients should implement a reasonable decaying retry
mechanism.

Clients are not expected to deal with any form of rate-limiting HTTP headers.
