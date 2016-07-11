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
| subtitle | Array of String | No | Work subtitles, including original language and translated |
| container-title | Array of String | No |
| reference-count | Number | Yes | Count of outbound references deposited with Crossref |
| issue | String | No | Issue number of an article's journal |
| volume | String | No | Volume number of an article's journal |
| page | String | No | Pages numbers of an article within its journal |
| source | String | Yes | Currently always `crossref` |
| prefix | String | Yes | |
| DOI | String | Yes | |
| member | String | Yes | |
| type | String | Yes | Enumeration, one of the type ids from `https://api.crossref.org/v1/types` |
| created | Date | Yes | |
| deposited | Date | Yes | |
| indexed | Date | Yes | |
| published-print | Date | No | |
| published-online | Date | No | |
| issued | Date | Yes | Eariest of published-print and published-online |
| subject | Array of String | No | |
| ISSN | Array of String | No | |
| ISBN | Array of String | No | |
| archive | Array of String | No | |
| license | Array of License | No | |
| funder | Array of Funder | No | |
| assertion | Array of Assertion | No | |
| author | Array of Contributor | No | |
| editor | Array of Contributor | No | |
| chair | Array of Contributor | No | |
| translator | Array of Contributor | No | |
| update-to | Array of Update | No | |
| update-policy | URL | No | |

## Work Nested Types

### Funder

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| DOI | String | No | |
| name | String | Yes | |
| award | Array of String | No | |
| doi-asserted-by | String | No | |

### Contributor

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| given-name | String | No | |
| family-name | String | Yes | |
| ORCID | String | No | |
| affiliation | Array of Affiliation | No | |

### Affiliation

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| given-name | String | No | |
| family-name | String | Yes | |
| ORCID | String | No | |
| affiliation | Array of Affiliation | No | |

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
| label | String | No | |
| name | String | Yes | |
| order | Number | No | |
| value | String | Yes | |
| group | Assertion Group | No | |

### Assertion Group

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| label | String | No | |
| name | String | Yes | |

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
| content-type | String | No | |
| URL | URL | Yes | |
