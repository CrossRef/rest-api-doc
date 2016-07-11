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
| source | String | Yes | Currently always `crossref` |
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

## Work Nested Types

### Funder

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | String | Yes | |
| DOI | String | No | |
| award | Array of String | No | |
| doi-asserted-by | String | No | |

### Contributor

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| family-name | String | Yes | |
| given-name | String | No | |
| ORCID | String | No | |
| affiliation | Array of [Affiliation](#affiliation) | No | |

### Affiliation

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | String | Yes | |

### Date

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| date-parts | Array of Number | Yes | |
| timestamp | Number | No | |
| date-time | String | No | |

### Update

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| updated | Date | Yes | |
| DOI | String | Yes | |
| type | String | Yes | |
| label | String | No | |

### Assertion

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | String | Yes | |
| value | String | Yes | |
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
| content-version | String | Yes | |
| delay-in-days | Number | Yes | |
| start | Date | Yes | |
| URL | URL | Yes | |

### Resource Link

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| intended-application | String | Yes | |
| content-version | String | Yes | |
| URL | URL | Yes | |
| content-type | String | No | |
