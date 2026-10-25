# Common PE bugs, by severity

What testers did to find bugs in past CS2103/T Practical Exams (PE), grouped into the four PE severity levels.
Use it to check TutorReach before the PE, and as a test plan when we test another team's app.

* [Try these first](#try-these-first)
* [Where the numbers come from](#where-the-numbers-come-from)
* [Very high (PE: High)](#very-high-pe-high)
* [High (PE: Medium)](#high-pe-medium)
* [Moderate (PE: Low)](#moderate-pe-low)
* [Low (PE: VeryLow)](#low-pe-verylow)
* [Why reports get rejected](#why-reports-get-rejected)
* [Method](#method)

## Try these first

These are the inputs behind the most High-rated reports. The commands are written for an AB3-style app; adapt the prefixes.

1. **Edit the data file by hand.** Close the app. In the JSON file, set a field to an invalid value or `null`, copy one entry so it appears twice, or delete a bracket. Relaunch.
2. **Edit an entry into a duplicate**, such as `edit 2 p/<phone of entry 1>`. Exit and relaunch.
3. **Add near-duplicates:** `add n/John Doe ...`, then `add n/john doe ...` and `add n/John  Doe ...` (two spaces).
4. **Change a record that others link to.** Edit, rename, delete or change the status of it, then check the records on the other side.
5. **Run every UG example word for word**, in order, on the sample data, including the quick start and the help window.
6. **Launch exactly as documented:** the released jar name, a double-click, and `java -jar` from another folder.
7. **Close the app with the window's X button and relaunch.** Also run two copies at once.
8. **Filter, then act by index:** `find alex`, then `edit 1 ...` or `delete 1`. Do the same after `sort` or after switching views.
9. **Dates and times:** `30 Feb`, `29 Feb 2023`, `31 Apr`, `24:00`, an end before the start, year `0000`, overlapping slots.
10. **Prefixes:** `edit 1 n/A n/B`, `edit 1 t/`, `edit 1 x/abc`, `add n/Johnp/91234567`, `find find alex`.
11. **Phones and emails:** `p/+65 9123 4567`, `p/+919123456789`, `p/000`, `e/..john@example.com`, `e/john__doe@example.com`, `e/a@b`.
12. **Names:** `n/Kumar s/o Ravi`, `n/Siti d/o Ahmad`, `n/Connor O'Brien`, `n/Jean-Pierre`, `n/José`, `n/李明`.
13. **Numbers:** `delete 0`, `delete -1`, `delete 2147483648`, `delete 1.5`, `delete 1 1`, negative or huge amounts.
14. **Layout:** a 100-character name, the smallest window, a second monitor, enough entries to need scrolling.
15. **Help window:** open it in macOS full screen, close it, minimise it, then run `help` again.
16. **Clearing:** `clear`, `clear 1` or a clear alias, then `undo`.

## Where the numbers come from

* **49,572 bug reports** from two sources:
  * every final PE report from AY20/21 S1 to AY24/25 S2, from the course's public `nus-cs2103-AY.../pe-dev-response` repositories;
  * 2,313 AY25/26 PE dry-run reports, which testers filed on CS2103/T team repositories as `[PE-D]` issues. AY25/26's final PE reports are not public.
* **41,155 reports after removing duplicates.** Testers rated 1,590 of them High, 8,566 Medium, 18,208 Low and 12,790 VeryLow.
* **The tier is the tester's rating.** Teams then accepted or rejected each report, and could lower its severity. The tables show how often teams accepted each pattern.
* **Every High report was read and grouped.** For the other tiers, a random sample was read: 700 Medium, 700 Low and 500 VeryLow reports. High counts are exact; the other tiers give each pattern's share of the sample.
* **Examples are copied word for word** from the linked report, typos included.

Teams pushed back on most reports, and rarely agreed that a bug was High:

| Tester's rating | Final PE reports | Team accepted | Team accepted at the tester's rating |
|---|---|---|---|
| High | 1,412 | 479 (34%) | 60 (4%) |
| Medium | 8,028 | 2,755 (34%) | 694 (9%) |
| Low | 17,214 | 5,951 (35%) | 4,351 (25%) |
| VeryLow | 12,208 | 5,268 (43%) | 5,232 (43%) |

The dry-run reports have no team response, so they are left out of this table.
These are team responses only: a tester can dispute a response, and the teaching team decides disputed reports.

## Very high (PE: High)

> *A flaw that affects most users and causes major problems for them, i.e., only problems that make the product almost unusable for most users should have this label.* (PE page)

### What teams agreed was High

Of the 1,412 final PE reports rated High, teams accepted 60 at High. Most fall into these groups:

* **An edit silently deletes linked data.**
  “Edit command clears existing appointments” [AY2324S2#475](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/475),
  “Editing the order name removes the deadline date somehow” [AY2324S2#5164](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/5164),
  “Removal of attendance values if edit used with blank tag values” [AY2324S1#2480](https://github.com/nus-cs2103-AY2324S1/pe-dev-response/issues/2480)
* **A duplicate made by edit or copy empties the app on the next launch.**
  “Editting a buyer detail does not check for duplicate phone number” [AY2223S1#4997](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/4997),
  “Not verifying duplicates for date when editing date” [AY2122S1#1479](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/1479),
  “Copy feature integration” [AY2223S2#1906](https://github.com/nus-cs2103-AY2223S2/pe-dev-response/issues/1906)
* **The wrong record, or every record, is changed.**
  “Schedule edit is editing the wrong index” [AY2425S2#1086](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1086),
  “Error with cancelling schedule, wrong schedule is cancelled” [AY2425S2#709](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/709),
  “Running some commands results in all contacts being deleted” [AY2324S2#725](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/725)
* **Changes are lost after a restart.**
  “The grade status is not saved when the application is closed and opened again” [AY2223S1#3211](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/3211),
  “Previously edited person become new contact upon restart” [AY2425S1#598](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/598)
* **The app stops working.**
  “Application stops working when trying to update a day that does not exist.” [AY2021S1#4711](https://github.com/nus-cs2103-AY2021S1/pe-dev-response/issues/4711),
  “Invalid find command followed by valid find command breaks patients list” [AY2425S1#1200](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1200),
  “App stops loading when upload pdf file” [AY2223S2#180](https://github.com/nus-cs2103-AY2223S2/pe-dev-response/issues/180),
  “Change compatibility in json file to value > 100 -> RuntimeException” [AY2122S1#4634](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/4634)
* **Data cannot be seen.**
  “Cannot scroll down list of weddings” [AY2425S2#1631](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1631),
  “Unable to view my logs past a certain number” [AY2425S1#1114](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1114),
  “The window size is inappropriate when starting the application.” [AY2021S1#4450](https://github.com/nus-cs2103-AY2021S1/pe-dev-response/issues/4450)
* **A documented command does not work.**
  “Edit Command Does not work for modifying certian fields” [AY2425S1#623](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/623),
  “filter-c not working.” [AY2021S1#4160](https://github.com/nus-cs2103-AY2021S1/pe-dev-response/issues/4160)
* **The docs mislead from the start.**
  “Example in Quick Start not working” [AY2021S1#966](https://github.com/nus-cs2103-AY2021S1/pe-dev-response/issues/966),
  “Links in DG redirect to addressbook code base” [AY2021S1#1358](https://github.com/nus-cs2103-AY2021S1/pe-dev-response/issues/1358),
  “Missing Manual Testing instructions” [AY2021S2#2763](https://github.com/nus-cs2103-AY2021S2/pe-dev-response/issues/2763)

### Everything testers rated High

All 1,590 High reports, by what the tester did. 61 fit no pattern.

| # | What testers did | Reports | Team accepted | Examples |
|---|---|---|---|---|
| 1 | **DG content.** Checked each UML diagram's notation and names against the code, read every use case and its extensions, tested the NFRs and ran every manual-testing step. | 147 | 51% | “Return arrow in sequence diagram is not dashed” [AY2223S2#3964](https://github.com/nus-cs2103-AY2223S2/pe-dev-response/issues/3964)<br>“Use cases for deleting, undo, redo, filtering note, deleting note, etc, is missing.” [AY2425S2#133](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/133) |
| 2 | **UG examples and documented behaviour.** Copied every UG, quick-start and help-window example into a fresh app, and tried every documented option. | 129 | 40% | `add n/John Doe p/98765432 a/John street, Block 123, #01-01` [AY2425S1#2119](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/2119)<br>`find n/jo ap` [AY2425S1#3185](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/3185) |
| 3 | **Duplicate detection.** Added or edited an entry that differs from another only in letter case or spaces, edited an entry into a copy of another, or added two real people who share a name. | 127 | 23% | `add n/john doe hp/98765432 eml/johnd@example.com a/John street, block 123,#01-01 dob/25/12/1999` [AY2122S1#981](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/981)<br>`edit 9 p/88888887` [AY2425S2#1267](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1267)<br>`add n/Google r/SWE d/15-03-2026 s/Pending t/tech t/Tech` [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103T-W11-3/tp/issues/286) |
| 4 | **UG constraints and formats.** Compared each UG section with the help window, the error messages and the app: prefixes, optional fields, limits, unique fields, what `clear` deletes. | 106 | 37% | “the `c/` parameter appears in the User Guide but is missing from the Help window” [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103T-T08-1/tp/issues/373)<br>“The user guide should mention how `clear` will clear the whole address book and not just people.” [AY2425S2#1901](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1901) |
| 5 | **Hand-edited data file.** Closed the app, edited the JSON (invalid value, `null`, deleted key or bracket, duplicated entry) and relaunched. | 105 | 27% | “Replace tags with [null]” [AY2324S2#4187](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/4187)<br>“if i close the app, duplicate a person in the json file and open the app, everything is gone” [AY2122S2#2771](https://github.com/nus-cs2103-AY2122S2/pe-dev-response/issues/2771) |
| 6 | **Missing operations.** Tried to edit, delete, list and undo every kind of entry and field by typing only. | 82 | 11% | “There is no way to delete a customer's order.” [AY2324S2#3654](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/3654)<br>“If I wrongly used the `clear` command, all data will be lost.” [AY2324S1#4327](https://github.com/nus-cs2103-AY2324S1/pe-dev-response/issues/4327) |
| 7 | **Dates, times and clashes.** 30 Feb, 29 Feb in a non-leap year, 31 Apr, 24:00, an end before the start, far past or future years, overlapping bookings. | 80 | 34% | `addevent sp/BBW t/COM t/BIZ d/2024 02 30 1800 v/USC pa/Muthu` [AY2425S1#1149](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1149)<br>`add_task n/Celebrate Lucas Birthday d/29/02/2023` [AY2223S2#1644](https://github.com/nus-cs2103-AY2223S2/pe-dev-response/issues/1644)<br>`add_schedule title/Meeting d/CS2103T start/2024-04-14 21:00 end/2024-04-13 22:00` [AY2324S2#4542](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/4542) |
| 8 | **Linked records.** Linked two records, then edited, renamed, deleted or changed the status of one and checked the other and any totals. | 78 | 31% | “I deleted E1001 but E1001 remains as team lead of T0004.” [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103-F12-1/tp/issues/246)<br>“Despite deleting a person, they still show up on the schedule, and be displayed with payroll.” [AY2324S2#2128](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/2128) |
| 9 | **Long values, small windows, many entries.** Very long names or tags, the smallest window or 800x600, a second monitor, enough entries to need scrolling. | 72 | 26% | `editM 1 n/AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA d/2022-10-20 23:59` [AY2223S1#3862](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/3862)<br>“Drag Application into 2nd screen and close application.” [AY2122S1#3913](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/3913) |
| 10 | **Find, filter and sort.** Partial words, several keywords or prefixes, symbols such as `+`, fields other than the name, mixed-case sorting. | 63 | 17% | `find n/John n/Lisa` [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103T-T13-2/tp/issues/217)<br>`Find +` [AY2122S1#628](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/628)<br>`find blk 11` [AY2223S1#2689](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/2689) |
| 11 | **Repeated, unknown or empty prefixes.** The same prefix twice, an unknown prefix, an empty value, prefix-like text inside a value, the command word twice. | 58 | 26% | `edit dog 2 s/male s/female` [AY2021S2#2954](https://github.com/nus-cs2103-AY2021S2/pe-dev-response/issues/2954)<br>`add n/Roopa p/1248345643 lol/wasuehrg e/par@example.com a/X street` [AY2425S1#457](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/457)<br>`find find dog` [AY2021S1#5563](https://github.com/nus-cs2103-AY2021S1/pe-dev-response/issues/5563) |
| 12 | **Launching.** Followed the quick start literally: the released jar name, a double-click, `java -jar` from another folder, other operating systems. | 55 | 35% | `java -jar addressbook.jar` [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103T-F11-1/tp/issues/419)<br>“The description says to double click the jar file, but double clickign gives an exception.” [AY2324S2#747](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/747) |
| 13 | **Phone, email and ID formats.** Country codes, `+`, spaces, very long or all-zero numbers; emails with leading or doubled dots, underscores or no top-level domain. | 53 | 26% | `add n/James Ho p/+919123456789 e/jamesho@example.com a/123, Clementi Rd, 1234665 t/java t/python` [AY2425S2#1535](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1535)<br>`customer add n/Alan Tan p/81236789 e/..example@mail.com a/21 Lower Kent Ridge` [AY2021S2#2072](https://github.com/nus-cs2103-AY2021S2/pe-dev-response/issues/2072) |
| 14 | **Saving and reloading.** Made changes, closed the window with its X button and relaunched; ran two copies; launched from another folder. | 47 | 30% | “close the window and the reopen the app with java -jar ___” [AY2425S2#339](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/339)<br>“assigning a user to a task with commas in their name will clear the addressbook on next launch” [AY2122S2#499](https://github.com/nus-cs2103-AY2122S2/pe-dev-response/issues/499) |
| 15 | **AB3 leftovers and links.** Searched the UG, DG and app messages for AddressBook and AB3, and clicked every link. | 43 | 60% | “the link leads to the AB3 release page (https://github.com/se-edu/addressbook-level3/releases) instead of the GhostConnect release page” [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103T-W12-2/tp/issues/200)<br>“The "clear" command wipes the app, and the output is "Address book has been cleared!".” [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103T-W09-2/tp/issues/201) |
| 16 | **Extreme numbers and indexes.** 0, -1, 2147483648, huge or negative amounts, the same index twice. | 42 | 21% | `loan 2 c 500 2147483647 2026-06-15` [AY2425S2#127](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/127)<br>`edit 1 d/Lunch at Harbourfront Centre a/-100` [AY2122S2#909](https://github.com/nus-cs2103-AY2122S2/pe-dev-response/issues/909)<br>`delete 10 10 10 10` [AY2021S2#2381](https://github.com/nus-cs2103-AY2021S2/pe-dev-response/issues/2381) |
| 17 | **Freezes.** Opened and closed the help window in macOS full screen, minimised pop-ups, repeated commands, uploaded the wrong file type. | 39 | 18% | “Open the app in fullscreen.” [AY2425S1#1315](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1315)<br>“upon closing the window by pressing the x button at the top left-hand corner, the program hangs and does not respond to any input.” [AY2425S1#3230](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/3230) |
| 18 | **Commands on a filtered list or another view.** Ran `find` or `sort` or switched views, then acted by index or ran a command on every shown entry. | 34 | 29% | `edit 1 n/new shirt` [AY2021S2#296](https://github.com/nus-cs2103-AY2021S2/pe-dev-response/issues/296)<br>“After filtering the paid students, then use unpay all command, some students are still with the status of paid.” [AY2425S2#1203](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1203) |
| 19 | **Real names.** s/o, d/o, apostrophes, hyphens, accents, non-Latin scripts. | 31 | 23% | `add n/John s/o Doe p/98765432 e/johnd@example.com a/John street, block 123, #01-01` [AY2425S2#134](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/134)<br>`edit 1 n/차은우` [AY2425S1#1587](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1587) |
| 20 | **Crashes.** Overflowing numbers, commands on an empty list, renaming an entry to its current name, cancelling a file dialog. | 31 | 35% | `add_member n/Dave Lee r/48709127390112093812309723902173-3-20 t/davelee` [AY2425S1#2158](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/2158)<br>“tried to rename "John Doe" to "John Doe"” [AY2223S1#940](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/940)<br>“Cancel the export” [AY2223S2#3415](https://github.com/nus-cs2103-AY2223S2/pe-dev-response/issues/3415) |
| 21 | **Wrong record changed.** Acted by index after the list reordered, gave several indexes out of order, or targeted a partial or shared name. | 27 | 30% | `delete n/alex yeoh` [AY2425S2#2448](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/2448)<br>“If we insert indices to deleteclientmult in non ascending order” [AY2425S2#868](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/868) |
| 22 | **Clear and undo.** Ran `clear`, its alias or `clear 1`, then `undo`; undid bulk or failed commands. | 25 | 32% | `clear 1` [AY2324S2#3414](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/3414)<br>“add 3 people, then delete 12 10, then undo” [AY2425S2#691](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/691) |
| 23 | **Import and export.** Exported, then imported the same file; exported values that contain commas. | 19 | 21% | “When trying to import the same file which was exported from the app, error shown is that 4 entries had errors even though the file was not edited.” [AY2425S1#2337](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/2337)<br>“This occurs always when there are commas within the address itself” [AY2324S2#3902](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/3902) |
| 24 | **Display not refreshed.** After add, assign or edit, checked every list and panel without listing again. | 15 | 13% | `assign 1 i/1` [AY2122S2#3009](https://github.com/nus-cs2103-AY2122S2/pe-dev-response/issues/3009)<br>`addSched 1 s/Exam start/2024-04-19 16:00 end/2024-04-19 18:00` [AY2324S2#2712](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/2712) |
| 25 | **Missing or misleading errors.** Empty values, prefixes in the wrong case, commands on an empty list. | 12 | 42% | `drinkadd n/ p/4.50 c/Coffee` [AY2425S2#2409](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/2409)<br>“used 'M/' instead of 'm/'” [AY2021S1#5562](https://github.com/nus-cs2103-AY2021S1/pe-dev-response/issues/5562) |
| 26 | **Editing tags.** Edited one tag on an entry that has several. | 9 | 22% | “if I edit a tag, or I simply want to add a tag, all existing tags get replaced.” [AY2324S1#5932](https://github.com/nus-cs2103-AY2324S1/pe-dev-response/issues/5932) |

## High (PE: Medium)

> *A flaw that causes occasional inconvenience to some users, but they can continue to use the product.* (PE page)

A random sample of 700 of the 8,566 Medium reports. 30 fit no pattern.

| # | What testers did | Share | Team accepted | Examples |
|---|---|---|---|---|
| 1 | **UG gaps.** Looked for undocumented formats, limits and defaults, and for vague or contradictory wording. | 8.9% | 39% | “What date format should the user use for the `DATE` field?” [AY2223S1#3908](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/3908)<br>“The UG only says they are automatedly saved, without mentioning it is saved after each command” [AY2324S1#3489](https://github.com/nus-cs2103-AY2324S1/pe-dev-response/issues/3489) |
| 2 | **Duplicate detection.** Entries that differ only in case or spaces; different people who share a phone or a name. | 7.9% | 16% | `add-b n/john Doe ph/98765432 e/johnd@example.com a/311, Clementi Ave 2, #02-25 l/USA` [AY2223S1#3042](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/3042)<br>`add n/Chloe L ee e/chloexlee@gmail.com t/chloe201 g/jchloechloe tg/CS2101 G08` [AY2122S2#3368](https://github.com/nus-cs2103-AY2122S2/pe-dev-response/issues/3368) |
| 3 | **DG use cases, NFRs and manual tests.** Checked use-case steps and extensions, ran each manual test. | 6.0% | 38% | “Extension 2b is missing "Use case resumes at step 3."” [AY2425S2#504](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/504)<br>“There are multiple use cases in the DG that are not actually implemented.” [AY2425S1#2717](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/2717) |
| 4 | **Phone, email and other formats.** Country codes, very long numbers, doubled dots, a domain with no dot, underscores only. | 5.9% | 17% | `edit 1 p/09472304723047203742037402934` [AY2425S1#1135](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1135)<br>`add n/Test r/Dev d/01-01-2026 s/Pending e/a@bc` [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103T-W11-3/tp/issues/302) |
| 5 | **DG UML diagrams.** Notation, activation bars, multiplicities, classes left over from AB3. | 5.7% | 55% | “Entire diagram is absent of any activation bar” [AY2425S2#153](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/153)<br>“Diagram not updated - still have JsonAdaptedTag and don't have JsonAdaptedTeam and JsonAdaptedSkill” [AY2122S2#3255](https://github.com/nus-cs2103-AY2122S2/pe-dev-response/issues/3255) |
| 6 | **Find, filter and sort.** Partial words, phone numbers, several tags. | 5.3% | 27% | `find Irf` [AY2425S2#1962](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1962)<br>`find t/Hunters t/Antihero` [AY2223S2#2791](https://github.com/nus-cs2103-AY2223S2/pe-dev-response/issues/2791) |
| 7 | **Doc links and AB3 leftovers.** Clicked every link, compared the help-window URL, searched for AddressBook. | 4.9% | 29% | “There are still many traces of AddressBook in the DG” [AY2425S1#1038](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1038)<br>“the pop-up window displays a URL pointing to the standard AddressBook-Level3 (AB3) User Guide” [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103T-T16-2/tp/issues/197) |
| 8 | **Long values, small windows, help window.** | 4.7% | 39% | `add n/hello3eorjgioerjgoiejrgoiejrgiojeorgjerjgejrigjeorgjeoirjgoierjgoiejriogjeiorgjioerjgoiejrgioj p/3123 e/hello@gmail.com a/hello` [AY2122S2#862](https://github.com/nus-cs2103-AY2122S2/pe-dev-response/issues/862)<br>“Typing `help` command / clicking on the Help button shows a 1px wide help window” [AY2223S1#983](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/983) |
| 9 | **Impossible dates and times.** | 4.3% | 27% | `addappt ad/2025-02-30 10:15 dn/S1234567B pn/S1234567A` [AY2324S2#5190](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/5190)<br>`order add n/2 dt/14-02-2021 18:70 d/1 q/1 d/3 q/1` [AY2021S2#2070](https://github.com/nus-cs2103-AY2021S2/pe-dev-response/issues/2070) |
| 10 | **Real names.** s/o, hyphens, non-Latin scripts. | 4.0% | 29% | `add n/Kumar s/o Suresh p/98765432 e/kumar@example.com i/T1234123A a/John Street, Block 123, #01-01 t/3` [AY2425S1#2224](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/2224)<br>`add n/Jean-Pierre rk/Diamond rl/Top c/Gnar` [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103T-F08b-1/tp/issues/254) |
| 11 | **UG example fails.** Ran each example and documented shortcut. | 4.0% | 61% | `add n/Betsy Crowe t/friend e/betsycrowe@example.com a/Newgate Prison p/1234567 t/criminal` [AY2425S2#1094](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1094)<br>“The UG mentions that I can use the arrow keys to navigate through previous commands, but I am unable to do so” [AY2425S1#2862](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/2862) |
| 12 | **Vague or wrong messages.** One mistake per command, then read the message. | 3.7% | 50% | `redit 1 r/3` [AY2425S1#1388](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1388)<br>`editStock 1 c/7` [AY2223S1#3250](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/3250) |
| 13 | **Missing space or prefix-like text in a value.** | 3.4% | 29% | `addc n/John Doen/a p/98765432 e/johnd@example.com a/John street, block 123, #01-01` [AY2425S2#778](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/778)<br>`edit client 1 -n John -p97432170` [AY2425S2#486](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/486) |
| 14 | **Commands on a filtered list or another view.** | 3.3% | 13% | `editpatient 2 tag/Asthmatic` [AY2021S1#5063](https://github.com/nus-cs2103-AY2021S1/pe-dev-response/issues/5063)<br>`view me` [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103T-W09-1/tp/issues/247) |
| 15 | **Missing or stale information on screen.** | 3.3% | 26% | `edit 1 desc/Random description` [AY2425S2#1526](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1526)<br>`delete -p 8` [AY2223S1#373](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/373) |
| 16 | **Missing capability.** Several items per person, editing every field. | 3.3% | 4% | “There isn't an edit command for order.” [AY2324S2#3656](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/3656)<br>“After using the schedule command, my previous meeting with Alex Yeoh was gone.” [AY2122S2#183](https://github.com/nus-cs2103-AY2122S2/pe-dev-response/issues/183) |
| 17 | **Hand-edited or missing data file.** | 3.1% | 36% | “When there is null put into the tags section of the json file, the app does not launch.” [AY2324S1#5420](https://github.com/nus-cs2103-AY2324S1/pe-dev-response/issues/5420)<br>“Set save file to read only” [AY2122S1#2245](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/2245) |
| 18 | **Hard-to-type or case-sensitive commands.** | 3.0% | 14% | “`meetingAdd`, `meetingRemove` are excessively long” [AY2223S2#2510](https://github.com/nus-cs2103-AY2223S2/pe-dev-response/issues/2510)<br>`add n/Ravi Kumar p/12345678 e/ravi@hotmail.com a/nasjk jkasdn m/active` [AY2425S2#907](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/907) |
| 19 | **Empty or cleared fields.** A prefix with no value; clearing an optional field with edit. | 2.4% | 35% | `edit 1 h/` [AY2324S2#4531](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/4531)<br>`remark 1` [AY2425S2#2188](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/2188) |
| 20 | **Linked records out of sync.** | 1.7% | 17% | “when editing menu item name, it is not reflected in the order history” [AY2021S2#1323](https://github.com/nus-cs2103-AY2021S2/pe-dev-response/issues/1323) |
| 21 | **Money and numeric bounds.** Negative, huge or malformed amounts. | 1.7% | 50% | `ladd 6 date/28 Feb 2021 time/2100-2200 subject/abc rates/-5.0` [AY2122S1#4136](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/4136)<br>`drinkadd n/Lemon Juice p/3.90.1 c/Juice` [AY2425S2#2064](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/2064) |
| 22 | **The same prefix or value twice.** | 1.7% | 8% | `edit 1 n/alex n/alex2` [AY2021S1#5493](https://github.com/nus-cs2103-AY2021S1/pe-dev-response/issues/5493)<br>`restore 1 1` [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103T-T12-1/tp/issues/199) |
| 23 | **Destructive commands without confirmation.** | 1.6% | 27% | “running `clear` or `c` clears the data” [AY2425S1#3442](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/3442) |
| 24 | **Index 0, negative or above 2147483647.** | 1.0% | 29% | `delete 2147483648 from/ Contacted` [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103T-F14a-2/findr/issues/269)<br>`edit -1` [AY2425S2#1591](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1591) |
| 25 | **Undo and redo sequences.** | 1.0% | 43% | “one command executed allows for 4 undoes before it throws an error message” [AY2223S1#1872](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/1872) |

## Moderate (PE: Low)

> *A flaw that is unlikely to affect normal operations of the product. Appears only in very rare situations and causes only a minor inconvenience.* (PE page)

A random sample of 700 of the 18,208 Low reports. 24 fit no pattern.

| # | What testers did | Share | Team accepted | Examples |
|---|---|---|---|---|
| 1 | **DG appendices.** Use-case extensions, user stories, measurable NFRs, glossary, manual tests for new commands. | 9.7% | 41% | “Instructions for manual testing in DG do not include a lot of new features” [AY2324S2#1041](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/1041)<br>“Missing extension in use case” [AY2223S2#318](https://github.com/nus-cs2103-AY2223S2/pe-dev-response/issues/318) |
| 2 | **DG UML diagrams.** Notation, outdated classes, missing diagrams, legibility. | 7.9% | 47% | “The UI Class Diagram above shows that MainWindow contains a StatusBarFooter.” [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103T-T13-4/tp/issues/292)<br>“Some fields have multiplicities and some do not” [AY2425S1#1663](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1663) |
| 3 | **Doc typos, links and screenshots.** | 6.9% | 58% | “The `Quick Start` hyperlink does not redirect to anywhere and cannot be clicked on.” [AY2425S2#937](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/937)<br>“Inconsistent use of capitalisation in command summary” [AY2324S1#2769](https://github.com/nus-cs2103-AY2324S1/pe-dev-response/issues/2769) |
| 4 | **Duplicate detection.** Case, a doubled space, an upper-case email, editing a name into another's. | 6.3% | 36% | `add n/John  Doe e/johnd@example.com p/98765432 c/Apple j/Software` [AY2425S2#1317](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1317)<br>`edit 2 n/John Doe` [AY2425S2#1619](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1619) |
| 5 | **Phone, email and address validation.** | 6.0% | 29% | `add n/John Doe e/johndoe@u.nus.edu p/+97198765432` [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103-F11-2/tp/issues/489)<br>`edit 1 e/newemail@com` [AY2425S1#1852](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1852) |
| 6 | **UG omits a rule.** Whenever the app rejected something, checked whether the UG says so. | 5.3% | 54% | “In the user guide, it is not indicated that filter is only based on the currently displayed list.” [AY2425S1#3927](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/3927)<br>“There is no mention of which fields are uniquely identifiable in the UG or DG.” [AY2425S1#1748](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1748) |
| 7 | **Index 0, negative, blank or above 2147483647.** Compared the messages. | 4.9% | 24% | `view 0` [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103T-F09-3/tp/issues/299)<br>`delete 2147483648` [AY2425S2#1278](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1278) |
| 8 | **UG format differs from the app.** `[ ]` and `...` markers, prefixes, the command summary. | 4.4% | 61% | `menu add n/Chicken Chop p/5.10` [AY2021S2#211](https://github.com/nus-cs2103-AY2021S2/pe-dev-response/issues/211)<br>`fp n/ doe` [AY2324S2#4059](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/4059) |
| 9 | **GUI layout and help window.** Resizing, full screen, minimising help. | 4.4% | 13% | “It does not show the help pop-up again when the help window is minimized.” [AY2223S2#4066](https://github.com/nus-cs2103-AY2223S2/pe-dev-response/issues/4066)<br>“The error text extends horiztontally outside the result box, as shown.” [AY2122S1#895](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/895) |
| 10 | **Unclear wording or undefined terms.** | 4.3% | 30% | “Unclear what a KEY_MILESTONE is” [AY2324S1#1731](https://github.com/nus-cs2103-AY2324S1/pe-dev-response/issues/1731) |
| 11 | **Missing or restrictive features.** Every field compulsory, mouse-only actions. | 4.1% | 10% | “the `edito` command does not allow me to change the customer of that order” [AY2223S2#1827](https://github.com/nus-cs2103-AY2223S2/pe-dev-response/issues/1827) |
| 12 | **Vague or wrong error messages.** A valid format with an invalid value; a duplicate field. | 3.6% | 40% | “Although the format is right and the time is not valid, the error message is saying "Reservation time should be HHmm format".” [AY2324S2#3702](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/3702)<br>“Whether I enter a duplicate email or phone number for a musician, I get the same error message” [AY2324S1#1035](https://github.com/nus-cs2103-AY2324S1/pe-dev-response/issues/1035) |
| 13 | **Impossible or unrealistic dates.** 29 Feb 2025, 30 Feb, year 1000, 24:59. | 3.4% | 42% | `add n/Test g/M dob/29/02/2025 p/91234567 e/test@example.com a/123 Main St` [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103-F08-1a/tp/issues/353)<br>`edit 1 b/19-04-1000` [AY2324S2#5058](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/5058) |
| 14 | **Find, filter and sort.** Partial words, commas between keywords, sorting by an empty field. | 3.0% | 24% | `find alex, john` [AY2425S1#1317](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1317)<br>`sort n/` [AY2021S2#2339](https://github.com/nus-cs2103-AY2021S2/pe-dev-response/issues/2339) |
| 15 | **Very long input.** | 2.7% | 26% | `remark 1 r/koikoikoikoikoikoikoikoikoikoikoikoi` [AY2122S1#193](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/193) |
| 16 | **Repeated or unknown prefixes, extra words.** | 2.6% | 11% | `clear 1` [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103-F13-4/tp/issues/276)<br>`list ahahaha` [AY2122S2#2482](https://github.com/nus-cs2103-AY2122S2/pe-dev-response/issues/2482) |
| 17 | **AB3 leftovers.** | 2.6% | 22% | “Exiting Address Book as requested ...” [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103T-W09-1/tp/issues/243)<br>“use the java -jar addressbook.jar command to run the application.” [AY2425S1#1954](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1954) |
| 18 | **Zero, negative, decimal or huge numbers.** | 2.4% | 29% | `increase 1 l/0.00 d/2025-11-04` [AY2425S2#2261](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/2261) |
| 19 | **Case-sensitive commands and values.** | 2.3% | 25% | `LIST` [AY2425S2#584](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/584)<br>`view i/s1234567l` [AY2122S2#1869](https://github.com/nus-cs2103-AY2122S2/pe-dev-response/issues/1869) |
| 20 | **UG examples copied from the PDF.** | 2.1% | 40% | `hire n/alex yeoh j/SOFTWAREengineer` [AY2425S1#1913](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1913) |
| 21 | **Names and text with s/o, a/p or a leading apostrophe.** | 2.0% | 21% | `add n/John a/p Doe a/addr p/88888888 e/a@a.com` [AY2425S2#2475](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/2475)<br>`'Ohana` [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103-F08-1a/tp/issues/333) |
| 22 | **Missing or extra spaces, empty values.** | 2.0% | 21% | `edit jamesn/reeseee` [AY2425S1#2857](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/2857)<br>`fitdelete 1 n/bench  press` [AY2324S2#2618](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/2618) |
| 23 | **Tags with spaces, symbols or mixed case.** | 1.3% | 22% | `edit 1 t/friends t/Friends` [AY2324S2#585](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/585) |
| 24 | **Hand-edited data file.** | 1.3% | 22% | “Assign a person to the same wedding twice using the .json data file.” [AY2425S1#1701](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/1701) |
| 25 | **An edit that changes nothing reports success.** | 1.1% | 38% | `edit 1 n/Alex Yeoh` [AY2324S1#2273](https://github.com/nus-cs2103-AY2324S1/pe-dev-response/issues/2273) |

## Low (PE: VeryLow)

> *A flaw that is purely cosmetic and does not affect usage e.g., typo/spacing/layout/color/font issues in the docs or the UI. Only cosmetic problems should have this label.* (PE page)

A random sample of 500 of the 12,790 VeryLow reports. 5 fit no pattern.

| # | What testers did | Share | Team accepted | Examples |
|---|---|---|---|---|
| 1 | **Typos and grammar in the UG and DG**, including text left from AB3. | 11.8% | 56% | “A GUI similar to the below should appear in a few seconds.” [AY2324S2#4135](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/4135) |
| 2 | **UML notation.** `:Class` lifelines, dashed return arrows, an X at deletion, multiplicities, guard conditions. | 10.0% | 44% | “It should be :LogicManager instead of LogicManager.” [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103T-F10-1/tp/issues/235)<br>“No "X" at the end of lifeline to signify object deletion” [AY2122S2#1881](https://github.com/nus-cs2103-AY2122S2/pe-dev-response/issues/1881) |
| 3 | **PDF layout.** Page breaks, split tables, browser headers and footers. | 8.2% | 41% | “Tables cut off by page break” [AY2223S2#4274](https://github.com/nus-cs2103-AY2223S2/pe-dev-response/issues/4274)<br>“the User Guide has headers as it was not saved without headers and footers” [AY2425S2#1210](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1210) |
| 4 | **Inconsistent capitalisation, punctuation and terms.** | 5.6% | 64% | “Inconsistent full stops in UG” [AY2223S1#1468](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/1468)<br>“Code block format not consistently used for code.” [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103T-F10-1/tp/issues/219) |
| 5 | **Use cases and user stories.** | 4.8% | 46% | “Missing extension when the list is empty” [AY2223S1#5113](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/5113) |
| 6 | **Outdated UG screenshots.** Compared a fresh launch with the quick-start image. | 4.4% | 36% | “you guys seem to be using the same default sample data that was provided by AB3” [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103-F10-1/tp/issues/202) |
| 7 | **Docs or usage messages contradict the app.** | 4.0% | 75% | “In ug, the command is `expired`. In the dg, it is mentioned as `expire`.” [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103T-W14-2/tp/issues/361) |
| 8 | **UG leaves out behaviour or definitions.** | 4.0% | 35% | “it does not specify that a single invalid index will cause the entire command to fail.” [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103T-F10-2/tp/issues/267)<br>“No definition given for home folder” [AY2122S1#2278](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/2278) |
| 9 | **Diagrams too small or blurry** at 100% zoom in the PDF. | 4.0% | 60% | “The text is to small in this UML diagram” [AY2324S2#569](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/569) |
| 10 | **Unclear or badly ordered docs.** | 3.8% | 32% | “Order of UG commands is a bit weird, clear should be the last few commands” [AY2122S1#875](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/875) |
| 11 | **Misleading error messages.** An unknown prefix, a space inside a phone number. | 3.8% | 37% | `edit 1 i/12345` [AY2122S1#3098](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/3098)<br>`addMember n/John Doe p/9876 5432 e/johnd@example.com r/President r/Treasurer` [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103T-T11-3/tp/issues/270) |
| 12 | **AB3 leftovers.** Log messages, the save-file name, the icon. | 3.6% | 33% | “Initialising Address Book” [AY2324S2#2492](https://github.com/nus-cs2103-AY2324S2/pe-dev-response/issues/2492)<br>“The icon could be customised instead of using the original AB3 Icon.” [AY2425S1#865](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/865) |
| 13 | **Validation too strict or too lax.** | 3.4% | 24% | `+65 98763494` [AY2223S1#5322](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/5322)<br>“Can add a phone number with just brackets” [AY2223S1#2523](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/2523) |
| 14 | **Usability suggestions.** | 3.4% | 18% | “Confirmation of delete does not happen” [AY2223S2#699](https://github.com/nus-cs2103-AY2223S2/pe-dev-response/issues/699) |
| 15 | **A small window.** | 3.0% | 27% | “shrink window size.” [AY2425S2#514](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/514) |
| 16 | **Missing, empty or unmeasurable DG sections.** | 3.0% | 40% | “Empty section "Extra notes on testing clearpast" in DG.” [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103T-T15-1/tp/issues/245) |
| 17 | **Repeated or irrelevant doc content.** | 2.8% | 36% | “The logic component is repeated in the DG” [AY2021S1#4773](https://github.com/nus-cs2103-AY2021S1/pe-dev-response/issues/4773) |
| 18 | **Command summary, table of contents and links.** | 2.8% | 43% | “command `exit` should be included in UG command summary” [AY2425S1#2435](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/2435) |
| 19 | **Typos in app messages**, such as a one-result `find`. | 2.6% | 62% | `find alex` [AY2526S1 PE-D](https://github.com/AY2526S1-CS2103-F13-2/tp/issues/306)<br>`help clear` [AY2223S1#3118](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/3118) |
| 20 | **UI cosmetics.** Empty boxes, icons with no data, theme gaps. | 2.4% | 33% | “When a user does not have any tags, the tag icon is still there” [AY2122S1#908](https://github.com/nus-cs2103-AY2122S1/pe-dev-response/issues/908) |
| 21 | **Very long values cut off.** | 2.0% | 20% | `add n/John DoeJohn DoeJohn DoeJohn DoeJohn DoeJohn DoeJohn DoeJohn DoeJohn Doe p/98765432 e/johnd@example.com a/311, Clementi Ave 2, #02-25 b/2001-04-08 t/friends t/owesMoney` [AY2425S1#2758](https://github.com/nus-cs2103-AY2425S1/pe-dev-response/issues/2758) |
| 22 | **Extra words or a repeated prefix accepted.** | 1.8% | 22% | `sort nonsense` [AY2526S2 PE-D](https://github.com/AY2526S2-CS2103T-T09-1/tp/issues/284)<br>`client edit 5 n/Katya 46 n/Katya 49` [AY2021S1#2026](https://github.com/nus-cs2103-AY2021S1/pe-dev-response/issues/2026) |
| 23 | **UG examples fail.** | 1.8% | 44% | “the exported csv file is saved as .csv.csv” [AY2223S1#745](https://github.com/nus-cs2103-AY2223S1/pe-dev-response/issues/745) |
| 24 | **Impossible dates and times accepted.** | 1.2% | 17% | `T add n/new task d/31/02/2020` [AY2021S1#5305](https://github.com/nus-cs2103-AY2021S1/pe-dev-response/issues/5305) |
| 25 | **Help window that only links to the website.** | 0.8% | 25% | “The help command instructs user to go the given link, might hinder the user if they do not currently have internet acess” [AY2425S2#1793](https://github.com/nus-cs2103-AY2425S2/pe-dev-response/issues/1793) |

## Why reports get rejected

For the 1,412 final PE reports that testers rated High, teams rejected 581 (41%), marked 201 (14%) not in scope and could not reproduce 132 (9%).
These were the usual reasons:

* **The UG already says so.** Duplicate rules, "extra parameters are ignored", "only the last occurrence of a repeated prefix is taken", "an invalid data file is discarded" and phone formats were all rejected when the UG documented them. For us: document every limit. For testers: report against what the UG says.
* **Unrealistic input.** The PE page says a problem that happens only through deliberate sabotage, such as a 30-digit phone number, is not a bug. A likely user mistake, such as a missing space between parameters, must still not crash the app, corrupt the data or make it unusable.
* **Feature requests.** Missing commands and confirmations were mostly marked not in scope, unless the feature is hardly usable without them.
* **Cannot reproduce.** Freezes when closing the help window in macOS full screen, and one-off crashes. Give exact steps from launch, the OS and a screenshot.
* **PDF copy-paste.** Examples that failed only because copying from the PDF dropped a space.
* **Part II reports not about documentation quality.** The teaching team lets teams reject these.
* **Severity limits set by the PE page.** Very long text that only spoils the layout is VeryLow, unless it hides what the user needs. Calling an invalid value a format error is Low. Disallowing symbols is acceptable with a reason, but can still be a feature flaw, as with `s/o` in names.

## Method

* Collected on 2026-10-26 through the GitHub API: all issues in `nus-cs2103-AY2021S1/pe-dev-response` to `nus-cs2103-AY2425S2/pe-dev-response`, and the `[PE-D]` issues created between 2025-08-01 and 2026-07-31 in `AY2526S1-CS2103*` and `AY2526S2-CS2103*` repositories.
* The tester's rating comes from each report's `[original labels: ...]` footer; dry-run reports carry it as their label.
* Each report was put in at most one pattern by reading its title and the start of its body. The shares for the sampled tiers are estimates.
* "Team accepted" is the share of a pattern's reports with the response `Accepted`, at any severity. AY25/26 dry-run reports have no response, so they count as not accepted. They are 11% of the High tier.
