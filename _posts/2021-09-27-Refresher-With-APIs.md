---
layout: post
title: Refresher with APIs
subtitle: Working with the Google Cloud Platform to finish PyNance
---

It's been a while since the last entry. Unfortunately, COVID-19 has somewhat destroyed any 
sense of continuity to my professional schedule. This has somewhat led to burn out and a 
severe lack of motivation to work on anything that wasn't related to my job. But! All of 
these things eventually get better, and I'm finally out of lockdown and able to get back 
to a somewhat "normal" routine (as far as wearing masks and that has become the new normal).


In the meantime, I started working on a small project called PyNance. This project is a 
budget tracking app with an emphasis on complete automation. The goal is for the program to 
grab payslips on a fortnightly basis from a specified Gmail address and process the data. Once 
the required data has been extracted, it is all pushed to a companion Google Spreadsheet that 
outlines my current budget recommendations based on specified values. 

The project started off pretty easy, with the creation of the PDF parser. This was for the most 
part pretty seamless, and just a case of scraping the PDF and indexing data based on known 
keywords within the payslips.

The harder bit came to push data to the spreadsheet. I remembered way back when working with the 
GSheets API, but couldn't remember the flow of the API. As it turns out, it's a relatively stock 
standard API with a token system. Once the session/token handler was completed, the sheet handler 
was written up. It uses the token, a specified spreadsheet ID (which I've had to make secret 
for privacy/security reasons on GitHub), and a GSheet scope. It is important to note that if the 
scopes change, a new token needs to be generated. This will become important later when I add Google 
scopes to enable PyNance to access Gmail. 

The sheet is still relatively unpopulated, but the data transfer works. The next step of the process 
is to build the Gmail handler. This is proving harder than I thought, considering the docs are far more 
sparse (at least in regard to any python specific syntax/methods). I'm running into various 
unexpected key errors on the objects I'm receiving back from Gmail which is an issue. Looks like 
I've got some more reading to do. Another feature that is in the back of my mind that I'm not entirely 
sure how to solve is the scheduling of the program. I've tried a few 3rd party tools that already 
exist - including an AppScript applet that is attached to a Google Sheet. This allows for scheduled 
scraping of a Gmail Inbox and grab attachments. The only issue: It's a paid service for any more than 
5 emails at a time, which knowing my inbox will be a definite issue. A second tool developed by
[autiomate.io](https://automate.io/) allows a user to create a "Bot" of sorts that uses a drag and 
drop system for customizable inputs and outputs. The catch? Once again limited API use and the 
complete lack of support for Gmail attachments. This was a bit of a blow, since after hours of 
searching there doesn't seem to be a clear-cut solution to my automation problem. This is something 
I've put on the backbone for now and will come back to later.