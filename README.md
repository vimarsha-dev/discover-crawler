# discoverbot

`discoverbot` is a crawler that reads **public job postings** from employers' own hiring systems: the public,
unauthenticated job-board endpoints that applicant-tracking-system vendors publish for their customers' careers pages.

**User-Agent:** `discoverbot/0.1 (+https://github.com/vimarsha-dev/discover-crawler)`
The robots.txt product token is `discoverbot`.

## Contact
Open an issue in this repository: https://github.com/vimarsha-dev/discover-crawler/issues.
We read every issue. To stop the crawler immediately, use robots.txt (below); it takes effect on our next read of
your robots file.

## What it reads
- Public job-board endpoints of applicant-tracking systems (for example Greenhouse, Ashby, Lever, Recruitee,
  SmartRecruiters, Workable, Gem, and Workday career sites), each for the boards their employers publish.
- `robots.txt` files, and, for some employers, their own published job sitemaps (only to find posting identifiers;
  each posting is then read from the employer's hiring system).

It never logs in, never uses accounts, keys or tokens, never submits applications or forms, and never reads anything
that is not publicly served without authentication.

## The rules it keeps
- **robots.txt (RFC 9309)** is honoured on every page-type request and on every redirect hop, before the connection
  is made. A `Crawl-delay` is honoured. A robots file that cannot be retrieved (network error, 5xx, 429) is treated
  as "disallow everything".
- **Documented public APIs** are used as their vendor documents them, within the vendor's published terms and rate
  limits; the citation for each is kept on record.
- **Politeness:** at most 4 requests in flight per registrable domain, with a minimum gap between request starts.
  `429 Too Many Requests` and `Retry-After` are honoured for the whole domain, with no upper cap on the wait.
- **Schedule:** two passes a day, starting at 11:00 and 23:00 UTC, with short follow-up re-reads of boards whose read
  did not complete.
- Every request is logged with its time, its result and the rules in force.

## How to block or limit it
```
User-agent: discoverbot
Disallow: /
```
Narrower `Disallow` rules and `Crawl-delay` are honoured too. If you would rather we stop reading a specific board or
endpoint, open an issue and say which.

## Network
Requests come from a residential connection whose IP address can change; there is no reverse-DNS name. Identify the
crawler by its User-Agent. If you see traffic claiming to be `discoverbot` that breaks these rules, please open an issue.
