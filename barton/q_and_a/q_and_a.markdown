## Acquisitions

_How do I merge records?_

Go to the cataloging search and search for the records you want to merge. Select the records you want to merge. Select which record you want to be the main record and then select the fields you want to merge. 

_How do I view patron suggestions from multiple branches?_

On the suggestions page, click the "Acquisition information" heading, then select "Any" under the "For" pulldown. Click "Go" 

## Batch Deletions

_How can I delete Ebook records in batch?_

If you have a file of urls or 'bookids' from your vendor, we may be able to run a script to delete these bibs for you.  Send us the file you received from the vendor! 

## Cataloging

_How do I move an order from one bib to another?_

You have to transfer the item from one bib to another. Go the catalog, find the bib you want to transfer to and click edit. Then, click Attach item and enter the barcode for the item you want to transfer. 

_Where are the material types set that control the icons in the OPAC?_

Marc leader and marc 008. See <http://manual.koha-community.org/16.11/en/cataloging.html#basicatalog>

_Why aren't item records being created when I stage MARC records for import?_

The location used in the MARC modification template didn't use the branch code. 

_Why am i prompted to enter a duedate when checking an item out?_

Check circ rules for hard duedate 

## Cronjobs

_Can I have headers added to the cron reports sent to me?_

Yes, you can using a select and union of the report.  Example: 


    SELECT
        "Current" AS "Current",
        "Cost total" AS "Cost total",
        "Replacement total" AS "Replacement total","Total" AS "Total"UNION ALL
    SELECT
        'Current',
        SUM(price) AS "Cost total",
        SUM(replacementprice) AS "Replacement total",SUM(price)+SUM(replacementprice) AS "Total"
    FROM
        items 

_Items are not moving from cart, why not?_

a) check that permanent location is not set to CART b) check cron job. 

_Why aren't membership expiry notices being sent?_

The MEMBERSHIP\_EXPIRY notice is sent by the membership\_expiry.pl cron job. If this isn't running, the notices will not be sent. 


## OPACUsersJS

_Why is the opac URL being prepended to the Online Resource link?_

If 856$u doesn't start with `http://`, Koha will prepend OpacBaseURL. 

_(EDS) What is the field/subfield that contains a persistent, unique identifier for each record?_

This depends on whether they're looking at bibliographic data or item level (holdings) data. The primary key for bibliographic data is stored in MARC 999$c. The primary key for item data is stored in MARC 952$9 

## Fines/Accounts

_When checking out an item, Why is the due date is prior to the checkout date?_

In the circulation and fine rule table, check to see if this item type and patron category have a hard due date set - and if this date is in the past. 

## Holds

_How do I keep a patron from placing a hold on an item they already have checked out?_

Set AllowHoldsOnPatronsPossessions to "Don't allow" 

_Number of reserves that can be placed does not respect circulation rules?_

Check MaxReserves system preference. 

## Itiva

_Hold notices are going out by phone, but overdue notices are not. Why not?_

Make sure that 'phone' is checked in the overdue notice triggers. 

## jquery

_How do I create a search widget in a separate web page?_

See <http://bywatersolutions.com/2011/11/28/embedable-koha-search-form/>

## LDAP/SAML

_Why am I seeing an LDAP software error?_

You must define a username and password block, even if you aren't using them 

## Notifications

_How do I turn  ACCTDETAILS notices on and off?_

This is controlled by the syspref AutoEmailOPACUser which can be set to "send" or "don't send". 

## OPAC

_How do I hide the DDC classification on the OPAC?_

There needs to be some jquery added to your opacuserjs system preference to hide this:

    $(document).ready(function(){  $(".ddc").hide();}); 

_Why am I getting a cover image for a different title?_

The ISBN may be wrong; look up the ISBN in google and make sure that the title found matches the bib title. 

## Patrons

_how do I delete a patron?_

Go to 'more' menu on patron menu-bar on all patron screens. 

_Why isn't Koha detecting duplicate patrons?_

Koha detects duplicate patrons using Firstname, Surname and Date of Birth. Unless all three are an exact match, the patron isn't considered a dup. 

## Plugin System

_How do I change coverflow style?_

In the 'mapping' section of the coverflow configuration, change 'style:' from 'coverflow' to 'carousel', 'wheel' or 'flat'. 

## Production Data Changes

_Batch editing 30,000 items 1000 at a time is going to take forever. Can I make that faster?_

The system preference MaxItemsToProcessForBatchMod can be increased from 1000. Probably best to re-set it to 1000 when done with a large batch. 

_How do I fix the "stacked page numbers" on the staged marc management page?_

Look at [bug 19643: Pagination buttons on staged marc management are stacking instead of inline](https://bugs.koha-community.org/bugzilla3/show_bug.cgi?id=19643#c1) -- add that CSS to intranetusercss syspref.

## Reports

_How do I generate CSV/TSV for report that's too big to run from the staff client?_

Bywater staff can Log into the server, run `runreport.pl` from the command line.

## Reports - Cataloging

_How do I delete ebooks?_

Migrations has a script to do this, to do this by book\_id. (If there are no book\_ids, migrations will have to take care of this on a case by case basis) 

## Reports - Circulation

_Where can partners go to learn SQL Reports?_

W3schools SQL tutorial: <http://www.w3schools.com/sql/>

This isn't koha specific but once you get the basic structure you can use the Koha reports library and the Koha schema to begin to build out reports.

<https://wiki.koha-community.org/wiki/SQL_Reports_Library>

<http://schema.koha-community.org/>

Taking reports from the library and making changes is a good way to learn too. The basic syntax for a report is a combination of three statements:

1 - `SELECT`: This is where you define the information you want, and you determine the name of a column by checking the table in the schema e.g. for biblio look here: 

* <http://schema.koha-community.org/master/tables/biblio.html>
* find column names: author, title, notes, biblionumber...

2 - `FROM`: This is where you name the table you are using, see schema for tables available (e.g. biblio, borrowers, issues, etc.)

3 - `WHERE`: These are conditions on the data. E.g. 'Items that are checked out' translates to items.onloan IS NOT NULL

## Searching

_The availablity numbers don't look right on the search results screen, why not?_

There is a limit to how many items Koha will check in search results -- look at the system preference MaxSearchResultsItemsPerRecordStatusCheck 

## Searching

_Why am I getting an internal server error when I search?_

The item homebranch or holdingbranch is set to a branchcode that is not defined in the branches table.

Or

`items.onloan` is set to `0000-00-00 00:00:00`.

_Why do some items show as available and checked out in search results at the same time?_

Who an item was checked out to is stored in the 'issues' table in Koha. The due date is also stored in `items.onloan`, which sometimes gets st to null.

_Why don't bibs with lots of items (e.g. serials) show in zebra search?_

This is caused by [Bug 15399 MARCXML records larger than 1 MB (1048576 bytes) are not searchable](https://bugs.koha-community.org/bugzilla3/show_bug.cgi?id=15399)

Bywater has some work-arounds for this.

_I have an bib that has many items attached, why do I only see 20 in the search results?_

This is controlled by the maxItemsInSearchResults system preference. 

## Slips and Notices

_Cost savings on ISSUESLIP?_

<http://bywatersolutions.com/2015/09/21/koha-receipt-cost-savings/>

_Item information missing in Advance Notices?_

<http://bywatersolutions.com/2016/02/01/proper-koha-notice-syntax/>

## Staff Client

_How do I set up item types in purchase suggestions?_

See <http://bywatersolutions.com/2017/05/17/purchase-suggestions-opac/>

## System Administration

_Why are we getting 503 errors when navigating to staff / opac urls?_

This may be an indication that the site is IP restricted/ip address has changed. 

## System Error

_Why do I get a system error when deleting a patron?_

Check to make sure that the patron has card number populated. 

## Tools

_Why do I get "Upload status: Failed" on stage-marc-import.pl?_

The file `/tmp/koha\_*\_upload` must be owned by the instance user.

