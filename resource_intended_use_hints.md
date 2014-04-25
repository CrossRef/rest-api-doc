
# Resource Intended Use Hints Through CrossRef Metadata

## Version History

- V1 2014-01-08, Initial draft
- V2 2014-04-23, Add examples
- V3 2014-04-24, Add 'any' collection property type

## The Problem

There is concern that the CrossRef schema does not give enough information to users about which resources are appropriate for different user classes and/or applications. For example, a publisher might want to be able to provide different lists of resources for the following use cases:

- A human subscriber accessing the content to read
- A human non-subscriber accessing the content to read
- A human from an entitled funding agency accessing the content to read

- A bot run by a subscriber accessing the content to TDM
- A bot run by a non-subscriber accessing the content to TDM
- A bot run by an entitled funding agency accessing the content to TDM

- A bot run by a subscriber accessing the content to syndicate
- A bot run by a non-subscriber accessing the content to syndicate
- A bot run by an entitled funding agency accessing the content to syndicate

In the above cases, the content in question might or be either "restricted" (e.g.  requiring a subscription to access) or "unrestricted" (open access, free). Furthermore, any restrictions may be time-boxed via embargoes. This effectively means that their are sub-cases where the desired behavior might change depending on when the user (bot or human) tries to access the content.

 So for the above use cases the publisher might want the user to select a resource that points to either:

- A human readable landing page on the publisher's main website.
- A human readable representation of the VOR (e.g. PDF, HTML, etc.) on the publisher's main website.
- A human readable representation of the AAM (e.g. PDF, HTML, etc.) on the publisher's main website.

- A machine readable representation of the VOR (e.g. XML, Plain Text) on the publisher's TDM server
- A machine readable representation of the AAM (e.g. XML, Plain Text) on the publisher's TDM server

- A human readable representation of the VOR (e.g. PDF, HTML, etc.) on the publisher's syndication server.
- A human readable representation of the AAM (e.g. PDF, HTML, etc.) on the publisher's syndication server.


Selecting the appropriate resource would seem to hinge on three criteria:

- Is the agent a human or a bot?
- Is the content under restricted access? For example, is a subscription needed?, is the content currently under embargo?
- is the agent entitled at this particular time?
- What is the desired application? Viewing, indexing, syndication, tdm?


We have used the term "BAV" to describe the "Best Available Version."- where "version" refers to the JATS version of a document (i.e. AAM, VOR). The idea is that some versions of the document (e.g. VOR) may be under restricted access (e.g. under embargo, require a subscription) and are thus "unavailable" to certain classes of user, while other versions of the document (e.g. AAM) are not under restrictions and are thus "available" to the respective class of user.

But the above use-cases introduce another facet that may need to be considered when selecting a resource- the "Appropriate Application Resource" (AAR). That is, the use-cases above imply that publishers might want to direct users at resources pointing to systems that are optimised to support a particular application. For example, a publisher might have a server that is reserved for search engines, another for syndication partners, etc.

## Providing Hints For Selecting a BAV

The normative HTTP mechanism to handle these requirements technically, would be to use content negotiation. That is, an agent could request a resource using a GET and be shown the options for different available versions of the resource in the response headers. The user agent would then select the BAV/AAR from the options presented in the publisher's response. In other words, ideally the system would work the same way that content negotiation works for languages or for browser feature sets.

However, CrossRef recognises that, in the short/medium term, it would be impractical to expect all of our members to integrate such content negotiation in their publishing sites. Thus we have added the ability for publisher to record attributes of the resource that one would normally use content negotiation for in selecting a BAV. Specifically, we have added the following two attributes to the resource element:

- mime_type
- content_version

 This allows the publisher to record resources for all the combinations of representation (PDF,HTML, Etc.) and version (VOR, AAM) that the publisher supports. In turn, the user can use these attributes to determine a-priori (i.e. without having to do an HTTP query for content negotiation)  which versions of the content **exist**. However, it does give the user any information to a-priori determine which versions of the content are likely to be **available**. In other words, just because the HTTP URI of a particular representation of a particular version of the content is listed, that doesn't mean that the user is guaranteed to be able to access the resource. The resource might require a subscription, it might be under embargo, etc.

 The addition of the `AccessIndicators` section to the CrossRef schema provides a mechanism that allows publishers to provide resource-specific information about the *purported accessibility* of the content pointed to by the resource using extended versions of the NISO-recommended `<license_reference>` and `<free-to-read>` elements. The `//AccessIndicators/license_ref` element has been extended with an "applies_to" attribute which can be used to map specific licenses (identified by HTTP URIs) to specific resources based on their content_version. So, for example, this enables a publisher to link one license to the resource that points to the VOR of the content and a different license to the resource that points to the AAM of the content. Similarly, the publisher will be able to map the `//AccessIndicators/free-to-read` element to specific resources identified by the content_version attribute in order to indicate that the content pointed to by a resource is available to "view" without any payment or registration preconditions.

 The `<license_ref>` element can also include a `start_date` attribute, which can be used to indicate when a license takes effect. This, in turn, can be used to record simple embargo information and to map that information to a specific version of the `<resource>` identified by the `content_type` attribute.

 For example, the following records that the VOR of the content is under a proprietary license from its date of publication on February 3, 2014 and that it is under a CC-BY license a year later on February 3, 2015 **and* that the AAM of the content is available from the date of publication under a CC-BY license:


    <resource content_version="vor">http://www.psychoceramics.org/fulltext/vor/10.5555/12345678</resource>
    <resource content_version="am">http://www.psychoceramics.org/fulltext/am/10.5555/12345678</resource>
    <!-- … -->
    <license_ref applies_to="VOR" start_date="2014-02-03">http://www.psychoceramics.org/license_v1.html</license_ref>
    <license_ref applies_to="VOR" start_date="2015-02-03">http://creativecommons.org/licenses/by/3.0/deed.en_US</license_ref>
    <license_ref applies_to="AAM" start_date="2014-02-03">http://creativecommons.org/licenses/by/3.0/deed.en_US</license_ref>


The addition of the `AccessIndicators` allows the publisher to indicate not only what versions of the resource **exist** , but also for the publisher to provide a mechanism for the user to determine a-priori (without doing an HTTP get for content negotiation) which versions of the resource are likely to be **available**. In short, the combination of `//AccessIndicator` elements and `//collection/resource` elements allows the user to determine the **BAV**.

## Providing Hints For Selecting an AAR

But this still leaves us with the question of how to provide the publisher to provide the user with hints as to which version of a resource is appropriate for a particular application. So-far we have identified the following applications:

 1. Viewing (e.g. on the publisher's web site, via landing page, etc.)
 2. Indexing (e.g. for search engines, etc.)


 Currently we already have three existing mechanisms for labeling different 'applications' of the resource.

 The first mechanism is implicit. That is  `/doi_data/resource` attributes have normatively recorded a link to the publisher landing page, from which the human reader can get a "viewable" version of the resource.

 The second two are encoded in the the `property` attribute of the `<collection>` container element. We currently support several values for this property, including `crawler-based` which was intended to be used to identify resources for search engines (i.e. indexing) and `text-mining` which has been introduced for resources designed for TDM bots. What seems to be missing is a value for "syndication." If we were to add a `syndication` attribute value then publishers would be able to record `/collection/resource` elements that meet all the use-cases identified above.  

## Putting It All Together

 If we combine the concept of the BAV and the AAR, we can meet all of the requirements outlined at the start of this document.

 The following example for the sample DOI 10.5555/12345678 says:

1. The VOR is under a year embargo (proprietary license) from the date of publication  
2. After the year, the VOR is available under a CC-BY license
3. The AAM is available under a CC-BY license from the date of publication
4. The humans clicking on the link will be directed to the landing page on the publisher's site
5. That robots requesting the content for a search engine (VOR only) should look for the PDF on the server `docstore.psychoceramics.org`. Access will be restricted as per the publisher's access control system.
6. That robots requesting the content for syndication (AAM or VOR) should look for the PDF on the server `docstore.psychoceramics.org`. Access will be restricted as per the publisher's access control system.
7. That robots requesting the content for text and data mining (AAM or VOR) should look for the XML on the server `marklogic.psychoceramics.org`. Access will be restricted as per the publisher's access control system.

### XML Example

    <doi_data>
        <doi>10.5555/12345678</doi>
        <timestamp>20121025161509</timestamp>
        <resource>
          http://psychoceramics.labs.crossref.org/10.5555-12345678.html
        </resource>
    </doi_data>

    <!-- … -->
    <license_ref applies_to="vor" start_date="2014-02-03">http://www.psychoceramics.org/license_v1.html</license_ref>
    <license_ref applies_to="vor" start_date="2015-02-03">http://creativecommons.org/licenses/by/3.0/deed.en_US</license_ref>
    <license_ref applies_to="am" start_date="2014-02-03">http://creativecommons.org/licenses/by/3.0/deed.en_US</license_ref>

    <collection property="text-mining">
      <resource content_version="vor" content_type="application/pdf">
          http://marklogic.psychoceramics.org/fulltext/vor/10.5555/12345678.xml
      </resource>
      <resource content_version="am" content_type="application/pdf">
          http://marklogic.psychoceramics.org/fulltext/am/10.5555/12345678.xml
      </resource>
    </collection>

    <collection property="syndication">
      <resource content_version="vor" content_type="application/pdf">
        http://docstore.psychoceramics.org/fulltext/vor/10.5555/12345678.pdf
      </resource>
      <resource content_version="am" content_type="application/pdf">
        http://docstore.psychoceramics.org/fulltext/am/10.5555/12345678.pdf
      </resource>
    </collection>

    <collection property="crawler-based">
        <item crawler="google">
            <resource content_version="vor" content_type="application/pdf">
                http://docstore.psychoceramics.org/fulltext/vor/10.5555/12345678.pdf
            </resource>
        </item>
        <item crawler="scirus">
            <resource content_version="vor" content_type="application/pdf">
                http://docstore.psychoceramics.org/fulltext/vor/10.5555/12345678.pdf
            </resource>
        </item>
    </collection>

## An 'any' Use Type

Some members will not want to provide intended use hints. These members can avoid repetition, and ignore intended use hints
altogether by using the 'any' collection property:

    <doi_data>
		<doi>10.5555/12345678</doi>
        <timestamp>20121025161509</timestamp>
        <resource>
          http://psychoceramics.labs.crossref.org/10.5555-12345678.html
        </resource>
    </doi_data>

    <!-- … -->
    <license_ref applies_to="vor" start_date="2014-02-03">http://www.psychoceramics.org/license_v1.html</license_ref>
    <license_ref applies_to="vor" start_date="2015-02-03">http://creativecommons.org/licenses/by/3.0/deed.en_US</license_ref>
    <license_ref applies_to="am" start_date="2014-02-03">http://creativecommons.org/licenses/by/3.0/deed.en_US</license_ref>

    <collection property="any">
      <resource content_version="vor" content_type="text/xml">
          http://docstore.psychoceramics.org/fulltext/vor/10.5555/12345678.xml
      </resource>
      <resource content_version="am" content_type="text/xml">
          http://docstore.psychoceramics.org/fulltext/am/10.5555/12345678.xml
      </resource>
	  <resource content_version="vor" content_type="application/pdf">
        http://docstore.psychoceramics.org/fulltext/vor/10.5555/12345678.pdf
      </resource>
      <resource content_version="am" content_type="application/pdf">
        http://docstore.psychoceramics.org/fulltext/am/10.5555/12345678.pdf
      </resource>
    </collection>

## Open Questions

We have suggested that the following values be used for AAR

- crawler-based (search engines)
- text-mining
- syndication
- any

Are they enough?

## Limitations of Proposed System

The hints provided in CrossRef metadata are just that, hints. There is nothing that CrossRef can do to force users to select an particular resource appropriate to their application. Thus, it is still going to be the responsibility of the publisher to check requests and to route them appropriately.

We have made a tradeoff in implementation. We have not normalized the element/attribute combinations and, therefor therefor the suytem is quite verbose- particularly for the simplest case where a publisher wants to record the same resources for each AAR.

## Access Control

Intended use hints do not imply the existence or lack of access control. A publisher may choose to, or choose not to incorporate access control into links intended for text and data mining or content syndication. This follows the same situation for DOI resolution URLs, where some publishers place access control on some article landing pages. The decision to use or not use access control is left to the publisher, and will most likely be made after consideration of content licensing, collection of APCs, journal business model, government policy and participation in various industry-wide initiatives.

## Removal of 'tdm' content_version

This proposed system negates the need for a 'tdm' content version. An implementation of this proposal will also remove the 'tdm' option from
the content_version resource attribute.
