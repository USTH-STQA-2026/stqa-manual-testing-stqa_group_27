# Test Summary — Test Summary Report

> This is a Quality Assurance activity — evaluating the overall quality of the software based on manual test results.

---

## 1. Team information

| Item | Details |
| --- | --- |
| **Group** | `Group 27` |
| **Class** | `ICT Class 1` |
| **Report date** | `11/06/2026` |
| **Test system** | https://stqa.rbc.vn — v1.0 |

---

## 2. Results overview

| Metric | Value |
| --- | --- |
| Total test cases | 33 |
| Pass | 24 |
| Fail | 9 |
| Blocked | 0 |
| Not Run | 0 |
| **Pass rate** | 72.7% |
| **Bugs found** | 9 |

### Distribution by function group

| Function group | TC | Pass | Fail | Bug | Assessment |
| --- | ---: | ---: | ---: | --- | --- |
| Login (REQ-01) | 4 | 4 | 0 | 0 | Good |
| View book list (REQ-02) | 3 | 3 | 0 | 0 | Good |
| Search & filter book (REQ-03) | 5 | 5 | 0 | 0 | Good |
| Borrow book (REQ-04) | 7 | 5 | 2 | 2 | Needs improvement |
| Return book (REQ-05) | 3 | 2 | 1 | 1 | Needs improvement |
| Handle overdue (REQ-06) | 3 | 2 | 1 | 1 | Needs improvement |
| Member management (REQ-07) | 5 | 1 | 4 | 4 | Many critical bugs |
| Borrow record inquiry (REQ-08) | 3 | 2 | 1 | 1 | Needs improvement |

### Bug distribution by severity

| Severity | Count | Bug IDs |
| --- | ---: | --- |
| High | 4 | BUG-01, BUG-04, BUG-05, BUG-09 |
| Medium | 5 | BUG-02, BUG-03, BUG-06, BUG-07, BUG-08 |
| Low | 0 | - |

---

## 3. Test design techniques used

| Technique | Applied to which REQs? | Number of TCs | How it was applied |
| --- | --- | ---: | --- |
| Equivalence Partitioning (EP) | REQ-01, REQ-03, REQ-07 | 15 | Divided inputs into valid and invalid partitions (existing/non-existing email, valid/invalid search keywords, valid/invalid email formats). |
| Boundary Value Analysis (BVA) | REQ-04 | 5 | Tested boundary values for the number of allowed borrowed books (2, 3, 4 books). |
| Decision Table Testing | REQ-04, REQ-05 | 6 | Combined conditions such as member status, book status, and borrow limit to determine expected outcomes. |

---

## 4. Software quality analysis

### 4.1 Strengths

- Login functionality works reliably and handles valid and invalid cases correctly.
- The book list displays the required information completely.
- Search functionality works well and supports case-insensitive searches.
- Basic authorization between Librarian and Member is implemented reasonably.
- The UI is simple, user-friendly, and responsive.

### 4.2 Weaknesses

- Borrowing logic does not fully enforce important business rules.
- Overdue handling is not functioning correctly.
- Member management shows multiple data-validation related errors.
- There are access control and authorization issues where a member can view data they should not access.
- Some error messages are unclear or do not reflect the actual cause.

---

## 5. Bug fix priority recommendations

> Prioritization is based on business impact and system security.

| Priority | Bug | Severity | Reason for priority |
| --- | --- | --- | --- |
| 1 | BUG-09 | High | Access control violation, risk of user data leakage. |
| 2 | BUG-01 | High | Allows borrowing beyond the limit, directly impacts library business rules. |
| 3 | BUG-04 | High | Overdue management function does not work. |
| 4 | BUG-05 | High | Cannot add new members. |
| 5 | BUG-07 | Medium | Valid email is rejected, impacts operations. |
| 6 | BUG-06 | Medium | Invalid email accepted, reduces data quality. |
| 7 | BUG-03 | Medium | No overdue-return warning displayed. |
| 8 | BUG-02 | Medium | Rejection messages are not specific to the reason. |
| 9 | BUG-08 | Medium | Duplicate-email message is unclear. |

---

## 6. Conclusion

The team executed 33 test cases covering all 8 functional requirements. Results show 24 test cases passed and 9 failed, with a pass rate of 72.7%.

Core functions such as Login, View Book List, and Search work relatively stably. However, the system still has several critical issues related to member management, overdue handling, and access control.

Because there are remaining High-severity bugs that impact business rules and security, the team assesses the system as **not ready for official release**. High-priority issues should be fixed and retested before deployment to real users.

---

## 7. Lessons learned

- Understanding the SRS is a prerequisite to accurately determine expected results.
- Applying EP, BVA, and Decision Table techniques increases test coverage.
- Many business-rule bugs only appear when testing boundary and unusual cases.
- Clear bug reports improve communication between Testers and Developers.

---

## 8. AI usage declaration

| AI tool | Used for which parts | How the outputs were reviewed/edited |
| --- | --- | --- |
| Cursor AI | Helped summarize SRS requirements, suggested initial test cases, assisted drafting test reports and bug reports | Team members analyzed the requirements themselves, executed tests on https://stqa.rbc.vn, compared actual results with SRS, and updated all test cases, execution results and bug reports before submission |
