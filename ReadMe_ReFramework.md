Objective:
To design an UiPath program to automate the matching of job skill-set required by a new job position with a list of resumes and to extract out best-matched resumes base on the sorting of percent matches.

Programming Plans:
1.	The main Program: MainCV2 will extract user pre-defined job skill-set keywords from an input excel file and then compare with the contents of every CV fetched from CV lists that saved in local folders for matching. The best matched CVs with their % matches information will be exported on an output excel file in which, the CV file names , email and phone information as well as the matched keywords will be presented.
2.	Included a sub sequence: ReadPDFtoChangeFileName.xaml for the purpose of renaming unnamed pdf CVs. By running this program, all the pdf CV files in URI_pdf(default at "Data\pdf", can be changed if needed)by the respective email names(without @ and Domain name) extracted from the CV's content. 
3.	Included also a sub sequence:Add_hyper_link for user to convert more CV names on the output file to hyperlink for immediate viewing the CV's content.
4.	After successfully implemented prototype, the final product will be presented in REFramework format.

The Prototype UiPath program info:
1. User need to create 2 data folders named Data and Old_Data under the default UiPath directory to house the input(config file) and output files(self generated) as well as CV file folders named CVs_pdf, CVs_docx for keepting CV files writen in pdf and docx formats respectively.
2.  User might need to update the following path/file in the program’s variables default setting if needed:
inputFile = “Data\Config.xlsx” (Config.xlsx must be generated upfront and cannot be deleted)
URI_docx = “Data\CVs_docx”  (Under UiPath default folder for keeping CVs list in docx format)	
URI_pdf = “Data\CVs_pdf”  (Under UiPath default folder for keeping CVs list in pdf format)
URI_Data = "Data"         (Under UiPath default folder)
3. The input file Config.xlsx is for User to enter min 1 and up to max 7 skill-set keywords at sheet name:Keywords under the header:keyword.
4. The matching information of each CV will append to self-generated output intermediate file matchOut.csv which will then be converted to matchOut.xlsx at the end of program for sorting out the best matches CVs.
5. Below are the description of headers defined in matchOut.xlsx:
CV_nbr : The incremental index number assigned to each CV when being fetched during the For Each Loop.
Name : The original CV file name. The best matched CV name will be converted to hyper link for user to read the CV's content by clicking the name.
MatchCount: The number of keywords that match.
Match% : The % matching rate based on the total number of keywords used.
Email : The extracted email id from the CV that hits >50% match.
Phone : The extracted phone number from the CV that hits >50% match.
MatchKeywords : List out the keywords of which the matches found.
6. An output log file: OutputLog.txt will also be auto generated to record status info during runtime. (note: this file size will grow for each rerun due to new logs are appended)
7. All the CVs inside the 2 folders will be combined during runtime and being fetched by the program for execution. 
Any unreadable CVs will be skipped by the program but will be recorded in OutputLog.txt.
8. The email id and phone nbr of the CV will be extracted to output file only if it meets >50% matching rate. The messages of “No email id detected” or "No phone nbr detected" will be presented if the informations are not available or not being readable in that CV.
9. The first 10 best match CV names listed in the output xlsx will be converted as hyperlink so that user can view the content of the CV by clicking on the hyperlink.
User can change the default setting of 10 via variable:nbrOfHyperlink and/or run the sub sequence named:Add_hyper_link to convert more CV names to hyperlink if needed.

MainCV programming history:
MainCV - 23Jul2021 : Initial release.
MainCV1 - 27Jul2021: 1. Allow user to enter any number of skill-set keywords from minimum 1 up to maximum of 7.
                     2. Auto fetch CV files from 2 folders and auto detect docx and pdf file to execute accordingly. 
MainCV2 - 29Jul2021: 1. Convert output file from csv to xlsx and then convert the first 10 best match CV names as hyperlink pointing the corresponding CV file.
                     2. Install BalaReva.Easy.Excel.Activities Package on UiPath required to generate hyperlink on xlsx sheet.
MainReFramework - 1Aug2021: 1. Convert MainCV2 program structure from Sequential flow to ReFramework flow.(The MainCV2 program is still executable)
                            2. All the programmable paths(URI_docx, URI_pdf, URI_Data) and input/output file variables(inputFile,outputcsvFile,outputxlsxFile,outputlogFile) and options(nbrOfHyperlink) are moved to Config.xlsx file for the ReFramework program to fetch during initialization.