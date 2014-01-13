# Content "Syndication" through CrossRef Metadata
## Current situation

### Normative behavior for resolving CrossRef DOIs

When CrossRef DOIs are resolved via a browser, they  redirect to the HTTP URI recorded by the publisher in the element `doi_data/resource`. The URI recorded in `doi_data/resource` typically points to a "landing page. This landing page, in turn, typically presents:

1. Human readable bibliographic metadata 
2. Link(s) that enable one to acquire one or more human-readable full text representations of the resource (e.g. PDF, HTML, ePub) and/or one or more versions of the resource (e.g. AAM, VOR, etc.) 

Because the publisher controls the landing page, they can show/hide appropriate acquisition links according to their access control requirements and the status of the user viewing the landing page.

Note that not all CrossRef DOIs behave this way, but the vast majority do. The above is normative behavior for CrossRef DOIs.

Some aspects of this normative behavior are worth highlighting:

1) The default behavior is designed for humans
2) The publisher system needs to make decisions about what the user is allowed to access from the landing page
3) This mechanism is currently used (abused?) by numerous text mining tools, etc. via screen scraping techniques as it is often the most convenient (only) way to get full text representations of the content identified by the DOI.

### Extended CrossRef DOI resolution mechanisms  

#### "As-Crawled URLs"

In order to better support the use of CrossRef DOIs by search engines, the CrossRef metadata schema was extended. Specifically, CrossRef members wanted to be able to:

1. Direct search engines to platforms that were designed to handle the additional load imposed by search engines
2. Provide search engines with representations that were optimized for indexing (and, occasionally, which where degraded for "reading"). 

 CrossRef introduced the `<collection>` element. The collection element is a container element that can include any number of "alternative" `<resource>` elements. Each `/collection/resource` element, in turn, includes a "crawler" attribute so that specific search engine crawlers could be directed at specific `resource` URIs.

In order to use this mechanism, search engines needed to either:

a) Make use of CrossRef's proprietary API for accessing the metadata for a DOI and looking up the alternative resources.
b) Subscribe to dumps of CrossRef's metadata.

As such, no major search engine uses this mechanism for indexing CrossRef member web sites. Similarly, very few CrossRef members make use of the `collection/resource` element to support search engines. Of those that do record "as-crawled" URLs, none record separate URIs for different crawlers.

A typical section for as-crawled resources looks like this:

    <collection property="crawler-based">
              <item crawler="google">
                <resource>http://downloads.hindawi.com/journals/ijps/2013/435073.pdf</resource>
              </item>
              <item crawler="msn">
                <resource>http://downloads.hindawi.com/journals/ijps/2013/435073.pdf</resource>
              </item>
              <item crawler="altavista">
                <resource>http://downloads.hindawi.com/journals/ijps/2013/435073.pdf</resource>
              </item>
              <item crawler="yahoo">
                <resource>http://downloads.hindawi.com/journals/ijps/2013/435073.pdf</resource>
              </item>
              <item crawler="scirus">
                <resource>http://downloads.hindawi.com/journals/ijps/2013/435073.pdf</resource>
              </item>
     </collection>


In 2007, the "crawler" attribute was extended to support the iParadigms crawler for CrossCheck, but again, very few publishers make use of it.

#### Content Negotiation

In 2011 CrossRef introduced content negotiation for CrossRef DOIs. This mechanism provides a standard way for tools to request machine-readable representations of a CrossRef DOI's metadata. It also encourages both publishers and users of CrossRef metadata to follow standard linked-data mechanisms for recording alternative and related resource representations in their metadata using HTTP URIs.

#### Text and Data Mining (aka Prospect)  

In order to support text and data mining applications, CrossRef is encouraging publishers to has proposed using content negotiation and the current CrossRef metadata schema to support recording links to resource representations designed for text and data mining. 



But it is entirely up to the user as to how they make use of this information.

This is a critical point, because even though we are providing 'hints' to the user on which version of the resource is likely to be available to them, there is nothing the CrossRef can do to force the user to make use of these hints. There is nothing we can do to stop an un-subscribed user from trying to retrieve the VOR of content that is restricted to subscribers. It is still entirely up to the publisher to appropriately **enforce** the guidelines that they have documented in the CrossRef metadata. 



"Appropriate Application Resource"

















crawler="subsciber"
crawler="public"
crawler="syndicator"


(TBD- note that multiple resolution slightly more complicated)

This effectively means that doi_data/resource 


<resource> means mediated access for humans



In theory, this resource can point to "the thing itself" or to "something abou"

we then have following sections:

"as-crawled" for search engines

"text-mining" misnomer- but set that aside for a bit

What are things we want to distnguish:

Batch v Query

Human v Bot

*minimally* they want to text mine what they can see- e.g. the PDF. So conflating this made sense- until Elsevier came around.

OPDS has what they call "aquisition links" where rel attribute is set to a URI indicating what is being pointed to. They have the following types:

http://opds-spec.org/acquisition/open-access for Open Access publications
http://opds-spec.org/acquisition/buy for publications that you can buy
http://opds-spec.org/acquisition/borrow for publications that you can borrow
http://opds-spec.org/acquisition/subscribe for publications that you can subscribe to
http://opds-spec.org/acquisition/sample to sample a publication
http://opds-spec.org/acquisition when none of the other values are appropriate or you don't have additional information

These are problematic because they conflate various things. For example, something can CC-BY-SA , but paid for. For example:

http://www.amazon.co.uk/Little-Brother-Cory-Doctorow-ebook/dp/B008TGKXWW/ref=sr_1_1?s=books&ie=UTF8&qid=1389085868&sr=1-1&keywords=Little+Brother

And at the same time CC-BY-SA and for free:

http://craphound.com/littlebrother/download/


<collection property="text-mining" setbyID="creftest">
              <item>
                <resource mime_type="application/pdf">http://annalsofpsychoceramics.labs.crossref.org/fulltext/10.5555/515151.pdf</resource>
              </item>
              <item>
                <resource mime_type="application/xml">http://annalsofpsychoceramics.labs.crossref.org/fulltext/10.5555/515151.xml</resource>
              </item>
            </collection>


