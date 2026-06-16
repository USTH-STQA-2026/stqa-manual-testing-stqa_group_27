# Test Cases — Test Case Table

> **Guideline**: Write a minimum of **20 TC** to cover all main functions (REQ-01 → REQ-08).
> See [examples/sample-test-case.md](../examples/sample-test-case.md) to understand how to write good TC.
> Organize and group test cases in the most appropriate way.

| Information | |
|---|---|
| **Group** | `Group 27` |
| **Created Date** | `22/04/2026` |
| **System** | https://stqa.rbc.vn |
| **Reference** | SRS v1.0 |

---

## Step 1: Input Domain Modeling (IDM)

> 📖 **Textbook:** Chapter 6 — *Input Domain Modeling*, Paul Ammann & Jeff Offutt.
>
> **Before writing Test Cases**, the group **must** analyze the input domain using the IDM table below.
> Each function needs to identify: **Characteristic**, **Block/Partition**, and **Representative Value**.

### IDM — Login (REQ-01)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Does email exist in DB? | Yes | `librarian@library.com` | Login successful |
| | No | `no0ne@email.com` | Error message |
| Is password correct? | Yes | `admin123` | Login successful |
| | No | `wrongpass` | Error message |
| Is input field empty? | No | (any value) | Normal processing |
| | Yes | `""` | Display "Please enter..." |

### IDM — View Book List (REQ-02)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| User role | Librarian | LIB001 | User an view book list |
| User role | Member | MEM002 | User can view book list |
| Book status | Available | BOOK001 | Display "Available" |
| Book status | Borrowed | BOOK003 | Display "Borrowed" |
| Book status | Lost | BOOK007 | Display "Lost" |
| Status change | After borrowing | BOOK001 | Update realtime to "Borrowed" |
| Status change | After returning | BOOK003 | Update realtime to "Available" |

### IDM — Search Book (REQ-03)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Does keyword exist in DB? | Yes (book name) | `"Flutter"` | Display books containing "Flutter" |
| | Yes (author name) | `"Nguyen"` | Display books by author Nguyen |
| | No | `"XYZ123"` | Empty list |
| Case sensitive? | Lowercase | `"flutter"` | Same result as "Flutter" |
| | Uppercase | `"FLUTTER"` | Same result as "Flutter" |

### IDM — Borrow Book (REQ-04, REQ-05)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Book status? | Available | BOOK001 | Allow borrow |
| | Borrowed | BOOK003 | Reject |
| | Lost | BOOK007 | Reject |
| Member status? | Active | MEM002 | Allow borrow |
| | Suspended | MEM004 | Reject, display error |
| | Expired | MEM005 | Reject, display error |
| Books borrowed? | < 3 (BVA: 0, 1, 2) | MEM006 (0 books) | Allow borrow |
| | = 3 (BVA: limit) | Member with 3 books | Reject, exceeded limit |

### IDM — Return Book (REQ-05)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Is member borrowing? | Yes | MEM002 - BOOK003 | Allow return |
| Is member borrowing? | No | MEM002 - BOOK013 | Reject return |
| Borrow record status | On time | BR003 | Return successful |
| Borrow record status | Overdue | BR001 | Return successful with warning |
| Book status after return | Borrowed | BOOK003 | Change to "Available" |

### IDM — Handle Overdue Books (REQ-06)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Due date | <= today | BR001 (15/09/2024) | Mark "Overdue" |
| Due date | > today | BR003 (15/10/2024) | Do not mark overdue |
| User role | Librarian | LIB001 | View all overdue records |
| User role | Member | MEM002 | View only own overdue records |
| Check overdue action | Click "Check Overdue" | Librarian | System updates overdue status |

### IDM — Member Management (REQ-07)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Email format | Valid | newuser@test.com | Create member successful |
| Email format | Missing @ | newusertest.com | Display email error |
| Email format | Missing dot in domain | newuser@test | Display email error |
| Email format | Empty | "" | Display email error |
| Email exists? | Yes | librarian@library.com | Display duplicate error |
| Email exists? | No | abc123@test.com | Create member successful |
| Phone number | Valid | 0987654321 | Create member successful |
| Full name | Has data | Nguyen Van Anh | Create member successful |

### IDM — Borrow Record Inquiry (REQ-08)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| User role | Librarian | LIB001 | View all borrow records |
| User role | Member | MEM002 | View only own records |
| Member ID inquiry | Exists | MEM002 | Display record list |
| Member ID inquiry | Not exists | MEM999 | Empty or no data |
| Record status | Borrowing | BR003 | Display correct status |
| Record status | Returned | BR002 | Display correct status |
| Record status | Overdue | BR001 | Display correct status |

## Decision Table — Borrow Book (REQ-04)

| Condition | Rule 1 | Rule 2 | Rule 3 | Rule 4 |
|---|---|---|---|---|
| Book Available | Y | N | Y | Y |
| Member Active | Y | Y | N | Y |
| Borrow Count < 3 | Y | Y | Y | N |
| Result | Borrow | Reject | Reject | Reject |

---

## Step 2: Test Cases

<!-- Organize test case table: can group by function, by REQ, or by business flow — group decides. -->
<!-- Each TC must map back to at least 1 row in the IDM table in Step 1. -->

| TC ID | Test Objective | Precondition | Steps | Test Data | Expected Result | REQ | Technique |
| ----- | ------ | ----------- | -------- | --------- | ----------- | ------ | ------ |
| TC-01 | Login successful with Librarian account | At Login screen | 1. Enter email 2. Enter password 3. Click Login | librarian@library.com / admin123 | Login successful, display name and Librarian role | REQ-01 | EP |
| TC-02 | Login with non-existent email | At Login screen | Enter non-existent email and any password | noone@email.com / admin123 | Display "Member not found" | REQ-01 | EP |
| TC-03 | Login with wrong password | At Login screen | Enter valid email and wrong password | librarian@library.com / wrongpass | Display "Incorrect password" | REQ-01 | EP |
| TC-04 | Login with empty information | At Login screen | Leave email and password empty, click Login | "" | Display "Please enter email and password" | REQ-01 | EP |
| TC-05 | Member views book list | Logged in as Member | Open Books tab | MEM002 | Display book list | REQ-02 | EP |
| TC-06 | Librarian views book list | Logged in as Librarian | Open Books tab | LIB001 | Display book list | REQ-02 | EP |
| TC-07 | Book status updates after borrowing | Book available | Borrow BOOK001 | BOOK001 | Status changes from Available to Borrowed | REQ-02 | EP |
| TC-08 | Search by book name | Logged in successfully | Enter search keyword | Flutter | Display books containing Flutter | REQ-03 | EP |
| TC-09 | Search by author name | Logged in successfully | Search author | Nguyen Minh Duc | Display books by author | REQ-03 | EP |
| TC-10 | Search case-insensitive | Logged in successfully | Search | flutter | Results match Flutter | REQ-03 | EP |
| TC-11 | Filter by category | Logged in successfully | Select Technology category | Technology | Display only Technology books | REQ-03 | EP |
| TC-12 | Search with no results | Logged in successfully | Search non-existent keyword | XYZ123 | Display "No books found" | REQ-03 | EP |
| TC-13 | Borrow book successfully | Active member | Borrow BOOK001 | MEM002 + BOOK001 | Create borrow record successful | REQ-04 | Decision Table |
| TC-14 | Cannot borrow already borrowed book | BOOK003 is borrowed | Attempt to borrow | BOOK003 | Reject borrow | REQ-04 | Decision Table |
| TC-15 | Cannot borrow when member suspended | Logged in as MEM004 | Borrow BOOK001 | MEM004 | Display rejection due to suspension | REQ-04 | Decision Table |
| TC-16 | Cannot borrow when member expired | Logged in as MEM005 | Borrow BOOK001 | MEM005 | Display rejection due to expiration | REQ-04 | Decision Table |
| TC-17 | Member with 2 books can borrow more | Member has 2 books | Borrow 1 more | Borrow count = 2 | Allow borrow | REQ-04 | BVA |
| TC-18 | Member with 3 books cannot borrow more | Member has 3 books | Attempt to borrow | Borrow count = 3 | Reject, limit exceeded | REQ-04 | BVA |
| TC-19 | Check rejection message reason | Members MEM004 and MEM005 | Try to borrow | MEM004/MEM005 | Different messages for suspension vs expiration | REQ-04 | Decision Table |
| TC-20 | Return borrowed book successfully | Member has borrowed book | Return BOOK003 | BOOK003 | Return successful | REQ-05 | EP |
| TC-21 | Book status updates after return | Book returned | Check book list | BOOK003 | Status is Available | REQ-05 | EP |
| TC-22 | Return overdue book | Has overdue record | Return book | BR001 | Display overdue warning | REQ-05 | EP |
| TC-23 | Librarian checks overdue | Logged in as Librarian | Click "Check Overdue" | BR001 | System scans and updates | REQ-06 | EP |
| TC-24 | Overdue record marked correctly | Checked overdue | Check BR001 | BR001 | Status = Overdue | REQ-06 | EP |
| TC-25 | Member sees own overdue record | Logged in as MEM002 | Open borrow list | BR001 | Display Overdue status | REQ-06 | EP |
| TC-26 | Add member with valid email | Logged in as Librarian | Open Add Member | abc@test.com | Create successful | REQ-07 | EP |
| TC-27 | Email missing @ symbol | Logged in as Librarian | Create member | abctest.com | Display email error | REQ-07 | EP |
| TC-28 | Email missing dot in domain | Logged in as Librarian | Create member | abc@test | Display email error | REQ-07 | EP |
| TC-29 | Standard valid email | Logged in as Librarian | Create member | abc@domain.com | Create successful | REQ-07 | EP |
| TC-30 | Cannot use duplicate email | Logged in as Librarian | Create member | librarian@library.com | Display duplicate error | REQ-07 | EP |
| TC-31 | Librarian views all borrow records | Logged in as librarian | Open Borrow/Return tab | LIB001 | Display all records | REQ-08 | EP |
| TC-32 | Member views only own records | Logged in as MEM002 | Open Borrow/Return tab | MEM002 | Display only MEM002 records | REQ-08 | EP |
| TC-33 | Member cannot view other's records | Logged in as MEM002 | Search MEM003 | MEM003 | No data displayed for MEM003 | REQ-08 | EP |

---

## Summary

| Function Group | # TC | REQ Covered | IDM Technique Applied |
|----------------|-------|---------|----------------------|
| Login | 4 | REQ-01 | EP (valid email, correct password, empty input) |
| View Book List | 3 | REQ-02 | EP (role, book status, realtime update) |
| Search Book | 5 | REQ-03 | EP (book name, author, case-insensitive, category, no result) |
| Borrow Book | 7 | REQ-04 | Decision Table (3 conditions) + BVA (limit 3 books) |
| Return Book | 3 | REQ-05 | EP (successful return, status update, overdue return) |
| Handle Overdue | 3 | REQ-06 | EP (check due date, role, check overdue) |
| Member Management | 5 | REQ-07 | EP (valid/invalid email format, duplicate, phone, name) |
| Borrow Record Inquiry | 3 | REQ-08 | EP (role, member ID, record status) |
| **TOTAL** | **33** | **REQ-01 → REQ-08** | **EP (26 TC), Decision Table (5 TC), BVA (2 TC)** |