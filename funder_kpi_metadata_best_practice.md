# Using Crossref metadata to enable auditing of conformance to funder mandates: A Guide for publishers

## Table of Contents

<!-- TOC depth:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Using Crossref metadata to enable auditing of conformance to funder mandates: A Guide for publishers](#using-crossref-metadata-to-enable-auditing-of-conformance-to-funder-mandates-a-guide-for-publishers)
	- [Table of Contents](#table-of-contents)
	- [Version History](#version-history)
	- [Contact info](#contact-info)
	- [Background](#background)
	- [Conventions](#conventions)
	- [Summary](#summary)
	- [Funding information](#funding-information)
	- [License information](#license-information)
		- [Embargoes](#embargoes)
	- [Recording links to full text and/or archived versions of documents, etc.](#recording-links-to-full-text-andor-archived-versions-of-documents-etc)
	- [Different licenses for different versions of the content](#different-licenses-for-different-versions-of-the-content)
	- ["Libre" vs "Gratis"](#libre-vs-gratis)
	- [Recording third party archive arrangements](#recording-third-party-archive-arrangements)
	- [Assigning and registering DOIs at acceptance](#assigning-and-registering-dois-at-acceptance)
		- [Assigning and registering DOIs for manuscripts that the publisher *has* made avaialble online](#assigning-and-registering-dois-for-manuscripts-that-the-publisher-has-made-avaialble-online)
		- [Assigning and registering DOIs for manuscripts that the publisher *has not yet* made available online](#assigning-and-registering-dois-for-manuscripts-that-the-publisher-has-not-yet-made-available-online)
	- [Bonus points](#bonus-points)
			- [Distributing standard bibliographic metadata](#distributing-standard-bibliographic-metadata)
			- [Distributing references](#distributing-references)
			- [CrossMark](#crossmark)
			- [Abstracts](#abstracts)
			- [ORCIDs](#orcids)
	- [Frequently Asked Questions](#frequently-asked-questions)
	- [XML Deposit Examples](#xml-deposit-examples)
		- [Full Deposits](#full-deposits)
		- [Partial Deposits](#partial-deposits)
		- [Registerd Content Deposits](#registerd-content-deposits)
<!-- /TOC -->


## Version History

- V01: 2013-09-08, first draft.
- V02: 2013-09-09, add examples + links.
- V03: 2013-09-10, adjust title. Correct typos.
- V04: 2013-09-12, changed AAM to AM.
- V05: 2013-09-18, incorporated corrections, suggestions from D. Shotton.
- V06: 2013-09-23, added `<free-to-read>` element info. Updated warning.
- V07: 2013-09-24, emphasize that publishers __must__ deposit funder identifiers, when they exist in the FundRef Registry.
- V08: 2013-11-04, Added FAQ about schema interpretation and usage
- V09: 2013-12-02, Added XML deposit examples
- v10: 2013-12-03, Updated `<free-to-read/>` element documentation to reflect latest NISO work. Added information about licensing CrossRef metadata to FAQ (hint, no license required for free APIs). Added labs email address. Changed formatting.
- v11: 2013-12-11, Added third party archive arrangements section. Updated examples to include archive locations.
- v12: 2015-11-27, Revisions to describe deposit workflow to support alerting funders/instituions when content has been "accepted". Pointers to latest schemas. General cleanup.

## Contact info

If you encounter problems with the API or the documentation, please report them to:

![](http://labs.crossref.org/wp-content/uploads/2013/01/labs_email.png)

## Background

Funders are increasingly setting mandates around publications that result from research they have funded. The mandates include specifications about licenses, embargoes, and notifications of publication acceptance and/or publication. This poses logistical problems for all the parties involved. Funders will need a way to track outputs from thousands of publishers. Publishers will need a standard and efficient way to demonstrate conformance to the mandates. All the stakeholders in the process (funders, publishers, institutions and researchers) will span disciplines, institutions, geographies and jurisdictions. Crossref was setup specifically to deal with these sorts of multiple bilateral relationships.

Crossref has extended its metadata schemas and Application Programming Interfaces (APIs) to enable funding agencies, institutions and publishers to use CrossRef as a metadata source that can be used to track research that is subject to these mandates and to ensure that said research is being disseminated according to the requirements of the mandates.

Funders, institutions, publishers and third parties providing research information management tools (e.g. *[CHORUS](http://publishers.org/press/107/)*, *[SHARE](http://www.arl.org/news/arl-news/2773-shared-access-research-ecosystem-proposed-by-aau-aplu-arl)*,*[Symplectic](http://symplectic.co.uk)* can make use of Crossref APIs and metadata in order to identify:

- Publications relating to research supported by particular funders.
- Publications from particular researchers identified by their ORCID ID.
- The bibliographic metadata for said publications.
- The licenses under which said publications have been released.
- Any embargoes applied to said publications.
- The location of the full text of the Best Available Version (BAV) for said publications for both reading and text & data mining (TDM) applications.
- The long-term preservation arrangements that have been made for the VOR of said publications.
- The ORCIDs associated with those publications.
- Any updates (errata, corrigenda, retractions) applied to said publications.

This data can be propagated by publisher at any time after publication acceptance- according to the requirements of the relevant mandates.

The CrossRef extended APIs, of course, will only work if publishers supply the appropriate metadata and follow the specified metadata deposit workflows. This document outlines the metadata that publishers will need to provide and the metadata deposit workflows they will need to support in order to advertise their conformance to funder mandates.

## Conventions

Although this document is not an RFC, it will follow the conventions of __[rfc2119](http://www.ietf.org/rfc/rfc2119.txt)__ in the use of the following terms:

1. __must__ - This word, or the terms "REQUIRED" or "SHALL", mean that the definition is an absolute requirement for meeting best practice.
2. __must not__ - This phrase, or the phrase "SHALL NOT", mean that the definition is an absolute prohibition for meeting best practice.
3. __should__ - This word, or the adjective "RECOMMENDED", mean that there may exist valid reasons in particular circumstances to ignore a particular item, but the full implications must be understood and carefully weighed before choosing a different course.
4. __should not__ - This phrase, or the phrase "NOT RECOMMENDED",  mean that there may exist valid reasons in particular circumstances when the particular behavior is acceptable or even useful, but the full implications should be understood and the case carefully weighed before implementing any behavior described with this label.
5. __may__   This word, or the adjective "OPTIONAL", mean that an item is truly optional.  One vendor may choose to include the item because a particular marketplace requires it or because the vendor feels that it enhances the product while another vendor may omit the same item. An implementation which does not include a particular option MUST be prepared to interoperate with another implementation which does include the option, though perhaps with reduced functionality. In the same vein an implementation which does include a particular option MUST be prepared to interoperate with another implementation which does not include the option (except, of course, for the feature the option provides.)

## Summary

In order to advertise conformance to funder mandates, Crossref members:

- __must__ record funder information in their CrossRef deposits  
- __must__ deposit the FundRef funder identifiers corresponding to their funder names where these exist in the FundRef Registry
-  __should__ record award numbers when possible.
- __should__ record funding information within CrossMark records __if__ they are either implementing CrossMark or are planning to implement CrossMark within the next two years.
- __should__ record licensing information if they have it by means of a URI specifying the license under which the publication is made.
- If publishers do not have licensing information, they __should__ record a placeholder URI and fill in the target of the URI once they have agreed on licensing information.
- __should__ record full text links to the readable version(s) of the document. This may include different resources for the Version of Record (VOR) and Author Accepted Manuscript (AM).
- __should__ record full text links to representations of the document that are made available for TDM. These may be the same or different to the "readable" versions of the document pointed to above.
- Where they are recording multiple versions of the document (e.g. AM & VOR), the publisher __should__ map licensing information to the specific resource versions.
- __should__ record full text links to archived versions of the document identified by the CrossRef DOI.
- __should__ record archive arrangements made with third party archiving organizations where the document identified by the CrossRef DOI is archived with the third party.

In order to enhance the utility of CrossRef metadata to funders and in order to enable more sophisticated funder/publisher KPIs, Crossref members:

- __should__ consider participating in CrossMark in order to record updates, errata, corrigenda,retractions and withdrawals.
- __should__ consider depositing abstracts using CrossRef's JATS abstract element support.
- __should__ consider collecting and depositing ORCIDs for publication authors.
- __should__ consider making the bibliographic metadata and references for documents resulting from agency funding maximally available by overriding CrossRef opt-outs using the `<metadata_distribution_opt>` and `<reference_distribution_opt>` elements.

In order to alert funders of relevant publications as soon as possible, Crossref members:

- __should__ consider assigning and registering DOIs at acceptance

## Funding information

CrossRef supports the recording of funding information for a publication via its [FundRef](http://www.crossref.org/fundref/) program. FundRef defines an open, standard [registry of funder names and funder identifiers](http://www.crossref.org/fundref/fundref_registry.html) that can be used in order to increase the accuracy of the funding information recorded. Although FundRef supports recording award_numbers along with funder identifiers, FundRef does __not__ define standards for recording award numbers as practice varies greatly across funders.

To support funder KPIs, members __must__ deposits funder metadata using the specifications defined for the FundRef program. Specifically, when depositing metadata you:


1. __must__ include funder information.
2. __must not__ deposit your funder names without at least trying to map them to FundRef identifiers in the FundRef registry. Depositing funder names that are included in the FundRef registry, but without their respective FundRef Funder Identifiers, will pollute the FundRef metadata and lower the value of the service for all participants. Note that the KPI APIs will only work for Funder metadata that includes FundRef Funder Identifiers.
3. __should__ include award numbers in FundRef metadata when possible. Although the standard KPI API does not make direct use of award numbers, individual agencies may be able to make use of included award numbers where found.
4. __should__ deposit FundRef data as part of a CrossMark record if you (the publisher) already are (or *are planning* to become) a participant in CrossMark. There are two reasons for this: First, it ensures that the Funder Metadata is available __both__ in a standard machine readable format __AND__ via a standard UI for readers. Second, it ensures that the Funder metadata is made maximally reusable via a CC Zero license waiver. Note that publishers do not __need__ to have implemented CrossMark yet to deposit Funder metadata via CrossMark. We expect that publishers may take a year or more before they have fully implemented all of CrossMark's features.

See CrossRef's Help pages for [Technical details on depositing FundRef metadata.](http://help.crossref.org/#fundref)

## License information

One of the main drivers for the FunRef KPI API is that many funders are required to report on the public availability of the results and publications arising from funder-financed research. Funders are therefor interested in understanding how publications related to funded research are licensed.

To deposit license information, publishers __must__ use the `<license_ref>` element. The value of the `<license_ref>` element __must__ be a stable HTTP URI which points to a human-readable document that either includes (or guides the reader to) any copyright and/or licensing information related to the CrossRef DOI of the content. The URI __must__ point either to a location on the publisher's site or to the stable location of any well-known licenses such as those of the Creative Commons.  

Note that it is entirely acceptable to record a `<license_ref>` URI as a "placeholder." If you are still working out specific licensing terms, the URI you record __should__ point to a blank page or even a simple re-assertion of the document's copyright. There is a big difference between recording at least some `<license_ref>` URI and recording no `<license_ref>` URI at all. The former indicates an intent to eventually clarify licensing information, whereas the latter indicates that the licensing information is likely to remain ambiguous.

Use of the `<license_ref>` element is best explained through examples.

The `<license_ref>` for content licensed under the popular [CC-BY](https://creativecommons.org/licenses/by/3.0/deed.en_US) license, would look like this:

    <license_ref>http://creativecommons.org/licenses/by/3.0/deed.en_US</license_ref>

Where as the *Journal of Psychoceramics* might record that their content is licensed under a proprietary license like this:

    <license_ref>http://www.psychoceramics.org/license_v1.html</license_ref>

You can deposit multiple `<license_ref>` elements- so the following would indicate that a document was available under a dual license (e.g. one for commercial applications and one for non-commercial applications).

    <license_ref>http://www.psychoceramics.org/non_commercial_license_v1.html</license_ref>
    <license_ref>http://www.psychoceramics.org/commercial_license_v1.html</license_ref>

### Embargoes

Publishers may want to record that a document is under embargo. In other words, that it is available under access control and a proprietary license for a set period of time, after which it is available under an open license. Publishers wishing to record embargoes __should__ use the optional `start_date` attribute on the `<license_ref>` element.

For example, the following records that the content is under a proprietary license from its date of publication on February 3, 2014 and that it is under a CC-BY license a year later on February 3, 2015:

    <license_ref start_date="2014-02-03">http://www.psychoceramics.org/license_v1.html</license_ref>
    <license_ref start_date="2015-02-03">http://creativecommons.org/licenses/by/3.0/deed.en_US</license_ref>

Note that the value of the start_date element __must__ be recorded using the format `YYYY-MM-DD`
The `start_date` attribute can be combined with multiple `<license_ref>` elements to indicate that a document is under a proprietary license during an embargo, but that it is then under a dual (commercial/non-commercial) license a year later)

    <license_ref start_date="2014-02-03">http://www.psychoceramics.org/license_v1.html</license_ref>

    <license_ref start_date="2015-02-03">http://www.psychoceramics.org/non_commercial_license_v1.html</license_ref>

    <license_ref start_date="2015-02-03">http://www.psychoceramics.org/commercial_license_v1.html</license_ref>

Note that there is __no__ corresponding `end_date` attribute for the `<license_ref>` element. This is because including end dates could introduce ambiguities. For example:

- Open Licenses, such as CC, do not have "end dates".
- With end dates, it would be possible to inadvertently record "gaps" between licenses.

You might ask why one should record a license that starts in the future? Wouldn't it be better to just update the `<license_ref>` element at the time the license changes? By recording that another license takes effect in the future, you are informing the consumer of the metadata that the current restricted license is only for the embargo period. In short, you are recording the __intent__ to change the license when the embargo is done. Furthermore, providing additional metadata for a current publication at some future date is an additional chore for the publisher that might well be overlooked.

In the above examples, the `<license_ref>` element is unqualified and should therefor be considered to apply to the content pointed to by any `<resource>` URIs included in the CrossRef metadata. The CrossRef metadata schema supports recording different license for different versions of the resource and this will be discussed below. However, first let's look at at the role the `<resource>` element plays in providing funding agency KPIs.

## Recording links to full text and/or archived versions of documents, etc.

Funders are not just interested in reporting on the licensing terms of publications resulting from funder-financed research. They are also interested in making sure that the full text content of the BAV is made available for reading, automated processing and archiving.

To this end, publishers need to be able to record links to the full text of the content to which a DOI refers. Additionally, publishers will want to offer different versions (e.g. AM or VOR) and different representations (e.g. PDF for viewing, XML for TDM, etc.) of the content tailored for specific applications.

The `<resource>` element in CrossRef metadata is most often used to record an HTTP URI pointing at the publisher's landing page for the publication identified by the CrossRef DOI in question. However, the CrossRef schema has long supported the recording of multiple `<resource>` elements in order to enable, for example:

- Multiple resolution
- Search engine indexing
- CrossCheck indexing

CrossRef has extended the ability to record multiple `<resource>` elements in order to allow the recording of URIs which point to the full text of content identified by the CrossRef DOI. The publisher can record multiple representations of the full text (e.g. PDF, XML, plain text) using the new `mime_type` attribute and then, through their access control systems, control who is able to reach which representation and under which conditions.

Note that, by recording a `<resource>` that points to the full text, you are not necessarily guaranteeing that the URI will be accessible

Note also that the publisher could theoretically choose only to deposit `<resource>` elements for full text representations once an embargo has ended. However, this approach may prove fraught, as any mistakes or delays in the redeposit process might lead the funding agency to believe that the publisher has not made the relevant content accessible at the end of the embargo period.

Further detail on using the `<resource>` element for recording links to full text can be found on the [Prospect support site](http://prospectsupport.labs.crossref.org/full-text-uris-technical-details/) and in the CrossRef deposit schema documentation for the [ `<collection>` ](http://www.crossref.org/schema/documentation/4.3.4/4.3.4.html#collection) and [ `<resource>` ](http://www.crossref.org/schema/documentation/4.3.4/4.3.4.html#resource) elements.

## Different licenses for different versions of the content

Some publishers may want to record different licenses for different versions of the `<resource>` element recorded in CrossRef metadata. For example, one `<resource>` element may point to a URI intended for subscribed readers, while another `<resource>` element may point to a version of the document intended for Text and Data Mining (TDM) applications. Similarly, a publisher may choose to apply one license to the "Author Accepted Manuscript" (AM) and another to the "Version of Record" (VOR).

To accommodate these scenarios, the `<license_ref>` element supports an `applies_to` element. Similarly, the `<resource>` element has been extended to support a `content_version` attribute. Publishers can use these element/attribute combinations to apply specific licenses to specific versions of the resource. For example, to indicate the "VOR" version of a document is licensed under a proprietary license, but that the "AM" version of the same document is licensed under an open license, the `<license_ref>` and `<resource>` elements could be combined like this:

    <license_ref applies_to="vor">http://www.psychoceramics.org/license_v1.html</license_ref>

    <!-- … -->

    <license_ref applies_to="am">http://creativecommons.org/licenses/by/3.0/deed.en_US</license_ref>

    <!--- other CrossRef metadata -->

    <resource content_version="vor">http://www.psychoceramics.org/fulltext/vor/10.5555/12345678</resource>

    <!-- … -->

    <resource content_version="am">http://www.psychoceramics.org/fulltext/am/10.5555/12345678</resource>

The `<license_ref>` and `<resource>` elements along with their respective `start_date`, `applies_to`, and `content_type` attributes can all be combined to support more complex assertions. So, for example the following example says that a document is only available under a proprietary license for readers during an embargo period, but is then available to the public for reading under a more open license and for non-commercial TDM applications under a specific TDM license:

    <license_ref start_date="2014-02-03" applies_to="vor">http://www.psychoceramics.org/license_v1.html</license_ref>

    <!-- … -->

    <license_ref start_date="2015-02-03" applies_to="am">http://www.psychoceramics.org/open_license.html</license_ref>

    <!-- … -->

    <license_ref start_date="2015-02-03" applies_to="tdm">http://www.psychoceramics.org/nc_tdm_license.html</license_ref>

    <!--- other CrossRef Metadata -->

    <resource content_version="vor">http://www.psychoceramics.org/fulltext/vor/10.5555/12345678</resource>

    <!-- … -->

    <resource content_version="am">http://www.psychoceramics.org/fulltext/am/10.5555/12345678</resource>

    <resource content_version="tdm">http://www.psychoceramics.org/fulltext/tdm/10.5555/12345678.xml</resource>

Detailed information on recording licensing information in CrossRef metadata can be found in the CrossRef schema documentation for the [ `<license_ref>` ](http://www.crossref.org/schema/documentation/4.3.4/AccessIndicators_xsd.html#license_ref) element.

## "Libre" vs "Gratis"

The license information recorded in the `<licence_ref>` element can tell you what you are allowed to do with the resources the licenses point to, but they do not say anything about whether or not there is a monetary charge involved.
In order to allow a publisher to record whether access to the content requires payment, CrossRef supports a new `<free-to-read/>` element. The `<free-to-read/>` element is an empty element. It can include two attributes, a `start_date` and an `end_date`. The `<free-to-read/>` elements works as follows:

 - The presence of a <free-to-read/> element in CrossRef metadata __should_ be interpreted to mean that the full text content pointed to by the  DOI resource is available "gratis" during the time period specified by the start_date and end_date attributes.
 - If the <free-to-read/> element only includes a `start_date` attribute, then the element __should__ be interpreted to mean that the content pointed to by the DOI resource will be made gratis from `start_date` on.
 - If the <free-to-read/> element only includes a `end_date` attribute, then the element __should__ be interpreted to mean that the content pointed to by the DOI resource will be made gratis from the publication date to and including the `end_date`.
 - If the <free-to-read/> element has __no__ `start_date` __or__ `end_date` attributes, then the element __should__ be interpreted to mean that the content pointed to by the DOI resource is available "gratis" from the date of publication on.
 - If the <free-to-read/> element is not present in the DOI record, one __should not__ assume that the resource pointed at by the DOI is available to read "gratis".


 When the `<free-to-read>` element is combined with the `<license_ref>` element, the publisher can record sophisticated information about the availability and reusability of content. For example:

**restrictive licenses and possibly a payment**

    <license_ref>http://tinypublisher.org/licenses/proprietary.html</license_ref>

**restrictive licenses and no payment (e.g free copy of an article from a subscription journal)**

    <free-to-read/>
    <!-- … -->
    <license_ref>http://tinypublisher.org/licenses/proprietary.html</license_ref>

**have unrestrictive licenses and a possibly a payment (e.g. a CC-BY licensed novel for sale on Amazon)**

    <license_ref>http://creativecommons.org/licenses/by/3.0/deed.en_US</license_ref>

**have unrestricted licenses and NO payment**

    <free-to-read/>
    <!-- … -->
    <license_ref>http://creativecommons.org/licenses/by/3.0/deed.en_US</license_ref>

## Recording third party archive arrangements

Funders may be concerned that publisher links to full-text content will become unavailable in exceptional circumstances. They may stipulate that content is archived with a third party archiving organization, and may even suggest a list of acceptable archive organizations with which documents should be archived.

Publishers can record the archive arrangement or archive intention of a document using the `<archive_locations>` element in CrossRef deposit metadata. Any number of archive locations can be specified, for example a document may be archived with both `Portico` and `CLOCKSS`:

    <archive_locations>
	  <archive name="CLOCKSS"/>
	  <archive name="Portico"/>
    </archive_locations>

CrossRef maintains a vocabulary of archive locations within the CrossRef deposit schema. The latest list of possible archive location values can be found in the documentation for the [ `<archive>` element ](http://www.crossref.org/help/schema_doc/4.3.6/4.3.6.html).

## Assigning and registering DOIs at acceptance

Funders and institutions would like to be notified of impending publications as early as possible. Some mandates, like the [one issued by HEFCE](http://www.hefce.ac.uk/pubs/year/2015/CL,202015/), even require that institutional repositories be notified of an impending relevant publication as soon as the manuscript is accepted- even if said manuscript has not yet been made available online.

### Assigning and registering DOIs for manuscripts that the publisher *has* made avaialble online

Crossref has always supported the deposit of DOIs for accepted manuscripts __if__ said manuscripts have also been made available online. This is a common practice that goes under various names including “publish ahead of print,” “article in progress,” “article in press,” “online ahead of print,” “online first”, etc. Crossref rules state that in this situation, Crossref members:

 - __may__ assign DOIs to accepted manuscripts
 - __should__ carry over the DOI assigned to the accepted manuscript to the final published version.
 - __should__ update the metadata for the DOI with that of the final published version.

 These rules reflect that, though there may be significant value added by he publisher between acceptance and final publication,  the accepted and the final published version are interchangeable from a citation point of view. This is because the transition from acceptance to the final published version should not introduce any changes that are likely to effect the interpretation of crediting of the work.  

### Assigning and registering DOIs for manuscripts that the publisher *has not yet* made available online

*(Please note that this section includes a draft recommendation which have not yet been ratified by our members or implemented. We are soliciting feedback on this section of the  document.)*

Crossref will support a new mechanism and workflow to support the registration of DOIs for accepted manuscripts __before__ they are made publicly available online. This feature can be used by publishers as a mechanism for informing funders and institutions of impending publications. To use this, publishers will deposit a special type of Crossref record called "registered content."

The schema and rules governing the "registered content" type attempt to balance the publisher's desire to control publicity around their content with the requirements that funders and institutions have to know as soon as possible when content governed under their mandates has been accepted for publication. Once the publication is made available online (either as an accepted manuscript or version of record), then the publisher can simply redeposit and replace the "registered content" record with a full metadata record using a Crossref schema appropriate to the publication (e.g. journal article).

DOIs for "registered content" will not resolve to a publisher's landing page. Rather, they will resolve to a landing page controlled by Crossref. This landing page will __minimally__ display the DOI, the acceptance date, and an "intent to publish statement" which, by default will read as follows (with the appropriate {variables} filled in):

>> The DOI {DOI} has been registered for content that was accepted for publication by {publisher name} on {date_of_acceptance}. When this content is available, the publisher will update this DOI, at which point it will automatically redirect you to the copy on the publisher's site.

Publishers will able to apply limited customizations to the landing page. These include:

- a custom "intent to publish" statement which will replace the default one provided by Crossref.
- a publisher logo to display at the top of the landing page.
- the display of all provided optional extra metadata such as funder identifiers, ORCID ids, license information, etc.
- a CrossMark, to handle situations in which a publisher rescinds an acceptance.

If the publisher provides metadata beyond that required, it will be displayed on the landing page below the "intent to publish" statement. "Registered content" records will also be made available through the CrossRef REST API and through Crossref metadata search.

By having Crossref control the landing page for "registered content" we can ensure that:

- there is always a landing page- even for content that it not yet available online
- metadata is displayed consistently for registered content
- members do not abuse the lightweight metadata requirements of "registered content" in order to register other content types more easily.

The schema for "registered content" will only support a minimal subset of metadata that can be used by funders and institutions to detect and flag impending publications that are relevant to them.

Registered content DOI records:

- __must__ include a DOI
- __must__ include a date of acceptance
- __must__ include the publisher name
- __must__ must be replaced with appropriate full metadata using an appropriate schmea for the content type when the publisher makes the content publically available
- __should__ include an "intent to publish statement." If the publisher does not provide an "intent to publish statement" of their own, then Crossref will provide a default statement.
- __may__ include a logo to diplay at the top of the landing page
- __may__ include a custom "intent to publish statement."
- __should__ include funder information
- __should__ include FundRef funder identifiers corresponding to their funder names where these exist in the FundRef Registry
- __should__ include ORCIDs
- __should__ include license information
- __should__ author affiliation information
- __may__ include the publication title
- __may__ include the item title (e.g. article title)

The "registered content" type is specifically designed for publishers who want to be able to inform funders and institutions that an article has been accepted for publication, even when the publisher is for some reason unable to make the accepted manuscript publicly available. The landing page and metadata for the "registered content" type allow the publisher to provide as much data as is needed to notify interested parties, yet not reveal commercially or promotionally sensitive information about their upcoming publications. When the publisher is ready to make the content available, they simply have to replace the temporary "registered content" record with a permanent DOI record.

## Bonus points

The more metadata that publishers record for publications arising from agency funded research, the more useful that metadata will be to said agencies and the more value they will see from publishers. Where as the above sections details metadata elements that agencies will __expect__ in order to be able to compile basic KPIs and offer portal services, additional metadata will allow agencies to create even more sophisticated KPIs and services. As such, publishers should seriously consider depositing the following additional metadata elements in their CrossRef deposits.

#### Distributing standard bibliographic metadata

Metadata deposited to CrossRef is made available freely via numerous CrossRef query APIs. However all deposited metadata is subject to opt-outs in the case of bulk distribution APIs and data dumps. In order to make sure that bibliographic metadata for publications arising from agency funding is maximally available, publishers __should__ consider setting the value of the `<metadata_distribution_opts>` element for DOIs to `any`.  Further details can be found in [CrossRef's schema documentation for the `<metadata_distribution_opts>` element.](http://www.crossref.org/schema/documentation/4.3.4/NO_NAMESPACE.html#metadata_distribution_opts.att_metadata_distribution_opts)

#### Distributing references

References made in publications arising from agency funding can provide agencies with an overview of what literature is considered important in the fields that they fund. Many publishers deposit references to CrossRef as part of their participation CrossRef's [CitedBy](http://www.crossref.org/citedby/index.html) service. However, participation in CitedBy does not automatically make references available via CrossRef's standard APIs. In order for publishers to distribute references along with standard bibliographic metadata, publishers need to set the `<reference_distribution_opt>` element to `any` for each DOI deposit where they want to make references openly available. By setting this element, references for the DOI will be distributed without restriction through all of CrossRefs APIs and bulk metadata dumps. Further details can be found in [CrossRef's schema documentation for the `<reference_distribution_opt>` element.](http://www.crossref.org/schema/documentation/4.3.4/4.3.4.html#reference_distribution_opts.att)

#### CrossMark

[CrossMark](http://www.crossref.org/crossmark/) provides a standard mechanism for alerting researchers to updates to published documents- including corrections, errata, corrigenda retractions and withdrawals. Use of the CrossMark service sends a signal to researchers and agencies that publishers are committed to maintaining the integrity of the scholarly record.

Additionally, CrossMark also provides a standard, cross-publisher, user interface that researchers can use to view FundRef information and licensing information. This user interface works both from publisher landing pages and from published PDFs. More information can be found on the [CrossMark support site](http://crossmarksupport.crossref.org/)

#### Abstracts

Many funding agencies are interested in building custom portals that highlight agency-funded research. In order to provide users of these portals with the best experience, agencies will want, where possible, to display abstracts of publications along with their standard bibliographic metadata.

CrossRef supports the deposit of abstracts conforming to the [JATS](http://jats.nlm.nih.gov/) abstract element. Further details can be found in the [CrossRef Schema Documentation of the `<abstract>` element](http://www.crossref.org/schema/documentation/4.3.4/JATS1.html#abstract).

#### ORCIDs

[ORCID](http://www.orcid.org/)s are unique identifiers for researchers. CrossRef supports the deposit of ORCIDs for authors. The presence of ORCIDs in CrossRef metadata will, in turn, allow agencies to tie agency funded research publications directly to researchers. Widespread use of ORCIDs in CrossRef deposits could even let agencies start to develop publication KPIs for researchers that they fund. Further details on CrossRef's ORCID support can be found in the [CrossRef Schema Documentation of the `<ORCID>` element](http://www.crossref.org/schema/documentation/4.3.4/4.3.4.html#ORCID)


## Frequently Asked Questions

**Q:** What license applies to the metadata retrieved by the [CrossRef APIs to support key performance indicators (KPIs) for funding agencies](funder_kpi_api.html)?
<br />
**A:** CrossRef asserts no claims of ownership to individual items of bibliographic metadata and associated Digital Object Identifiers (DOIs) acquired through the use of the CrossRef Free Services. Individual items of bibliographic metadata and associated DOIs may be cached and incorporated into the user's content and systems. More information can be found [on our web site](http://www.crossref.org/requestaccount/).

**Q:** What does it mean if a `<license_ref>` element has no `start_date` attribute?
<br />
**A:** This should be interpreted to mean that the `<license_ref>` applies from the earliest publication date.


**Q:** What does is mean if there is no `applies_to` attribute for the `<license_ref>` element?
<br />
**A:** This should be interpreted to mean that the `license_ref` applies to **all** the `<resource>` elements in the record.


**Q:** What does it mean if the `<resource>` element doesn't have a `content_version` attribute?
<br />
**A:** This should be interpreted to mean that any `<resource>` elements point to the version of record ('vor')

**Q:** What does it mean if there is no correspondence between existing `<license_ref>` `applies_to` attributes and existing `<resource>` `content_version` attributes?
<br />
**A:** This probably means the publisher made a mistake depositing the metadata.  

## XML Deposit Examples

### Full Deposits

Full deposits use the [standard deposit schema](http://www.crossref.org/schema/deposit/crossref4.3.4.xsd).

- [Full deposit](examples/full.xml)
- [Full deposit with CrossMark](examples/full-crossmark.xml)

### Partial Deposits

Partial deposits use the [resource deposit schema](http://doi.crossref.org/schemas/doi_resources4.3.2.xsd).

Partial deposits update only part of a DOI's metadata. In the CrossRef help system
they are referred to as **resource deposits**, but it is not just resources that can
be provided as a partial deposit. Licenses, funding information and CrossMarks can also
be provided as partial deposits.

Many partial deposits can be provided in a single batch deposit. The `<body>` element can
contain any number of partial deposits of any type, some of which may be partial deposits for
the same DOI. For example, two partial deposits could be provided for the same DOI,
one updating funding information, the other updating license information.

- [Partial deposit of additional resource links](examples/partial-resources.xml)
- [Partial deposit of funding information without CrossMark](examples/partial-funders.xml)
- [Partial deposit of license information without CrossMark](examples/partial-licenses.xml)
- [Partial deposit of a CrossMark with license and funding information](examples/partial-crossmark.xml)

### Registerd Content Deposits

*coming soon*
