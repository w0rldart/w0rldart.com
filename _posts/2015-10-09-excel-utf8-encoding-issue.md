---
layout: post
title: "CSV UTF-8 encoding issues in Excel"
tags: encoding csv hacks
---

There's a bug which causes some accented characters to become corrupt before / during / after import or just document edit, when you use Microsoft Excel – this is a known Microsoft bug which has never been fixed.
This frustrating csv UTF-8 encoding issue, occurs only with Microsoft Excel.

The solution is use either [OpenOffice](https://www.openoffice.org/download/index.html) or Google Docs' Spreadsheet, to load the document and force save as a CSV file with the right encoding.

## Step-by-step for OpenOffice

1. Open your source data in Open Office (Spreadsheet), if it’s in CSV format (text, comma delimited) you will be prompted to select character encoding. _By default, it’s set to ‘Western European’_.
Check the data in the preview window to see if accented characters look OK, if they do… jump to step **3.**

2. If the characters do look corrupted (unexpected capitals or weird symbols in place of lower case accented characters, etc) you will need to change the character encoding. This will usually just be a change to UTF-8 (not UTF-7) – but check the accented characters!

3. Click OK and import the data, it should open and look like a normal spreadsheet.

4. Now proceed to save it in a correctly encoded format by choosing to **Save As** (Ctrl + Shift + S) and choosing _Text CSV (.csv)_ from the _formats_ drop down, just above the save button, and sticking to _"Use Text CSV Format"_ in the following prompt box.

5. Done!

This csv UTF-8 encoding issue seems to be fairly common where data imports / exports are prevalent between SaaS systems from different suppliers.
