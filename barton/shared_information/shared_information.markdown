# Sharing information in the Koha community.

One of the strongest parts of an open source community is the way that
information is shared among its members. This happens in a number of
different ways. We share information in mailing lists, on IRC, via bug
reports and via the Koha wiki. When you're first getting started, joining
the IRC channel and subscribing to the mailing list is a great way to
join the community, both to benefit from the knowledge of others, and to
contribute to the dialog. This raw information is collated in various ways
-- via [dashboards](http://dashboard.koha-community.org/), in the form of
[source code](http://git.koha-community.org/gitweb/?p=koha.git;a=tree),
and in [documentation](https://koha-community.org/documentation/).

Most of the aggregated information seems to require some special knowledge
-- the ability to program, the expertise to write correct documentation
with the full knowledge of the software that backs it up. Once you've
lurked on the mailing list for a while, and gotten to know some of the
IRC regulars, often, it's difficult to know where to take the next step
in terms of contributing knowledge.

One place where it's easy to start and still add
real useful information is on the Koha wiki's [Shared
resources](https://wiki.koha-community.org/wiki/Category:Shared_resources)
page. This started with the [SQL Reports
Library](https://wiki.koha-community.org/wiki/SQL_Reports_Library)
and [HTML & CSS
Library](https://wiki.koha-community.org/wiki/HTML_%26_CSS_Library),
but the page has been expanded to include other types of information.

I'm going to add a "Hardware known to work with Koha" page to the wiki, here's
the information that I've harvested from the ByWater mailing list so far:

## Printers

### By manufacturer

* Bixolon
    * SPR 350 (not verified as working)
* Dymo
    * LabelWriter 400
    * LabelWriter 400 Turbo
    * LabelWriter 450 twin Turbo
        * "on one side I print spine labels on 1 inch x 1 inch labels
        and the other side I have set up to print out author,title and
        accession date on "address labels". What a time saver and the
        best part was the printer only cost $100."
* Epson
    * T-20
    * TM-T88III
        * http://www.nexpresslibrary.org/go-live/configure-your-receipt-printers/
    * T88IV
    * T88V
    * TSP100
    * TSP143LAN
        * Ubuntu 12.04.
            * https://wiki.koha-community.org/wiki/TSP100_thermal_receipt_printers_on_Ubuntu_12.04
            * http://bywatersolutions.com/2014/03/07/receipt-printers-ubuntu-koha/
* POS-X
    * EVO-PT3-1HUE
        * "This is a great printer.  Fast, and easy to setup on the network."
* Star
    * SP 512
    * TSP100
        * "Stars were the most reliable and relatively easy to install."
        * "They are quiet, fast and it uses the thermal paper."
    * TSP 600
    * TSP 600 Plus
        * "I know within our library group we have these receipt printers working well."
* Zebra
    * GK420T Spine Label Printer
    * Advanced desktop label printers
        * https://www.zebra.com/us/en/products/printers/desktop/advanced-desktop-printers.html
        * "They're not cheap, but they work very well for us. The
          quality of the labels is very good and the print layout
          options are very flexible."
    * TLP 2844
        * "Our Zebra TLP 2844 printers are, unfortunately, not interacting
        with Koha.  We copy the call number from Koha and paste it into
        the free ZebraDesigner software, add line breaks, and print.
        (I use the ZebraDesigner software also to print barcodes.)<br />
        I *wish* the Zebra printers would interact with Koha to print
        spine labels, but we can't get Koha to format the LC call
        numbers on spine labels with the line breaks where we want them.
        (Not with the Zebra printers, and certainly not with the sheets
        of labels.)"

### Notes about setting up printers

#### Chrome
1. Open Chrome
1. Go to the 3 horizontal lines in the upper left that take you to settings etc.
1. Choose print
    1. Set the receipt printer as the printer
    1. Go to the +More setting
    1. Choose margins
        1. Set to minimum
        1. Unclick headers and footers
1. I set up the home page in settings/on startup
1. Close Chrome
1. Make a shortcut for Chrome
    1. Right click on the shortcut, choose properties
    1. Add "--kiosk-prinng" to the end of the target (d't include the quotes, you need two dashes before "kiosk" and one dash after)
1. Launch Chrome from that shortcut.

#### Firefox
* [Resetting Firefox Print Settings](https://support.mozilla.org/en-US/kb/fix-printing-problems-firefox#w_reset-firefox-printer-setting)
* ["The Nuclear Option"](https://support.mozilla.org/en-US/kb/fix-printing-problems-firefox#w_reset-all-firefox-printer-settings)

## Bar-code Scanners

### Bar-code Scanners
* Adaptus 3800
* Datalogic Gryphon GD4130BK
* Honeywell
    * 1900g
        * " I started buying Honeywell 1900g series scanners because
          they have the ability to read 1d and 2D barcodes.  We weren't
          using 2D barcodes on anything yet, but I wanted LCLD to be
          in a position that if we needed to start scanning any 2D
          barcodes, we were ready for that without the need for a large
          hardware upgrade.  Since the 1900g scanners were more-or-less
          the same price as the 3800 series scanners, the upgrade to the
          more advanced technology made sense to me when I was there.
          Latah County had circulation of 275,000 items per year at
          7 locations and we had about 20 barcode scanners in use at
          any given time.  I budgeted for 3-5 new barcode scanners per
          year and always made sure I had a supply of 7 new scanners on
          hand at the beginning of each fiscal year (one new scanner per
          branch) as well as a supply of repaired scanners.  I usually
          found that if someone sent in a scanner with a note saying
          "This doesn't work" that the scanner might be good but it had
          a bad cable, while another one would have a good cord but had
          been dropped too many times so I usually had a supply of 2 or
          3 second hand scanners available as well as 7 brand new units.
          My experience was that a new scanner would last 3-5 years -
          mostly depending on how many times staff dropped it or it
          got knocked off of its stand onto the floor."
* Metrologic Voyager MS9540
* Handheld 3800
* Symbol LS4208
