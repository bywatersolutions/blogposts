# The Koha Search System

Koha has a broad, powerful search system that affects many parts of the
ILS. I've recently been working to add Zebra search indexes and custom
XSLT transformations, and a broad overview of how all the pieces fit
together can often be helpful.

What does the koha search system do? In broad terms, it takes
bibliographic data stored in marcxml format and stores it in a special
database so that it can be quickly retrieved by a user. The data must
then be written back to Koha so that it can be displayed on screen. The
same database is also used when de-duplicating bibliographic data
during import.

The database is indexed, which aids lookup speed and accuracy.

## Bibliographic data

Information search-able in Koha is stored in a couple of
places. Bibliographic information is stored in a table  `biblio_metadata`
in a field called `metadata`. Item information is stored in the `items` table,
then added to the bibliographic data before it is indexed.

## The search engine and database

Koha currently uses a search engine called _Zebra_, written by 
[Index Data](https://www.indexdata.com/resources/software/zebra/). Zebra
handles MARC data well, and is built around Z39.50. The Koha community
is also working to use [Elastic Search](https://en.wikipedia.org/wiki/Elasticsearch)
as its search engine. This is in beta release as of Koha 16.11. This
write up will document how koha search is configured via Zebra, but
I will also show where Elastic Search fits into the system.
