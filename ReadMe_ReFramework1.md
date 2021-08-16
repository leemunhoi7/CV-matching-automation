Objective:
To design an UiPath program via REFramework approach to automate the matching of job skill-sets required by a job position with a list of CVs in either docx and pdf format and output the best matched CV's file names to a spreadsheet and with hyper-link added so that user can view the CV's content by clicking the filename..

Programming Plans:
1.	The main Program will extract user pre-defined job skill-set keywords from an input excel file and then compare with the contents of every CV fetched from CV lists that saved in local folders for matching. The best matched CVs with their % matches information will be exported on an output excel file in which, the CV file names , email and phone information, matched keyowrds as well as the transaction status will be presented.
2.	Include a sub sequence: ReadPDFtoChangeFileName.xaml for the purpose of renaming unnamed pdf CVs. By running this program, the file name of pdf files that kept in URI_pdf(default to "Data\pdf_orig") will be replaced by the email id(without @ and Domain name) extracted from the CV's content and save in new folder named pdf_renamed.
3.	Included another sub sequence:Add_hyper_link for user to convert more CV names on the output file to hyperlink to facilitate the immediate viewing of the CV's content.
4.	After successfully implemented prototype, the final product will be presented in REFramework approach but with orchestrator queue be replaced by files array.
5.	Include also a sub sequence :Email_Automation_for_Shortlisted_Candidates.xaml to automate the sending of interview invitation emails to shortlisted candidates after user reviewed the content of best matched CVs.

The Prototype UiPath program info:
1. User need to create a folder(Data) under the default UiPath directory to house the input file(Config.xlsx) as well as CV file folders named CVs_pdf, CVs_docx for keepting CV file lists writen in pdf and docx formats respectively. There is no limitation on CV files number but all must be in either docx or pdf formats. 
2. All user options are to be updated in Config.xlsx under sheet name:Keyowrds(for job skill-set keywords) and Settings(for input/output file paths and hyperlinks default number, see below).
Name		Value
----		-----
URI_Data	Data
URI_OldData	Old_Data        (auto created if not exist- for keeping older output files)
URI_docx	Data\CVs_docx
URI_pdf		Data\CVs_pdf
inputFile	Data\Config.xlsx 
outputcsvFile	Data\matchOut.csv (This is intermediate file will be moved to Old_Data after creating xlsx output file)
outputxlsxFile	Data\matchOut.xlsx (This output file will be auto created at end of program. Older file will be moved to Old_Data if detected)
outputlogFile	Data\OutputLog.txt (This log file will be auto created if not exist. New log data from new run will be appended)
nbrOfHyperlink	10   (This will convert first 10 best matched CV name to hyperlink. User can change this number if needed)

3. User can enter min 1 or max up to 7 skill-set keywords in Config.xlsx 
4. The matched information of each CV will first append to an intermediate file matchOut.csv which will then be converted to matchOut.xlsx after all the CV files are transacted. Sorting of best matched CV and matched % will be calcuated in the xlsx file.
5. Below are the description of headers(column names) defined in matchOut.xlsx:

CV_nbr : The incremental index number assigned to each CV when being fetched during the For Each Loop.
Name : The CV file name excluding the path info. It can be converted to hyper link for user to read the CV's content by clicking on the name.
MatchCount: The number of keywords that match.
Match% : The % matching rate based on the total number of keywords used.
Email : The extracted email id from the CV that hits >50% match.
Phone : The extracted phone number from the CV that hits >50% match.
MatchKeywords : all the matched keywords.
Status : Transaction Successful or Business rule exception or application exception.
Candidate Name: For interview invitation email automation - User to enter the shortlisted candiate name here.
Interview Date: For interview invitation email automation - User to enter the proposed date of interview.
Interview Time: For interview invitation email automation - User to enter the proposed time of interview.

6. An output log file: OutputLog.txt will also be auto generated to record status info during runtime. (note: this file size will grow for each rerun due to new logs data are appended)
7. All the CVs inside the 2 folders will be combined during runtime and being fetched by the program for execution. 
Any unreadable CVs will be thrown as Business Exception and skipped by the program but will be recorded in OutputLog.txt.
8. The messages of “No email id detected” or "No phone nbr detected" will be presented if these informations are not available or not readable in that CV that is qualified for extracting the email/phone info.
9. When running Add_hyper_link.xaml to convert more CV names to hyperlink, user will be prompted to input the starting row and ending row.
10. Before running the sub sequence:Email_Automation_for_Shortlisted_Candidates.xaml, user need to manually fill-in the candidate's names, the proposed interview date and time and also to ensure the candidate's CV have valid email id. 
11. The above sub-sequence will extract the candidate's name, intervew data and time and update to an interview invitation email template and send it out to candidate's email one at a time automatically. 

MainCV programming history:
MainCV - 23Jul2021 : Initial release.
MainCV1 - 27Jul2021: 1. Allow user to enter any number of skill-set keywords from minimum 1 up to maximum of 7.
                     2. Auto fetch CV files from 2 folders and auto detect docx and pdf file to execute accordingly. 
MainCV2 - 29Jul2021: 1. Convert output file from csv to xlsx and then convert the first 10 best match CV names as hyperlink pointing the corresponding CV file.
                     2. Install BalaReva.Easy.Excel.Activities Package on UiPath required to generate hyperlink on xlsx sheet.
MainReFramework - 1Aug2021: 1. Convert MainCV2 program structure from Sequential flow to ReFramework flow.(The MainCV2 program is still executable)
                            2. All the programmable paths(URI_docx, URI_pdf, URI_Data) and input/output file variables(inputFile,outputcsvFile,outputxlsxFile,outputlogFile) and options                               (nbrOfHyperlink) are moved to Config.xlsx file for the ReFramework program to fetch during initialization.
                            3. Added sub-sequence:Email_Automation_for_Shortlisted_Candidates.xaml for automate sending email to shortlisted candidates.
                - 15Aug2021: Install BalaReva.Excel.Activities Package to enable data visualization to automate the ploting of bar and pie charts.
