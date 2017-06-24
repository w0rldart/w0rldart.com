---
layout: post
title: "CSV UTF-8 encoding issues in Excel"
date: 2015-10-09 16:31:01 -0600
categories: encoding hacks
---

This frustrating csv UTF-8 encoding issue, most of the times, occurs after using Microsoft Excel for your CSV file, and that is due to a bug which causes some accented characters to become corrupted before / during / after import or just document edit (depending on when you use Microsoft Excel) – this is a known Microsoft bug which has never been fixed.   The solution is to either install and use [OpenOffice](https://www.openoffice.org/download/index.html) or Google Docs' Spreadsheet function for smaller files, load the document and force save as a CSV file with the right encoding.   **Step-by-step for OpenOffice**

1.  Open your source data in Open Office (Spreadsheet), if it’s in CSV format (text, comma delimited) you will be prompted to select character encoding. By default, it’s set to ‘Western European’. Check the data in that window to see if accented characters look OK, if they do… proceed to step **3.**
2.  Assuming the characters look corrupted (unexpected capitals or weird symbols in place of lower case accented characters, etc) you will need to change the character encoding. This will usually just be a change to UTF-8 (not UTF-7) – but check the accented characters!
3.  Click OK and import the data, it should open and look like a normal spreadsheet.
4.  Now proceed to save it in a correctly encoded format by choosing to **Save As **(Ctrl + Shift + S) and choosing _Text CSV (.csv) _from the _formats_ drop down, just above the save button, and sticking to _"Use Text CSV Format" _in the following prompt box.
5.  Done!

This csv UTF-8 encoding issue was first discovered by my friend Alex, whilst transferring data between event registration systems (including one run by GES, a large event services firm) and Silverpop (now called IBM Silverpop Engage, part of the IBM marketing cloud), but the problem seems to be fairly common where data imports / exports are prevalent between SaaS systems from different suppliers. _In collaboration with Alex Lawrence-Berkeley_
