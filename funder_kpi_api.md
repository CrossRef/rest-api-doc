# CrossRef APIs to support key performance indicators (KPIs) for funding agencies

## Version History

- V1: 2013-09-08, first draft.
- V2: 2013-09-24, reference platform deployed

## Background

See the document, [CrossRef metadata best practice to support key performance indicators (KPIs) for funding agencies](http://fundref.crossref.org/docs/funder_kpi_metadata_best_practice.html), for background.

## Warning

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

## Overview

The API is generally RESTFUL and returns results in JSON.

## Results Overview

All results are returned in JSON. There are two general types of results:

- Singletons
- Lists

### Singletons

Singletons are single results. Retrieving metadata for a specific identifier (e.g. DOI, ISSN, funder_identifier) typically returns in a singleton result.

### Lists
Lists results can contain multiple entries. Searching or filtering typically returns a list result. A list has two parts:

- Summary, which include the following information:
    - status (e.g. "ok", error)
    - message-type (e.g. "funder-work-result-list" )
    - message-version (e.g. 1.0.0 )
-Items, which will will contain the items matching the query. 

Normally, and API list result will return both the summary and the items. If you want to just retrieve the summary, you can do so by specifying that the number of rows returned should be zero. 

##  HTTP Headers

What do we say about mime types? 


## Resource Components
Major resource components supported by the CrossRef API are:

- works
- funders
- publishers

These can be used alone like this

| resource      | description                       |
|:--------------|:----------------------------------|
| `/works`      | returns a list of all works (journal articles, conference proceedings, books, components, etc), 20 per page, sorted by XXX |
| `/funders`    |returns a list of all funders (??? all in registry or all that have works in index) |
| `/publishers` |returns a list of all publishers|

### Selecting random results

Being able to select random results is useful for both testing and sampling. You can specify that you want random results by appending '/random' to any resource component. You can also specify the number of random results you want returned.

| resource           | description                       |
|:-------------------|:----------------------------------|
| `/works/random`    | returns metadata for 20 (default) random CrossRef DOI records |
| `/works/random/500`  | returns metadata for 500 random CrossRef DOI records |
| `/funders/random/10`  | returns metadata for 10 random FundRef funders |
| `/publishers/random/5`  | returns metadata for 5 random CrossRef publishers |

TBD How random is random?

### Resource components and identifiers
Resource components can be used in conjunction with identifiers to retrieve the metadata for that identifier.

| resource                    | description                       |
|:----------------------------|:----------------------------------|
| `/works/{doi}`              | returns metadata for the specified CrossRef DOI |
| `/funders/{funder_id}`      | returns metadata for specified funder |
| `/publishers/{owner_prefix}` | returns metadata for the specified publisher |

### Combining resource components

Resource components can be combined to narrow down selections.

| resource                    | description                       |
|:----------------------------|:----------------------------------|
| `/works/{doi}/funders`      | returns list of funders associated with the specified CrossRef `DOI` |
| `/funders/{funder_id}/works`| returns list of works associated with the specified `funder_id` |
| `/publishers/{owner_prefix}/works` | returns list of works associated with specified `owner_prefix` |
| `/publishers/{owner_prefix}/works/random/5` | returns 5 random works associated with the specified `owner_prefix` |


## Parameters

Parameters can be used to query, filter and control the results returned by the CrossRef API. They can be passed as normal URI parameters or as JSON in the body of the request.

| parameter                    | description                 |
|:-----------------------------|:----------------------------|
| `query`                      | limited [DisMax](https://wiki.apache.org/solr/DisMax) query terms |
| `filter={filter_name}:{value}`| filter results by specific fields |
| `rows={#}`                   | results per per page | 
| `offset={#}`                 | page offset |                         
| `sample={#}`                 | return random [^1] N results |



[^1]: Being able to select random results is useful for both testing and sampling. 

### Example query using URI parameters

    http://fundref.crossref.org/v1/funders/100000015/works?query=electron+pairs&filter=has-orcid:true&rows=1

### Example query using JSON in body of GET request

Note that, if you include a body in your get request, and URI parameters will be ignored. In short, you cannot mix URI parameters and JSON queries.

To use the API using JSON, pass the JSON in the body of the HTTP GET request like this:

    curl -X GET -H "Content-Type: application/json" -d '{"query": "psychoceramics", "offset": 40, "rows": 20, "filter": {"has-orcid": true, "publisher": "10.5555"}}'  http://fundref.crossref.org/works

## Queries

Queries support a subset of [DisMax](https://wiki.apache.org/solr/DisMax), so, for example you can refine queries as follows.

Works that include "Renear" but not "Ontologies"

    http://fundref.crossref.org/v1/funders/100000015/works?query=electron+pairs

## Filter Names
Filters allow you to narrow queries. All filter results are lists. Note that dates in filters should always be of the form `YYYY-MM--DD`. The following filters are supported:


| filter     | possible values | description|
|:-----------|:----------------|:-----------|
| from-update-date | date | earliest date metadata was last updated |
| until-update-date | date | latest date metadata was last updated |
| from-pub-date | date | earliest published date |
| until-pub-date | date | latest published date |
| has-full-text | [content type] | does metadata include link to full text resource [of content type]|
| has-license |  [content type] | does metadata include license URI [for content type] |
| has-references | | is distribute_references set? |
| has-archive | | does metadata include name of archive partner(s) |
| has-orcid | | metadata includes one or more ORCIDs |
| orcid | orcid | contains matching ORCID |
| has-representation | mime type | metadata includes URI to representation of mime type |
| license | URI | metadata includes license identified by URI |
| publisher | owner prefix | metadata belongs to published identified by owner prefix |
| funder | funder DOI | metadata include funder_doi in FndRef data |

## Result controls

### Examples

**All works funded by funder_id that have a CC-BY license**

**All works published by owner prefix 10.5555 in February 2015 that have a CC-BY license**

**All works funded by X where license = CC-BY and license start date - published_date < Y**

## Result controls

You can control the size, quantity and selection criteria using the parameters:



## Versioning
There are two ways in which versioning effects the API. 

1. The syntax of the requests can change
2. The representation of the responses can change

Versions of the request syntax and response representation can vary independently. That is- the syntax of a request can change without necessarily changing the response representation and the response representation can change without the request syntax necessarily changing. 

In both cases, the API will use a semantic versioning scheme whereby the version number is divided into three parts delimited by periods. The first number represents the "major" release number. The second represents a "minor" release number and the third represents an "internal" release number.  
      
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

### How to manage versions of the request syntax

If you need to tie your implementation to a specific major version of the API, you can do so by prepending your URI paths with the major version number. So, for example, the following is bound to **latest** version of the API:

    http://fundref.crossref.org/funders/100000015/works?query=electron+pairs

And the following is bound to **version 1** of the API

    http://fundref.crossref.org/v1/funders/100000015/works?query=electron+pairs

### How to manage versions of the response representation

If you need to tie your implementation to a specific major version of the response representation, you can do so by using content negotiation. The following will return the latest response representation:

And the following is bound to **version 1** of the response representation:







## Error messages

There will be no errors, and therefor error messages will be unnecessary. But seriously… coming soon.
