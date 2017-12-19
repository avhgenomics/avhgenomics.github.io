---
layout: page
title: PrimerDB
subtitle: Increasing the functionality and accessibility of primer storage
googlefonts: ["Raleway"]
bigimg: /img/adv_planet_initial_scene.png
published: yes
---

# What is PrimerDB?

[PrimerDB](https://www.avonhan.shinyapps.io/primer_dashboard/) is a project I started developing when our group began the process of updating / reorganizing our collection of primers.  As time goes on, tubes inevitably get placed in wrong locations, etc. PrimerDB was made to create an easy to search, efficient database that can be updated in a streamlined format.


Many labs utilize an Excel file to maintain these records, which is ideal for several that are not tech-savvy or lack the access to true database systems (SQL, etc).


To this end, PrimerDB was made to address this.  While not a true database, this can utilize a variety of different functions in Shiny to generate lists, quickly find what people are looking for, and provide additional statistics about the health of the current DB; all while working off a local csv file.


# Why use PrimerDB instead of Excel?

Excel is an acceptable solution to maintaining a record of primer locations; however, spreadsheets have several shortcomings.

* **Propagated Mistakes.** Small mistakes in larger datasets may go unnoticed, leading to inaccurate records.  (copy/pasting part of a record in a different location for instance)


* **Resources.**  While a spreadsheet containing 5,000 primers is not going to show too much wear on the system, Excel does not handle larger datasets well.


* **Ease of use.** Excel is easy for someone to use; but it is inefficient when trying to obtain a list of specific criteria to save.  You could use pivot tables, but that is a relatively non-intuitive approach and not so user-friendly.


PrimerDB works using R and Shiny to query a csv file containing a list of all primers with their information.  This addresses the issues above by:

* **Acting on datasets as a whole.** Users do not individually access and alter variables in the program.  Additionally, the original csv file is not touched after being loaded, so any mistakes or alterations that are made do not get propagated beyond the session.


* **Resource Efficient.** R is a statistical computing language that is much more efficient at querying larger datasets than Excel.  While "big data" still has better alternatives, a data frame of 5000 observations is extremely manageable in R.  


* **Easy to use.** To be a viable alternative, PrimerDB has to offer usability and functionality that exceed what Excel can perform.  This is done through the implementation of common functions to address why a user would be accessing the database in the first place.  By thinking from the user perspective, functions can be implemented that highlight primer locations with errors, locate free space available for new primers, quickly generate a printable list for sets of primers, and additionally provide other statistics or meaningful information for a given set of primers.


[Check out the PrimerDB!](https://www.avonhan.shinyapps.io/primer_dashboard/)

(Example csv sets to be provided soon.)
