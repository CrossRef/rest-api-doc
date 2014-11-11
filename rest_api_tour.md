# CrossRef TDM API Tour

## Version History

- V1: 2014-04-18, first draft.

## Overview

The following short examples show how the CrossRef REST APIs can be used to provide CrossPublisher suport for TDM applications. This demonstration is a bit of a paradox- it is targeted at a  non-technical audience who wants to understand a little but about the technical infrastructure that researchers can leverage for TDM applications.

## Technical notes

In many cases, you can simply paste the example URIs into a browser's URL box, but if you want to see the resulting JSON formatted correctly, then we recommend that you install one of the following plugins:

- Firefox Users:
- Chrome Users:

Some of the demonstrations here require the use of content negotiation. This is difficult to demonstrate in a browser unless you use a special plugin. Accordingly, the URIs for this demo have also been included as a project for the XXXX plugin available for Chrome.

## A fake problem

A researcher is interested in text mining all a set of literature mentioning the word "blood"

## Identifying a Corpus

Once a researcher has identified a problem, they need to identify the corpus that they want to explore using TDM. This corpus might be small (a few hundred items) or it might be large (millions of items). But in either case, it is likely that the corpus will span multiple publishers in multiple countries and with multiple business models.

CrossRef is not a discovery service. We expect that many third parties, both for profit and non-profit, will provide researchers with the discovery tools needed in order to identify a the Corpus of literature that they wish to mine for their particular application. As long as these third-party services allow the researcher to easily download the DOIs of the content they wish to mine, then

Having said that, CrossRef does provide some primitive metadata-based discovery tools which we will use to demonstrate a process for identifying a Corpus.

## Finding out what is in the CrossRef system

How many members does CrossRef have?

    http://api.crossref.org/members?rows=0

Who are they? Let's look at first 100 members

    http://api.crossref.org/members?rows=100

And the second 100 members

    http://api.crossref.org/members?rows=100&offset=100

How many DOI records does CrossRef have?

    http://api.crossref.org/works?rows=0

What content types does CrossRef have?

    http://api.crossref.org/types

How many journal article DOIs does CrossRef have?

    http://api.crossref.org/types/journal-article/works?rows=0

How many proceedings articles DOIs does CrossRef have?

    http://api.crossref.org/types/proceedings-article/works?rows=0

But eventually you will probably want to start looking at metadata records. Lets search for records that have the word "blood" in the metadata and see how many there are.

    http://api.crossref.org/works?query=%22blood%22&rows=0

Lets look at some of the results.

    http://api.crossref.org/works?query=%22blood%22&

Now lets look at one of the records

    http://api.crossref.org/works/10.1155/2014/413629

Interesting. The record has ORCIDs, fulltext links, and license links. You need license and fulltext links to text and data mine the content.

How many works have license information?

    http://api.crossref.org/works?filter=has-license:true&rows=0

How many license types are there?

    http://api.crossref.org/licenses?rows=0

How many works have a CC-BY license?

    http://api.crossref.org/works?rows=0&filter=license.url:http://creativecommons.org/licenses/by/3.0/


Ok, lets see how many records with the word "blood" in the metadata have license information and full text links

    http://api.crossref.org/works?filter=has-license:true,has-full-text:true&query=blood&rows=0

Let's download the results and download the content locally to TDM

We could just get them all:

    http://api.crossref.org/works?filter=has-license:true,has-full-text:true&query=blood&rows=884

But for the purposes of this demo, let's subdivide them by publisher.


First let's get a sample of Elsevier titles:

What is Elsevier's CrossRef member id number? 

    http://api.crossref.org/members?query=elsevier

Now what DOIs do  Elsevier have that match our criteria?

    http://api.crossref.org/members/78/works?filter=has-license:true,has-full-text:true&query=blood&rows=50

Now what is Hindawi's CrossRef member id number?

    http://api.crossref.org/members?query=elsevier

Now lets get some Hindawi articles that match the criteria:

    http://api.crossref.org/members/98/works?filter=has-license:true,has-full-text:true&query=blood&rows=50




## Other examples

See how many works have funder information

    http://api.crossref.org/works?filter=has-funder:true&rows=0

See how many Hindawi works have funder information

    http://api.crossref.org/members/98/works?filter=has-funder:true&rows=0

  or

    http://api.crossref.org/works?filter=member:98,has-funder:true&rows=0

See how many Elsevier works have funder information

    http://api.crossref.org/works?filter=member:78,has-funder:true&rows=0


Overview of Hindawi's particpation in CrossRef

    http://api.crossref.org/members?query=hindawi


Overview of Elsevier's particpation in CrossRef

    http://api.crossref.org/members?query=elsevier

How many works does Elsevier have?

    http://api.crossref.org/members/78/works?rows=0

First 25 Elsevier works

    http://api.crossref.org/members/78/works?rows=25&offset=0

Second 25 Elsevier works

    http://api.crossref.org/members/78/works?rows=25&offset=25

How many Elsevier works have license links?

    http://api.crossref.org/members/78/works?filter=has-license:true&rows=0

How many Elsevier works have full text links

    http://api.crossref.org/members/78/works?filter=has-full-text:true&rows=0

What license types does Elsevier support?

    http://api.crossref.org/licenses?filter=member:78

What license types does Hindawi support?

    http://api.crossref.org/licenses?filter=member:98

What license type does the journal with a particular ISSN support

    http://api.crossref.org/licenses?filter=issn:2090-8091

What licenses does a reasearcher with a particular ORCID publish under

    http://api.crossref.org/licenses?filter=orcid:0000-0003-1340-5202
