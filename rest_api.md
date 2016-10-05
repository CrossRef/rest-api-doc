# CrossRef REST API

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
- v14: 2014-02-14, new `has-funder` filter.
- v15: 2014-02-27, new `/licenses` route
- v16: 2014-05-19, new `/journals` route, new CrossMark (updates and update policy) filters, new `sort` and `order` parameters
- v17: 2014-05-19, new `facet` query parameter
- v18: 2014-05-29, new `/works/{doi}/agency` route
- v19: 2014-06-23, new textual filters - `container-title`, `publisher-name`, `category-name`.
- v20: 2014-06-24, OR filter queries, `type-name` filter.
- v21: 2014-07-01, new `award.number` and `award.funder` relational filters.
- v22: 2014-07-16, changed title to more accurately reflect scope of API. 
- v23, 2014-09-01, semantics of mutliple filters, dot filters
- v24, 2014-10-15, Added info on license of CrossRef metadata itself. Doh.
- v25, 2015-05-06, Added link to issue tracker. Removed Warning section.
- v26, 2015-10-20, Added new filters - `from-created-date`, `until-created-date`, `affiliation`, `has-affiliation`, `assertion-group`, `assertion`, `article-number`, `alternative-id`
- v27, 2015-10-30, Added `cursor` parameter to `/works` resources
- v28, 2016-05-09, Added link to source of category lables
- v29, 2016-05-24, Added field queries
- v30, 2016-09-26, Highlight issue tracker
- v31, 2016-10-05, document `has-clinical-trial-number` and `has-abstract` filters

## Background

See the document, [CrossRef metadata best practice to support key performance indicators (KPIs) for funding agencies](https://github.com/CrossRef/rest-api-doc/blob/master/funder_kpi_metadata_best_practice.md), for background.

## Reporting issues, requesting features

Please report problems with the API or the documentation on our [issue tracker](https://github.com/CrossRef/rest-api-doc/issues).
 

## License

CrossRef asserts no claims of ownership to individual items of bibliographic metadata and associated Digital Object Identifiers (DOIs) acquired through the use of the CrossRef Free Services. Individual items of bibliographic metadata and associated DOIs may be cached and incorporated into the user's content and systems. More information can be found [on our web site](http://www.crossref.org/requestaccount/).

## Overview

The API is generally RESTFUL and returns results in JSON. JSON formats returned by the API are documented [here](https://github.com/CrossRef/rest-api-doc/blob/master/api_format.md).

The API will only work for CrossRef DOIs. You can test the registration agency for a DOI using the following route:

`http://api.crossref.org/works/{doi}/agency`

Testing the following CrossRef DOI:

`10.1037/0003-066X.59.1.29`

Using the URL:

`http://api.crossref.org/works/10.1037/0003-066X.59.1.29/agency`

Will return the following result:

    {
      status: "ok",
      message-type: "work-agency",
      message-version: "1.0.0",
      message: {
        DOI: "10.1037/0003-066x.59.1.29",
        agency: {
          id: "crossref",
          label: "CrossRef"
        }
      }
    }

If you use any of the API calls listed below with a non-CrossRef DOI, you will get a `404` HTTP status response. Typical agency IDs include `crossref`, `datacite`, `medra` and also `public` for test DOIs.

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

Note that the "message-type" returned will differ from the mime-type:

- funder (singleton)
- prefix (singleton)
- member (singleton)
- work (singleton)
- work-list (list)
- funder-list (list)
- prefix-list (list)
- member-list (list)

Normally, an API list result will return both the summary and the items. If you want to just retrieve the summary, you can do so by specifying that the number of rows returned should be zero. 
### Sort order

If the API call includes a query, then the sort order will be by the relevance score. If no query is included, then the sort order will be by DOI update date.


## Resource Components
Major resource components supported by the CrossRef API are:

- works
- funders
- members
- prefixes
- types
- journals

These can be used alone like this

| resource      | description                       |
|:--------------|:----------------------------------|
| `/works`      | returns a list of all works (journal articles, conference proceedings, books, components, etc), 20 per page
| `/funders`    | returns a list of all funders in the [FundRef Registry](http://www.crossref.org/fundref/fundref_registry.html)
| `/members` | returns a list of all CrossRef members (mostly publishers) |
| `/types`      | returns a list of valid work types | 
| `/licenses`  | return a list of licenses applied to works in CrossRef metadata |
| `/journals` | return a list of journals in the CrossRef database |


### Resource components and identifiers
Resource components can be used in conjunction with identifiers to retrieve the metadata for that identifier.

| resource                    | description                       |
|:----------------------------|:----------------------------------|
| `/works/{doi}`              | returns metadata for the specified CrossRef DOI. |
| `/funders/{funder_id}`      | returns metadata for specified funder **and** its suborganizations |
| `/prefixes/{owner_prefix}` | returns metadata for the DOI owner prefix |
| `/members/{member_id}` | returns metadata for a CrossRef member |
| `/types/{type_id}` | returns information about a metadata work type |
| `/journals/{issn}` | returns information about a journal with the given ISSN |

### Combining resource components

The works component can be appended to other resources.

| resource                    | description                       |
|:----------------------------|:----------------------------------|
| `/works/{doi}`      | returns information about the specified CrossRef `DOI` |
| `/funders/{funder_id}/works`| returns list of works associated with the specified `funder_id` |
| `/types/{type_id}/works` | returns list of works of type `type` |
| `/prefixes/{owner_prefix}/works` | returns list of works associated with specified `owner_prefix` |
| `/members/{member_id}/works` | returns list of works associated with a CrossRef member (deposited by a CrossRef member) |
| `/journals/{issn}/works` | returns a list of works in the given journal |

## Parameters

Parameters can be used to query, filter and control the results returned by the CrossRef API. They can be passed as normal URI parameters or as JSON in the body of the request.

| parameter                    | description                 |
|:-----------------------------|:----------------------------|
| `query`                      | limited [DisMax](https://wiki.apache.org/solr/DisMax) query terms |
| `filter={filter_name}:{value}`| filter results by specific fields |
| `rows={#}`                   | results per per page | 
| `offset={#}`                 | result offset |                         
| `sample={#}`                 | return random N results |
| `sort={#}`                   | sort results by a certain field |
| `order={#}`                  | set the sort order to `asc` or `desc` |
| `facet=t`                    | enable facet information in responses |

Multiple filters can be specified by separating name:value pairs with a comma:

    http://api.crossref.org/works?filter=has-orcid:true,from-pub-date:2004-04-04

### Example query using URI parameters

    http://api.crossref.org/funders/100000015/works?query=global+state&filter=has-orcid:true&rows=1

## Queries

Queries support a subset of [DisMax](https://wiki.apache.org/solr/DisMax), so, for example you 
can refine queries as follows.

Works that include "renear" but not "ontologies":

    http://api.crossref.org/works?query=renear+-ontologies
	
## Field Queries

Field queries are available on some routes and allow for queries that match only particular fields
of metadata. For example, this query matches records that contain the tokens `richard` or `feynman` (or both)
in any author field:

    http://api.crossref.org/works?query.author=richard+feynman
	
Field queries can be combined with the general `query` paramter and each other. Each query parameter
is ANDed with the others:

    http://api.crossref.org/works?query.title=room+at+the+bottom&query.author=richard+feynman
	
### `/works` Field Queries

These field queries are available on the `/works` route:

| Field query parameter | Description |
|-----------------------|-------------|
| `query.title` | Query `title` and `subtitle` |
| `query.container-title` | Query `container-title` aka. publication name |
| `query.author` | Query author first and given names |
| `query.editor` | Query editor first and given names |
| `query.chair` | Query chair first and given names |
| `query.translator` | Query translator first and given names |
| `query.contributor` | Query author, editor, chair and translator first and given names |

## Sorting

Results from a listy response can be sorted by applying the `sort` and `order` parameters. Order
sets the result ordering, either `asc` or `desc`. Sort sets the field by which results will be
sorted. Possible values are:

| Sort value | Description |
|------------|-------------|
| `score` or `relevance` | Sort by relevance score |
| `updated` | Sort by date of most recent change to metadata. Currently the same as `deposited`. |
| `deposited` | Sort by time of most recent deposit |
| `indexed` | Sort by time of most recent index |
| `published` | Sort by publication date |

An example that sorts results in order of publication, beginning with the least recent:

    http://api.crossref.org/works?query=josiah+carberry&sort=published&order=asc

## Facet Counts

Facet counts can be retrieved by enabling faceting; `facet=t` (or `1`, `true`). Facet counts
give counts per field value for an entire result set.

## Filter Names

Filters allow you to narrow queries. All filter results are lists.  The following filters are supported:

| filter     | possible values | description|
|:-----------|:----------------|:-----------|
| `has-funder` | | metadata which includes one or more funder entry |
| `funder` | `{funder_id}` | metadata which include the `{funder_id}` in FundRef data |
| `prefix` | `{owner_prefix}` | metadata belonging to a DOI owner prefix `{owner_prefix}` (e.g. `10.1016` ) |
| `member` | `{member_id}` | metadata belonging to a CrossRef member |
| `from-index-date` | `{date}` | metadata indexed since (inclusive) `{date}` |
| `until-index-date` | `{date}` | metadata indexed before (inclusive) `{date}` |
| `from-deposit-date` | `{date}` | metadata last (re)deposited since (inclusive) `{date}` |
| `until-deposit-date` | `{date}` | metadata last (re)deposited before (inclusive) `{date}` |
| `from-update-date` | `{date}` | Metadata updated since (inclusive) `{date}`. Currently the same as `from-deposit-date`. |
| `until-update-date` | `{date}` | Metadata updated before (inclusive) `{date}`. Currently the same as `until-deposit-date`. |
| `from-created-date` | `{date}` | metadata first deposited since (inclusive) `{date}` |
| `until-created-date` | `{date}` | metadata first deposited before (inclusive) `{date}` |
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
| `has-references` | | metadata for works that have a list of references |
| `has-archive` | | metadata which include name of archive partner |
| `archive` | `{string}` | metadata which where value of archive partner is `{string}` |
| `has-orcid` | | metadata which includes one or more ORCIDs |
| `orcid` | `{orcid}` | metadata where `<orcid>` element's value = `{orcid}` |
| `issn` | `{issn}` | metadata where record has an ISSN = `{issn}`. Format is `xxxx-xxxx`. |
| `type` | `{type}` | metadata records whose type = `{type}`. Type must be an ID value from the list of types returned by the `/types` resource |
| `directory` | `{directory}` | metadata records whose article or serial are mentioned in the given `{directory}`. Currently the only supported value is `doaj`. |
| `doi` | `{doi}` | metadata describing the DOI `{doi}` |
| `updates` | `{doi}` | metadata for records that represent editorial updates to the DOI `{doi}` |
| `is-update` | | metadata for records that represent editorial updates |
| `has-update-policy` | | metadata for records that include a link to an editorial update policy |
| `container-title` | | metadata for records with a publication title exactly with an exact match |
| `publisher-name` | | metadata for records with an exact matching publisher name |
| `category-name` | | metadata for records with an exact matching category label. Category labels come from [this list](https://www.elsevier.com/solutions/scopus/content) published by Scopus |
| `type-name` | | metadata for records with an exacty matching type label |
| `award.number` | `{award_number}` | metadata for records with a matching award nunber. Optionally combine with `award.funder` |
| `award.funder` | `{funder doi or id}` | metadata for records with an award with matching funder. Optionally combine with `award.number` |
| `assertion-group` | | metadata for records with an assertion in a particular group |
| `assertion` | | metadata for records with a particular named assertion |
| `affiliation` | | metadata for records with at least one contributor with the given affiliation |
| `has-affiliation` | | metadata for records that have any affiliation information |
| `alternative-id` | | metadata for records with the given alternative ID, which may be a publisher-specific ID, or any other identifier a publisher may have provided |
| `article-number` | | metadata for records with a given article number |
| `has-abstract` | | metadata for records which include an abstract |
| `has-clinical-trial-number` | | metadata for records which include a clinical trial number |

### Multiple filters

Multiple filters can be specified in a single query. In such a case, different filters will be applied with AND semantics, while specifying the same filter multiple times will result in OR semantics - that is, specifying the filters:

- `is-update:true`
- `from-pub-date:2014-03-03`
- `funder:10.13039/100000001`
- `funder:10.13039/100000050`

would locate documents that are updates, were published on or after 3rd March 2014 and were funded by either the National Science Foundation (`10.13039/100000001`) or the National Heart, Lung, and Blood Institute (`10.13039/100000050`). These filters would be specified by joining each filter together with a comma:

    /works?filter=is-update:true,from-pub-date:2014-03-03,funder:10.13039/100000001,funder:10.13039/100000050
    
### Dot filters

A filter with a dot in its name is special. The dot signifies that the filter will be applied to some other record type that is related to primary resource record type. For example, with work queries, one can filter on works that have an award, where the same award has a particular award number and award-gving funding agency:

    /works?filter=award.number:CBET-0756451,award.funder:10.13039/100000001
    
Here we filter on works that have an award by the National Science Foundation that also has the award number `CBET-0756451`.

### Notes on owner prefixes

The prefix of a CrossRef DOI does **NOT** indicate who currently owns the DOI. It only reflects who originally registered the DOI. CrossRef metadata has an **owner_prefix** element that records the current owner of the CrossRef DOI in question. 

CrossRef also has member IDs for depositing organisations. A single member may control multiple owner prefixes, which in turn may control a number of DOIs. When looking at works published by a certain organisaton, member IDs and the member routes should be used.

### Notes on dates

Note that dates in filters should always be of the form `YYYY-MM-DD`, `YYYY-MM` or `YYYY`. Also note that date information in CrossRef metadata can often be incomplete. So, for example, a publisher may only include the year and month of publication for a journal article. For a monograph they might just include the year. In these cases the API selects the earliest possible date given the information provided. So, for instance, if the publisher only provided 2013-02 as the published date, then the date would be treated as 2013-02-01. Similarly, if the publisher only provided the year 2013 as the date, it would be treated at 2013-01-01. 

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

### Deep Paging with Cursors

Using large `offset` values can result in extremely long response times. Offsets in the 100,000s and beyond will likely cause a timeout before the API is able to respond. An alternative to paging through very large result sets (like a corpus used for text and data mining) it to use the API's exposure of Solr's deep paging cursors. Any combination of query, filters and facets may be used with deep paging cursors. While `rows` may be specified along with `cursor`, `offset` and `sample` cannot be used. To use deep paging make a query as normal, but include the `cursor` parameter with a value of `*`. In this example we will page through all `journal-article` works from member `311`:

    http://api.crossref.org/members/311/works?filter=type:journal-article&cursor=*

A `next-cursor` field will be provided in the JSON response. To get the next page of results, pass the value of `next-cursor` as the `cursor` parameter:

    http://api.crossref.org/members/311/works?filter=type:journal-article&cursor=AoE/CGh0dHA6Ly9keC5kb2kub3JnLzEwLjEwMDIvdGRtX2xpY2Vuc2VfMQ==
 
Clients should check the number of returned items. If the number of returned items is fewer than the number of expected rows then the end of the result set has been reached. Using `next-cursor` beyond this point will result in responses with an empty items list.

The `cursor` parameter is available on all `/works` resources.

### Sample

Being able to select random results is useful for both testing and sampling. You can use the `sample` parameter to retrieve random results. So, for example, the following select 10 random works:

    http://api.crossref.org/works?sample=10
    
Note that when you use the `sample` parameter, the `rows` and `offset` parameters are ignored.


### Example Queries

**All works published by owner prefix `10.1016` in January 2010**

    http://api.crossref.org/prefixes/10.1016/works?filter=from-pub-date:2010-01,until-pub-date:2010-01

**All works funded by `10.13039/100000001` that have a CC-BY license**

    http://api.crossref.org/funders/10.13039/100000001/works?filter=license.url:http://creativecommons.org/licenses/by/3.0/deed.en_US

**All works published by owner prefix 10.5555 from February 2010 to February 2013 that have a CC-BY license**

    http://api.crossref.org/prefixes/10.5555/works?filter=license.url:http://creativecommons.org/licenses/by/3.0/deed.en_US,from-pub-date:2010-02,until-pub-date:2013-02

**All works funded by `10.13039/100000015` where license = CC-BY and embargo <= 365 days**

    http://api.crossref.org/funders/10.13039/100000015/works?filter=license.url:http://creativecommons.org/licenses/by/3.0/deed.en_US,license.delay:365

Note that the filters for license URL and maximum license embargo period (license.url and license.delay) combine to filter each document's metadata for a license with both of these properties.

**All works where the archive partner listed = 'CLOCKSS'**

    http://api.crossref.org/works?filter=archive:CLOCKSS
    
**All members with `hind` in their name (e.g. Hindawi)**

    http://api.crossref.org/members?query=hind
    
**All licenses linked to works published by Elsevier**

    http://api.crossref.org/licenses?filter=member:78
    
**All licenses applied to works published in the journal `Pathology Research International`**

    http://api.crossref.org/licenses?filter=issn:2090-8091
    
**All works with an award numbered roughly `1 F31 MH11745` also awarded by funder with ID `10.13039/100000025`:

    http://api.crossref.org/works?filter=award.number:1F31MH11745,award.funder:10.13039/100000025

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
