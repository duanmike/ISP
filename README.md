# SecurityReferenceMonitor-repy_V2

## Repy_V2

RepyV2 is a cross-platform, Restricted Python environment and is used most prominently in [Seattle](https://github.com/SeattleTestbed), the open peer-to-peer testbed. More detailed
information is available from the
[Seattle documentation](https://github.com/SeattleTestbed/docs/blob/master/UnderstandingSeattle/README.md).

## Initial

Based on: https://github.com/SeattleTestbed/docs/blob/master/EducationalAssignments/ABStoragePartOne.md

Build a a security layer which keeps a backup copy of a file in case it is written incorrectly. For this project, a valid file must start with the character 'S' and end with the character 'E'. If any other characters (including lowercase 's', 'e', etc.) are the first or last characters, then the file is considered invalid.

Applications use ABopenfile() to create or open a file. Files are created by setting create=True when calling ABopenfile(), the reference monitor will create a valid backup file called filename.a and an empty file we will write to called filename.b. When close() is called on the file, if both filename.a and filename.b are valid, the original file's data is replaced with the data of filename.b. If filename.b is not valid, no changes are made.

The Reference Monitor Must:
1. Not modify or disable any functionality of any [RepyV2 API calls](../Programming/RepyV2API.md), such as:
   * Creating new files  
   * Opening an existing file  
   * Reading valid backup using readat()  
   * Writing to file using writeat(). This includes invalid writes, because  'S' and 'E'
may later be written to the begining and end of the file respectively.  
2. Check if file contents starts with 'S' and ends with 'E', only when close() is called.
3. Update the original file with the new data IF the new data is valid on close().
4. Not produce any errors  
   * Normal operations should not be blocked or produce any output  
   * Invalid operations should not produce any output to the user
The Reference Monitor Should:
1. Create two copies of the same file (filename.a and filename.b)   
   * One is a valid backup to read from, and the other is written to
2. When an app calls ABopenfile(), the method opens the A/B files, which 
 you should name filename.a and filename.b.
3. When the app calls readat(), all reads must be performed on the valid backup.
4. When the app calls writeat(), all writes must be performed on the written to file. 

Three design paradigms are at work in this assignment: accuracy,
efficiency, and security.

 * Accuracy: The security layer should only stop certain actions from being
blocked. All other actions should be allowed. For example, if an app
tries to read data from the backup file, this must succeed as per normal and
must not be blocked.  All situations that are not described above *must*
match that of the underlying API.

 * Efficiency: The security layer should use a minimum number of resources,
so performance is not compromised.  For example, keeping a complete copy of 
every file on disk in memory would be forbidden.

 * Security: The attacker should not be able to circumvent the security
layer. For example, if the attacker can cause an invalid file to be saved, read the "write to" file, or can
write to the backup file we read from, then the security is compromised.

## Attack Cases

Based on: https://github.com/SeattleTestbed/docs/blob/master/EducationalAssignments/ABStoragePartTwo.md

Attack other reference monitors in attempt to circumvent security. Attempt to read, write, and append data without having the proper missions enabled. If able to do so, then the security layer is not secure. 
