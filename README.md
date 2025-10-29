# Case Study #1: Open Discover® Workflow Management System's (WMS) Document Processing Performance in Microsoft Azure

**Nearly 600 GB/hour processing rate based on collection (matter) end-to-end processing time.
The end-to-end processing time includes:**
- Container extraction
- Document text, metadata, language identification, embedded object/attachment extraction
- [PII/PHI/FERPA entity extraction](https://dotfurther.github.io/OpenDiscoverSDK/html/2caef568-f7bd-69fc-89c4-aa0d3e2c497b.htm)
- De-Nist of all documents
- Deduplication of all documents
- Lucene index creation
- Load file creation (document and entity Relativity Dynamic Object (RDO) load files)

### Processed Collection (Matter) for this Study:
- 1.65 TB of client data (see Table 1 for a summary of processed documents by their Open Discover classification type).
- Over 3.3 TB's expanded size after processing all containers, attachments, and embedded documents (see Image 1)
- Executable and other 'junk' file types were excluded from processing. Excluded file types and NIST documents get an entry in WMS's SQL database but once identified are not processed further, exported, or indexed.
- OCR was not required for this project. However, dotFurther's WMS has a scalable solution for OCR.

### Azure Virtual Machine Configuration:
| Hosted Software 	| VM Count | VM Configuration| 	
|---	|---	|---	|
|Workflow Management System/RabbitMQ    |1 |8-core, 128 GB  RAM  |
|SQL Server      |1 |32-core, 256 GB RAM  |
|Workers         |3 |48-core, 256 GB RAM  (144 virtual cores used by 21 WMS distributed Workers in total) |

### Processing Summary Results:
|||
|---	|---	|
|Collection (Matter) input document and container size:      |1.65 TB |
|Total expanded document size after processing:              |3.34 TB |
|Collection (Matter) input document and container count:     |1,711,427 |
|Total expanded document count after processing:             |8,546,973 |
|Total original document count:                              |4,319,434 | 
|Total excluded document count:                              |4,037 | 
|Total empty (zero-byte) document count:                     |26,371 | 
|Total container unextractable document count:               |546 | 
|Total NIST document count:                                  |121,867 |
|Total elapsed processing time (end-to-end):                 |5.86 hours|
|Expanded document processing rate:                          |577.8 GB/hour|

See Image 1 for a WMS processing summary screen shot.

- An 'original' document is the document chosen to be the representing document from a duplicate set of documents.
- A 'container unextractable document' is a document that could not be extracted from a container (e.g., a password protected ZIP archive where no password was provided to extract archive documents).

Observation: 
WMS tasks that update the SQL database storage did not rate-limit (bottleneck) processing at all. We believe we could double the amount of workers (worker VMs) to achieve 
greater than 1 TB / hour end-to-end processing rate.

Future Improvement:
We have an easy performance improvement in the works that will increase WMS Worker processing/extraction performance by another 30-40%.

### Image 1: Processing Summary
Screen shot of the collection's post-processing report. 
Note: An 'original' document is the document chosen to be the representing document from a duplicate set of documents.

![ProcessingSummary](https://github.com/dotfurther/Open-Discover-WhitePaper-1/assets/52750989/4f0a2598-4d28-4e69-8496-1626fba6b0a8)


### Image 2: PII/PHI/FERPA Entity Extraction Summary Report
Screen shot of the collection's post-processing entity extraction report.

![EntityReport_Overview](https://github.com/dotfurther/Open-Discover-WhitePaper-1/assets/52750989/21845b8b-51f9-461e-8fc3-06b783866005)


### Image 3: PII/PHI/FERPA Entity Filter Report
Screen shot of the collection's post-processing entity filter report. The user can select a subset of PII/PHI/FERPA entities
to hit search for in all 'original' documents. The 'Person Count' is always returned as part of the report and
is the number (non-duplicated) person names found in a document.

![EntityReport_Filter](https://github.com/dotfurther/Open-Discover-WhitePaper-1/assets/52750989/622be00c-b8fd-422d-8e58-e67491bd5470)


### Table 1: Summary Report of Collection Document Totals by Classification Type
- '[Classification Type](https://dotfurther.github.io/OpenDiscoverSDK/html/2caef568-f7bd-69fc-89c4-aa0d3e2c497b.htm)' is the type of a document's file format. 
- 'Document Count' is the total number of documents per the classification type (includes all document records).
- 'Original Document Count' is de-deduplicated count of 'Document Count'
- All document size columns are in giga-bytes (GB)
  
| Classification Type | Document Count | Original Document Count | Document Size GB | Original Document Size GB | NIST Count | Excluded Count |
|---	|---	|---	|---	|---	|---	|--- |
| RasterImage 	| 2,840,428 	| 338,707 	| 151.87 	| 64.24 	| 72,734 	| 0 	|
| Email 	| 2,638,860 	| 2,230,614 	| 576.26 	| 439.71 	| 0 	| 0 	|
| DocumentExchange 	| 1,002,315 	| 710,790 	| 814.01 	| 614.67 	| 8 	| 0 	|
| Multimedia 	| 880,304 	| 492,967 	| 220.24 	| 98.87 	| 53 	| 0 	|
| Calendar 	| 279,100 	| 197,533 	| 24.48	| 18.71 	| 0 	| 0 	|
| Spreadsheet 	| 205,503 	| 108,928 	| 224.68 	| 125.30 	| 4 	| 0 	|
| Other 	| 173,521 	| 47,251 	| 98.81 	| 7.95 	| 41,449 	| 0 	|
| Markup 	| 129,931 	| 63,576 	| 9.30 	| 6.00 	| 4,284 	| 0 	|
| WordProcessing 	| 89,421 	| 45,085 	| 15.38 	|7.76 	| 1,142 	| 0 	|
| VectorImage 	| 52,474 	| 12,886 	| 13.29 	| 4.85 	| 465 	| 0 	|
| Archive 	| 24,917 	| 8,343 	| 410.95	| 366.21 	| 12 	| 31 	|
| Presentation 	| 24,114 	| 10,153 	| 80.05 	|28.01 	| 1 	| 0 	|
| Contacts 	| 22,983 	| 21,989 	|0.34 	| 0.33 	| 0 	| 0 	|
| Text 	| 18,963 	| 5,526 	| 3.14 	| 2.40 	| 1,489 	| 0 	|
| SourceCode 	| 18,697 	| 9,836 	| 2.23 	| 0.86 	| 43 	| 0 	|
| DataFile 	| 13,847 	| 10,222 	| 3.37 	| 2.54	| 75 	| 0 	|
| Fonts 	| 4,402 	| 130 	| 0.13 	| 0.01 	| 45 	| 0 	|
| Executable 	| 3,970 	| 3,824 	| 1.32 	| 1.26 	| 59 	| 3,970 	|
| Database 	| 458 	| 329 	| 2.51 	| 2.36 	| 0 	| 0 	|
| NotesAndResearch 	| 360 	| 360 	|1.39 	| 1.39 	| 0 	| 0 	|
| SecurityAndEncryption 	| 183 	| 162 	| 0.04 	| 0.04 	| 4 	| 0 	|
| MailStore  	| 149 	| 81 	| 730.99 	| 524.26 	| 0 	| 0 	|
| Financial 	| 76 	| 39 	| 2.94 	| 1.50 	| 0 	| 0 	|
| DesktopPublishing 	| 51 	| 31 	| 0.04 	| 0.03 	| 0 	| 0 	|
| Telecommunication 	| 37 	| 34 	| 0.00 	| 0.00 	| 0 	| 0 	|
| MediaImage 	| 24 	| 24 	| 0.15 	| 0.15 	| 0 	| 0 	|
| ProjectManagement 	| 14 	| 10 	| 0.01 	| 0.00 	| 0 	| 0 	|
| CadCamAnd3dModel 	| 4 	| 4 	| 0.01 	| 0.01 	| 0 	| 0 	|

### Table 2: Collection Document Totals by File Type
- 'FileType' column contains the [identified file (document) format type](https://dotfurther.github.io/OpenDiscoverSDK/html/6f1047fb-7367-c09c-5621-ae7632c8404b.htm).
- 'Document Count' is the total number of documents per the file format type (includes all document records).
- 'Original Document Count' is de-deduplicated count of 'Document Count'
- All document size columns are in giga-bytes (GB)
  
| FileType 	| Document Count 	| Original Document Count 	| Document Size GB 	| Original Document Size GB 	| NIST Count 	| Excluded Count 	|
|---	|---	|---	|---	|---	|---	|---	|
| OutlookMessage 	| 2,523,430 	| 2,115,866 	| 566.593 	| 430.097 	| 0 	| 0 	|
| JpegJFIF 	| 1,752,585 	| 97,874 	| 39.732 	| 10.966 	| 11,989 	| 0 	|
| Png 	| 890,084 	| 198,541 	| 56.351 	| 16.154 	| 1,307 	| 0 	|
| AdobePDF 	| 886,732 	| 618,039 	| 671.866 	| 484.377 	| 2 	| 0 	|
| RiffWave 	| 811,731 	| 470,541 	| 121.978 	| 67.723 	| 44 	| 0 	|
| OutlookAppointment 	| 146,454 	| 98,539 	| 13.675 	| 10.791 	| 0 	| 0 	|
| Unknown 	| 132,434 	| 39,935 	| 11.108 	| 6.142 	| 40,878 	| 0 	|
| Gif 	| 122,291 	| 18,825 	| 2.257 	| 0.606 	| 59,245 	| 0 	|
| AdobePDFAcroForm 	| 95,743 	| 78,933 	| 135.075 	| 126.217 	| 4 	| 0 	|
| XML 	| 80,629 	| 53,764 	| 8.227 	| 5.325 	| 317 	| 0 	|
| Excel2016 	| 66,753 	| 40,225 	| 77.154 	| 56.740 	| 0 	| 0 	|
| OutlookMeetingRequest 	| 58,338 	| 42,655 	| 7.286 	| 5.325 	| 0 	| 0 	|
| JpegEXIF 	| 54,765 	| 14,731 	| 36.209 	| 26.265 	| 2 	| 0 	|
| OutlookOLEClass 	| 53,831 	| 53,440 	| 3.185 	| 3.179 	| 0 	| 0 	|
| OutlookMeetingRequestAccept 	| 52,541 	| 39,685 	| 1.971 	| 1.513 	| 0 	| 0 	|
| Excel2010 	| 49,826 	| 24,128 	| 69.699 	| 30.252 	| 0 	| 0 	|
| WindowsEnhancedMetafile 	| 42,500 	| 9,844 	| 10.698 	| 4.261 	| 10 	| 0 	|
| Excel2003 	| 38,819 	| 21,792 	| 39.086 	| 22.096 	| 0 	| 0 	|
| Excel2013 	| 36,856 	| 14,028 	| 31.610 	| 12.065 	| 0 	| 0 	|
| OutlookMessageOther 	| 36,502 	| 36,502 	| 5.523 	| 5.523 	| 0 	| 0 	|
| Word2003 	| 32,479 	| 16,200 	| 6.628 	| 3.493 	| 130 	| 0 	|
| AdobeFlash 	| 29,442 	| 11,452 	| 1.576 	| 0.683 	| 3 	| 0 	|
| AdobeFlashZlib 	| 27,966 	| 7,576 	| 0.983 	| 0.209 	| 3 	| 0 	|
| EmptyFile 	| 26,371 	| 0 	| 0.000 	| 0.000 	| 0 	| 0 	|
| HTML 	| 24,723 	| 6,383 	| 0.387 	| 0.251 	| 98 	| 0 	|
| Word2016 	| 22,684 	| 12,775 	| 3.972 	| 2.141 	| 0 	| 0 	|
| OutlookContact 	| 21,789 	| 21,145 	| 0.339 	| 0.332 	| 0 	| 0 	|
| OutlookJournal 	| 21,292 	| 21,292 	| 0.276 	| 0.276 	| 0 	| 0 	|
| Word2010 	| 17,692 	| 7,124 	| 2.697 	| 1.051 	| 0 	| 0 	|
| JavaScriptFile 	| 17,675 	| 8,861 	| 2.215 	| 0.853 	| 37 	| 0 	|
| ArchiveGZip 	| 16,287 	| 5,260 	| 0.789 	| 0.296 	| 0 	| 0 	|
| Text7BitASCII 	| 16,264 	| 4,663 	| 2.137 	| 1.470 	| 1,472 	| 0 	|
| XMLSchemaDefinition 	| 12,634 	| 121 	| 0.071 	| 0.001 	| 3,857 	| 0 	|
| PowerPoint2016 	| 10,704 	| 4,512 	| 52.158 	| 14.383 	| 0 	| 0 	|
| OutlookMeetingCancellation 	| 9,759 	| 6,967 	| 1.228 	| 0.805 	| 0 	| 0 	|
| Tiff 	| 9,738 	| 5,058 	| 8.831 	| 5.533 	| 9 	| 0 	|
| Word2013 	| 9,656 	| 5,151 	| 1.446 	| 0.723 	| 0 	| 0 	|
| HTML5 	| 8,707 	| 2,359 	| 0.332 	| 0.275 	| 0 	| 0 	|
| ArchiveZip 	| 8,441 	| 2,924 	| 408.502 	| 364.278 	| 0 	| 0 	|
| JSON 	| 8,004 	| 6,444 	| 0.157 	| 0.074 	| 7 	| 0 	|
| AdobePDF_XFA 	| 6,968 	| 5,585 	| 1.501 	| 1.211 	| 0 	| 0 	|
| Excel2007 	| 5,667 	| 3,440 	| 2.476 	| 1.361 	| 0 	| 0 	|
| PowerPoint2010 	| 5,560 	| 2,313 	| 11.209 	| 5.009 	| 0 	| 0 	|
| DeviceIndependentBitmapOLE2Container 	| 5,378 	| 4,358 	| 1.081 	| 0.901 	| 0 	| 0 	|
| JpegRaw 	| 5,287 	| 1,448 	| 1.147 	| 0.249 	| 0 	| 0 	|
| Word2007 	| 5,052 	| 2,746 	| 0.399 	| 0.204 	| 0 	| 0 	|
| PowerPoint2013 	| 4,950 	| 2,479 	| 11.478 	| 5.940 	| 0 	| 0 	|
| CommaSeparatedValuesFile 	| 4,562 	| 2,988 	| 3.209 	| 2.457 	| 0 	| 0 	|
| MPEG4 	| 4,194 	| 738 	| 56.973 	| 14.437 	| 0 	| 0 	|
| OutlookMeetingRequestDecline 	| 3,913 	| 3,121 	| 0.146 	| 0.118 	| 0 	| 0 	|
| WebOpenFontFormat1 	| 3,884 	| 43 	| 0.111 	| 0.002 	| 0 	| 0 	|
| iCalendar 	| 3,823 	| 2,849 	| 0.015 	| 0.012 	| 0 	| 0 	|
| RichTextFormat 	| 3,759 	| 3,265 	| 1.488 	| 0.693 	| 0 	| 0 	|
| WindowsMetafile3 	| 3,615 	| 585 	| 1.780 	| 0.217 	| 0 	| 0 	|
| Excel2002 	| 3,451 	| 2,571 	| 0.403 	| 0.281 	| 0 	| 0 	|
| DotNetAssemblyAnyCpu 	| 3,141 	| 3,107 	| 0.253 	| 0.252 	| 6 	| 3,141 	|
| AdobePDF_Portfolio 	| 3,119 	| 1,683 	| 1.421 	| 0.731 	| 0 	| 0 	|
| AdobePDFEncrypted 	| 2,955 	| 1,735 	| 1.076 	| 0.529 	| 1 	| 0 	|
| PlaceableWindowsMetafile 	| 2,871 	| 953 	| 0.412 	| 0.110 	| 451 	| 0 	|
| AdobePDFAcroFormEncrypted 	| 2,610 	| 1,422 	| 1.239 	| 0.750 	| 0 	| 0 	|
| OutlookMeetingRequestTentative 	| 2,525 	| 1,992 	| 0.087 	| 0.070 	| 0 	| 0 	|
| MicrosoftOfficeOwnerFile 	| 2,506 	| 713 	| 0.000 	| 0.000 	| 0 	| 0 	|
| ISOBaseMediaFileFormat 	| 2,307 	| 739 	| 18.923 	| 5.993 	| 0 	| 0 	|
| JpegER 	| 2,042 	| 602 	| 0.139 	| 0.042 	| 8 	| 0 	|
| AdobePhotoshopPSD 	| 1,848 	| 704 	| 5.989 	| 3.729 	| 0 	| 0 	|
| CompressedWindowsEnhancedMetafile 	| 1,838 	| 754 	| 0.065 	| 0.029 	| 0 	| 0 	|
| XHTML 	| 1,810 	| 597 	| 0.010 	| 0.004 	| 0 	| 0 	|
| OutlookMeetingNotificationForward 	| 1,746 	| 1,724 	| 0.073 	| 0.072 	| 0 	| 0 	|
| MicrosoftForms2_HtmlCheckBox 	| 1,679 	| 539 	| 0.006 	| 0.002 	| 0 	| 0 	|
| TextUTF8 	| 1,499 	| 431 	| 0.826 	| 0.808 	| 6 	| 0 	|
| ThinkCellCharting 	| 1,410 	| 72 	| 0.013 	| 0.003 	| 0 	| 0 	|
| CSS 	| 1,281 	| 244 	| 0.165 	| 0.049 	| 12 	| 0 	|
| AppleDouble2 	| 1,237 	| 765 	| 0.002 	| 0.002 	| 68 	| 0 	|
| MPEG2AudioLayer3 	| 1,199 	| 458 	| 0.096 	| 0.046 	| 0 	| 0 	|
| vCard 	| 1,194 	| 844 	| 0.001 	| 0.001 	| 0 	| 0 	|
| Excel97 	| 1,107 	| 982 	| 0.511 	| 0.472 	| 1 	| 0 	|
| PowerPoint97 	| 1,076 	| 541 	| 3.149 	| 1.387 	| 0 	| 0 	|
| WindowsBitmap 	| 1,057 	| 479 	| 0.942 	| 0.497 	| 5 	| 0 	|
| MPEG1AudioLayer3 	| 1,029 	| 341 	| 0.393 	| 0.191 	| 3 	| 0 	|
| MicrosoftPhotoEditor3 	| 1,024 	| 133 	| 0.998 	| 0.127 	| 0 	| 0 	|
| SVG 	| 912 	| 418 	| 0.015 	| 0.009 	| 4 	| 0 	|
| JavaClass 	| 826 	| 824 	| 0.004 	| 0.004 	| 6 	| 0 	|
| OutlookReportNonDelivery 	| 694 	| 687 	| 0.277 	| 0.275 	| 0 	| 0 	|
| MicrosoftClipArtGallery 	| 656 	| 47 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Word2002 	| 632 	| 449 	| 0.113 	| 0.073 	| 0 	| 0 	|
| Text_IBM_866 	| 607 	| 37 	| 0.037 	| 0.014 	| 0 	| 0 	|
| Excel95 	| 571 	| 530 	| 0.991 	| 0.921 	| 0 	| 0 	|
| ContainerUnextractable 	| 546 	| 0 	| 84.354 	| 0.000 	| 0 	| 0 	|
| Excel2016Macro 	| 533 	| 223 	| 0.821 	| 0.335 	| 0 	| 0 	|
| MimeEmail 	| 527 	| 383 	| 0.044 	| 0.026 	| 0 	| 0 	|
| OutlookStickyNote 	| 499 	| 484 	| 0.004 	| 0.004 	| 0 	| 0 	|
| MacromediaFlash 	| 490 	| 230 	| 0.264 	| 0.149 	| 0 	| 0 	|
| WindowsMediaVideo 	| 446 	| 215 	| 11.882 	| 5.353 	| 0 	| 0 	|
| MPEG1AudioLayer2 	| 427 	| 105 	| 0.025 	| 0.011 	| 0 	| 0 	|
| Word2000 	| 414 	| 262 	| 0.040 	| 0.023 	| 230 	| 0 	|
| OutlookDocument 	| 409 	| 409 	| 0.218 	| 0.218 	| 0 	| 0 	|
| MicrosoftForms2_HtmlText 	| 402 	| 332 	| 0.001 	| 0.001 	| 0 	| 0 	|
| WindowsDLL32 	| 400 	| 364 	| 0.147 	| 0.128 	| 35 	| 400 	|
| TrueTypeFont 	| 397 	| 52 	| 0.014 	| 0.003 	| 44 	| 0 	|
| MicrosoftForms2_HtmlHidden 	| 395 	| 224 	| 0.001 	| 0.001 	| 0 	| 0 	|
| MicrosoftVBAProject 	| 394 	| 145 	| 0.032 	| 0.011 	| 0 	| 0 	|
| SonyRaw 	| 383 	| 246 	| 0.095 	| 0.064 	| 0 	| 0 	|
| Excel2007OnwardEncrypted 	| 379 	| 237 	| 0.162 	| 0.090 	| 0 	| 0 	|
| UnknownCompoundFile 	| 368 	| 186 	| 1.112 	| 0.686 	| 0 	| 0 	|
| Quicktime 	| 365 	| 190 	| 2.582 	| 1.351 	| 0 	| 0 	|
| TextUnicode16LE 	| 363 	| 225 	| 0.119 	| 0.100 	| 8 	| 0 	|
| OutlookTask 	| 351 	| 347 	| 0.038 	| 0.038 	| 0 	| 0 	|
| MicrosoftForms2_HtmlOption 	| 344 	| 137 	| 0.001 	| 0.001 	| 0 	| 0 	|
| Excel2013Macro 	| 331 	| 101 	| 1.023 	| 0.306 	| 0 	| 0 	|
| MicrosoftWordPicture 	| 316 	| 50 	| 0.020 	| 0.006 	| 0 	| 0 	|
| Visio2003 	| 297 	| 166 	| 0.258 	| 0.191 	| 0 	| 0 	|
| OutlookReport 	| 291 	| 291 	| 0.012 	| 0.012 	| 0 	| 0 	|
| Excel2003Xml 	| 274 	| 182 	| 0.030 	| 0.020 	| 0 	| 0 	|
| SQLiteDatabase3 	| 272 	| 251 	| 0.156 	| 0.148 	| 0 	| 0 	|
| MicrosoftForms2_CheckBox 	| 261 	| 147 	| 0.001 	| 0.001 	| 0 	| 0 	|
| MicrosoftGraph 	| 260 	| 43 	| 0.003 	| 0.001 	| 0 	| 0 	|
| OutlookSharing 	| 256 	| 242 	| 0.012 	| 0.012 	| 0 	| 0 	|
| AdobePDF_XFAEncrypted 	| 253 	| 38 	| 0.094 	| 0.011 	| 1 	| 0 	|
| CompressedWindowsMetafile 	| 244 	| 80 	| 0.010 	| 0.003 	| 0 	| 0 	|
| MSOneNote2010 	| 230 	| 230 	| 1.346 	| 1.346 	| 0 	| 0 	|
| Excel2003Encrypted 	| 226 	| 98 	| 0.189 	| 0.045 	| 0 	| 0 	|
| TiffBigEndian 	| 200 	| 96 	| 0.098 	| 0.061 	| 0 	| 0 	|
| Excel2016Binary 	| 185 	| 112 	| 0.200 	| 0.152 	| 0 	| 0 	|
| NonMp3WithID3v2Metadata 	| 185 	| 111 	| 0.043 	| 0.026 	| 0 	| 0 	|
| OutlookReportReadReceiptStandardMessage 	| 181 	| 181 	| 0.005 	| 0.005 	| 0 	| 0 	|
| Word97 	| 165 	| 124 	| 0.012 	| 0.011 	| 756 	| 0 	|
| PowerPoint2016SlideShow 	| 162 	| 101 	| 1.664 	| 1.123 	| 0 	| 0 	|
| PKCS7_SignedData_DER 	| 158 	| 147 	| 0.003 	| 0.003 	| 4 	| 0 	|
| WindowsExe32 	| 139 	| 107 	| 0.516 	| 0.482 	| 6 	| 139 	|
| PowerPoint2007 	| 136 	| 63 	| 0.144 	| 0.078 	| 0 	| 0 	|
| SQLFile 	| 136 	| 99 	| 0.001 	| 0.001 	| 0 	| 0 	|
| WindowsShortcut 	| 130 	| 72 	| 0.000 	| 0.000 	| 5 	| 0 	|
| WindowsDLL64 	| 125 	| 118 	| 0.084 	| 0.078 	| 7 	| 125 	|
| MPEG2ProgramStream 	| 120 	| 45 	| 1.235 	| 0.312 	| 0 	| 0 	|
| Mork 	| 118 	| 21 	| 0.151 	| 0.041 	| 0 	| 0 	|
| OutlookPSTUnicode 	| 111 	| 74 	| 730.926 	| 524.237 	| 0 	| 0 	|
| OutlookDistributionList 	| 110 	| 110 	| 0.004 	| 0.004 	| 0 	| 0 	|
| MimeOutlookEml 	| 102 	| 55 	| 0.017 	| 0.009 	| 0 	| 0 	|
| OutlookMessageRecall 	| 93 	| 91 	| 0.006 	| 0.006 	| 0 	| 0 	|
| Excel2010Binary 	| 89 	| 61 	| 0.063 	| 0.061 	| 0 	| 0 	|
| WebOpenFontFormat2 	| 88 	| 16 	| 0.003 	| 0.001 	| 0 	| 0 	|
| EncapsulatedPostScriptWithPreviewImage 	| 84 	| 28 	| 0.143 	| 0.064 	| 0 	| 0 	|
| MicrosoftXPS 	| 83 	| 58 	| 0.089 	| 0.073 	| 0 	| 0 	|
| MPEG1AudioLayer3_ID3v2 	| 81 	| 20 	| 0.366 	| 0.062 	| 0 	| 0 	|
| WindowsMultimediaASF 	| 81 	| 45 	| 2.389 	| 1.955 	| 0 	| 0 	|
| Text_Windows_1252 	| 78 	| 60 	| 0.003 	| 0.002 	| 2 	| 0 	|
| QuickBooksBackupFileCFD 	| 76 	| 39 	| 2.936 	| 1.504 	| 0 	| 0 	|
| WordPerfectGraphicsMetafile2 	| 76 	| 16 	| 0.001 	| 0.000 	| 0 	| 0 	|
| TnefEmail 	| 72 	| 43 	| 0.031 	| 0.011 	| 0 	| 0 	|
| XMP 	| 72 	| 55 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Excel2000 	| 71 	| 40 	| 0.006 	| 0.005 	| 0 	| 0 	|
| WindowsIcon 	| 69 	| 33 	| 0.008 	| 0.006 	| 169 	| 0 	|
| MHTML 	| 67 	| 48 	| 0.102 	| 0.092 	| 0 	| 0 	|
| MSOneNoteTOC2007 	| 67 	| 67 	| 0.001 	| 0.001 	| 0 	| 0 	|
| MicrosoftWordArt 	| 66 	| 3 	| 0.000 	| 0.000 	| 0 	| 0 	|
| TextUnicode16BE 	| 66 	| 38 	| 0.000 	| 0.000 	| 1 	| 0 	|
| MachObject64 	| 63 	| 63 	| 0.133 	| 0.133 	| 0 	| 63 	|
| MicrosoftEquationEditor3 	| 61 	| 61 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MSOneNote2010HTTP 	| 59 	| 59 	| 0.035 	| 0.035 	| 0 	| 0 	|
| Excel2013Template 	| 57 	| 11 	| 0.004 	| 0.001 	| 0 	| 0 	|
| Word2000Html 	| 56 	| 51 	| 0.001 	| 0.001 	| 25 	| 0 	|
| Excel2007Macro 	| 53 	| 19 	| 0.002 	| 0.001 	| 0 	| 0 	|
| ExcelMhtml 	| 53 	| 51 	| 0.004 	| 0.004 	| 0 	| 0 	|
| WebVTT 	| 53 	| 48 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MSPublisher2007to2016 	| 51 	| 31 	| 0.042 	| 0.026 	| 0 	| 0 	|
| OutlookRulesReplyTemplate 	| 50 	| 50 	| 0.001 	| 0.001 	| 0 	| 0 	|
| AccessDatabase2007to2016 	| 49 	| 44 	| 1.491 	| 1.465 	| 0 	| 0 	|
| iTunesAudio 	| 48 	| 20 	| 0.014 	| 0.006 	| 0 	| 0 	|
| Word2007Macro 	| 48 	| 16 	| 0.011 	| 0.004 	| 0 	| 0 	|
| Riff 	| 46 	| 26 	| 0.021 	| 0.014 	| 0 	| 0 	|
| Excel2007Corrupted 	| 45 	| 13 	| 0.109 	| 0.027 	| 0 	| 0 	|
| Excel2010Macro 	| 45 	| 19 	| 0.094 	| 0.034 	| 0 	| 0 	|
| HighEfficiencyImageFormat 	| 45 	| 42 	| 0.065 	| 0.060 	| 0 	| 0 	|
| MicrosoftForms2_HtmlSelect 	| 42 	| 41 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Visio2013 	| 42 	| 30 	| 0.028 	| 0.020 	| 0 	| 0 	|
| OutlookTemplateFile 	| 41 	| 20 	| 0.007 	| 0.005 	| 0 	| 0 	|
| WindowsExe64 	| 40 	| 32 	| 0.051 	| 0.050 	| 3 	| 40 	|
| ThumbsDB 	| 39 	| 26 	| 0.001 	| 0.001 	| 0 	| 0 	|
| WindowsMediaAudio 	| 39 	| 39 	| 0.007 	| 0.007 	| 0 	| 0 	|
| OutlookReportDeliveryStandardMessage 	| 38 	| 38 	| 0.001 	| 0.001 	| 0 	| 0 	|
| eFaxDocument 	| 37 	| 34 	| 0.004 	| 0.004 	| 0 	| 0 	|
| Mbox 	| 36 	| 6 	| 0.026 	| 0.004 	| 0 	| 0 	|
| ArchiveMSCompiledHelp 	| 33 	| 32 	| 0.007 	| 0.006 	| 0 	| 0 	|
| MachObject32 	| 33 	| 8 	| 0.005 	| 0.001 	| 0 	| 33 	|
| XmlPropertyList 	| 33 	| 18 	| 0.003 	| 0.002 	| 0 	| 0 	|
| ArchiveMSCab 	| 31 	| 29 	| 0.185 	| 0.184 	| 0 	| 31 	|
| ArchiveZipSplit 	| 31 	| 30 	| 0.028 	| 0.028 	| 0 	| 0 	|
| Excel2016Template 	| 31 	| 13 	| 0.008 	| 0.003 	| 0 	| 0 	|
| OpenTypeFont 	| 31 	| 17 	| 0.002 	| 0.001 	| 1 	| 0 	|
| Text_ISO_8859_1 	| 30 	| 21 	| 0.001 	| 0.001 	| 0 	| 0 	|
| OutlookRightsManagedEmailObject 	| 28 	| 24 	| 0.004 	| 0.004 	| 0 	| 0 	|
| PowerPoint2000Html 	| 28 	| 28 	| 0.000 	| 0.000 	| 1 	| 0 	|
| OutlookMissedCallNotification 	| 27 	| 27 	| 0.001 	| 0.001 	| 0 	| 0 	|
| LuaFile 	| 26 	| 26 	| 0.001 	| 0.001 	| 0 	| 0 	|
| PowerPoint2016Macro 	| 26 	| 8 	| 0.132 	| 0.023 	| 0 	| 0 	|
| Visio2016 	| 26 	| 20 	| 0.005 	| 0.005 	| 0 	| 0 	|
| MicrosoftForms2_HtmlSubmit 	| 25 	| 25 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MicrosoftForms2_OptionButton 	| 24 	| 4 	| 0.000 	| 0.000 	| 0 	| 0 	|
| RiffAVI 	| 23 	| 7 	| 0.335 	| 0.200 	| 0 	| 0 	|
| MicrosoftForms2_HtmlTextArea 	| 21 	| 17 	| 0.000 	| 0.000 	| 0 	| 0 	|
| OpenDocumentText11 	| 21 	| 9 	| 0.001 	| 0.000 	| 0 	| 0 	|
| EmptyCompoundFile 	| 20 	| 6 	| 0.017 	| 0.009 	| 551 	| 0 	|
| iWorkPages2013 	| 19 	| 12 	| 0.006 	| 0.004 	| 0 	| 0 	|
| MetafileOLE2Container 	| 18 	| 11 	| 0.017 	| 0.013 	| 0 	| 0 	|
| MicrosoftInstaller 	| 17 	| 17 	| 0.114 	| 0.114 	| 2 	| 0 	|
| OpenDocumentSpreadsheet11 	| 17 	| 8 	| 0.001 	| 0.000 	| 0 	| 0 	|
| X509_Certificate_PEM 	| 17 	| 7 	| 0.000 	| 0.000 	| 0 	| 0 	|
| ArchiveZipExe 	| 16 	| 4 	| 0.005 	| 0.004 	| 0 	| 0 	|
| OpenOfficeOrgWriter3 	| 16 	| 11 	| 0.005 	| 0.003 	| 0 	| 0 	|
| Word2016Template 	| 16 	| 3 	| 0.003 	| 0.001 	| 0 	| 0 	|
| Word95 	| 16 	| 5 	| 0.004 	| 0.004 	| 0 	| 0 	|
| PowerPoint2010Macro 	| 15 	| 6 	| 0.006 	| 0.004 	| 0 	| 0 	|
| PowerPoint2010SlideShow 	| 15 	| 4 	| 0.028 	| 0.012 	| 0 	| 0 	|
| Word2010Macro 	| 15 	| 12 	| 0.002 	| 0.001 	| 0 	| 0 	|
| ArchiveZipSplitSegment 	| 14 	| 14 	| 0.128 	| 0.128 	| 0 	| 0 	|
| ASPNetFile 	| 14 	| 12 	| 0.001 	| 0.000 	| 0 	| 0 	|
| CorelDraw8 	| 14 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Word2010Template 	| 14 	| 10 	| 0.003 	| 0.002 	| 0 	| 0 	|
| MSWorksWordProcessor4 	| 13 	| 12 	| 0.001 	| 0.001 	| 0 	| 0 	|
| Text_Windows_1250 	| 13 	| 13 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Word6 	| 13 	| 8 	| 0.000 	| 0.000 	| 0 	| 0 	|
| iTunesVideo 	| 12 	| 11 	| 0.149 	| 0.137 	| 0 	| 0 	|
| MicrosoftOfficeTheme 	| 12 	| 11 	| 0.011 	| 0.011 	| 15 	| 0 	|
| MSProject2010 	| 12 	| 8 	| 0.005 	| 0.004 	| 0 	| 0 	|
| WebPLossy 	| 12 	| 9 	| 0.001 	| 0.001 	| 0 	| 0 	|
| BinaryPropertyList 	| 11 	| 7 	| 0.001 	| 0.001 	| 0 	| 0 	|
| CorelDraw7 	| 11 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Excel2000Html 	| 11 	| 7 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MicrosoftForms2_TextBox 	| 11 	| 11 	| 0.000 	| 0.000 	| 0 	| 0 	|
| OutlookSMIME_MultipartSigned 	| 11 	| 10 	| 0.002 	| 0.002 	| 0 	| 0 	|
| OutlookTaskRequest 	| 11 	| 11 	| 0.001 	| 0.001 	| 0 	| 0 	|
| ArchiveArc 	| 10 	| 9 	| 0.001 	| 0.001 	| 6 	| 0 	|
| Text_EUC_KR 	| 10 	| 7 	| 0.014 	| 0.007 	| 0 	| 0 	|
| Word2003Xml 	| 10 	| 5 	| 0.003 	| 0.001 	| 0 	| 0 	|
| CorelPresentationsDrawing9 	| 9 	| 3 	| 0.000 	| 0.000 	| 0 	| 0 	|
| EncapsulatedPostScript 	| 9 	| 4 	| 0.020 	| 0.015 	| 0 	| 0 	|
| ArchiveTar 	| 8 	| 4 	| 0.835 	| 0.834 	| 0 	| 0 	|
| Excel5 	| 8 	| 8 	| 0.014 	| 0.014 	| 0 	| 0 	|
| MacPICTBinary 	| 8 	| 7 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MicrosoftWordPicture95 	| 8 	| 1 	| 0.002 	| 0.000 	| 0 	| 0 	|
| Word2007Template 	| 8 	| 4 	| 0.001 	| 0.001 	| 0 	| 0 	|
| Word2013Template 	| 8 	| 6 	| 0.004 	| 0.003 	| 1 	| 0 	|
| AccessDatabase2003 	| 7 	| 4 	| 0.006 	| 0.005 	| 0 	| 0 	|
| AdobeInDesign 	| 7 	| 6 	| 0.017 	| 0.015 	| 0 	| 0 	|
| AppleAssetCatalog 	| 7 	| 4 	| 0.001 	| 0.001 	| 0 	| 0 	|
| ArchiveArUnix 	| 7 	| 7 	| 0.004 	| 0.004 	| 0 	| 0 	|
| ArchiveCpio 	| 7 	| 4 	| 0.179 	| 0.177 	| 0 	| 0 	|
| ArchiveRar5 	| 7 	| 7 	| 0.001 	| 0.001 	| 0 	| 0 	|
| ArchiveWinAce 	| 7 	| 5 	| 0.005 	| 0.004 	| 0 	| 0 	|
| Excel2007Binary 	| 7 	| 5 	| 0.012 	| 0.012 	| 0 	| 0 	|
| Excel4 	| 7 	| 4 	| 0.001 	| 0.001 	| 0 	| 0 	|
| MediaImageDmg 	| 7 	| 7 	| 0.057 	| 0.057 	| 0 	| 0 	|
| MediaImageHfsPlus 	| 7 	| 7 	| 0.088 	| 0.088 	| 0 	| 0 	|
| MediaImageMasterBootRecord 	| 7 	| 7 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MPEG2AudioLayer3_ID3v2 	| 7 	| 2 	| 0.001 	| 0.000 	| 0 	| 0 	|
| OpenDocumentSpreadsheet12 	| 7 	| 5 	| 0.004 	| 0.001 	| 0 	| 0 	|
| PowerPoint2007OnwardEncrypted 	| 7 	| 5 	| 0.007 	| 0.005 	| 0 	| 0 	|
| SMimeGenericClearSigned 	| 7 	| 4 	| 0.001 	| 0.000 	| 0 	| 0 	|
| Word2007OnwardEncrypted 	| 7 	| 5 	| 0.000 	| 0.000 	| 0 	| 0 	|
| ApacheOpenOfficeWriter4 	| 6 	| 4 	| 0.000 	| 0.000 	| 0 	| 0 	|
| ArchiveLzhExe 	| 6 	| 5 	| 0.169 	| 0.155 	| 1 	| 0 	|
| ArchiveXar 	| 6 	| 3 	| 0.098 	| 0.097 	| 0 	| 0 	|
| DotNetAssembly32 	| 6 	| 5 	| 0.017 	| 0.017 	| 0 	| 6 	|
| ExcelCompoundFileCorrupted 	| 6 	| 3 	| 0.001 	| 0.000 	| 0 	| 0 	|
| MPEG2AudioLayer2 	| 6 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| OleLinkedObject 	| 6 	| 5 	| 0.000 	| 0.000 	| 0 	| 0 	|
| OutlookFileAttachment 	| 6 	| 3 	| 0.003 	| 0.001 	| 0 	| 0 	|
| WebPExtended 	| 6 	| 6 	| 0.002 	| 0.002 	| 0 	| 0 	|
| Word2013Macro 	| 6 	| 4 	| 0.004 	| 0.001 	| 0 	| 0 	|
| AdaptiveMultiRateAudio 	| 5 	| 5 	| 0.000 	| 0.000 	| 0 	| 0 	|
| iWorkPages 	| 5 	| 4 	| 0.002 	| 0.002 	| 0 	| 0 	|
| OpenDocumentChart12 	| 5 	| 5 	| 0.000 	| 0.000 	| 0 	| 0 	|
| OpenOfficeOrgCalc3 	| 5 	| 3 	| 0.000 	| 0.000 	| 0 	| 0 	|
| StarViewMetafile 	| 5 	| 5 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Text_EBCDIC_500 	| 5 	| 3 	| 0.001 	| 0.001 	| 0 	| 0 	|
| TextEmail_Windows_1252 	| 5 	| 3 	| 0.000 	| 0.000 	| 0 	| 0 	|
| TypeLibraryFile 	| 5 	| 3 	| 0.000 	| 0.000 	| 0 	| 0 	|
| WordPerfect6toX8 	| 5 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| AccessDatabase97 	| 4 	| 1 	| 0.001 	| 0.000 	| 0 	| 0 	|
| AppleIcon 	| 4 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Archive7Zip 	| 4 	| 4 	| 0.017 	| 0.017 	| 0 	| 0 	|
| DOSExe16 	| 4 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| InstallShieldCAB 	| 4 	| 4 	| 0.004 	| 0.004 	| 0 	| 0 	|
| LibreOfficeWriter3 	| 4 	| 4 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MicrosoftOfficeOwnerFileOLE 	| 4 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| OpenDocumentText12 	| 4 	| 4 	| 0.000 	| 0.000 	| 0 	| 0 	|
| OpenOfficeOrgCalc2 	| 4 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Text_Big5 	| 4 	| 4 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Text_GB18030 	| 4 	| 4 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Text_Shift_JIS 	| 4 	| 4 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Text_Windows_1251 	| 4 	| 4 	| 0.000 	| 0.000 	| 0 	| 0 	|
| TextEmail7BitAscii 	| 4 	| 3 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Visio2000 	| 4 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Word2016Macro 	| 4 	| 3 	| 0.003 	| 0.002 	| 0 	| 0 	|
| CorruptedCompoundFile 	| 3 	| 2 	| 0.056 	| 0.037 	| 0 	| 0 	|
| LibreOfficeCalc3 	| 3 	| 3 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MicrosoftForms2_HtmlImage 	| 3 	| 3 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MiniDumpFile 	| 3 	| 3 	| 0.002 	| 0.002 	| 0 	| 0 	|
| OrgPlus 	| 3 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| PowerPoint2016Template 	| 3 	| 2 	| 0.010 	| 0.006 	| 0 	| 0 	|
| SMimeEmailClearSigned 	| 3 	| 3 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Text_ISO_8859_2 	| 3 	| 3 	| 0.000 	| 0.000 	| 0 	| 0 	|
| WordCompoundFileCorrupted 	| 3 	| 3 	| 0.000 	| 0.000 	| 0 	| 0 	|
| AutoCADDrawing2013 	| 2 	| 2 	| 0.011 	| 0.011 	| 0 	| 0 	|
| CertificateRevocationList_DER 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| CorelDraw12 	| 2 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| CorelDraw9 	| 2 	| 1 	| 0.019 	| 0.009 	| 0 	| 0 	|
| EmbeddedOpenTypeFont 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| ESEDatabase 	| 2 	| 2 	| 0.004 	| 0.004 	| 0 	| 0 	|
| FileMakerPro5 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| iWorkKeynote 	| 2 	| 1 	| 0.012 	| 0.006 	| 0 	| 0 	|
| JavaArchive 	| 2 	| 2 	| 0.004 	| 0.004 	| 0 	| 0 	|
| KeePassPasswordDatabase 	| 2 	| 2 	| 0.036 	| 0.036 	| 0 	| 0 	|
| Lotus123Worksheet3 	| 2 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| LotusNotesStorageFacility 	| 2 	| 1 	| 0.035 	| 0.017 	| 0 	| 0 	|
| MarkdownFile 	| 2 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MediaImageISO 	| 2 	| 2 	| 0.004 	| 0.004 	| 0 	| 0 	|
| MicrosoftDocumentImagingFileFormat 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MSOneNotePackage 	| 2 	| 2 	| 0.011 	| 0.011 	| 0 	| 0 	|
| MSProject2013 	| 2 	| 2 	| 0.001 	| 0.001 	| 0 	| 0 	|
| OpenDocumentPresentation12 	| 2 	| 2 	| 0.001 	| 0.001 	| 0 	| 0 	|
| OpenOfficeOrgImpress3 	| 2 	| 2 	| 0.001 	| 0.001 	| 0 	| 0 	|
| OutlookTaskRequestAccept 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| PKCS12 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| PowerPoint2007SlideShow 	| 2 	| 1 	| 0.013 	| 0.006 	| 0 	| 0 	|
| PowerPoint2013Macro 	| 2 	| 2 	| 0.002 	| 0.002 	| 0 	| 0 	|
| PythonScriptFile 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| SqlServerDatabaseFile 	| 2 	| 2 	| 0.695 	| 0.695 	| 0 	| 0 	|
| StarOfficeWriter6to7 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Text_ANSI8 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Text_ISO_2022_KR 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Text_ISO_8859_9 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| WebM 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Word2013TemplateMacro 	| 2 	| 2 	| 0.000 	| 0.000 	| 0 	| 0 	|
| AccessDatabaseTemplate2007 	| 1 	| 1 	| 0.002 	| 0.002 	| 0 	| 0 	|
| AccessSnapshot 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| ArchiveRar4 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| ArchiveRar5Exe 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| AutoCADDrawing2010 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| AVIFImageFile 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| CertificateSigningRequest_PEM 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| CHeaderFile 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| CorelDraw5 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| CrystalReports 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| CSharpFile 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| DataInterchangeFormat 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| DotNetAssembly64 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 1 	|
| Excel2 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Excel3 	| 1 	| 1 	| 0.000 	| 0.000 	| 3 	| 0 	|
| MacPICT2 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MediaImageHfs 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MicrosoftForms2_HtmlReset 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MimeGeneric 	| 1 	| 1 	| 0.001 	| 0.001 	| 0 	| 0 	|
| MindManagerMapFile 	| 1 	| 1 	| 0.001 	| 0.001 	| 0 	| 0 	|
| MSOneNoteMhtml 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| MSWorksSpreadsheet6to9 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| OpenDocumentPresentation11 	| 1 	| 1 	| 0.001 	| 0.001 	| 0 	| 0 	|
| OpenDocumentText1 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| OS2GenericExe 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 1 	|
| PKCS7_Certificate_DER 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| PowerPoint2007Corrupted 	| 1 	| 1 	| 0.001 	| 0.001 	| 0 	| 0 	|
| PowerPoint2016SlideShowMacro 	| 1 	| 1 	| 0.003 	| 0.003 	| 0 	| 0 	|
| PowerPoint97Encrypted 	| 1 	| 1 	| 0.002 	| 0.002 	| 0 	| 0 	|
| Text_EUC_JP 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Text_Windows_1254 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| TextAppointment_Windows_1252 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| TextUnicode32LE 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| VectorWorks 	| 1 	| 1 	| 0.001 	| 0.001 	| 0 	| 0 	|
| WindowsRegistryFile 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| Word2010TemplateMacro 	| 1 	| 1 	| 0.000 	| 0.000 	| 0 	| 0 	|
| ArchiveZipEmpty 	| 0 	| 0 	| 0.000 	| 0.000 	| 5 	| 0 	|



### Image 4: Screen Shot of Collection (Matter) Workflow Tasks
WMS breaks collection input documents and containers into manageable tasks for the distributed workers. The screen shot below was captured while processing this collection and is the collection's user interface 'Tasks' view.

![TaskViewDuringProcessingView1](https://github.com/dotfurther/Open-Discover-WhitePaper-1/assets/52750989/fc635062-0cc3-4725-8dee-8dcdd6a742fd)


### Image 5: Workers Make Efficient Usage of VM Resources
While processing this collection we observed that the Worker VMs were making very good use of the allocated resources.

![VMWorkerCpuUsage](https://github.com/dotfurther/Open-Discover-WhitePaper-1/assets/52750989/937c7a74-3178-434d-bd95-2c799df3a5bb)




