Gmail2GDrive
============

Gmail2GDrive is a Google Apps Script that you can use to store your Gmail attachements and Gmail Threads (in PDF) into the Google Drive folder of your choice.


Features
--------

* Save your Gmail attachements and emails into your Google Drive.
* Filter for relevant emails.
* Specify the destination folder.
* Rename attachments and PDF threads files:
   * add timestamp
   * replace a word
   * use RegEx
   * add source: 'mail' or 'file'
   * add mail id
   * make the filename computer filesystem accepted
* Save the thread as a PDF File.
* Log the activity into a Google Spreadsheet.
* Label and/or archive the emails already treated.

* Set it up to run preiodically.

Limitations
-----------

Gmail2GDrive currently has the following limitations:

* **Processing is done on a per-thread basis with a single email message per thread.** This is so because marking already processed emails is done using labels and GMail only alows to attach labels to a whole thread not to single email messages. For typical usage scenarios this is not really a problem but it may be if you want to process emails that are grouped by GMail into a thread (e.g. forum messages).
* **Google Apps Scripts limitations.** https://developers.google.com/apps-script/guides/services/quotas#current_limitations Script runtime, Custom function runtime, Triggers...


Setup
-----

1. Open [Google Apps Script](https://script.google.com/).
2. Create an empty project.
3. Give the project a name (e.g. MyGmail2GDrive)
4. Replace the content of the created file Code.gs with the provided [Code.gs](https://github.com/ahochsteger/gmail2gdrive/blob/master/Code.gs) and save the changes.
5. Create a new script file with the name 'Config' and replace its content with the provided [Config.gs](https://github.com/ahochsteger/gmail2gdrive/blob/master/Config.gs) and save the changes.
6. Adjust the configuration to your needs. It is recommended to restrict the timeframe using 'newerThan' to prevent running into API quotas by Google.
7. Test the script by manually executing the function performGmail2GDrive.
8. Create a time based trigger which periodically executes 'Gmail2GDrive' (e.g. once per day) to automatically organize your Gmail attachments within Google Drive.

Global Configuration
--------------------
Update the file Config.gs accordingly.

* globalFilter: Global filter expression (see <https://support.google.com/mail/answer/7190?hl=en> for available search operators)
* processedLabel: The GMail label to mark processed threads (will be created, if not existing)
* sleepTime: Sleep time in milliseconds between processed messages
* maxRuntime: Maximum script runtime in seconds (Google Scripts will be killed after 5 minutes)
* newerThan: Only process message newer than (leave empty for no restriction; use d, m and y for day, month and year)
* rules: List of rules to be processed, an array including all users rules.

Rule Configuration
------------------
Update the file Config.gs accordingly.

A rule supports the following parameters documentation:

* filter (String, mandatory): completes Global Filter (gmail filter).
* folder (String, mandatory): a path to an existing Google Drive folder (will be created, if not existing). Surrounded by single quotes.
* archive (boolean, optional): archives the mail. (default: false)
* saveThreadPDF (boolean, optional): save the thread email as a pdf. (default: false)

* filenameFromRegexp (String, optional): A regular expression to specify only relevant attachments. 
* filenameFrom (String, optional): if this texte is found in the filename, then it will be replaced by filenameReplaceTo.
* filenameTo (String, optional): Look for filenameReplaceFrom in the filename and replaces by this. The pattern for the new filename of the attachment. If 'filenameFrom' is not given then this will be the new filename for all attachments. 
  * '%s' is replaced by the name fo the file / the subject of the mail
  * '%t' is replaced by the file source: 'mail' or 'file'
  * '%id' is remplaced by the 3 last caracters of the id of the mail
  * the simple quotes are required.
  * You can add date format patterns: 'yyyy' for year, 'MM' for month and 'dd' for day. Cf <https://developers.google.com/apps-script/reference/utilities/utilities#formatDate(Date,String,String)> for more information on the possible date format strings.

Example Configuration
---------------------

```javascript
  * rule.filter =              "subject:'sujet 1'"
  * rule.folder =              "GM2GD"
  * rule.saveThreadPDF =       true
  * rule.archive =             true
  * rule.filenameFrom =        ""
  * rule.filenameFromRegexp =  ""
  * rule.filenameTo =          "yyyyMMdd-'%id'-'%t'-'%s'"
  * rule.filenameReplaceFrom = "Automatic";
  * rule.filenameReplaceTo =   "";
```

Original Script and related Thanks
----------------------------------

The original script is accessible here https://github.com/ahochsteger/gmail2gdrive
I'd like to thank [Amit Agarwal](http://www.labnol.org/about/) who provided similar functionality in his article [Send your Gmail Attachments to Google Drive](http://www.labnol.org/internet/send-gmail-to-google-drive/21236/) from which Gmail2GDrive evolved to provide more flexibility.
Also thanks to all [contributors](https://github.com/ahochsteger/gmail2gdrive/graphs/contributors) that extended GMail2GDrive with new functionality.
