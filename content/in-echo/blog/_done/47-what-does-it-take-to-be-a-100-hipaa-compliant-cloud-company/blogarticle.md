---
Title: What does it take to be a 100% HIPAA compliant cloud company?
SeoTitle: What does it take to be a 100% HIPAA compliant cloud company?
Author: travis
Date: 06/25/2014
Summary: 
Lead: |
  The short answer to that question is, not surprisingly, **it take A LOT of time, energy, and money to be 100% HIPAA compliant**. At Catalyze we're focused on security and HIPAA compliance. It's a differentiator and we aren't reaching when we say *[It's in our DNA](https://catalyze.io/compliance/)*, referring to HIPAA compliance. Because of that focus, we have a lot of resources for compliance. That translates to a lot of time and money on security and organizational policies and procedures specifically to comply with HIPAA.

Tags: hipaa, audit
Category: company
Fullname: Travis Good, MD
---
We recently completed our 3rd external audit in 12 months. The audit was performed by Coalfire Systems, a large national 3rd party auditor. The results of that audit are a very lengthy report, about 20 pages, but the entire report can be boiled down to the image below (we can't share the entire report here because there are things in it that would be considered internal and sensitive to Catalyze, like IP addresses, specific controls, etc).

(image: compliance.png)

We're very proud of that image and the overall results! It's a pretty materially-compliant green. The truth is it took a lot of time and effort to get here, and we learned way more about HIPAA than we every expected. We did not outsource any of our compliance work because it is core to what we do. All of our policies and procedures are written, reviewed, and maintained in house, just like our code; policies are reviewed, using a similar pull request process that we use for our code review, as well as by external auditors, but our policies are owned wholly by Catalyze. Below are some high level things we've learned.

**Information security**. This, and policies below, are the most obvious parts of HIPAA. Fortunately for us, our fearless CTO and Chief Security Officer is an info sec expert, having worked with highly classified data for the DoD, built intrusion detection systems, and been mentioned in several information security texts. Our technology is built from the ground up to be secure and expose data only when access is specifically granted, never by default. Starting with a minimum access standard is hard when you are trying to build a flexible platform and API for developers, but we think we've accomplished combining simplicity, security, and flexibility, including our new [ACL features](https://docs.catalyze.io/guides/api/latest/permissions_and_acls/README.html).

There are countless innovative and powerful things we do to maintain the integrity, privacy, and security of data on Catalyze and we need to do a better job of sharing those with our customers and the cloud community as a whole. In addition to insight into how we secure our networks, systems, and data, we also intend to start open sourcing libraries we've written so stay tuned.

**Policies**. We have a lot of policies, and I don't think we fully expected to spend so much time developing and refining them. The last time I checked we had about 26, 000 words worth of policies, including our BAA. You can look at our policies [here](https://catalyze.io/policy/).

All of our policies are written in Markdown and we use Github for version control. We will be open sourcing our policies very soon, and hope that others can use them to simplify their lives. [Accountable](http://accountablehq.com/) is good source of policies and overall management of the administrative side of HIPAA.

**Procedures**. Policies are the easy part compared to procedures. Policies dictate the procedures that are required to adhere to those policies. Procedures are the processes that define specifically how we operate at Catalyze. Procedures can make for tedious work, especially when it comes to HIPAA compliance where documentation is essential at pretty much all everything. We did not anticipate how many procedures we'd need to put into place but we've been creative in their application and it's let us remain agile within a highly regulated industry.

**Forms and tracking.** Tightly tied to procedures above are the number of forms and the amount of tracking that we do. As an example, when you create a policy for [system access](https://catalyze.io/policy/#system-access-policy), you need a way to track requests and access grants to various systems and data. This is one of many examples, including changes to our infrastructure, firewall rules, etc. We lean heavily on Google Forms and Spreadsheets for this, and find them to be very effective. When we were going through our HIPAA audits and our HITRUST audit, this was a very easy way for us to show evidence of following policies and procedures.

**Training**. Part of the HIPAA requirements are that all workforce members (employees and contractors) have training on HIPAA. We looked at the available training options and felt that the majority were created for covered entities, people that handle paper records, and people that work with patients. We decided those had zero value to our employees so we wrote our own [HIPAA introductory training](https://training.catalyze.io/). All new employees at Catalyze complete this training. We also do weekly demonstrations that cover topics related to security, privacy, and health industry trends for all Catalyze team members.

**New technologies**. At Catalyze we don't have ready access to any ePHI, but we do store sensitive information and support our customers with their applications. As such, we made the decision early on to either own and manage systems that store sensitive information or work with companies that will sign BAAs with us. We utilize our own Gitlab server and have BAAs in place with both Box and Google Apps.

We took all of these steps, dedication all of these resources, and spent all of these resources because operating as a business associate, with all organization and admin requirements met, sets us apart from everybody else in the compliant infrastructure business. We don't just sign business associate agreements (BAAs) and secure our infrastructure, we comply with HIPAA to the full extent of the rules. It gives us additional insight into our customers' needs and keep us intimately close to the problem we are solving. It is also extremely valuable to our customers when they work with compliance offices and point to our audit reports and white papers.

When you decide to go down the path of HIPAA compliant hosting and APIs, we encourage you to spend time digging deeper than sales and into how the offerings and organizations available to you differ in approaches to HIPAA, and which are really solving your problem vs selling you a checkbox for a BAA. If you have a question about about compliance at Catalyze, please [email me](mailto:travis@catalyze.io) and I'll be happy to tell you everything we've learned and why we're so proactive about compliance and transparency.

