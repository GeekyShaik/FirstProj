
3	Authoring Environment
The Authoring Environment is a combination of capabilities from USAA and EMC Document Sciences xPression.  This section provides information about the functions in the Authoring Environment provided by USAA.
Because of limitations in xPression, USAA provided the following capabilities:
Variable Repository	A Variable Repository provides reusable variable definitions.  Multiple templates can reuse the same variable definitions.  
Variable Schema	A Variable Schema is used to gather the variables from multiple repositories into a schema which can be used by xPression.  Each xDesign document requires an associated schema.  
Template Profile	Template Profiles provide definitional information about each template which includes control information (e.g., Allow Free Text).  Templates can be associated with variables.  Master Templates can also be associated with Channels, Sections, and Enclosures.
Data Goget 	Gogets get data from pre-defined sources (e.g., CCIF, User Profile) and put the data into variables.
Search	Search allows the Author to specify criteria for ad-hoc searches of the Authoring information including variables, groups, templates, schemas, and data gogets.  The criteria the author is able to specify varies based upon the result list they specify.
Logic Tester	This is no longer used.
	
Migration	Migration is the function that allows authors to move documents to TEST or PRODUCTION.  It is highly recommended that you test the document using the Test Facility prior to moving it to production.  You can schedule a migration for a future date or request an expedited migration, which should occur within the next 15 minutes.

Test Case XML	This allows the author to upload a spreadsheet, containing all the test cases they need for xDesign unit testing, and have it formatted into xml and placed on their Authoring server (WCM). The spreadsheet format accepted by the Test Case generator is similar to the spreadsheet the authors were using for previous incarnations of infrastructure for xPression and was their preferred way of keeping that data. Note: xPression references all the test cases at an xPression Category level. This means that all the templates in a Category share the same list of test cases.

Test Facility	Test Facility enables the author to unit test their templates in a flow that is very close to how an MSR would interact with templates.
