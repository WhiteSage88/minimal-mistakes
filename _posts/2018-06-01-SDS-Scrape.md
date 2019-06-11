---
layout: posts
title: "SDS Scrape"
tags: Scrapy Selenium
---
#SDS Scrapy

Link to script Github:
<br><link>https://github.com/WhiteSage88/sds_scrape</link>
<br>
<h1>SDS Scrape for NJ RTK Compliance</h1> 

Beginning in March 2018, Stockton University recieved a surprise inspection by the New Jersey Right-to-Know (NJ-RTK) department
of the Department of Health. This deparment insures compliance of institutions in relation to workers and their environments.
I was to oversee what corrections needed to be done to be in or exceed compliance in the Unified Science Center, F-Wing and 
The Arts and Sciences Building. We were not in risk of being cited but recommendations were given to be corrected by the time 
the inspector returned. Most of them were just "Label XYZ chemical in this location" which was a small recommendation since all
it took was just writing up a label on the spot. The big project which I wrote a program for was to make sure EVERY SDS binder
in EVERY room in ALL buildings was filled with the chemicals each room housed.
<br>
This was a huge undertaking because not only did we need to check every binder but also locate the SDS for every chemical on
file.

<h3>Background Info</h3>

Safety Data Sheet(SDS) is a multi-page report for every chemical any manufacturer produces.
This was printed out by either the manufacturer and sent with the chemical or printed out by the reciever here at Stockton if 
the SDS wasn't supplied. 
<br>
Our inventory system at the time was Vertere. We purchased the system around 2006 and had been using it to tag and store information
of any chemical that came and left at Stockton. 
<h3>Purpose/Design</h3>

I had previous experience using <b>Scrapy</b> and <b>Selenium</b>. <b>Scrapy</b> is a <b>Python</b> scriptable scraper for HTML whileas <b>Selenium</b> is a scraper for Javascript enabled websites. With these tools, I decided that manually finding and downloading hundreds of files without redundancy (meaning there is no need to download the same sds again if it is in another room but a human might miss this) and be able to organize them for every room was going to take a lot of man-hours. I designed a script that would take all unique entries of "name, CAS number, manufacturer" and download those SDSs. Then the final script will read every chemical in each room and create a folder with copied SDS PDFs. This in turn will create a database of all SDSs for each room. 
<h3>Scrapy/Selenium</h3>
Using what I knew from Selenium, I had to be able to create a Spider for every website that the SDSs resided on. I created spiders for websites that we had greater than 25 unique chemicals for. I then ran the scripts to download the PDFs, rename them and save them in a central folder. This took a couple of hours but since it was automated I left it running overnight.
<br>
After that, I wrote a script that compared the inventroy file and created folders for every room, it then copied the files into each room folder. I then printed each room folder and organized it physically into every room. 
<h3>Conclusion</h3>
After doing the manual labor portion of the project (which took two days), the script was archived incase it was needed again in the future. We remained in compliance and exceeded the expectations of the inspector and I received recognition from the Risk Management Department on campus for my programming skills. 

