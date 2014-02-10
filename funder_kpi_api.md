# CrossRef APIs to support key performance indicators (KPIs) for funding agencies

## Version History

- V1: 2013-09-08, first draft.
- V2: 2013-09-24, reference platform deployed
- v3: 2013-09-25, reworked filters. Added API versioning doc 
- v4: 2013-09-25, more filter changes.
- v5: 2013-09-27, doc mime-type and message-type relationship
- v6: 2013-10-01, updated `sample` & added examples with filters
- v6: 2013-10-01, corrected warning date
- v7: 2013-10-02, fixed typos
- v8: 2013-10-17, updated warning. Added email address
- v9: 2013-12-13, update example urls
- v10: 2013-12-13, /types routes, type filter, issn filter
- v11: 2013-12-14, indexed timestamps, has-archive and archive implemented
- v12: 2014-01-06, directory filter
- v13: 2014-02-10, new `/members`, `/publishers` becomes `/prefixes`, new `member` filter, `publisher` filter becomes `prefix`

## Background

See the document, [CrossRef metadata best practice to support key performance indicators (KPIs) for funding agencies](http://api.crossref.org/docs/funder_kpi_metadata_best_practice.html), for background.

## Warning

The API described here is in alpha. If you encounter problems with the API or the documentation, please report them to:

![](http://labs.crossref.org/wp-content/uploads/2013/01/labs_email.png)


## Overview

The API is generally RESTFUL and returns results in JSON.

The API will only work for CrossRef DOIs. You can test the registration agency for a DOI using the following convention:

`http://doi.crossref.org/doiRA/{doi}`

So testing the following CrossRef DOI:

`10.1037/0003-066X.59.1.29`

Will return the following result:

    [
        {
             DOI: "10.1037/0003-066X.59.1.29",
             RA: "CrossRef"
        }
    ]


If you use any of the API calls listed below with a non-CrossRef DOI, you will get a `404` HTTP status response with the message "non-CrossRef DOI."

## Results Overview

All results are returned in JSON. There are two general types of results:

- Singletons
- Lists

The mime-type for API results is `application/vnd.crossref-api-message+json` 


### Singletons

Singletons are single results. Retrieving metadata for a specific identifier (e.g. DOI, ISSN, funder_identifier) typically returns in a singleton result.

### Lists
Lists results can contain multiple entries. Searching or filtering typically returns a list result. A list has two parts:

- Summary, which include the following information:
    - status (e.g. "ok", error)
    - message-type (e.g. "work-list" )
    - message-version (e.g. 1.0.0 )
- Items, which will will contain the items matching the query or filter. 


Note that the "message-type" returned will differ from the mime-type. There are six message-types:

- funder (singleton)
- publisher (singleton)
- work (singleton)
- work-result-list (list)
- funder-result-list (list)
- publisher-result-list (list) 


Normally, and API list result will return both the summary and the items. If you want to just retrieve the summary, you can do so by specifying that the number of rows returned should be zero. 
### Sort order

If the API call includes a query, then the sort order will be by the relevance score. If no query is included, then the sort order will be by DOI update date.


## Resource Components
Major resource components supported by the CrossRef API are:

- works
- funders
- publishers

These can be used alone like this

| resource      | description                       |
|:--------------|:----------------------------------|
| `/works`      | returns a list of all works (journal articles, conference proceedings, books, components, etc), 20 per page
| `/funders`    | returns a list of all funders in the [FundRef Registry](http://www.crossref.org/fundref/fundref_registry.html)
| `/publishers` | returns a list of all publishers|
| `/types`      | returns a list of valid work types | 


### Resource components and identifiers
Resource components can be used in conjunction with identifiers to retrieve the metadata for that identifier.

| resource                    | description                       |
|:----------------------------|:----------------------------------|
| `/works/{doi}`              | returns metadata for the specified CrossRef DOI. |
| `/funders/{funder_id}`      | returns metadata for specified funder **and** its suborganizations |
| `/publishers/{owner_prefix}` | returns metadata for the specified publisher |
| `/types/{type_id}` | returns information about a metadata work type |

### Combining resource components

The works component can be appended to other resources.

| resource                    | description                       |
|:----------------------------|:----------------------------------|
| `/works/{doi}`      | returns information about the specified CrossRef `DOI` |
| `/funders/{funder_id}/works`| returns list of works associated with the specified `funder_id` |
| `/types/{type}/works` | returns list of works of type `type` |
| `/publishers/{owner_prefix}/works` | returns list of works associated with specified `owner_prefix` |


## Parameters

Parameters can be used to query, filter and control the results returned by the CrossRef API. They can be passed as normal URI parameters or as JSON in the body of the request.

| parameter                    | description                 |
|:-----------------------------|:----------------------------|
| `query`                      | limited [DisMax](https://wiki.apache.org/solr/DisMax) query terms |
| `filter={filter_name}:{value}`| filter results by specific fields |
| `rows={#}`                   | results per per page | 
| `offset={#}`                 | result offset |                         
| `sample={#}`                 | return random N results |

Multiple filters can be specified by separating name:value pairs with a comma:

    http://api.crossref.org/works?filter=has-orcid:true,from-pub-date:2004-04-04

### Example query using URI parameters

    http://api.crossref.org/funders/100000015/works?query=global+state&filter=has-orcid:true&rows=1

### Example query using JSON in body of GET request

Note that if you include a body in your `GET` request, any URI parameters will be ignored. In short, you cannot mix URI parameters and JSON queries.

To use the API using JSON, pass the JSON in the body of the HTTP GET request like this:

    curl -X GET -H "Content-Type: application/json" -d '{"query": "psychoceramics", "offset": 40, "rows": 20, "filter": {"has-orcid": true, "publisher": "10.5555"}}'  http://api.crossref.org/works

## Queries

Queries support a subset of [DisMax](https://wiki.apache.org/solr/DisMax), so, for example you can refine queries as follows.

**Works that include "renear" but not "ontologies"**

    http://api.crossref.org/works?query=renear+-ontologies
    
or using JSON

    curl -X GET -H "Content-Type: application/json" -d '{"query": "renear -ontologies"}'  http://api.crossref.org/works
    


## Filter Names

Filters allow you to narrow queries. All filter results are lists.  The following filters are supported:


| filter     | possible values | description|
|:-----------|:----------------|:-----------|
| `funder` | `{funder_id}` | metadata which include the `{funder_id}` in FundRef data |
| `publisher` | `{owner_prefix}` | metadata belongs to published identified by `{owner_prefix}` (e.g. `10.1016` ) |
| `from-index-date` | `{date}` | metadata indexed since (inclusive) `{date}` |
| `until-index-date` | `{date}` | metadata indexed before (inclusive) `{date}` |
| `from-deposit-date` | `{date}` | metadata last (re)deposited since (inclusive) `{date}` |
| `until-deposit-date` | `{date}` | metadata last (re)deposited before (inclusive) `{date}` |
| `from-update-date` | `{date}` | Metadata updated since (inclusive) `{date}`. Currently the same as `from-deposit-date`. |
| `until-update-date` | `{date}` | Metadata updated before (inclusive) `{date}`. Currently the same as `until-deposit-date`. |
| `from-first-deposit-date` | `{date}` | metadata first deposited since (inclusive) `{date}` [^*] |
| `until-first-deposit-date` | `{date}` | metadata first deposited before (inclusive) `{date}` [^*] |
| `from-pub-date` | `{date}` | metadata where published date is since (inclusive) `{date}` |
| `until-pub-date` | `{date}` | metadata where published date is before (inclusive)  `{date}` |
| `has-license` | | metadata that includes any `<license_ref>` elements. |
| `license.url` | `{url}` | metadata where `<license_ref>` value equals `{url}` |
| `license.version` | `{string}` | metadata where the `<license_ref>`'s `applies_to` attribute  is `{string}`|
| `license.delay` | `{integer}` | metadata where difference between publication date and the `<license_ref>`'s `start_date` attribute is <= `{integer}` (in days)|
| `has-full-text` |  | metadata that includes any full text `<resource>` elements. |
| `full-text.version` | `{string}`  | metadata where `<resource>` element's `content_version` attribute is `{string}`. |
| `full-text.type` | `{mime_type}`  | metadata where `<resource>` element's `content_type` attribute is `{mime_type}` (e.g. `application/pdf`). |
| `public-references` | | metadata where publishers allow references to be distributed publically. [^*] |
| `has-archive` | | metadata which include name of archive partner |
| `archive` | `{string}` | metadata which where value of archive partner is `{string}` |
| `has-orcid` | | metadata which includes one or more ORCIDs |
| `orcid` | `{orcid}` | metadata where `<orcid>` element's value = `{orcid}` |
| `issn` | `{issn}` | metadata where record has an ISSN = `{issn}`. Format is `xxxx-xxxx`. |
| `type` | `{type}` | metadata records whose type = `{type}`. Type must be an ID value from the list of types returned by the `/types` resource |
| `directory` | `{directory}` | metadata records whose article or serial are mentioned in the given `{directory}`. Currently the only supported value is `doaj`. |

[^*]: Not implemented yet.

### Notes on owner prefixes

The prefix of a CrossRef DOI does **NOT** indicate who currently owns the DOI. It only reflects who originally registered the DOI. CrossRef metadata has an **owner_prefix** element that records the current owner of the CrossRef DOI in question. 

### Notes on dates

Note that dates in filters should always be of the form `YYYY-MM-DD`. Also not that date information in CrossRef metadata can often be incomplete. So, for example, a publisher may only include the year and month of publication for a journal article. For a monograph they might just include the year. In these cases the API selects the earliest possible date given the information provided. So, for instance, if the publisher only provided 2013-02 as the published date, then the date would be treated as 2013-02-01. Similarly, if the publisher only provided the year 2013 as the date, it would be treated at 2013-01-01. 

### Notes on incremental metadata updates

When using time filters to retrieve periodic, incremental metadata updates,
the `from-index-date` filter should be used over `from-update-date`,
`from-deposit-date`, `from-first-deposit-date` and `from-pub-date`. The
timestamp that `from-index-date` filters on is guaranteed to be updated
every time there is a change to metadata requiring a reindex.

## Result controls

You can control the delivery and selection  results using the `rows`, `offset` and `sample` parameters. 

### Rows 

Normally, results are returned 20 at a time. You can control the number of results returns by using the `rows` parameter. To limit results to 5, for example, you could do the following:

    http://api.crossref.org/works?query=allen+renear&rows=5

If you would just like to get the `summary` of the results, you can set the rows to 0 (zero).

    http://api.crossref.org/works?query=allen+renear&rows=0
    
The maximum number rows you can ask for in one query is `1000`.

### Offset

The number of returned items is controlled by the `rows` parameter, but you can select the offset into the result list by using the `offset` parameter.  So, for example, to select the second set of 5 results (i.e. results 6 through 10), you would do the following:

    http://api.crossref.org/works?query=allen+renear&rows=5&offset=5
    
### Sample

Being able to select random results is useful for both testing and sampling. You can use the `sample` parameter to retrieve random results. So, for example, the following select 10 random works:

    http://api.crossref.org/works?sample=10
    
Note that when you use the `sample` parameter, the `rows` and `offset` parameters are ignored.


### Example Queries

**All works published by owner prefix `10.1016` in January 2010**

    http://api.crossref.org/publishers/10.1016/works?filter=from-pub-date:2010-01,until-pub-date:2010-01

**All works funded by funder_id that have a CC-BY license**

    http://api.crossref.org/publishers/10.5555/works?filter=license.url:http://creativecommons.org/licenses/by/3.0/deed.en_US

**All works published by owner prefix 10.5555 from February 2010 to February 2013 that have a CC-BY license**

    http://api.crossref.org/publishers/10.5555/works?filter=license.url:http://creativecommons.org/licenses/by/3.0/deed.en_US,from-pub-date:2010-02,until-pub-date:2013-02

**All works funded by `10.13039/100000015` where license = CC-BY and embargo <= 365 days**

    http://api.crossref.org/funders/10.13039/100000015/works?filter=license.url:http://creativecommons.org/licenses/by/3.0/deed.en_US,license.delay:365

Note that the filters for license URL and maximum license embargo period (license.url and license.delay) combine to filter each document's metadata for a license with both of these properties.

**All works funded by X where the archive partner listed = 'LOCKSS'**

Coming soon.


## Versioning

In theory, the syntax of the API can vary independently of the result representations. In practice, major version changes in either will require changes to API clients and so versioning of the API will apply to both the API syntax and the result representation.

The API uses a semantic versioning scheme whereby the version number is divided into three parts delimited by periods. The first number represents the "major" release number. The second represents a "minor" release number and the third represents an "internal" release number.  
      
    Version 1.20.31
            ^  ^  ^
            |  |  |
        major  |  |
           minor  |
           internal

 **Major** version increments will are defined as releases that can break backwards compatibility. CrossRef will only commit to supporting the latest two major releases simultaneously and legacy major releases will be supported for no more than nine months. Exceptions to these rules may be made when major releases are required to ensure the security or stability of the system. 

**Minor** version increments are defined as backwards compatible. There is no limit on the number of minor versions that can CrossRef can roll out. Note that client applications should not have dependencies on minor versions.

**Internal** version increments are simply used to keep track of development versions of the API. They should never have any effect on client applications.

Adding syntax options or metadata to representations will normally be backwards compatible and will thus normally only trigger minor version changes. Renaming or restructuring syntax options of metadata tends not to be backward compatible and will thus typically trigger major version changes

### How to manage API versions

If you need to tie your implementation to a specific major version of the API, you can do so by using content-negotiation and specifying the version of the API in the `ACCEPT` header as follows:

     application/vnd.crossref-api-message+json; version=1.0

Minor version numbers will be ignored in `ACCEPT` headers as they are by definition backwards compatible.

If you omit a specific version in your `ACCEPT` header, the system will default to using the latest version of the API. 

Note that requesting a version of the API via content type is not yet supported.

## Error messages

There will be no errors, and therefor error messages will be unnecessary. But seriously… coming soon.
