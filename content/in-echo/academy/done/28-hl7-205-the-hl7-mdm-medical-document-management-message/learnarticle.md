---
Title: "HL7 205 - The HL7 MDM (Medical Document Management) Message"

Author: rick

Date: 05/11/2016

Tags: HL7

---
The Medical Document Management (MDM) message is a commonly used HL7 message type that provides information about new or updated notes or documents. Per the HL7 standard, it's main purpose is "to produce an accurate, legal, and legible document that serves as a comprehensive account of healthcare services provided to a patient".

There are several MDM message trigger events and they can be split into two categores - those that describe the status of the document and those that both describe the document and also contain the document itself:

| MDM Subtype | Trigger Event/Description |
|-------------|---------------------------|
| T01 | Original Document Notification |
| T02 | Original Document Notification with Content |
| T03 | Document Status Change Notification |
| T04 | Document Status Change Notification with Content |
| T05 | Document Addendum Notification |
| T06 | Document Addendum Notification with Content |
| T07 | Document Edit Notification |
| T08 | Document Edit Notification with Content |
| T09 | Document Replacement Notification |
| T10 | Document Replacement Notification with Content | 
| T11 | Document Cancel Notification |


Following the standard HL7 message structure, an MDM message is made up of segments and groups of segments, each of which may be required, optional, repeatable, or some combination thereof. A quick note on reading HL7 message examples which seem to contain a bunch of [],{} etc. The general rule is as follows:
- No brackets around it - **Required**
- [] - **Optional**
- { } - **Repeating**
- [{ }] - **Optional Repeating**

The MDM message structure is as follows.
```
MDM     Medical Document Management Message
MSH     Message Header
PID     Patient Identification
  [{NTE}]  Notes and Comments
PV1   Patient Visit
{[
  ORC    Common Order
  OBR Observation Request
    [NTE]  Notes and Comments
]}
TXA Document Notification
  [{NTE}] Notes and Comments
{
  OBX* Observation notes
    [{NTE}] Notes and Comments
}
```

Some notes on the above triggers and message structure:

- The most common of the MDM subtypes is the T02 because it notifies the receiving system that a new document has been created and also includes the document content itself. One typical workflow will have a vendor write notes back into the EMR via an MDM^T02 so that the notes are loaded into a patient's chart and available for physician review.

- The ORC/OBR segments are optional and are mostly only used for workflows like order-based transcriptions, where an order can be created or updated by the MDM message.

- Note the asterisk (*) next to the OBX segment. That's there to indicate that MDM messages will only include repeating OBX segments if they include the document content itself. The content is stored in the OBX segment(s) in a format dictated by the receiving system. Some possible formats include: plain text, formatted text (ie. RTF), or base64-encoded documents like PDFs. The document can be split across multiple OBX segments or stored in one single OBX dependent on the preference of the receiving system or the character limits of each segment. 

Now let's break down an example MDM HL7 message by each segment. The message we want to send is: a note containing a consulting physician's (James A. Matthews) clinical diagnosis has been created and should be attached to a patient's chart. The patient is Mike J. Thomas: male, African American, MRN:12983718, SSN:123-45-9999, born on January 1, 1970, account number: 18273652.

Based on this information, the required segments would be created as follows:

**MSH - Message Header**
Let's assume an all Epic environment, i.e. the sending and receiving systems are both Epic, and that the note is created on 2016/05/10 at 07:16:33. Also, the version of HL7 being used is v2.3. This would result in a segment that looks like this.

```
MSH|^~\&|Epic|Epic|||20160510071633||MDM^T02|12345|D|2.3
```

**PID - Patient Identification**
Here we identify which patient this note is about. We include the patient's identifier, full name, DOB, sex, race, address, phone number(s), account number, SSN and place of birth. Some of these are optional as described in the format specification:

```
PID|1||12983718^^^^EPI||Thomas^Mike^J^^MR.^||19500101|M||AfrAm|506 S. HAMILTON AVE^^MADISON^WI^53505^US^^^DN |DN|(608)123-9998|(608)123-5679||S||18273652|123-45-9999||||^^^WI^^
```

**PV1 - Patient Visit**
This usually contains information on the admission information, attending, referring and consulting, admitting physician IDs and names and so on. We'll just keep it simple and just have the consulting physician's info:

```
PV1|||^^^CARE HEALTH SYSTEMS^^^^^||||||1173^MATTHEWS^JAMES^A^^^||||||||||||
```

**TXA - Document Notification**
The TXA segment contains information about the transcribed document including the report type, date/time, provider info, a unique document number assigned by the sending system (ex: 12345), and a document status (ex: PA = Pre-Authenticated):

```
TXA||CN||20160510071633|1173^MATTHEWS^JAMES^A^^^|||||||^^12345|||||PA|
```

**OBX - Observation**
OBX segment(s) contain the document content itself. Here, we'll split out document contents into multiple OBX segments for clarity though this isn't a requirement. The receiving system should dictate their preference. In this example, TX indicates the content will be in plain text:

```
OBX|1|TX|||Clinical summary: Based on the information provided, the patient likely has viral sinusitis  commonly called a head cold.
OBX|2|TX|||Diagnosis: Viral Sinusitis
OBX|3|TX|||Diagnosis ICD: J01.90
OBX|4|TX|||Prescription: benzonatate (Tessalon Perles) 100mg oral tablet 30 tablets, 5 days supply. Take one to two tablets by mouth three times a day as needed. disp. 30. Refills: 0, Refill as needed: no, Allow substitutions: yes
```

Now, putting them all together we get the full HL7 MDM message.

```
MSH|^~\&|Epic|Epic|||20160510071633||MDM^T02|12345|D|2.3
PID|1||12983718^^^^EPI||Thomas^Mike^J^^MR.^||19500101|M||AfrAm|506 S. HAMILTON AVE^^MADISON^WI^53505^US^^^DN |DN|(608)123-9998|(608)123-5679||S||18273652|123-45-9999||||^^^WI^^
PV1|||^^^CARE HEALTH SYSTEMS^^^^^||||||1173^MATTHEWS^JAMES^A^^^||||||||||||
TXA||CN||20160510071633|1173^MATTHEWS^JAMES^A^^^|||||||^^12345|||||PA|
OBX|1|TX|||Clinical summary: Based on the information provided, the patient likely has viral sinusitis  commonly called a head cold.
OBX|2|TX|||Diagnosis: Viral Sinusitis
OBX|3|TX|||Diagnosis ICD: J01.90
OBX|4|TX|||Prescription: benzonatate (Tessalon Perles) 100mg oral tablet 30 tablets, 5 days supply. Take one to two tablets by mouth three times a day as needed. disp. 30. Refills: 0, Refill as needed: no, Allow substitutions: yes
```

That's it! If it looks complicated, don't worry - Catalyze is here to help.

If you're looking to integrate EHR data with your application without becoming an HL7 expert, Catalyze can help. Learn more about Redpoint, our hosted, HIPAA-compliant HL7 integration platform [here](https://catalyze.io/redpoint).

And don't forget to review the rest of our HL7 articles [here](https://catalyze.io/learn) to learn more about other available HL7 message types and workflows.
