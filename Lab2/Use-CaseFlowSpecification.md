# Use-Case Flow Specification

**Problem Statement #46 Remote Team Time-Tracking & Project Approver**

**Use Case:** UC-04 Approve or Reject Timesheet
**Primary Actor:** Engineering Manager
**Secondary Actor:** Remote Developer (receives outcome notification)

## Preconditions

- The Remote Developer has submitted a timesheet (UC-02) for the current billing period.
- The Engineering Manager is authenticated and has access to the pending-approvals queue.
- The submitted timesheet does not exceed 24 hours logged in any single calendar day (enforced at submission, per FR-001).

## Postconditions

- **Success:** The timesheet status is set to "Approved," the project's budget burn rate is recalculated (FR-004), and the Remote Developer is notified.
- **Failure:** The timesheet status is set to "Rejected" with a mandatory manager comment attached, and the Remote Developer is notified to revise and resubmit.

## Main Success Scenario

1. Engineering Manager opens the dashboard and selects the pending-approvals queue.
2. System displays the list of submitted timesheets, each showing developer name, logged hours, and linked Jira ticket IDs.
3. Engineering Manager selects a specific timesheet to review.
4. System displays the full timesheet detail, including a day-by-day breakdown and task links.
5. Engineering Manager reviews the entries and selects "Approve."
6. System validates that the timesheet is still in "Submitted" state (not already processed).
7. System updates the timesheet status to "Approved."
8. System recalculates the project's budget burn rate using the approved hours (FR-004).
9. System sends an approval notification to the Remote Developer.
10. Use case ends successfully.

## Alternate Flow 5a. Timesheet Rejected

- **5a1.** Engineering Manager selects "Reject" instead of "Approve."
- **5a2.** System prompts the Engineering Manager for a rejection comment.
- **5a3.** If the Engineering Manager attempts to submit the rejection with an empty comment field, the system displays a validation error and blocks submission (per FR-003).
- **5a4.** Engineering Manager enters a valid comment and confirms the rejection.
- **5a5.** System sets the timesheet status to "Rejected" and attaches the comment.
- **5a6.** System notifies the Remote Developer of the rejection and comment.
- **5a7.** Remote Developer may revise and resubmit the timesheet, re-entering the flow at UC-02.

## Alternate Flow 6a. Timesheet Already Processed

- **6a1.** System detects the timesheet is no longer in "Submitted" state (e.g., processed concurrently by another session).
- **6a2.** System displays an error and refreshes the queue to reflect the current state.
- **6a3.** Use case ends without changes.
