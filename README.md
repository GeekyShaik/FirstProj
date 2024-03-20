Concepts

The Message Publishers will use the Authoring Environment to create message templates that are used to generate messages that get delivered to the member and other recipients. 
What is a Message Template?  A Message Template is simply a pattern (or model) that will be used to generate a message containing content specific to the recipient of the message. But before the message template can be designed with content, you must first determine its type. There are several types of message templates; each type is listed below with a brief description. 

Master Template	A Master Template controls the overall flow of a message; it will contain the references to optional text (called Section Templates) and any Enclosure Templates that are to be included. 

Virtual Master Template	A Virtual Master Template is a metadata only template that controls the overall flow of a packet; COSA application can pass enclosures and form masters to be associated at Run time. The virtual master does not include any content of its own. 

Section Template	A Section Template is a section of content that may or may not be included depending on conditions or rules defined by the Message Publisher. It usually represents at least one paragraph.  A Master Template must include the Section Template into the message.  Multiple Master Templates can include the same Section Template (e.g., a Signature Section).  The text from section templates can flow together forming the content of a letter’s text.

Enclosure Template	An Enclosure Template appears after the body of a letter.  Enclosures are often considered attachments to a letter.  Enclosures start on new pages. 
 
Form Master Template	Form Masters unlike enclosure template can be either stand alone or included along with a virtual master template.  Form Master is similar to Master template but it does not allow enclosures association. [The infrastructure to call Form masters directly in ECT composition environment is yet to be built] 


Special Template	A Special Template is a type of Template having a special purpose which is separate from the letter’s body and attachments.  These include FAX cover letters, CC letters, CC FAX Cover Letter, and the special format of the member’s email address information required by the system for archiving. 
Figure 2-1 shows an example of a generated letter suitable for postal delivery to a recipient.  Like most postal letters, it contains an address section, the body, and a signature section.  Depending on the complexity, it may be necessary to break up the body into multiple section templates.

