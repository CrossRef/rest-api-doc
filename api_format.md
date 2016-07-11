# Crossref Metadata API JSON Format

## Versioning

| Version | Release Date | Comments |
|---------|--------------|----------|
| v1 | 11th July 2016 | First documented version |

## Work

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| publisher | String | Yes | Name of work's publisher |
| title | Array of String | Yes | Work titles, including original language title and translated titles |
| reference-count | Number | Yes | Count of outbound references deposited with Crossref |
| source | String | Yes | Currently always `CrossRef` |
| prefix | URL | Yes | DOI prefix identifier of the form `http://id.crossref.org/prefix/DOI_PREFIX` |
| DOI | String | Yes | DOI of the work |
| member | URL | Yes | Member identifier of the form `http://id.crossref.org/member/MEMBER_ID` |
| type | String | Yes | Enumeration, one of the type ids from `https://api.crossref.org/v1/types` |
| created | [Date](#date) | Yes | Date on which the DOI was first registered |
| deposited | [Date](#date) | Yes | Date on which the work metadata was most recently updated |
| indexed | [Date](#date) | Yes | Date on which the work metadata was most recently indexed. Re-indexing does not imply a metadata change, see `deposited` for the most recent metadata change date |
| issued | Date | Yes | Eariest of `published-print` and `published-online` |
| subtitle | Array of String | No | Work subtitles, including original language and translated |
| container-title | Array of String | No | Titles (full and abbreviated) of the containing work (usually a book or journal.) |
| issue | String | No | Issue number of an article's journal |
| volume | String | No | Volume number of an article's journal |
| page | String | No | Pages numbers of an article within its journal |
| article-number | String | No | |
| published-print | [Date](#date) | No | Date on which the work was published in print |
| published-online | [Date](#date) | No | Date on which the work was published online |
| subject | Array of String | No | Subject category names, a controlled vocabulary from Sci-Val. Available for most journal articles |
| ISSN | Array of String | No | |
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
| alternative-id | String | No | Other identifiers for the work provided by the depositing member |

## Work Nested Types

### Funder

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | String | Yes | Funding body primary name |
| DOI | String | No | Optional [Open Funder Registry](http://www.crossref.org/fundingdata/registry.html) DOI uniquely identifing the funding body |
| award | Array of String | No | Award number(s) for awards given by the funding body |
| doi-asserted-by | String | No | Either `crossref` or `publisher` |

### Clinical Trial Number

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| clinical-trial-number | String | Yes | |
| registry | String | Yes | |
| type | String | No | |

### Contributor

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| family-name | String | Yes | |
| given-name | String | No | |
| ORCID | URL | No | URL-form of an [ORCID](http://orcid.org) identifier |
| affiliation | Array of [Affiliation](#affiliation) | No | |

### Affiliation

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | String | Yes | |

### Date

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| date-parts | Array of Number | Yes | Ordered array of `year`, `month`, `day of month`. Only `year` is required |
| timestamp | Number | No | Seconds since UNIX epoch |
| date-time | String | No | ISO 8601 date time |

### Update

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| updated | Date | Yes | Date on which the update was published |
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
| start | Date | Yes | Date on which this license begins to take effect |
| URL | URL | Yes | Link to a web page describing this license |

### Resource Link

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| intended-application | String | Yes | Either `text-mining` or `unspecified` |
| content-version | String | Yes | Either `vor` (version of record,) `am` (accepted manuscript) or `unspecified` |
| URL | URL | Yes | Direct link to a full-text download location |
| content-type | String | No | Content type (or MIME type) of the full-text object |
