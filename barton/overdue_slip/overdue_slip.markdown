# From the Support Desk

## Using OVERDUES\_SLIP To Test Overdue Notices.

Setting up overdue slips, (Generally called `ODUE`, `ODUE2` and `ODUE3`)
can be a frustrating process.  Ideally when making edits to these slips,
you would like to make a small change, then immediately look at the
results. Unfortunately the `overdue_notices.pl` cron job only runs once
a day, and can't be triggered by staff, so it's impossible to edit and
get immediate feedback.

There is a way around this, however. Under _Home > Tools > Notices & slips_,
there is a notice called `OVERDUES_SLIP`, which uses the same format as
all the overdue notices.

![](images/edit_link.png)

Editing the notice, then choosing the *Print* section in the editor allows
you to set up the notice just like any of the 'ODUE' notices... for instance,
this slip uses the same `<item> ...</item>` syntax to display the overdue items.

![](images/edit_page.png)

Once the `OVERDUES_SLIP` has been edited, you can find patrons by creating
a report with the following SQL:

    SELECT
        CONCAT(
            '<a href=\"/cgi-bin/koha/members/moremember.pl?borrowernumber=',
            borrowernumber , '\">' , cardnumber , '</a>'
        ) AS 'cardnumber',
        firstname,
        surname,
        count(*) as overdues
    FROM
        borrowers
        INNER JOIN issues USING (borrowernumber)
    WHERE
        issues.date_due < CURRENT_TIMESTAMP
    GROUP BY borrowernumber
    ORDER BY overdues desc

The `cardnumber` column will then be a link to the patron with overdue items. The report
has the patron with the most overdue items at the top, which should give you
a feeling for how overdues look for patrons with many overdue items.

 Each of these patrons will have a *Print overdues* menu option under the *Print* button.

![](images/print_menu.png)

The output looks like this -- what you see in the editor should be
reflected on the printed page.

![](images/print_output.png)

Once you are done, you can copy text from `OVERDUES_SLIP` into one of your `ODUE` notices,
the output should look the same.
