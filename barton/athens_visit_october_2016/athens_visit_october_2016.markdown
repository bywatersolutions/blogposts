#  My visit to Nelsonville Public Library (Athens County, Ohio)

## Intro -- Why am I doing this, and what am I trying to learn?

## Monday -- Nelsonville Branch

### Meeting the director, Nick Tepe

I started the day talking to Nick Tepe "Pronounced like 'peppy'" and Owen Leonard [Web design, and ByWater contact]. We chatted for a few minutes; we talked about Nick's background [PhD in Philosophy, then moved into the library at Columbus -- "I realized immediately that that's what I should have been doing all along"].

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
* There's an ms-dos type window (probably some batch file running periodically). The staff thinks that this is part of the offline circ backup. It appears on the screen for a quarter of a second, then disappears. It's very distracting.
* Mary said that the scanner beeps, regardless of whether a barcode is read correctly or not... I mentioned that Koha best practices are to turn the scanner beep off, and use Koha's sounds instead... I could tell that this registered with her, but I don't think that she knew how to make that happen.
* They use sticky-dots to identify new items. It never occurred to me that the reason that libraries use those is because _they're easy to remove_. "Oh, that item isn't new any more". _*PEEL.*_
* A patron returned several DVDs -- one of the cases was missing the disk, and she caught that on the fly... she said "I was lucky that I caught that while the patron was still here."
    * That got me thinking that it would be helpful to put a hint on the overdues email for CDs and DVDs saying "Please remember to make sure the disk is in the case when you return it"... we could add a "patron\_message" to the itemtypes table...
* There was a kit containing a scratched CD and a broken jewel case. She sent this off for repair. She checked to see if there were any holds on the item. "If there are holds, we put a higher priority on the repair, because someone's waiting for it". Common sense, but I never would have considered it.
* Mary mentioned that it doesn't make sense for the "Currently available" facet *not* to show, at least on the staff client. I think she has a point, although I'd hate to have to do that work.

Mary's comment about eye contact made me think about and watch for the customer service aspects of the circ desk (which isn't quite the right word, because library patrons aren't customers, but 'human intetraction' doesn't connote the same type of helpfulness). She left, and I spent the rest of the morning working with Lorinda, who'se a real vetran -- she's worked in the library for 22 years, and is obviously a wealth of institutional knowledge.

Lorinda was checking items in and asked if the patron would like to put a hold on the next item in the series. I asked about that (because I had customer service on my mind). She said "Now that you mention it, that is something I've wondered about...". She proceeded to run a search on the series title, which had more than 20 titles.

"From the search results, I can select the first three, and place a hold... but if I navigate to the next screen and place a hold on a 4th item, it drops the selections from the first page". I thought about creating a list, but that seemed like too much overhead. This bothered me... I thought about it over night and opened the search screen, poked around in the help pages, and came to [adding items to carts](http://manual.koha-community.org/3.22/en/cart.html), which 
doesn't require as many steps.

The one thing that I noticed about work-flow is that searching for holds on the holds shelf is a slowdown, and there's time taken grooming and reorganizing that shelf. I wonder if the holds over report might help clearing a few items.

### Afternoon -- Athens 

Owen and I talked over lunch, then I drove back to Athens. The Athens library is spacious and has large windows providing a lot of natural light. The circulation area is round, with four check-out stations facing North, South, East and West. To the left of the Circ desk were bicycles for checkout.![bicycles for checkout](https://lh3.googleusercontent.com/68R6iPwIox604q-BRfiMrXFZHQCLQ3ULUkUxxyP4o_F34t_Cnt3cBEAwuwY8BALsgB3nw65uV6Kfi8EZT_KgBeN63OhXStD7Btegoi7T1zLu9GON9oH-dPwzVPg0U9ppXYNa850Oo1dRxn-4TosZJqM-gadHZ-4-_JPqJiWeTZK6zqSdk_jy1puzxTG8jVgCOS-OV_pZQMhZ_otnloVAxyZuP-Jm25gRiwbGzgshN0sp3IuGQeXBydX2fE7CNQ6bNXxodTVoU1DWvBnOdcAoz2eDnn71OcMhpKwmKwoauRS-Z8OEENPJUUsgwL-Je6VAUCDOdGV8xrgP7CrncgGG56IZ7-A1zz635Q7-VzwIeERsQUlB252QFRdoe7v1QoLOXMa7JrvRN2eMtOrz9-SA8Afa7_bmZPG916oX1vHwWJi-Uf3GD7mkcKJZ3oTHe1LTvkFo65tq1H8ujex7hsUtHg2IINeVBW-hwtwVdYGpoDl2_UlMAO7bi74G9Uh9ZcoEo_JSBImBy_kcSPywVI-N5trpQCeeU0I3E5Crmnp6xUOqPU1av-C-qptCVf9xIfakb3SDIb04C_E1sA26dQFdPmQNT3asGqdSXkzmSAjGPb0aDkxF2Q=w958-h539-no).


## Wednesday -- Cataloging
