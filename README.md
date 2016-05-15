# Kaltura chunked upload test
Simple Java code that splits a big video file into smaller chunks and uploads it to the Kaltura server using the uploadToken API.

This repo is meant as a proof of concept on how to utilize the uploadToken.upload() API to upload large files on the Kaltura server.
Seeing how Flash has 2G limitation where it comes to uploading files and the uploaded file size allowed can also be limited in the PHP settings, big files should sometimes be split into smaller ones and uploaded in chunks.

## Contents
UploadTest.java - a simple test class which can be used to test chunked upload from the command line
chunkedupload/ChunkedUpload.java - a help class which uses FileInputStream to read() a certain amount of bytes from the original file [thus spliting it] and upload it to the Kaltura server.

## Compiling the sample code
The code relies on the Kaltura Java client. When using the latest version, that can be obtained from:
http://search.maven.org/#search|ga|1|kaltura
or from:
https://github.com/kaltura/KalturaGeneratedAPIClientsJava

When using older versions of the Kaltura server, one should run:
\# php /opt/kaltura/app/generator/generate.php java
from the Kaltura server and then take the resulting package from /opt/kaltura/web/content/clientlibs/java

$ javac -cp /path/to/Kaltura/client/classes:/path/to/supporting/classes chunkedupload/ChunkedUpload.java
$ javac -cp /path/to/Kaltura/client/classes:/path/to/supporting/classes UploadTest.java

## Testing from CLI:
$ java -cp /path/to/Kaltura/client/classes:/path/to/supporting/classes UploadTest [service URL] [partner ID] [partner admin secret] [/path/to/large/vid/file]

You can also use the run.sh wrapper.

Note that to make bootstraping easier, the repository contains the following supporting JARs:
```
KalturaClient-3.3.1.jar
commons-codec-1.6.jar
commons-httpclient-3.1.jar
commons-logging-1.1.1.jar
httpclient-4.2.3.jar
json-20090211.jar
log4j-1.2.15.jar
```
You may want to update run.sh to use newer versions of these when available.

The above JARs were compiled using Java 7 [1.7], if you are using a different version, you will need use JARs compiled with your version instead.



## Sample output
```
Uploading a video file..." - (ChunkedUpload.java:118) 
[15 Dec 2015 14:12:15] INFO - "HASH:1316959668" - (ChunkedUpload.java:144) 
[15 Dec 2015 14:12:15] INFO - "Available bytes: 1070427397" - (ChunkedUpload.java:151) 
[15 Dec 2015 14:12:15] INFO - "Bytes read: 10240000" - (ChunkedUpload.java:159) 
[15 Dec 2015 14:12:15] INFO - "1 chunk[1] - uploaddFileSize: 1.024E7" - (ChunkedUpload.java:178) 
[15 Dec 2015 14:12:15] INFO - "uploaded 0%" - (ChunkedUpload.java:217) 
[15 Dec 2015 14:12:15] INFO - "Available bytes: 1060187397" - (ChunkedUpload.java:151) 
[15 Dec 2015 14:12:15] INFO - "Bytes read: 10240000" - (ChunkedUpload.java:159) 
[15 Dec 2015 14:12:16] INFO - "1 chunk[2] - uploaddFileSize: 2.048E7" - (ChunkedUpload.java:178) 
[15 Dec 2015 14:12:16] INFO - "uploaded 1%" - (ChunkedUpload.java:217) 
[15 Dec 2015 14:12:16] INFO - "Available bytes: 1049947397" - (ChunkedUpload.java:151) 
[15 Dec 2015 14:12:16] INFO - "Bytes read: 10240000" - (ChunkedUpload.java:159) 
...
[15 Dec 2015 14:12:42] INFO - "HASH:1219480864" - (ChunkedUpload.java:144) 
[15 Dec 2015 14:12:42] INFO - "Available bytes: 360041" - (ChunkedUpload.java:151) 
[15 Dec 2015 14:12:42] INFO - "Bytes read: 360041" - (ChunkedUpload.java:159) 
[15 Dec 2015 14:12:42] INFO - "was:false" - (ChunkedUpload.java:196) 
[15 Dec 2015 14:12:43] INFO - "n chunk[1] - uploaddFileSize: 360041.0" - (ChunkedUpload.java:208) 
[15 Dec 2015 14:12:43] INFO - "uploaded 100%" - (ChunkedUpload.java:217) 
[15 Dec 2015 14:12:43] INFO - "HASH:1219480864" - (ChunkedUpload.java:220) 
[15 Dec 2015 14:12:43] INFO - "

Uploaded a new Video file to entry: 0_m0q60mfz" - (ChunkedUpload.java:228)
```
