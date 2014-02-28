# CrossRef API Koans

## Version History

- V1: 2014-02-26, first draft.
- V2: 2014-02-28, added license examples

## Overview

The following short examples show how the CrossRef REST API can be used to issue sophisticated queries against the CrossRef system.

## How many DOI records does CrossRef have?

    http://api.crossref.org/works?rows=0

## How many journal article DOIs does CrossRef have?

    http://api.crossref.org/types/journal-article/works?rows=0

## How many report DOIs does CrossRef have?

    http://api.crossref.org/types/journal-article/works?rows=0

## How many works have license information?

    http://api.crossref.org/works?filter=has-license:true&rows=0

## Get first 25 works that have a license

    http://api.crossref.org/works?filter=has-license:true&rows=25&offset=0

## Get second 25 works that have a license

    http://api.crossref.org/works?filter=has-license:true&rows=25&offset=25

## How many license types are there?

    http://api.crossref.org/licenses?rows=0
    
## Show first 25 licneses (FIXME)

    http://api.crossref.org/licenses?rows=25&offset=0

## How many works have a CC-BY license?

    http://api.crossref.org/works?rows=0&filter=license.url:http://creativecommons.org/licenses/by/3.0/

## See how many works have funder information (coming soon)

    http://api.crossref.org/works?filter=has-funder:true&rows=0

## How many member publishers CrossRef has

    http://api.crossref.org/members?rows=0

## First 25 members:

    http://api.crossref.org/members?rows=25&offset=0

## Second 25 members:

    http://api.crossref.org/members?rows=25&offset=25

## Overview of Hindawi's particpation in CrossRef

    http://api.crossref.org/members?query=hindawi

### same as:

    http://api.crossref.org/members/98

## Overview of Elsevier's particpation in CrossRef

    http://api.crossref.org/members?query=elsevier

### same as:

    http://api.crossref.org/members/78

## How many works does Elsevier have?

    http://api.crossref.org/members/78/works?rows=0

## First 25 Elsevier works

    http://api.crossref.org/members/78/works?rows=25&offset=0

## Second 25 Elsevier works

    http://api.crossref.org/members/78/works?rows=25&offset=25

## How many Elsevier works have license links?

    http://api.crossref.org/members/78/works?filter=has-license:true&rows=0

## How many Elseveir works have full text links

    http://api.crossref.org/members/78/works?filter=has-full-text:true&rows=0
    
## What license types does Elsevier support?

    http://api.crossref.org/licenses?filter=member:78

## Overview of Hindawi's particpation in CrossRef

    http://api.crossref.org/members?query=hindawi

### same as:

    http://api.crossref.org/members/98
    
## How many works does Hindawi have?

    http://api.crossref.org/members/98/works?rows=0

## How many Hindawi works have license links?

    http://api.crossref.org/members/98/works?filter=has-license:true&rows=0

## How many Hindawi works have full text links

    http://api.crossref.org/members/98/works?filter=has-full-text:true&rows=0
    
## What license types does Hindawi support?

    http://api.crossref.org/licenses?filter=member:98
    
## What license type does the journal with a particular ISSN support

    http://api.crossref.org/licenses?filter=issn:2090-8091

## What licenses does a reasearcher with a particular ORCID publish under

    http://api.crossref.org/licenses?filter=orcid:0000-0003-1340-5202





