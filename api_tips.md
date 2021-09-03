## Tips for using the Crossref REST API (aka- how to avoid being blocked)

The [REST API documentation](https://api.crossref.org) is useful, but it doesn't tell you much about best practice. It also doesn't tell you how you can best avoid getting blocked. With this document we hope to remedy that.

Please read this entire document carefully- it includes some advice that may seem counter-intuitive.

It also includes some advice that might be obvious to professional programmers, but that might not be obvious to researchers or others who are just starting out with scripting or interactive research notebooks (e.g. Jupyter).

Instability in our APIs is almost always tracked down to a particular user who has created a script that either:

- performs needlessly complex and inefficient requests
- performs requests that repeatedly trigger server errors (sometimes related to above)
- polls the API too quickly (we rate limit at the IP level, but some users run distributed systems coming in on multiple IPs)
- performs redundant requests

These are not malicious actions. And they are easily correctable. And, in almost all cases, when we advise users that they need to fix their process, they do. And we are never concerned with them again. But, we can’t help everyone with every use case.

Our advice is split into three sections:

- Pick the right service.
- Understand the performance characteristics of the REST API.
- Optimise your requests and pay attention to errors.

### Consider not using the API

What? No, seriously- Crossref periodically releases [public data file](https://www.crossref.org/blog/new-public-data-file-120-million-metadata-records/) of all the metdata that is available via our public API. You can download this file from [Academic Torrents](https://academictorrents.com/) with your favorite torrent client and do a lot of work locally if you want to. At the very least, having the local file will allow you to focus on using the API only for metadata records that have been updated since the last public data file was released.


### Pick the right service level.



Consider using our “[Polite](https://github.com/CrossRef/rest-api-doc#good-manners--more-reliable-service)” or “[Plus](https://www.crossref.org/services/metadata-retrieval/metadata-plus/)” versions of the REST API. 

What does this mean?

There are three ways to access the REST API. In increasing levels of reliability and predictability- they are:

- Anonymously (aka Public)
- With self identification (aka Polite)
- With authentication. (aka Plus)

Why three ways? Because Crossref is committed to providing free, open and as-anonymous-as-possible access to scholarly metadata. We are committed to this because research can often involve controversial subjects. And what is considered “controversial” can vary widely across time and jurisdictions. It is extremely difficult to provide truly “anonymous” access to an internet service. We will always, for example, be able to tie a request to an IP address and we keep this IP information for 90 days. The best we can do is make sure that some people who have the need for extra precautions can access the API without needing to authenticate or explicitly identify themselves. 

But this semi-anonymous access is also hard for us to manage. Because the “Public” version of the REST API is free, traffic patterns can vary wildly. Because the service is semi-anonymous- it makes it very hard for us to contact people who are causing problems on the system.
 
So we offer a compromise as well. If you do not have a requirement for anonymity, you can also self-identify by including contact information in your requests. The service is still open and free, but  this way we can quickly get in touch with you if your scripts are causing problems. And in turn for providing this contact information, we redirect these requests to a specific “Polite” pool of servers. These servers are generally more reliable because we are more easily able to protect them from misbehaving scripts.

Note that, in asking you to self-identify, we are not asking you to completely give up privacy. We do not  sell (or give) this contact information to anybody else and we only use it to contact users who are causing problems. Also- any contact information that you provide in your requests will only stay in our logs for 90 days.

And finally, if you are using our REST API for a production service that requires high predictability- *you should really consider using our paid-for “Plus” service.* This service gets you an authentication token which, in turn, directs your request as a reserved pool of servers that are extremely predictable.

### Understand the performance characteristics of REST API queries.

If you are using the API for simple reference matching, and are not doing any post validation (e.g. your own ranking of the returned results), then just ask for the first two results (`rows=2`). This allows you to identify the best result and ignore any where there is a tie in score on the first two results (e.g. an inconclusive match). If you *are* analyzing and ranking the results yourself, then you can probably get away with just requesting five results (`rows=5`). Anything beyond that is very unlikely to be a match. In either case- restricting the number of rows returned will be more efficient for you and for the API.

<hr/>

For matching references (either complete or partial), use the `query.bibliographic` parameter and minimise the number of other parameters, filters and facets. Most additional parameters, filters and facets will make the query slower *and* less accurate. You might be surprised at this advice as it seems counterintuitive- but we assure you the advice is backed up by many millions of tests.

Specifically, do not construct your queries like this:
```
http://api.crossref.org/works?query.author="Josiah Carberry"&filter=from-pub-date:2008-08-13,until-pub-date:2008-08-13&query.container-title="Journal of Psychoceramics"&query="Toward a Unified Theory of High-Energy Metaphysics"&order=score&sort=desc
```
The above is a massively expensive and slow query. If it doesn’t time-out, you are likely to get a false negative anyway.

And also don’t do this:
```
http://api.crossref.org/works?query="Toward a Unified Theory of High-Energy Metaphysics, Josiah Carberry 2008-08-13"
```
Using the plain `query` parameter will search the entire record- including funder and other non bibliographic elements. This means that it will also match any record that includes the query text in these other elements- resulting in many, many false positives and distorted scores.

If you are trying to match references- the simplest approach is the best. Just use the `query.bibliographic` parameter. It restricts the matching to the bibliographic metadata and the default sort order and scoring mechanism will reliably list the best match first. Restricting the number of rows to `2` allows you to check to see if there is an ambiguous match (e.g. a “tie” in the scores of the first two items returned” (see above tip). So the best way to do the above queries is like this:

```
http://api.crossref.org/works?query.bibliographic="Toward a Unified Theory of High-Energy Metaphysics, Josiah Carberry 2008-08-13"&rows=2
```
### Don't use `rows`/`offsets` in the `/works` route . They are very expensive and slow. Use cursors instead.

Note much to say. We implimented rows/offsets early in the development of the API and regretted it emmediately. So we implimented cursors instead and kept rows/offsets so as to not break existing scripts. But NOBODY should use them.

### Optimise your requests and pay attention to errors.

If you have an overall error (`4XX` + `5XX`) rate >= 10%,  seriously- please *stop* your script and figure out what is going on. Don’t just leave it hammering the API and generating errors- you will just be making other users (and Crossref staff) miserable until you fix your script.

<hr/>

If you get a `404` (not found) when looking up a DOI, do not just endlessly poll Crossref to see if it ever resolves correctly. First check to make sure the DOI is a Crossref DOI. If it is not a Crossref DOI, you can stop checking it with us and try checking it with another registration agency’s API. You can check the registration agency to which a DOI belongs as follows:

```
https://api.crossref.org/works/{doi}/agency
``` 

<hr/>

Adhere to rate limits. We rate limit by IP- so *yes*, you can “get around” the rate limit by running your scripts on multiple machines with different IPs- but then all you are doing is being inconsiderate of other users. And that makes us grumpy. You won’t like us when we are grumpy. There can be other good reasons to run your scripts on multiple machines with different IPs- but if you do, please continue to respect the overall-rate limit by restricting each process to working at an appropriate sub-rate of the overall rate limit. 

<hr/>

Check your errors and respond to them. If you get an error - particularly a timeout error, a rate limit error (`429`), or a server error (`5XX`)- do not just repeat the request or immediately move onto the next request, back-off your request rate. Ideally, back-off exponentially. There are lots of libraries that make this very easy. Since a lot of our API users seem to use Python, here are links to a few libraries that allow you to do this properly:

- [Backoff](https://pypi.org/project/backoff/)
- [Retry](https://pypi.org/project/retry/)

But there are similar libraries for Java, Javascript, R, Ruby, PHP, Clojure, Golang, Rust, etc.
<hr/> 
Make sure you URL-encode DOIs. DOIs can contain lots of characters that need to be escaped properly. We see lots of errors that are simply the result of people not taking care to properly encode their requests. Don’t be one of those people.
<hr/>
Cache the results of your requests. We know a lot of our users are extracting DOIs from references or other sources and then looking up their metadata. This means that, often, they will end up looking up metadata for the same DOI multiple times. We recommend that, at a minimum, you cache the results of your requests so that subsequent requests for the same resource don’t hit the API directly. Again, there are some very easy ways to do this using standard libraries. In Python, for example, the following libraries allow you to easily add caching to any function with just a single line of code:

- [Requests-cache](https://pypi.org/project/requests-cache/)
- [Diskcache](https://pypi.org/project/diskcache/)
- [Cachew](https://github.com/karlicoss/cachew#what-is-cachew) 

There are similar libraries for other languages.

<hr/>
If you are using the Plus API, make sure that you are making intelligent use of the snapshots. Only use the API for requesting content that has changed since the start of the month- and use the metadata already in the snapshot for everything else.

<hr/>

Managing the snapshot can be cumbersome as it is inconveniently large-ish. Remember that you do *not have to uncompress and unarchive the snapshot in order to use it.* Most major programming languages have libraries that allow you to open and read files directly from a compressed archive. For example:

- [tarfile](https://docs.python.org/3/library/tarfile.html)

If you parallelize the process of reading data from the snapshot and loading it into your database, you should be able to scale the process linearly with the number of cores you are able to take advantage of.
