2.1	Introduction to Sections
Figure 2 2 Shows the Cancellation Letter separated into sections.  The letter was broken into five sections:  Address Section, Cancelled Policy Section, Issue New After Cancel Section, Automatic Payment Section, and Signature Section.  

Why break a letter into many section templates?  The most important reason to break it into many section templates is to promote reuse of the section templates.  Many Master Templates can reuse the same Address Section and Signature Section.  

There aren’t as many Master Templates that will use the other three sections, but it may be convenient to separate them for reuse in a few.  

Another reason to have sections involves the use of variables.  We will discuss that further below.  
Does this Message have any Enclosures? In addition to sections, the message might require an enclosure to be attached. For example, a message to the member that suggests an Automatic Payment Plan might need an Auto Pay Plan enclosure for the member to sign. Although the text for the enclosure isn’t shown in Figure 2-2, the Signature Section made reference to the existence of an enclosure.  Section 5.3 of this document describes how to define an Enclosure. 

2.2	Introduction to Variables
Every value in the body of a message which can vary based on a particular instance of a message should be a variable.  Some example variables:  USAA Number, Payment Amount, and Signature Name.  Variables receive values using any of the following:
•	Applications can pass variable data into ECT
•	MSRs can enter variable values using the Composition Environment
•	Message Publishers can provide default values when he/she defines the variable in the Authoring Environment
•	Message Publishers can specify a goget to get the value from a pre-defined database such as Corporate Customer Information File (CCIF) or the User Profile  
•	Message Publishers can assign a value to a variable.
Figure 2-3 has highlighted some values which should be variables in the Cancellation Letter.  Some of the variables and their values:
Address Line 1	Ms William R. Smith
Address Line 2	3252 78th PL NE
Address Line 3	Medina, WA 98039
Letter Process Date	May 17, 2010
Salutation	Dear Ms. Smith,
Payment Amount	$1000.00
Policy LOB	Auto
Policy Number	USAA 00000 00 05 70A
 
To make certain I/T Applications and MSRs provide good values for the variables, the Message Publisher can provide metadata about variables using the Authoring Environment.  He/she can specify the length, data type (e.g., date, alphabetic), and edit rule (e.g., in the range from 1 to 10000).  Additionally, the label shown to the user can be provided as part of the metadata. 
 

