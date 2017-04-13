# Relationships between tables in Koha

In an ongoing effort to help our partners write reports in Koha, I'd like to highlight the relationships between database tables in Koha.


## A review of the basics of SQL

Just for review, let's look at some elementary SQL. Let's say that we have two tables, `person` and `person_dates`. The `person` table contains an id number, the person's first name, and the person's surname, like so:

    +----+---------+-----------+
    | id | surname | firstname |
    +----+---------+-----------+
    | 1  | Doe     | John      |
    | 2  | Doe     | Jane      |
    +----+---------+-----------+

The `person_dates` table has its own id number, but also contains a `person_id` number that refers to the id number
from the `person` table:

    +----+-----------+---------------+------------+
    | id | person_id | label         | date       |
    +----+-----------+---------------+------------+
    | 1  | 1         | Date of Birth | 1970-11-22 |
    | 2  | 1         | Married       | 2004-05-12 |
    | 3  | 2         | Married       | 2004-05-12 |
    | 4  | 2         | Date of Birth | 1975-12-26 |
    +----+-----------+---------------+------------+

We can look up all of the values for `firstname` in the `person` table by using a `SELECT` statement:

    SELECT
        firstname
    FROM
        person

Which would give us

    +-----------+
    | firstname |
    +-----------+
    | John      |
    | Jane      |
    +-----------+

The sections in a SQL query are known as 'clauses'. Fields in the SELECT clause are separated by commas. If we want to see both the first name and the surname, we can write

    SELECT
        firstname,
        surname
    FROM
        person

Which would give us

    +-----------+---------+
    | firstname | surname |
    +-----------+---------+
    | John      | Doe     |
    | Jane      | Doe     |
    +-----------+---------+

If we only want to see that information for Jane Doe, we can use a `WHERE` clause:

    SELECT
        firstname, surname
    FROM
        person
    WHERE
        firstname = 'Jane'

    +-----------+---------+
    | firstname | surname |
    +-----------+---------+
    | Jane      | Doe     |
    +-----------+---------+

Running queries on a single table doesn't give us much information... who are John and Jane Doe? What's their relationship? Inquiring minds want to know! Taking a page from the `SELECT` clause, let's list the tables separated by commas:

    SELECT
        *
    FROM
        person,
        person_dates

    +----+---------+-----------+----+-----------+---------------+------------+
    | id | surname | firstname | id | person_id | label         | date       |
    +----+---------+-----------+----+-----------+---------------+------------+
    | 1  | Doe     | John      | 1  | 1         | Date of Birth | 1970-11-22 |
    | 1  | Doe     | John      | 2  | 1         | Married       | 2004-05-12 |
    | 1  | Doe     | John      | 3  | 2         | Married       | 2004-05-12 |
    | 1  | Doe     | John      | 4  | 2         | Date of Birth | 1975-12-26 |
    | 2  | Doe     | Jane      | 1  | 1         | Date of Birth | 1970-11-22 |
    | 2  | Doe     | Jane      | 2  | 1         | Married       | 2004-05-12 |
    | 2  | Doe     | Jane      | 3  | 2         | Married       | 2004-05-12 |
    | 2  | Doe     | Jane      | 4  | 2         | Date of Birth | 1975-12-26 |
    +----+---------+-----------+----+-----------+---------------+------------+

Well... that doesn't look quite right... we've got two people, but four lines that say `Date of Birth`. Unless John and Jane Doe have the rather exceptional quality of being born twice, there's a problem with our logic.

Let's do a bit of trouble shooting...

    SELECT
        *
    FROM
        person_dates;
    
    +----+-----------+---------------+------------+
    | id | person_id | label         | date       |
    +----+-----------+---------------+------------+
    | 1  | 1         | Date of Birth | 1970-11-22 |
    | 2  | 1         | Married       | 2004-05-12 |
    | 3  | 2         | Married       | 2004-05-12 |
    | 4  | 2         | Date of Birth | 1975-12-26 |
    +----+-----------+---------------+------------+

So ... we know that John Doe's Date of Birth is 1970-11-22, but line 4 shows his date of birth as 1975-12-26. Anything stand out on that line? Ah! the `person_id` is `2` ... that belongs to Jane Doe. So maybe we need to add a where clause:

    SELECT
        *
    FROM
        person, person_dates
    WHERE
        person.id = person_dates.person_id

    +----+---------+-----------+----+-----------+---------------+------------+
    | id | surname | firstname | id | person_id | label         | date       |
    +----+---------+-----------+----+-----------+---------------+------------+
    | 1  | Doe     | John      | 1  | 1         | Date of Birth | 1970-11-22 |
    | 1  | Doe     | John      | 2  | 1         | Married       | 2004-05-12 |
    | 2  | Doe     | Jane      | 3  | 2         | Married       | 2004-05-12 |
    | 2  | Doe     | Jane      | 4  | 2         | Date of Birth | 1975-12-26 |
    +----+---------+-----------+----+-----------+---------------+------------+

Ah, that looks better!

It turns out, there's a different way of writing the `FROM` clause, that makes this more explicit:

    SELECT
        *
    FROM
        person
        INNER JOIN person_dates ON person.id = person_dates.person_id

Let's say that we're doing some detective work... we're thinking that people who are married on the same day are probably married to each other. We can use a `WHERE` clause to suss that out:

    SELECT
        *
    FROM
        person
        INNER JOIN person_dates ON person.id = person_dates.person_id
    WHERE
        label = 'Married'

    +----+---------+-----------+----+-----------+---------+------------+
    | id | surname | firstname | id | person_id | label   | date       |
    +----+---------+-----------+----+-----------+---------+------------+
    | 1  | Doe     | John      | 2  | 1         | Married | 2004-05-12 |
    | 2  | Doe     | Jane      | 3  | 2         | Married | 2004-05-12 |
    +----+---------+-----------+----+-----------+---------+------------+

Oh, look they're married to each other. How sweet.

... and ...

    SELECT
        *
    FROM
        person
    WHERE
        id > 2

    +----+---------+-----------+
    | id | surname | firstname |
    +----+---------+-----------+
    | 3  | Doe     | J.K.      |
    +----+---------+-----------+

The miracle of life!

Now, let's try an `INNER JOIN` again ... we won't use the `WHERE` clause, because we know that little J.K. is too young to be married...

    SELECT
        *
    FROM
        person
        INNER JOIN person_dates ON person.id = person_dates.person_id

    +----+---------+-----------+----+-----------+---------------+------------+
    | id | surname | firstname | id | person_id | label         | date       |
    +----+---------+-----------+----+-----------+---------------+------------+
    | 1  | Doe     | John      | 1  | 1         | Date of Birth | 1970-11-22 |
    | 1  | Doe     | John      | 2  | 1         | Married       | 2004-05-12 |
    | 2  | Doe     | Jane      | 3  | 2         | Married       | 2004-05-12 |
    | 2  | Doe     | Jane      | 4  | 2         | Date of Birth | 1975-12-26 |
    +----+---------+-----------+----+-----------+---------------+------------+

Wait? _Where's little J.K.???_ Before we call the cops, let's take a careful look around...

    SELECT count(*) FROM person WHERE firstname = 'J.K.'

Shows `count(*)` is 1, so we know that 'J.K.' is still in the `person` table. (oh, thank goodness, I would never forgive myself... ). So what happened?

    SELECT * from person_dates where person_id = 3;

Returns no rows... the doctor hasn't filled out the birth certificate yet. In this case, we want a `LEFT JOIN`, instead of an `INNER JOIN`:

    SELECT
        *
    FROM
        person
        LEFT JOIN person_dates ON person.id = person_dates.person_id

    +----+---------+-----------+----+-----------+---------------+------------+
    | id | surname | firstname | id | person_id | label         | date       |
    +----+---------+-----------+----+-----------+---------------+------------+
    | 1  | Doe     | John      | 1  | 1         | Date of Birth | 1970-11-22 |
    | 1  | Doe     | John      | 2  | 1         | Married       | 2004-05-12 |
    | 2  | Doe     | Jane      | 3  | 2         | Married       | 2004-05-12 |
    | 2  | Doe     | Jane      | 4  | 2         | Date of Birth | 1975-12-26 |
    | 3  | Doe     | J.K.      |    |           |               |            |
    +----+---------+-----------+----+-----------+---------------+------------+

... and J.K. Doe makes an appearance!

## A Koha specific example

Most SQL queries are of the form `SELECT ... from table1 LEFT JOIN table2 ... WHERE ...` there are a few more wrinkles that _can_ be thrown in, but that's all you *need* to know about SQL. Here's an example of finding out who checked out what, and when:


    SELECT
        p.firstname,
        p.surname,
        b.title,
        co.issuedate as 'checked out'
    FROM
        borrowers p
        LEFT JOIN issues co using (borrowernumber)
        LEFT JOIN items i using (itemnumber)
        LEFT JOIN biblio b using (biblionumber)

There are a couple of things to notice here: in the `SELECT` clause, we've written `p.firstname` ... the `p` in this case is an _alias_, set in the `FROM` clause. Writing `borrowers p` says, essentially "We're calling the borrowers table `p`".

### A word about aliases

Common aliases used in queries against the Koha database:

* `biblio b` or `biblio bib`
* `items i`
* `biblioitems bi`
* `statistics s`
* `borrowers p` -- for **p**atrons
* `issues co` -- for **c**heck-**o**uts
* `reserves h` -- for **h**olds
* `deleteditems di` -- for **d**eleted **i**tems
* `borrower_attributes a` -- for **a**ttribute
* `aqorders o` -- for **o**rders

These are not required, but if you do use aliases, these work well, because they don't conflict with each other (i.e. you can't use `biblio b` and `borrowers b` in the same query).


The use of aliases isn't strictly necessary... you could write `SELECT borrowers.firstname, borrowers.surname, biblio.title...`, and the query would work just as well. In fact, as long as the column names are unique, you can leave off the table names entirely: `SELECT firstname, surname, ...`. There are cases where the column names are *not* unique ... for instance, both borrowers and biblio have a `title` field. They're entirely un-related. In this case, you *must* specify which table you're asking for:

    SELECT
        firstname,
        surname,
        title,
        issuedate as 'checked out'
    FROM
        borrowers p
        LEFT JOIN issues co using (borrowernumber)
        LEFT JOIN items i using (itemnumber)
        LEFT JOIN biblio b using (biblionumber)

Gives the error message `Column 'title' in field list is ambiguous`.

The other thing to note is that rather than writing `LEFT JOIN issues co ON co.borrowernumber = p.borrowernumber`, we're using `LEFT JOIN statistics co using (borrowernumber)`. This also has the advantage of making SQL aware of the fact that borrowernumber is exactly the same between both tables ... i.e. it's unique. You can only do this if the column names are exactly the same between tables.

## The rubber meets the road

Luckily, Koha makes this almost drop off a logs simple. In most cases, because you can almost always join on `biblionumber`, `itemnumber`, `borrowernumber` or `branchcode`. In Koha, there are 42 tables that use `borrowernumber`, 32 that use `branchcode`, 26 that use `biblionumber` and 20 that use `itemnumber`.

The `biblio` table is the basic unit of bibliographic information -- it contains `biblio.title`, which is the best way to show the title of a book in Koha. The `items` table shows actual copies that show up in Koha. The `issues` table shows currently checked out items -- it contains both `itemnumber` and `borrowernumber` 

Notice that the `items` table above is in the `FROM` clause, but not mentioned in the `SELECT` clause... this is necessary because there is no `biblionumber` field in the `issues` table. The art of writing tables lies in thinking about how to connect tables such as `borrowers` and `items` or `biblio` and `issues`.

In the case of Borrowers and Items, you have to think "What does a borrower do with an item?" ... the answer is that a borrower checks out an item... so we look at the checkouts table ... which, when Koha was designed, was thought of as 'issuing an item to a borrower', so we look in the issues table. Looking at the issues table, you can see that it has both `borrowernumber` and `itemnumber`, so it's well suited to joining the two.

The case of `biblio` and `issues` is slightly more subtle. In this case, The biblio record contains information, but it doesn't point to anything real that can be checked out... the biblio record is more like the idea of a book rather than a paper copy that you can lay your hands on. The item record actually points to the physical copy... you can't check out the idea of a book, you have to take the physical book to the circ desk and check *that* out ... and that's what the item record represents... so the item record has an itemnumber as well as a biblionumber. The issues table only contains the itemnumber. If you want to see the title of the book that was checked out, you have to join biblios to issues via items.


### Less obvious joins

After you get away from the big four columns  `biblionumber`, `itemnumber`, `borrowernumber` and `branchcode`, things get a bit more challenging. Here are a few more that you should know about.

#### Authorized Values

This is a lookup table for various codes in the system -- it holds human readable text to be displayed on the staff client or opac, for various codes in the system like collection codes or shelving location. Be aware, when using this table that it uses the British spelling `authorised_values` rather than the American `authorised_values`. The table consists of a `category` field, which specifies whether you're looking up collection codes ( category `ccode`), lost statuses (category `lost`), shelving locations (category `loc`), etc. The column `authorised_value` will match to the code being looked up in the database, and the columns `lib` and `lib_opac` are the values that display in the staff client and opac, respectively.

#### Items

* `items.itype` is used instead of `items.itemtype`. No one knows why.
* `items.homebranch`, `items.holdingbranch`. Both can join to `branchcode`.
* `items.itemlost` links to `authorised_values.authorised_value` where `authorised_values.category = 'lost'`.
* `items.location` links to `authorised_values` using `category = 'loc'`.
* `items.ccode` (Collection code) links to `authorised_values` using `category = 'ccode'`.

#### Serials

The `subscription` table is the backbone of the serials module. `subscriptionid` is the primary key, analogous to `biblionumber` for a biblio record - it is unique id). A subscription links to a bib record via `biblionumber` (allowing us to see the title). The subscription table also tracks the frequency with which serials are supposed to arrive (via `subscription_frequencies`) and the numbering of the serials (via `subscription_numberpatterns`).

Individual issues for a subscription are tracked as rows in the `serials` table. These may link to items using the `serialitems` table (if the subscription is set to create item records upon receipt of an issue). The serial table contains the key `serialid` as well as the `subscriptionid` and `biblionumber` (though not as foreign keys).

#### Logs

The `action_logs` table shows information logged when various things change in Koha -- bibs and items cataloged, borrower information changed, item check-in and check out, system preference changes, etc. Because so many different aspects of Koha can be logged, it's not always clear how `action_logs` links to other tables. You can find a table showing what logs are available, how they're enabled, and what fields are linked to in the action\_logs section of the [Koha Reports Library](https://wiki.koha-community.org/wiki/SQL_Reports_Library#action_logs).

#### Zebraqueue

The `zebraqueue` table contains information about whether or not an item has been indexed and is search-able in Koha. Because the zebra queue contains information about bout biblio data and authority record data, it must join to `biblionumber` via `zebraqueue.biblio_auth_id`.

## Where to go from here.

There are a few advanced topics in SQL that I didn't cover here -- some operations such as `min`, `max`, `sum` and `count` are called "aggregate operations" ... i.e. they work on groups of data rather than individual rows. These often require the use of a `GROUP BY` clause. You can find a simple example at [Circ actions on date](https://wiki.koha-community.org/wiki/SQL_Reports_Circulation#All_Circ_Actions_on_Date), which specifies a date in the where clause, then counts circulation grouped by type (issue, renew, return, etc.).

Querying marc data: `biblioitems.marcxml` contains MARC data in XML format. This can be queried using MySQL's `ExtractValue` function. See [Query MARC](https://wiki.koha-community.org/wiki/SQL_Reports_Library#Query_MARC). Please note that in Koha 17.05, marc XML data will be stored in a table called `biblio_metadata`. Koha will have a tool available which will convert your old `biblioitems.marcxml` queries to use the new meta data table instead.

---

## Cheat Sheet

Most queries have the form

    SELECT
        A.aaa,
        B.bbb
    FROM
        ASDF A
        LEFT JOIN BEEN_THERE_DONE_THAT B using (key)
    WHERE
        A.xyz = 100

`LEFT JOIN` allows you to see the results from A and B, even if there is no matching row in B.(you will see blank fields on any rows selected from B).

`INNER JOIN` will not show any rows if data in B does not match (think about poor J.K. Doe, missing because her birthday hadn't been entered into the database yet).

If you are joining on two tables where the names of the keys differ, you must use the syntax
`... on T1.some_key = T2.some_other_key` rather than `... USING (nifty_key)`

Many tables in Koha can be joined by `itemnumber`, `biblionumber`, `borrowernumber` or `branchcode`.

These aliases are used a lot by the community because they don't conflict with each other:

`biblio b` or `biblio bib`, `items i`, `biblioitems bi`, `statistics s`, `borrowers p`, 
`issues co`, `reserves h`, `deleteditems di`, `borrower_attributes a`, `aqorders o`.

### Useful links

* Koha database schema: <http://schema.koha-community.org/master/>
* Koha reports library: <https://wiki.koha-community.org/wiki/SQL_Reports_Library>
* Circulation reports: <https://wiki.koha-community.org/wiki/SQL_Reports_Circulation>
* Holds reports: <https://wiki.koha-community.org/wiki/SQL_Reports_Holds>
* Patron reports: <https://wiki.koha-community.org/wiki/SQL_Reports_Patrons>

---
