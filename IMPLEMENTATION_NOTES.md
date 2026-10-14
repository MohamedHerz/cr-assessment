# Implementation Notes


## 1. What I changed
<!-- Grouped by task: bugs fixed and features implemented (component + template). -->

- `SKU-A` quantity changed from 10 to 11, but it was classified as unchanged. Fixed the logic in the loop in `diff.util.ts` to check for changes in price or quantity, instead of unit price only. Refrained from checking description changes, as there is no before/after for them in the detail UI.

- The Approve/Reject buttons were enabled for a read-only user because `canApprove` only checked whether the CR status was pending, without checking the current session user's permissions. Fixed by importing `canApprovePolicy` from `common/permissions.ts` and checking it as well.

- Added logic for filtering rows in the list component. "ALL" is selected by default, and when it is changed, the rows matching the selected filter are shown in the UI.

- In the list template, the table is no longer shown if the API returns no rows, or if the API returns rows but the filter matches none. The "No change requests to show." text is also shown if the visible rows are 0, even when the API does not return an empty list.


## 2. Component & state model
<!-- The screens, the view-state each component exposes, and how data flows from the mock API into the
template. -->

- The list page loads the change request summaries and allows the user to filter by status.
- The detail page shows the selected CR and a summary of its details.
- The user can only approve or reject a CR when permissions allow it.

- The mock API loads CR details from fixtures.
- The list component stores the loaded rows.
- The detail component loads the selected CR and maps the change data into diff rows and timeline entries.
- The template renders those values and the buttons based on the CR state and user permissions.

- The model in `cr.models.ts` defines the CR summary, detail, and timeline.
- The session and permission helpers are in `session.service.ts`.
- `permissions.ts` determines whether the user can read, approve, or execute.

## 3. Invariants I keep
<!-- Which properties the UI guarantees, and where in the component/template each is enforced. -->

| Invariant | How / where |
|---|---|
| Permitted users can see or enable Approve or Reject. | In the detail component and the template condition. |
| Read-only users see the data but no action controls. | Template logic checks the permission model before rendering the buttons. |
| The timeline is chronological. | Entries are ordered in the detail component. |


## 4. Testing strategy
<!-- What you tested (component/DOM vs pure) and why; what you deliberately skipped given the budget. -->

- Focused on component testing and interacting with the UI in the browser, as the user sees it.
- Changed `latencyMs` and `failNext` in the mock API to test a slow network and failing request results, by manually changing the values in `cr-api.service.ts`.
- Tested loading and empty rendering behavior.
- Tested classification for a changed row.
- Tested permission gating for Approve and Reject.

## 5. Assumptions

- Validation for the reason: an input containing only spaces is rejected, using the `trim` function.
- Clear the detail page if the filter is changed, for better UX.
- Reload the detail page when the selected CR changes, because the page only loaded once and clicking on change requests did nothing.
- I did not understand the approve scope very well. I have not seen a strict business requirement for the approver scope, so I assumed that all of the scopes are allowed to approve, as long as their permissions allow it (`cr_a_*`).

## 6. Where I used AI

- To explain the requirements from the `.md` files in plain English in shorter sentences, and to expand on the requirements when more detail was needed.
- To review the component state models and identify likely root causes before implementation.
- To explain the project structure and the framework.
- To find the Angular date pipe format `date:'M/d/yyyy h:mm:ss a'`, which is easier to read this way.
- To write a test for the empty timeline history and no line-item changes.
- To explain the difference between hooks/`useEffect` in React and how to do it in Angular.
- To look up generic helper functions.
- To explain how to use props (parent and child component communication).
- To fix spelling and grammar errors, add bullet points, and generally organize the implementation notes file.

## 7. What I'd improve with more time

- I would like to work more on the layout with flexbox or CSS Grid.
