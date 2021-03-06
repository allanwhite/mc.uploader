---
Title: POODLE Impact
SeoTitle: POODLE Impact
Author: catalyze
Date: 10/15/2014
Summary: Another OpenSSL vulnerablility is affecting servers running SSL v3.0 - but it's impact is low.
Lead: |
  Another OpenSSL vulnerablility is affecting servers running SSL v3.0. This vulnerability, nicknamed **POODLE**, does not impact any Catalyze servers hosting or transmitting customer data. In fact, we disable SSLv3 (and SSLv2 too!) in all of our server configurations where PHI and customer data is stored. The only configurations we ended up changing were on nginx and apache2 servers that served up static site content (sorry IE6 users!). Our goal is to always err on the side of caution for these types of vulnerabilities.

Tags: poodle, security
Category: company
Fullname: Ben Uphoff, PhD
---
To read up on the details of the vulnerability please see the original [Google disclosure](http://googleonlinesecurity.blogspot.com/2014/10/this-poodle-bites-exploiting-ssl-30.html).

