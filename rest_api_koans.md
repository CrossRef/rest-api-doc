# CrossRef API Koans

## Version History

- V1: 2014-02-26, first draft.
- V2: 2014-02-28, added license examples

## Overview

The following short examples show how the CrossRef REST API can be used to issue sophisticated queries against the CrossRef system.

## Finding out what is in the CrossRef system

You might start by wondering how much and what kinds of data exist in the CrossRef system.

### How many DOI records does CrossRef have?

    http://api.crossref.org/works?rows=0

### What content types does CrossRef have?

    http://api.crossref.org/types

### How many journal article DOIs does CrossRef have?

    http://api.crossref.org/types/journal-article/works?rows=0

### How many report DOIs does CrossRef have?

    http://api.crossref.org/types/report/works?rows=0

But eventually you will probably want to start looking at metadata records

### Example 1

    TBD

### Example 2

    TBD

And then you will want to start looking for metadata records that contain specific terms

### Example 1

    TDB
   
### Example 2

    TBD

So let's look at specific records

### Example 1

    TBD

### Example 2

    TBD

Interesting. There is license information in there and full text links.

## How many works have license information?

    http://api.crossref.org/works?filter=has-license:true&rows=0

## Get first 25 works that have a license

    http://api.crossref.org/works?filter=has-license:true&rows=25&offset=0

## Get second 25 works that have a license

    http://api.crossref.org/works?filter=has-license:true&rows=25&offset=25

## How many license types are there?

    http://api.crossref.org/licenses?rows=0
    
## How many works have a CC-BY license?

    http://api.crossref.org/works?rows=0&filter=license.url:http://creativecommons.org/licenses/by/3.0/

## See how many works have funder information

    http://api.crossref.org/works?filter=has-funder:true&rows=0
    
## See how many Hindawi works have funder information

    http://api.crossref.org/members/98/works?filter=has-funder:true&rows=0
    
  or
    
    http://api.crossref.org/works?filter=member:98,has-funder:true&rows=0
    
## See how many Elsevier works have funder information

    http://api.crossref.org/member/78/works?filter=has-funder:true&rows=0

## How many member publishers CrossRef has

    http://api.crossref.org/members?rows=0

## First 25 members:

    http://api.crossref.org/members?rows=25&offset=0

## Second 25 members:

    http://api.crossref.org/members?rows=25&offset=25

## Overview of Hindawi's particpation in CrossRef

    http://api.crossref.org/members?query=hindawi

though once you have a member ID ('98', in this case), you should use that instead. So above is the same as:

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





