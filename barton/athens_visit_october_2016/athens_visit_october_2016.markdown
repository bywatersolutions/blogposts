#  My visit to Nelsonville Public Library (Athens County, Ohio)

## Intro -- Why am I doing this, and what am I trying to learn?

## Monday -- Nelsonville Branch

### Meeting the director, Nick Tepe

I started the day talking to Nick Tepe "Pronounced like 'peppy'" and Owen Leonard [Web design, and ByWater contact].

![Nick Tepe & Owen Leonard](images/nick_and_owen.jpg)

We chatted for a few minutes; we talked about Nick's background [PhD in Philosophy, then moved into the library at Columbus -- "I realized immediately that that's what I should have been doing all along"].

Nick explained that the Columbus library system had a home-grown ILS built in Cobol in the early 80s. They had a programing staff of 12. In the early 90s, they realized that they were very over-committed to that system, and they needed to either a) sell the system commercially so that they could amortize their programmer cost over multiple libraries, or b) phase out their programmer staff... they eventually did the latter. Nick was impressed with the knowledge of the programming staff, and said that he liked the flexibility of being able add new features. During the analysis phase, it was determined that the one thing that the in-house system did better than anything else on the market was holds and branch transfers. Nick got a wistful look and said "I've never worked with a system that did a better job with that."

Nick eventually moved into an administrative position at the Chillicothe public library, which was using Horizon 7. Horizon was promising great things in version 8, but was instead bought out by Sirsi Dynix. Nick wasn't happy with that system, and made enough noise that Sirsi eventually sent programmers to talk to them. When the programmer finally arrived and talked to Nick, he listened carefully, and said "Oh... I'd never really looked at it like that...". I got the distinct impression that this was too little, too late.

Eventually, Nick moved into the directorship at Athens. He likes the open-source nature of Koha -- having a system that can be improved, and has been improved over time gives him a feeling of agency and control that is closer to the Columbus library system where he started.

We talked a little about what I'm going to be doing here -- I explained my background, and that I'm looking for a more hands-on experience, bridging the gap between my technical knowledge, and firsthand knowledge of how a library works. Owen and Nick are interested in seeing if there are work-flows in Koha that are newer, and simply haven't been implemented at Athens.

### Chat during the nickel tour 

Owen gave me a quick tour of the Nelsonville Public Library -- it's a small-to-medium sized branch, with administrative offices and a conference room up a flight of stairs. We were talking before the circ staff arrived (the doors open at 11). I asked Owen if there were any pain points in Koha... he had to stop and consider, and said "Well, I'd have to think about things that we do outside of Koha..." and then he couldn't really come up with an answer... but a little later, when we were talking, he mentioned that they don't really like allowing item level holds, because borrowers tend to end up waiting for items that they wouldn't otherwise have to -- but they _do_ allow holds on periodicals, and the inability to place a hold on a specific issue there was a problem. 

### Circ Desk -- Book drop time

Owen's original plan was perhaps for me to talk to the cataloging staff on Monday morning, but we hadn't really made any firm plans, and they weren't really ready... so we decided that I would work with the circ staff at Nelsonville, just to start slowly -- trial by fire will be working circ at the Athens branch in mid-afternoon. Here are a few observations:

* Hold slips are written by hand -- budget concerns a few years ago precluded receipt printers, but it may be time to revisit that.
* They don't use any type of library Kiosk software.
* When pulling holds, serials cause a slowdown -- it's easy to find the title, but harder to find the issue. I had a brainstorm about creating a gravatar style icon for each issue, viewable on the barcode label and the hold slip.
* The circ screen has a nice "This patron has X remaining checkouts" dashboard: <br /> ![Checkout summary](images/checkout_summary.png) <br />... I'm not sure if it's part of standard koha and I'd just never noticed it, or if it's some special sauce in intrenetuserjs.
* One thing that I hadn't considered, because Koha doesn't really look at things this way, is that branch transferred items are grouped and shipped together.
    * At the circ desk there are bins for grouping the items by destination branch<br /> ![transfer bins](images/transfer_bins.jpg)
    * These are bagged and shipped to other branches. Each bag has an ID number, and those ID numbers are tracked. ![transfer bags](images/transfer_bags.jpg)
    * They've had problems losing bags of items.
    * As the items are added to a bag, This is tracked outside of Koha.
    * Koha's view of items is 'Checked in' or 'Checked out' with side routines for 'Holds shelf', 'Branch transfer', and there's the whole holds cart thing... but there's no way to actually track status in the sense of "I'm going to scan these things as I put them into this box, and then ship it over there, and the other party is going to scan them on receipt, and we'll see if they match" type of process.
* Thanks to the circ staff at Nelsonville: Eric, Deborah and Abie (I'm looking up the band _Good Charlotte_ right now, on Abie's recommendation).


## Tuesday -- Circulation

### Morning -- Nelsonville 
I went back down to the Nelsonville circ desk, to check in on the morning return of book-drop items. They were also working on returning items delivered by Cargo. Again, part of their work flow involved grouping items -- this time it was the items on hold, identified by the hold slips, which were held in the books like bookmarks. That *isn't* anything that Koha could really improve on from a process perspective, but it did mean that the staff didn't have to switch focus between checking in items and capturing holds.

I talked with Mary, one of the staff whom I had seen but not talked to on Monday -- she gave me a bit of a running cometary on what she was doing as she was doing her work, which was quite helpful. Here are a few of the things she said along the way:
  
* Making eye contact with patrons is important, and this is hard to do when scanning items for checkout.
* There's an ms-dos type window (probably some batch file running periodically). The staff thinks that this is part of the offline circ backup. It appears on the screen for a quarter of a  second, then disappears. It's very distracting.
* Mary said that the scanner beeps, regardless of whether a barcode is read correctly or not... I mentioned that Koha best practices are to turn the scanner beep off, and use Koha's sounds instead... I could tell that this registered with her, but I don't think that she knew how to make that happen.
* They use sticky-dots to identify new items. It never occurred to me that the reason that libraries use those is because _they're easy to remove_. "Oh, that item isn't new any more". _*PEEL.*_
* A patron returned several DVDs -- one of the cases was missing the disk, and she caught that on the fly... she said "I was lucky that I caught that while the patron was still here."
    * That got me thinking that it would be helpful to put a hint on the overdues email for CDs and DVDs saying "Please remember to make sure the disk is in the case when you return it"... we could add a "patron\_message" to the itemtypes table...
* There was a kit containing a scratched CD and a broken jewel case. She sent this off for repair. She checked to see if there were any holds on the item. "If there are holds, we put a higher priority on the repair, because someone's waiting for it". Common sense, but I never would have considered it.
* Mary mentioned that it doesn't make sense for the "Currently available" facet *not* to show, at least on the staff client. I think she has a point, although I'd hate to have to do that work.

Mary's comment about eye contact made me think about and watch for the customer service aspects of the circ desk (which isn't quite the right word, because library patrons aren't customers, but 'human interaction' doesn't connote the same type of helpfulness). She left, and I spent the rest of the morning working with Lorinda, whose a real veteran -- she's worked in the library for 22 years, and is obviously a wealth of institutional knowledge.

Lorinda was checking items in and asked if the patron would like to put a hold on the next item in the series. I asked about that (because I had customer service on my mind). She said "Now that you mention it, that is something I've wondered about...". She proceeded to run a search on the series title, which had more than 20 titles.

"From the search results, I can select the first three, and place a hold... but if I navigate to the next screen and place a hold on a 4th item, it drops the selections from the first page". I thought about creating a list, but that seemed like too much overhead. This bothered me... I thought about it over night and opened the search screen, poked around in the help pages, and came to [adding items to carts](http://manual.koha-community.org/3.22/en/cart.html), which 
doesn't require as many steps.

The one thing that I noticed about work-flow is that searching for holds on the holds shelf is a slowdown, and there's time taken grooming and reorganizing that shelf. I wonder if the holds over report might help clearing a few items.

### Afternoon -- Athens 

Owen and I talked over lunch, then I drove back to Athens. The Athens library is spacious and has large windows providing a lot of natural light. The circulation area is round, with four check-out stations facing North, South, East and West. To the left of the Circ desk were bicycles for checkout.

![bicycles for checkout](images/bicycles.jpg)

Athens is the largest library in the system, and the circulation desk is busy enough that the check-out and return functions are handled in separate areas. The Athens County Public Library system doesn't charge daily fines -- they do charge replacement fees for lost items, but that's it. This means that _all_ items may be returned in the book drop, and the circulation desk is focused entirely helping patrons. Athens doesn't use any type of library Kiosk system for computer usage -- the circ staff simply hands out cards for computers in use.

![cards for computers in use](images/computers_in_use.jpg)

Marilyn showed me the check-in process. She noted that the staff relies a great deal on the differences in sounds between successful returns, mis-scanned barcodes and items with holds. She also mentioned that paying close attention to the screen during check-in and check-out is critical -- Koha can make the text on the screen as big as they want, but if the circ staff isn't looking at the screen, errors will occur.

Once items are checked in, items go on a series of shelves in the returns area, where they are sorted, placed on carts, and shelved. The shelving is largely done by volunteers.

FIX-ME -- get name --, works as the volunteer coordinator for the Athens branch. He's detail oriented and had questions about the order of items displayed on the checkout screen -- the most recent items are added at the bottom of the list. This means that in the event that someone wants to check an item in the list, they have to scroll to the bottom, which is an issue for patrons with lots of checkouts. I checked bugzilla, but didn't find anything.

Thanks to Marilyn Zwayer, the branch manager at Athens, as well as Jenny, Amber and Brian working circ and returns, as well as several others whose names I'm afraid that I missed.

## Wednesday

### Morning -- Cataloging

Athens has two catalogers, Terry and Bethany, who work as a tight team. Terry walked me through the cataloging process, they don't use acquisitions, so most of their records are copy-catalogued via Z39.50. Items are added manually. Owen had set up a page containing custom reports for the catalogers, containing reports like books missing ISBN.

Terry mentioned that the addition of batch item modification had changed the way they do cataloging, because it enabled them to do a lot more cataloging far in advance, and then simply mark the items as available as batches were received.

I told them about the `UpdateNotForLoanStatusOnCheckin` system preference, which should eliminate the need for the bulk update -- items could be sent to branches and would automatically change status when they were scanned at the branch. Coincidentally, Owen arrived shortly after; he said that he had looked at that system preference, and found that it didn't _quite_ fit their process in some important way. I'll have to follow up with him about that.

Terry and Bethany talked a lot about processes and work-flows. The previous catalogers, had kept a fairly tight rein on batch item deletion (understandable, considering its history in Koha)... but this caused issues in the branches, because items tended to end up back on the shelves, because staff are very proactive about checking items in and getting them re-shelved -- so the branch managers at each branch now have permissions to run batch deletions. This has worked well for them.

Terry asked me about the punctuation on the search results page -- there's a period either in the template or in the XSLT after the Author name and dates field -- they were driving themselves nuts because they couldn't tell if that had been added to the marc record or whether it was being added by Koha itself. I showed them how to export the marc records so they could see with certainty.

They had tried to use the advanced marc editor, but found that it wasn't quite working for them -- they're looking forward to seeing it in action once it comes out of beta. They're going to attend the advanced editor macros webinar, and that may clear up some questions.

### Afternoon -- Circulation at The Plains branch

I talked with Ken, the branch manager at The Plains (Yes, it's the name of a town. Ohio has interesting place names).

Ken has been working at the Library for 20+ years. He's an interesting combination of very experienced, with good technical knowledge, and laid back. He's seen the entire history of Koha at Athens.

I asked if there were any issues that he'd seen... there were only two of any consequence -- he was bothered by the fact that koha only shows facets for items on the current search page, and also, as a branch manager, he would like to edit shelving locations on items -- permissions he doesn't currently have. I was able to check on their ``maxRecordsForFacets`` settings and looked up the permissions for editing items. I gave Owen recommendations on setting the ``edit\_items`` and ``edit\_items\_restricted`` permissions, as well as the ``subfieldsToAllowForRestrictedEditing`` system-preference, which should address both issues.

## Thursday

### Athens Circ, round II
