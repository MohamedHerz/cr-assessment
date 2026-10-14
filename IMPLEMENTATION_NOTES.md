# Implementation Notes

> Fill this in as part of your submission. 1–2 pages, bullet points are fine. Delete these
> instructions before submitting.

## 1. What I changed
<!-- Grouped by task: bugs fixed and features implemented (component + template). -->

-

sku-a quantity is changed from 10 to 11 but it is classifed as unchanged fixed the logic in the loop at diff.util.ts from checking unit price only to check changes in price or quantity, retrained from checking description changes as there is no before/after for it in detail UI


approve/reject button is enabled for a read only user because the canApprove function 
only checks if the CR status is pending without checking for current sessions user perms. fixed by imoporting canapprovepolicy func from common/permissions.ts and is now included to be checked if true to enable the button for users

added logic for filtering rows in list component, all is selected by default but if it is changed then the selected filter views on ui

in list tamplate removed the table from showing if the api returns no rows or if the api returns rows but the filter is switched to an empty table 
also now it shows the No change requests to show text if vis rows are 0 when the filter is switched to an empty table even if api doesnt return empty 


## 2. Component & state model
<!-- The screens, the view-state each component exposes, and how data flows from the mock API into the
template. -->

-
the list page loads change request summeries and allows user to filter by status
the detail page shows selected CR and shows a summery of its details
the user can only approve or reject CR when perms allow it

the mock api load CR details from fixtures
the list component stores loaded rows
the detail comp loads selected CR and maps the change data into diff rows and timeline entries
the tamplate renders those values and buttons based on CR state and user perms

the model in cr.models.ts defines CR summary, detail, timeline.
the session and premissions helpers in serssion.service.ts
permissions.ts determine if the user can read or approve or execute 

## 3. Invariants I keep
<!-- Which properties the UI guarantees, and where in the component/template each is enforced. -->

| Invariant | How / where |
|---|---|
|permitted users can see or enable approve or reject|in detail component and tamplate condition|
|read only users see data but no action control|template logic checks the permission model before rendering buttons|
|timeline is chronological| enteries are ordered in detail component|


## 4. Testing strategy
<!-- What you tested (component/DOM vs pure) and why; what you deliberately skipped given the budget. -->

-focused on component testing and intracting with the UI in browser as the users sees it 
tested loading, empty rendering behvaior 
tested classification for changed row
tested premmision gating for approve and reject

## 5. Assumptions
<!-- Where the requirements left room for interpretation, the calls you made and why. -->

-
decided to not show an empty table if there is no visable data for user to look at


## 6. Where I used AI
-
To explain the requirments from .md files in plain english in shorter sentences and expand more on requirments when needed more details

review component state models and identify likely root causes before implementation

explain project structure

used ai to find angular date pipe format: date:'M/d/yyyy h:mm:ss a, easier to read this way



## 7. What I'd improve with more time
-
