---
Title: HipaaGram - A HIPAA Compliant Messaging App
SeoTitle: HipaaGram - A HIPAA Compliant Messaging App
Author: josh
Date: 07/22/2014
Summary: 
Lead: |
  ##Initial Planning

Tags: app, iOS, api, sms, messaging
Category: company
Fullname: Josh Ault
---
We recently had an internal hackathon here at Catalyze. We were trying to accomplish things we didn't think were possible on our HIPAA compliant APIs as well as try to break everything that we could. And of course, we succeeded in doing both. But don't worry, everything we broke is now fixed. But it's what we tried to accomplish that we didn't think was possible that turned out to be the most interesting. One of our projects involved creating a fully HIPAA compliant messaging app.

Messaging apps are big in healthcare, and there's tons of interest in these types of apps to replace the current tools in place - mainly SMS/iMessage, MMS, and email - none of which are HIPAA compliant. There are a lot of companies that have launched messaging solutions for healthcare, and there are many organizations piloting or fulling replacing pagers with modern messaging solutions. It's been one of the big use cases for mHealth. We thought it was an interesting use case to showcase what can be done with our APIs.

###A Few Bad Ideas

We had tons of ideas floating around about how we might do this. We could create a custom class to hold all the messages and just record who they're from and who they were sent to. We just had to hope that our users wouldn't try and use their account for harm because any data in that custom class would be free game to all users. I know I wouldn't like my messages being read by someone that shouldn't have access to them, so that idea was out the window. We then thought maybe we could create a new custom class for each conversation between two individuals but the same issue arose. Anyone could access any data within any class as long as they knew the name. We also have usage limits in place that would make this a short lived application, but internally we removed those for the time being.

###Our Best Bad Idea

Finally, we decided that we would create a new Application model for each conversation, sign up the two users for the application with the same username/password as the main app and then everything would just work. There were still a few holes in this plan like anyone being able to sign up for that application as long as they got the API Key and app ID. But our biggest concern was that this would require a middle man. Who is going to manage the username/passwords for these users and automatically sign them up for different apps, and remember all of their sessions for each of those apps? So we ended up writing a python server to do this for us. This wasn't HIPAA compliant and ultimately wouldn't work in a production environment. We also wanted as little supervisor intervention as possible with the application to make it completely automated. But we learned what our API is lacking here at Catalyze and we knew what needed to be done to fix it.

##New Features

To make HipaaGram a fully HIPAA compliant messaging app that did not require a middle man we had to add a few features to our API. Most notably, ACLs. As part of our [API Guide](https://docs.catalyze.io/guides/api/latest/permissions_and_acls/README.html), ACLs are discussed in chapter 7. Now you can grant permissions to a given user for certain data. This can be done at an entry level in a custom class, or even for an entire Application. ACLs are a powerful feature we've been asked for repeatedly, and something that is required to assure only authorized individuals and groups can access protected health information (PHI).

##A Complete Rewrite

After we rolled out a whole slew of new features around ACLs we needed to prove that they worked. What better way than to rewrite the app that these features were made for? HipaaGram was rewritten to take advantage of ACLs. We started with the standard procedure, create an org, an app, and an API Key. Then we needed 3 different custom classes. The first being `contacts`. Whenever a user signed up for HipaaGram, they would add themselves to the `contacts` class with the `usersId` and `username`. The default permission levels for this class are `create` and `retrieve`. Any user can create data in this class and any user can read any data that is in this class. The schema for `contacts` looks like this

<script src="https://gist.github.com/jault3/137653d03fe413bb565f.js"></script>

The second class was `conversations`. This class included `sender`, `recipient`, and the `sender_id` and `recipient_id`. Essentially the usernames and usersId of each participant in the conversation. A different permission level of `create` is applied to this class. This means anyone can create data in this class and you can retrieve any data you created (or data that was created for you). This way, if you wanted to start a conversation with another user you would first lookup their details in the `contacts` class. Then create an entry in the `conversations` class for that other user. They can read that entry because they own the data and you can read that entry because you are the author. A simple query with no parameters on that `conversations` class will return all entries that you are allowed to see. The `conversations` schema is

<script src="https://gist.github.com/jault3/2390d41171dcc52ea1ee.js"></script>

The last class holds all the messages for all users titled `messages`. Much like `conversations`, when you send a message you create the entry for the other user so you both can read it. After this was done, we had completed our HIPAA compliant messaging app. You only have access to data that you should have access to and it can be built entirely on the Catalyze HIPAA compliant API. `messages` schema is a bit larger

<script src="https://gist.github.com/jault3/6c0f61e91fb7aa02421f.js"></script>

##Screenshots

|1|2|3|
|-|-|-|
|(image: hipaagram1.png)|(image: hipaagram2.png)|(image: hipaagram3.png)|

You can check out the source code for HipaaGram on github [here](https://github.com/catalyzeio/HipaaGram). Be sure to put in your own API Key and app ID. To get started, [signup](https://dashboard.catalyze.io/signup) for a free Catalyze trial account or check out our [documentation](https://dashboard.catalyze.io/resources). Feel free to [email us](mailto:hello@catalyze.io) if you have any questions or feedback.

