iCloudDocumentSync
==================

iCloud Document Sync allows developers to integrate iCloud into their document projects with one-line code methods. Sync, download, save, and remove documents to and from iCloud with only ten lines of code.

## Integration
Adding iCloud Document sync to your project is easy. Follow these steps to get everything up and running:  
1. Drag `iCloud.h` and `iCloud.m` to your project  
2. Add `#import "iCloud.h"` to the header file of any class with which you wish to use iCloud Document Sync  
3. Add the `'iCloudDelegate` delegate and add the iCloud class to all of the files that import "iCloud.h". Here's what your document should look like:

    #import "iCloud.h"
    @class iCloud
    @interface ViewController : UIViewController < iCloudDelegate >  
Steps three and four are very important! Don't skip them!  
4. In your `viewDidLoad` or `viewDidAppear` make sure to call the following functions to setup iCloud:  

    iCloud *cloud = [[iCloud alloc] init];
    [cloud setDelegate:self];  
It is important that you set the iCloud delegate, otherwise some methods may not be called.  
5. Add the following **required** delegate method: `- (void)fileList:(NSArray *)files`  
6. If you like, you can add the following optional methods. The **first** method `documentWasSaved` will tell you when iCloud Document Sync has finished uploading a single file after calling the `createDocumentWithData` method. The **second** method `documentWasDeleted` will tell you when iCloud Document Sync has finished deleting a single document after calling the `removeDocumentWithName` method.

## Checking for iCloud Availability
To check if iCloud is available, simply add the following method call anywhere in your code where you'd like to know if iCloud is available: `[iCloud checkCloudAvailability];`. This method will return a boolean value (YES / NO). You can retrieve the value like this:

    BOOL cloudIsAvailable = [iCloud checkCloudAvailability];
    if (cloudIsAvailable) {
        //YES
    } else {
        //NO
    }

## Uploading Documents
To upload all documents stored locally in your application's documents directory, simly call: `[iCloud uploadDocumentsWithDelegate:self];`. Files in the local documents directory that do not already exist in iCloud will be *moved* into iCloud one by one. This process involves lots of file manipulation and as a result it may take a long time. This process will be performed on the background thread to avoid any lag or memory problems. Two delegate methods: `documentsStartedUploading` and `documentsFinishedUploading` will be called when the upload process starts and when all uploads end.

## Listing Documents
To get iCloud Sync to initialize for the first time, and continue to update when there are changes please follow these steps (same as steps three and four of the integration process):  
1. Add the iCloud class to the top of the header of all the files that import "iCloud.h": `@class iCloud`  
2. Call `[[iCloud alloc] init];` to start syncing with iCloud for the first time and in the future.    
 iCloud Document Sync will automatically sync all documents stored in the documents folder whenever there are changes in those documents. When something changes the following delegate method will be called and will return an NSMutableArray of all the files that are stored in iCloud. Here's the method (it's the only required one): 

    - (void)fileList:(NSMutableArray *)files

<del>Known Problem:  When syncing with iCloud, the `fileList` delegate method does not get called on the actual delegate, only in the delegates class. </del>  **FIXED** Please refer to the revised step four of the "Integration" section to fix the problem.

## Saving Documents
Documents can be created and then moved / saved to iCloud by using the following line: `[iCloud createDocumentWithData:NSDATA withName:@"DOCUMENT_NAME" withDelegate:self];`.  Make sure to replace NSDATA with your own NSData, and make sure to replace DOCUMENT_NAME with your own document name. All documents are saved to the local documents directory inside of your application's sandbox / directory and *then* uploaded to your application's iCloud folder and placed in a directory titled "Documents".

## Removing Documents
Documents can be deleted from iCloud and the local documents directory by using the following line: `[iCloud removeDocumentWithName:@"DOCUMENT_NAME" withDelegate:self];`.  Make sure to replace DOCUMENT_NAME with the name of a document that exists in both iCloud and the local directory. If it only exists in one location, it will only be deleted from that location.

## Retrieving Document Data
You can get the NSData from a file stored in your iCloud directory with the following line: `+ (NSData*)retrieveDocumentDatawithName:(NSString *)name`. This method will return NSData from the file specified. You can retrieve the data like this:

      NSData *myData = [iCloud retrieveDocumentDatawithName:@"DOCUMENT_NAME"];   
Files retreived are always searched for in your iCloud app's documents directory. If a file does not exist it will return a `nil` value instead of actual data.

## Known Problems
- Saving a local document to iCloud with the same name as one that already exists in iCloud will cause the document not to save to iCloud, instead it will remain in the local directory
- Document downloading to a local directory is not yet implemented
- <del>When syncing with iCloud, the `fileList` delegate method does not get called on the actual delegate, only in the delegates class. </del> **FIXED**: Please refer to the revised step four of the "Integration" section to fix the problem.
- <del>Saving a document will move it to iCloud rather than copying it<del>

## Change Log
**iCloud Document Sync is a work in progress**. Please help us get all features working and working well. We believe that this project will help many developers by easing the burden of iCloud. Below are the changes for each major commit:

Version 4.3.1 - License Update. Readme Update.  
Version 4.3 - New delegate methods for error reporting and file downloading. File downloaing introduced but not implemented. Updated Readme.  
Version 4.2 - Fixed errors when uploading files  
Version 4.1 - Updated Readme  
Version 4.0 - Upload and retrieve files with greater ease  

Version 3.0 - iCloud Syncing now allows for the uploading of all files in the local directory with one call. Gets changes everytime iCloud notifies of a change

Version 2.1 - Changed the File List to an `NSMutableArray` for better flexibility  
Version 2.0 - New delegate methods  

Version 1.1 - Add ability to remove documents from iCloud and local directory  
Version 1.0 - Initial Commit

## License
<a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/deed.en_US"><img alt="Creative Commons License" style="border-width:0" src="http://i.creativecommons.org/l/by-sa/3.0/88x31.png" /></a><br />This work by iRare Media</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/deed.en_US">Creative Commons Attribution-ShareAlike 3.0 Unported License</a>. This license lets you remix, tweak, and build upon our work even for commercial purposes, as long as you credit us ( **only when redistributing** this code - **no attribution required in your app**) and license your new creations under the identical terms.