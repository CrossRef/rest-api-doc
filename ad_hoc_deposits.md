A Guide to Making Ad hoc XML Deposits Using CrossRef's Manual XML Deposit Interface
===================================================================================

# Change History

| Date | Changes |
|------|---------|
| 2013-11-26 | Initial version |

# Background

CrossRef members participating in pilot projects, either those proposed by CrossRef or by other organisations
working with CrossRef, may be asked to deposit pieces of metadata that are not handled by their own internal 
publishing systems which interact with the CrossRef deposit system. This may leave a member without any
ready means of depositing new pieces of metadata and meeting their responsibility as participants of a pilot project.

An alternative deposit process exists that allows a member to deposit XML with CrossRef in an ad hoc manor, outside
of members' normal processes of XML deposit. This alternative involves three steps:

1. Optionally retrieve deposit XML via [CrossRef My Deposits](http://mydeposits.labs.crossref.org).
2. Manually or programatically alter or create deposit XML.
3. Manually deposit XML files via the [CrossRef admin interface](http://doi.crossref.org).

# Step 1. XML Retrieval (Optional)

A member may wish to deposit additional metadata against DOI records that have already been deposited with CrossRef.
However they may find it difficult to locate existing XML deposits within their own systems because:

- XML deposits can be incremental, requiring that a member understand a series of XML deposits for any given
  DOI and then stitch the series of deposits together into a single deposit.
- A member's systems that automatically interact with CrossRef may not record XML deposits.

CrossRef provides a tool that allows a member to download the most recent XML deposit for a given set of DOIs. This
XML accounts for incremental and replacement updates to DOI records, and can be retrieved as a single XML file for
multiple DOIs.

A member may use the [CrossRef My Deposits](http://mydeposits.labs.crossref.org) tool to download existing deposit XML.
A list of DOIs must be prepared, where each DOI is presented on its own line:

    10.5555/12345678
    10.5151/12345678
    10.1234/56789
    10.5555/1234

# Step 2. XML Alteration or Creation

Once an XML deposit file has been retrieved for some list of DOIs, either via the CrossRef My Deposits tool or from
a member's publishing systems, alterations must be made to reflect additional metadata. A member may also wish to create
new deposit records from scratch. In all cases, it will be useful to review CrossRef's online help resources that
document the CrossRef deposit schema:

- [Oxygen documentation of the CrossRef deposit schema](http://www.crossref.org/schema/documentation/4.3.3/4.3.3.html)
- [CrossRef online help portal](http://help.crossref.org)
- [CrossRef deposit schema XSD](http://www.crossref.org/schema/deposit/crossref4.3.3.xsd)

## Depositor E-mail Address

CrossRef deposit XML records the e-mail address of a person making a deposit. This is used to send updates on the success
or failure of processing a deposit, so it is important that it is set to an accessible e-mail address that can receive messages.
Set the depositor e-mail address by inserting XML similar to the following:

    <depositor>
      <name>creftest</name>
      <email_address>john@crossrefmember.com</email_address>
    </depositor>

The name element must be set to a depositing member's CrossRef account name, not the name of the person making a deposit.

## Specifying XML Namespaces

Include the latest CrossRef deposit schema as a namespace in the root element of modified XML deposits. Pilot projects
often use recent additions to the deposit schema that may not be available in older schema versions. An example of a root element
correctly specifying the latest version of the CrossRef deposit schema (4.3.3 as of writing):

    <doi_batch version="4.3.3"
               xsi:schemaLocation="http://www.crossref.org/schema/4.3.3
                                   http://www.crossref.org/schema/deposit/crossref4.3.3.xsd">

## XML Validation

It is possible to validate CrossRef deposit XML before depositing it. Use [CrossRef's XML parser](http://www.crossref.org/02publishers/parser.html)
to validate manually modified or created XML against the CrossRef deposit schema.

# Step 3. Manual XML Deposit

CrossRef deposit XML can be deposited manually using the [CrossRef admin interface](http://doi.crossref.org). Once signed
in, select 'Upload submissions', then select 'Metadata' as the type of submission and choose an XML file to upload. While
the submission system may immediately accept a deposit, it may not be valid. The deposit will be queued and its validity
will be checked as part of a process to include it in the CrossRef system. A deposit should be be considered complete
only when a successful e-mail response is received.
