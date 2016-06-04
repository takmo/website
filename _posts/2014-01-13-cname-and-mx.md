---
layout: post
title: "TIL: Never Mix CNAME and MX Records"
---

...Or CNAME and any records for that matter.

Today I learned that my (unused) email system has
been disfunctional for the last few weeks  because of
a DNS issue.

Here was an excerpt from my DNS records, prior to today:

    bitwisehero.com.	15406	IN	CNAME	fang.bitwisehero.com.
    bitwisehero.com.	14400	IN	MX	10 mail.bitwisehero.com.

Looks simple enough, right? The root domain entry redirects
to my webserver, fang.bitwisehero.com, and all mail goes to
my mail server, mail.bitwisehero.com. At first glance, it
looks fine.

Unfortunately, the setup was bugged. Outoing mail worked fine,
but not a single nameserver would return the correct MX record.

Evidently, I had neglected an
[important rule (3.6.2)](http://tools.ietf.org/html/rfc1034)
in the domain name RFC:

> If a CNAME RR is present at a node, no other data should be
> present

As soon as a remote SMTP server did a lookup on bitwisehero.com,
it would immediately hit the CNAME and ignore the MX record
completely. Put simply, for the last few weeks, any incoming
email has been going straight to my email-ignorant web server.

Simple solution: replace the CNAME with a standard A record.
Maybe not the most elegant solution, but this allows nameservers
to display the correct MX record when queried.

This might be common knowledge for most people, but I spent a
good hour hunting down the problem. Just thought I might
share my findings in case anyone else ever has this problem.

Moral of the story: never use CNAME alongside any other records.
