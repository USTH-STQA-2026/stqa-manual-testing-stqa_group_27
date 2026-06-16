# Test Execution — Test Execution Results

> **Guideline**: Run each TC on the system https://stqa.rbc.vn, record actual results.
> Conclusion: **Pass** (correct result), **Fail** (incorrect result → create bug report), **Blocked** (cannot execute due to blocking error), **Not Run** (not yet run).

| Information | |
| --- | --- |
| **Group** | `Group 27` |
| **Execution Date** | `11/06/2026` |
| **Browser** | `Google Chrome` |
| **Operating System** | `Windows / macOS` |

---

## Detailed Results

| TC ID | Function Group | Expected Result (Summary) | Actual Result | Status | Bug |
| --- | --- | --- | --- | --- | --- |
| TC-01 | Login | Login successful with Librarian account | System logged in successfully | Pass | - |
| TC-02 | Login | Display error for non-existent email | Correct error message displayed | Pass | - |
| TC-03 | Login | Display error for wrong password | Correct error message displayed | Pass | - |
| TC-04 | Login | Display error for empty information | Correct error message displayed | Pass | - |
| TC-05 | View Book List | Member can view book list | Full list displayed | Pass | - |
| TC-06 | View Book List | Librarian can view book list | Full list displayed | Pass | - |
| TC-07 | View Book List | Book status updates realtime | Status updated correctly | Pass | - |
| TC-08 | Search Book | Search by book name | Correct results returned | Pass | - |
| TC-09 | Search Book | Search by author name | Correct results returned | Pass | - |
| TC-10 | Search Book | Case-insensitive search | Works correctly | Pass | - |
| TC-11 | Search Book | Filter by category | Filter results accurate | Pass | - |
| TC-12 | Search Book | Display "No books found" | Correct message displayed | Pass | - |
| TC-13 | Borrow Book | Borrow book successfully | Borrow successful | Pass | - |
| TC-14 | Borrow Book | Cannot borrow already borrowed book | System correctly rejects | Pass | - |
| TC-15 | Borrow Book | Reject suspended member | System correctly rejects | Pass | - |
| TC-16 | Borrow Book | Reject expired member | System correctly rejects | Pass | - |
| TC-17 | Borrow Book | Allow borrow under limit | Borrow successful | Pass | - |
| TC-18 | Borrow Book | Reject when 3 books borrowed | System still allows borrow | Fail | BUG-01 |
| TC-19 | Borrow Book | Correct rejection message | Incorrect message reason | Fail | BUG-02 |
| TC-20 | Return Book | Return book successfully | Return successful | Pass | - |
| TC-21 | Return Book | Book status changes to Available | Updated correctly | Pass | - |
| TC-22 | Return Book | Display overdue warning | No warning displayed | Fail | BUG-03 |
| TC-23 | Handle Overdue | Run overdue check | System executes successfully | Pass | - |
| TC-24 | Handle Overdue | Mark overdue record | Overdue not marked | Fail | BUG-04 |
| TC-25 | Handle Overdue | Member sees own overdue record | Correct overdue record displayed | Pass | - |
| TC-26 | Member Management | Add valid member | Cannot add valid member | Fail | BUG-05 |
| TC-27 | Member Management | Reject email missing @ | Error displayed correctly | Pass | - |
| TC-28 | Member Management | Reject email missing dot in domain | System processes incorrectly | Fail | BUG-06 |
| TC-29 | Member Management | Accept valid email | System rejects valid email | Fail | BUG-07 |
| TC-30 | Member Management | Cannot use duplicate email | Message handling incorrect | Fail | BUG-08 |
| TC-31 | Borrow Record Inquiry | Librarian views all records | Works correctly | Pass | - |
| TC-32 | Borrow Record Inquiry | Member views only own records | Works correctly | Pass | - |
| TC-33 | Borrow Record Inquiry | Cannot view other's records | Can still view other's records | Fail | BUG-09 |

---

## Test Result Summary

| Metric | Value |
| --- | --- |
| Total Test Cases | 33 |
| Pass | 24 |
| Fail | 9 |
| Blocked | 0 |
| Not Run | 0 |
| **Pass Rate** | 72.7% |

### Results by Function Group

| Function Group | Total TC | Pass | Fail | Pass Rate |
| --- | --- | --- | --- | --- |
| Login (REQ-01) | 4 | 4 | 0 | 100% |
| View Book List (REQ-02) | 3 | 3 | 0 | 100% |
| Search & Filter Book (REQ-03) | 5 | 5 | 0 | 100% |
| Borrow Book (REQ-04) | 7 | 5 | 2 | 71.4% |
| Return Book (REQ-05) | 3 | 2 | 1 | 66.7% |
| Handle Overdue (REQ-06) | 3 | 2 | 1 | 66.7% |
| Member Management (REQ-07) | 5 | 1 | 4 | 20.0% |
| Borrow Record Inquiry (REQ-08) | 3 | 2 | 1 | 66.7% |
