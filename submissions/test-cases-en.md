# Test Cases — Test Case Table

> **Instructions**: Write at least **20 test cases** covering the main features (REQ-01 → REQ-08).
> See [examples/sample-test-case.md](../examples/sample-test-case.md) for guidance on writing good test cases.
> Organize and group test cases in the most logical way for your team.

| Information | |
|---|---|
| **Group** | `Group 27` |
| **Creation date** | `22/04/2026` |
| **System** | https://stqa.rbc.vn |
| **Reference** | SRS v1.0 |

---

## Step 1: Input Domain Modeling (IDM)

> Textbook: Chapter 6 — Input Domain Modeling, Paul Ammann & Jeff Offutt.
>
> Before writing Test Cases, the team MUST analyze the input domain using the IDM tables below.
> For each feature identify: **Characteristic**, **Partition (Block)**, and **Representative Value**.

### IDM — Login (REQ-01)

| Characteristic | Partition (Block) | Representative Value | Expected Result |
|---|---|---|---|
| Is the email present in DB? | Yes | librarian@library.com | Login success |
| | No | noone@email.com | Show error message |
| Is the password correct? | Correct | admin123 | Login success |
| | Incorrect | wrongpass | Show error message |
| Is the input field empty? | Not empty | (any value) | Normal processing |
| | Empty | "" | Show "Please enter..." |

### IDM — View Book List (REQ-02)

| Characteristic | Partition (Block) | Representative Value | Expected Result |
|---|---|---|---|
| User role | Librarian | LIB001 | Can view book list |
| User role | Member | MEM002 | Can view book list |
| Book status | Available | BOOK001 | Display "Available" |
| Book status | Borrowed | BOOK003 | Display "Borrowed" |
| Book status | Lost | BOOK007 | Display "Lost" |
| Status change | After borrowing | BOOK001 | Updates to "Borrowed" in realtime |
| Status change | After returning | BOOK003 | Updates to "Available" in realtime |

### IDM — Search Books (REQ-03)

| Characteristic | Partition (Block) | Representative Value | Expected Result |
|---|---|---|---|
| Keyword exists in DB? | Yes (title) | "Flutter" | Shows books containing "Flutter" |
| | Yes (author) | "Nguyễn" | Shows books by author Nguyễn |
| | No | "XYZ123" | Empty result list |
| Case sensitivity? | Lowercase | "flutter" | Same results as "Flutter" |
| | Uppercase | "FLUTTER" | Same results as "Flutter" |

### IDM — Borrow Book (REQ-04, REQ-05)

| Characteristic | Partition (Block) | Representative Value | Expected Result |
|---|---|---|---|
| Book status? | Available | BOOK001 | Allow borrow |
| | Borrowed | BOOK003 | Not allowed |
| | Lost | BOOK007 | Not allowed |
| Member status? | Active | MEM002 | Allow borrow |
| | Suspended | MEM004 | Reject with error |
| | Expired | MEM005 | Reject with error |
| Number of currently borrowed books? | < 3 (BVA: 0,1,2) | MEM006 (0 books) | Allow borrow |
| | = 3 (BVA: limit) | Member has borrowed 3 books | Reject, show limit exceeded |

### IDM — Return Book (REQ-05)

| Characteristic | Partition (Block) | Representative Value | Expected Result |
|---|---|---|---|
| Is the member currently borrowing the book? | Yes | MEM002 - BOOK003 | Allow return |
| Is the member currently borrowing the book? | No | MEM002 - BOOK013 | Disallow return |
| Loan record status | On time | BR003 | Successful return |
| Loan record status | Overdue | BR001 | Successful return + overdue warning |
| Book status after return | Borrowed | BOOK003 | Changes to "Available" |

### IDM — Overdue Handling (REQ-06)

| Characteristic | Partition (Block) | Representative Value | Expected Result |
|---|---|---|---|
| Due date | <= current date | BR001 (15/09/2024) | Mark as "Overdue" |
| Due date | > current date | BR003 (15/10/2024) | Not marked overdue |
| User role | Librarian | LIB001 | See all overdue records |
| User role | Member | MEM002 | See only their own overdue records |
| Overdue check action | Click "Check Overdue" | Librarian | System updates overdue statuses |

### IDM — Member Management (REQ-07)

| Characteristic | Partition (Block) | Representative Value | Expected Result |
|---|---|---|---|
| Email format | Valid | newuser@test.com | Member creation success |
| Email format | Missing @ | newusertest.com | Show email error |
| Email format | Missing dot in domain | newuser@test | Show email error |
| Email format | Empty | "" | Show email error |
| Email already exists? | Yes | librarian@library.com | Show duplicate email error |
| Email already exists? | No | abc123@test.com | Member creation success |
| Phone number | Valid | 0987654321 | Member creation success |
| Full name | Present | Nguyễn Văn A | Member creation success |

### IDM — Lookup Loan Records (REQ-08)

| Characteristic | Partition (Block) | Representative Value | Expected Result |
|---|---|---|---|
| User role | Librarian | LIB001 | See all loan records |
| User role | Member | MEM002 | See only their own loan records |
| Member ID lookup | Exists | MEM002 | Display loan list |
| Member ID lookup | Not exists | MEM999 | No data or empty list |
| Loan record status | Borrowed | BR003 | Show correct status |
| Loan record status | Returned | BR002 | Show correct status |
| Loan record status | Overdue | BR001 | Show correct status |

## Decision Table — Borrow Book (REQ-04)

| Condition | Rule 1 | Rule 2 | Rule 3 | Rule 4 |
|---|---|---|---|---|
| Book Available | Y | N | Y | Y |
| Member Active | Y | Y | N | Y |
| Borrow Count < 3 | Y | Y | Y | N |
| Result | Borrow | Reject | Reject | Reject |

> Technical hint: use Equivalence Partitioning (EP) for discrete partitions and Boundary Value Analysis (BVA) for numeric partitions (e.g., borrow limit 3). See textbook §6.1–6.3.

---

## Step 2: Test Cases

<!-- Organize the test case table as you prefer: by feature, by REQ, or by workflow. -->
<!-- Each TC must map back to at least one row in the IDM tables from Step 1. -->

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
| ----- | -------------- | ------------ | ----- | ---------- | --------------- | --- | --------- |
| TC-01 | Successful login with Librarian account | On Login screen | 1. Enter email 2. Enter password 3. Click Login | librarian@library.com / admin123 | Login successful; display name and Librarian role | REQ-01 | EP |
| TC-02 | Login with non-existing email | On Login screen | Enter non-existing email and any password | noone@email.com / admin123 | Show "Member not found" | REQ-01 | EP |
| TC-03 | Login with incorrect password | On Login screen | Enter valid email and wrong password | librarian@library.com / wrongpass | Show "Incorrect password" | REQ-01 | EP |
| TC-04 | Login with empty inputs | On Login screen | Leave email and password empty, click Login | "" | Show "Please enter email and password" | REQ-01 | EP |
| TC-05 | Member views book list | Logged in as Member | Open Books tab | MEM002 | Displays book list | REQ-02 | EP |
| TC-06 | Librarian views book list | Logged in as Librarian | Open Books tab | LIB001 | Displays book list | REQ-02 | EP |
| TC-07 | Book status updates after borrowing | Book is Available | Borrow BOOK001 | BOOK001 | Status changes from Available to Borrowed | REQ-02 | EP |
| TC-08 | Search by book title | Logged in | Enter search keyword | Flutter | Shows books containing Flutter | REQ-03 | EP |
| TC-09 | Search by author | Logged in | Search by author name | Nguyễn Minh Đức | Shows books by the author | REQ-03 | EP |
| TC-10 | Case-insensitive search | Logged in | Search using lowercase | flutter | Same results as Flutter | REQ-03 | EP |
| TC-11 | Filter by category | Logged in | Select category Technology | Technology | Displays only Technology books | REQ-03 | EP |
| TC-12 | No search results | Logged in | Search unknown keyword | XYZ123 | Show "No books found" | REQ-03 | EP |
| TC-13 | Successful borrow | Member active | Borrow BOOK001 | MEM002 + BOOK001 | Creates loan record successfully | REQ-04 | Decision Table |
| TC-14 | Cannot borrow already borrowed book | BOOK003 is borrowed | Attempt borrow | BOOK003 | Borrow request rejected | REQ-04 | Decision Table |
| TC-15 | Cannot borrow when member suspended | Logged in as MEM004 | Attempt borrow BOOK001 | MEM004 | Reject with suspension message | REQ-04 | Decision Table |
| TC-16 | Cannot borrow when member expired | Logged in as MEM005 | Attempt borrow BOOK001 | MEM005 | Reject with expiration message | REQ-04 | Decision Table |
| TC-17 | Member with 2 borrowed books can borrow one more | Member has 2 books | Borrow another book | Borrow count = 2 | Allow borrow | REQ-04 | BVA |
| TC-18 | Member with 3 borrowed books cannot borrow more | Member has 3 books | Attempt borrow | Borrow count = 3 | Reject, limit exceeded | REQ-04 | BVA |
| TC-19 | Check correct rejection reason messages | MEM004 and MEM005 | Attempt borrow | MEM004 / MEM005 | Different messages for suspended vs expired | REQ-04 | Decision Table |
| TC-20 | Successful return of a borrowed book | Member is borrowing the book | Return BOOK003 | BOOK003 | Return successful | REQ-05 | EP |
| TC-21 | Book status updates after return | Book returned | Check book list | BOOK003 | Status becomes Available | REQ-05 | EP |
| TC-22 | Return overdue book | Overdue loan record exists | Return book | BR001 | Show overdue warning | REQ-05 | EP |
| TC-23 | Librarian triggers overdue check | Logged in as Librarian | Click "Check Overdue" | BR001 | System scans and updates overdue statuses | REQ-06 | EP |
| TC-24 | Overdue records are marked correctly | Overdue check executed | Inspect BR001 | BR001 | Status = Overdue | REQ-06 | EP |
| TC-25 | Member sees their own overdue records | Logged in as MEM002 | Open loan records list | BR001 | Shows Overdue status | REQ-06 | EP |
| TC-26 | Add member with valid email | Logged in as Librarian | Open Add Member | abc@test.com | Creation successful | REQ-07 | EP |
| TC-27 | Email missing @ symbol | Logged in as Librarian | Create member | abctest.com | Show email format error | REQ-07 | EP |
| TC-28 | Email missing dot in domain | Logged in as Librarian | Create member | abc@test | Show email format error | REQ-07 | EP |
| TC-29 | Valid standard email | Logged in as Librarian | Create member | abc@domain.com | Creation successful | REQ-07 | EP |
| TC-30 | Duplicate email not allowed | Logged in as Librarian | Create member | librarian@library.com | Show duplicate email error | REQ-07 | EP |
| TC-31 | Librarian views all loan records | Logged in as Librarian | Open Loans tab | LIB001 | Shows all loan records | REQ-08 | EP |
| TC-32 | Member views only their loan records | Logged in as MEM002 | Open Loans tab | MEM002 | Shows only MEM002 records | REQ-08 | EP |
| TC-33 | Member cannot view others' loan records | Logged in as MEM002 | Lookup MEM003 | MEM003 | No data shown for MEM003 | REQ-08 | EP |

---

## Summary

| Feature group | Number of TC | REQs covered | IDM techniques applied |
|---------------|-------------:|------------:|----------------------|
| | | | |
| **Total** | **<!-- ≥ 20 -->** | | |
