# Crossref Metadata API JSON Format

## Versioning

| Version | Release Date | Comments |
|---------|--------------|----------|
| v1 | 11th July 2016 | First documented version |
| v2 | 26th July 2017 | Add abstract, authenticated-orcid, fix contributor fields |
| v3 | 15th May 2018 | Add peer review fields |

## Work

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| publisher | String | Yes | Name of work's publisher |
| title | Array of String | Yes | Work titles, including translated titles |
| original-title | Array of String | No | Work titles in the work's original publication language |
| short-title | Array of String | No | Short or abbreviated work titles |
| abstract | XML String | No | Abstract as a JSON string or a JATS XML snippet encoded into a JSON string |
| reference-count | Number | Yes | *Deprecated* Same as `references-count` |
| references-count | Number | Yes | Count of outbound references deposited with Crossref |
| is-referenced-by-count | Number | Yes | Count of inbound references deposited with Crossref |
| source | String | Yes | Currently always `Crossref` |
| prefix | String | Yes | DOI prefix identifier of the form `http://id.crossref.org/prefix/DOI_PREFIX` |
| DOI | String | Yes | DOI of the work |
| URL | URL | Yes | URL form of the work's DOI |
| member | String | Yes | Member identifier of the form `http://id.crossref.org/member/MEMBER_ID` |
| type | String | Yes | Enumeration, one of the type ids from `https://api.crossref.org/v1/types` |
| created | [Date](#date) | Yes | Date on which the DOI was first registered |
| deposited | [Date](#date) | Yes | Date on which the work metadata was most recently updated |
| indexed | [Date](#date) | Yes | Date on which the work metadata was most recently indexed. Re-indexing does not imply a metadata change, see `deposited` for the most recent metadata change date |
| issued | [Partial Date](#partial-date) | Yes | Earliest of `published-print` and `published-online` |
| posted | [Partial Date](#partial-date) | No | Date on which posted content was made available online |
| accepted | [Partial Date](#partial-date) | No | Date on which a work was accepted, after being submitted, during a submission process |
| subtitle | Array of String | No | Work subtitles, including original language and translated |
| container-title | Array of String | No | Full titles of the containing work (usually a book or journal) |
| short-container-title | Array of String | No | Abbreviated titles of the containing work |
| group-title | String | No | Group title for posted content |
| issue | String | No | Issue number of an article's journal |
| volume | String | No | Volume number of an article's journal |
| page | String | No | Pages numbers of an article within its journal |
| article-number | String | No | |
| published-print | [Partial Date](#partial-date) | No | Date on which the work was published in print |
| published-online | [Partial Date](#partial-date) | No | Date on which the work was published online |
| subject | Array of String | No | Subject category names, a controlled vocabulary from Sci-Val. Available for most journal articles |
| ISSN | Array of String | No | |
| issn-type | Array of [ISSN with Type](#issn-with-type) | No | List of ISSNs with ISSN type information |
| ISBN | Array of String | No | |
| archive | Array of String | No | |
| license | Array of [License](#license) | No | |
| funder | Array of [Funder](#funder) | No | |
| assertion | Array of [Assertion](#assertion) | No | |
| author | Array of [Contributor](#contributor) | No | |
| editor | Array of [Contributor](#contributor) | No | |
| chair | Array of [Contributor](#contributor) | No | |
| translator | Array of [Contributor](#contributor) | No | |
| update-to | Array of [Update](#update) | No | |
| update-policy | URL | No | Link to an update policy covering Crossmark updates for this work |
| link | Array of [Resource Link](#resource-link) | No | URLs to full-text locations |
| clinical-trial-number | Array of [Clinical Trial Number](#clinical-trial-number) | No | |
| alternative-id | Array of String | No | Other identifiers for the work provided by the depositing member |
| reference | Array of [Reference](#reference) | No | List of references made by the work |
| content-domain | [Content Domain](#content-domain) | No | Information on domains that support Crossmark for this work |
| relation | [Relations](#relations) | No | Relations to other works |
| review | [Review](#review) | No | Peer review metadata |
| publisher-location | String | No | Location of work's publisher |


## Work Nested Types

### Funder

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | String | Yes | Funding body primary name |
| DOI | String | No | Optional [Open Funder Registry](http://www.crossref.org/fundingdata/registry.html) DOI uniquely identifying the funding body |
| award | Array of String | No | Award number(s) for awards given by the funding body |
| doi-asserted-by | String | No | Either `crossref` or `publisher` |

### Clinical Trial Number

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| clinical-trial-number | String | Yes | Identifier of the clinical trial |
| registry | String | Yes | DOI of the clinical trial registry that assigned the trial number |
| type | String | No | One of `preResults`, `results` or `postResults` |

### Contributor

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| family | String | Yes | |
| given | String | No | |
| ORCID | URL | No | URL-form of an [ORCID](http://orcid.org) identifier |
| authenticated-orcid | Boolean | No | If true, record owner asserts that the ORCID user completed ORCID OAuth authentication |
| affiliation | Array of [Affiliation](#affiliation) | No | |

### Affiliation

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | String | Yes | |

### Date

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| date-parts | Array of Number | Yes | Contains an ordered array of `year`, `month`, `day of month`. Note that the field contains a nested array, e.g. `[ [ 2006, 5, 19 ] ]` to conform to citeproc JSON dates |
| timestamp | Number | Yes | Seconds since UNIX epoch |
| date-time | String | Yes | ISO 8601 date time |

### Partial Date

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| date-parts | Array of Number | Yes | Contains an ordered array of `year`, `month`, `day of month`. Only `year` is required. Note that the field contains a nested array, e.g. `[ [ 2006, 5, 19 ] ]` to conform to citeproc JSON dates |

### Update

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| updated | [Partial Date](#partial-date) | Yes | Date on which the update was published |
| DOI | String | Yes | DOI of the updated work |
| type | String | Yes | The type of update, for example `retraction` or `correction` |
| label | String | No | A display-friendly label for the update type |

### Assertion

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | String | Yes | |
| value | String | Yes | |
| URL | URL | No | |
| explanation | URL | No | |
| label | String | No | |
| order | Number | No | |
| group | [Assertion Group](#assertion-group) | No | |

### Assertion Group

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | String | Yes | |
| label | String | No | |

### License

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| content-version | String | Yes | Either `vor` (version of record,) `am` (accepted manuscript,) `tdm` (text and data mining) or `unspecified` |
| delay-in-days | Number | Yes | Number of days between the publication date of the work and the start date of this license |
| start | [Partial Date](#partial-date) | Yes | Date on which this license begins to take effect |
| URL | URL | Yes | Link to a web page describing this license |

### Resource Link

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| intended-application | String | Yes | Either `text-mining`, `similarity-checking` or `unspecified` |
| content-version | String | Yes | Either `vor` (version of record,) `am` (accepted manuscript) or `unspecified` |
| URL | URL | Yes | Direct link to a full-text download location |
| content-type | String | No | Content type (or MIME type) of the full-text object |

### Reference

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| key | String | Yes | |
| DOI | String | No | |
| doi-asserted-by | String | No | One of `crossref` or `publisher` |
| issue | String | No | |
| first-page | String | No | |
| volume | String | No | |
| edition | String | No | |
| component | String | No | |
| standard-designator | String | No | |
| standards-body | String | No | |
| author | String | No | |
| year | String | No | |
| unstructured | String | No | |
| journal-title | String | No | |
| article-title | String | No | |
| series-title | String | No | |
| volume-title | String | No | |
| ISSN | String | No | |
| issn-type | String | No | One of `pissn` or `eissn` |
| ISBN | String | No | |
| isbn-type | String | No | |

### ISSN with Type

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| value | String | Yes | |
| type | String | Yes | One of `eissn`, `pissn` or `lissn` |

### Content Domain

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| domain | Array of String | Yes | |
| crossmark-restriction | Boolean | Yes | |

### Relations

A hashmap containing relation name, [Relation](#relation) pairs.

### Relation

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id-type | String | Yes | |
| id | String | Yes | |
| asserted-by | String | Yes | One of `subject` or `object` |


### Review

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| running-number | String | No | |
| revision-round | String | No | |
| stage | String | No | One of `pre-publication` or `post-publication` |
| recommendation | String | No | One of `major-revision` or `minor-revision` or `reject` or `reject-with-resubmit` or `accept` |
| type | String | No | One of `referee-report` or `editor-report` or `author-comment` or `community-comment` or `aggregate` |
| competing-interest-statement | String | No | |
| language | String | No | |
