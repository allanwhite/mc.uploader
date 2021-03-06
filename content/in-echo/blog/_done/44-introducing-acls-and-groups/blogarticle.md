---
Title: Introducing ACLs and Groups
SeoTitle: Introducing ACLs and Groups
Author: ben
Date: 06/19/2014
Summary: 
Lead: |
  Building mobile and web applications in Health Care IT presents many risks and challenges to developers due to the need protect sensitive user information. Protected Health Information (PHI) must be protected or developers will expose themselves to potential litigation and fines. In the US, HIPAA defines the penalties for developers who's apps leak PHI. Other countries have similar rules and regulations, for example PIPEDA in Canada and DPA in the UK.

Tags: phi, security
Category: company
Fullname: Ben Uphoff, PhD
---
Catalyze HIPAA Compliant [Mobile Backend as a Service](/baas) (Baas) helps mitigate risks to developers by providing a HIPAA-compliant datastore and infrastructure. Catalyze's compliant API handles critical details like managing secure servers, providing end-to-end encryption, encrypting data at rest, logging, disaster recovery and, of course, controlling access to users' PHI.

Up until now, the permission model has been very simple: users are the only ones that can access their data. This **default deny** policy is important for protecting PHI, but limits the types of apps that can be built on the platform. With our latest release we have enabled Access Control Lists (ACLs) and Groups to provide developers with a more flexible permissions model that is easy to work with and still provides a great deal of power in enabling various data access scenarios.

The key features are:

* Group management: create groups, add users, remove users
* The ability to assign *Create*, *Read*, *Update*, *Delete* (CRUD) permissions to a user or groups on a custom class or the applications file store
* Creating a custom class or file entry on behalf of another user

### Documentation
This short post cannot explain all the details of the new features. We have recently published the [Catalyze Baas Guide](https://docs.catalyze.io/guides/api/latest/) to help getting started with Catalyze's mobile backend. While some sections are works in progress, the [Permissions and ACLs](https://docs.catalyze.io/guides/api/latest/permissions_and_acls/README.html) section fully covers the features discussed in this post and more. See the [Introduction](https://docs.catalyze.io/guides/api/latest/) and [Getting Started](https://docs.catalyze.io/guides/api/latest/getting_started/README.html) sections to get up to speed and set up your environment to test out the ACL features covered in the [permissions section](https://docs.catalyze.io/guides/api/latest/permissions_and_acls/README.html).

Also be sure to refer to the ACLs and Groups section of the [API reference documentation](https://docs.catalyze.io/api/latest/) for additional details. Refer to the Dashboard's [Resources](https://dashboard.catalyze.io/resources) page for additional documentation.

### Upcoming features
While this new set of features provides a great deal of power for developers, we will continue to roll out new capabilities in the coming months.

Here is a list of features on our roadmap:

* Per-entry (data item) ACLS
    * The current release allows for model-level ACLs. For example, update permission can be granted to a user on an entire custom class but not an individual entry within the class.
    * The next release will allow ACLs to be applied to individual data items (entries).
* Application-level ACLs
    * Permissions can be set at the application level.
* A new permission, *Grant*, to allow the administrator to assign another user Grant permission at the application or model level. 
* A *Revoke* permission to compliment Grant will be built as well
* Support for ACLs in clincal data models
    * We will be extending ACLs to be application for clincial data models like Problems, Procedures and Medications.

If you have any questions or suggestions, please [email](mailto:support@catalyze.io) us.

