# Bug Reports — Bug Report

| Information | |
| --- | --- |
| **Group** | `Group 27` |
| **Report Date** | `11/06/2026` |

---

# BUG-01

| Attribute | Details |
| --- | --- |
| **Bug ID** | BUG-01 |
| **Related TC** | TC-18 |
| **Related REQ** | REQ-04 |
| **Severity** | High |
| **Found By** | Ngo Hai Huyen |
| **Found Date** | 11/06/2026 |
| **Status** | Open |

**Title:**
System allows member to borrow book when limit of 3 books is already reached.

**Steps to Reproduce:**

1. Login with member account.
2. Ensure member already has 3 books borrowed.
3. Select a book with "Available" status.
4. Click "Borrow Book" button.

**Expected Result:**
System rejects borrow and displays message about exceeding 3-book limit.

**Actual Result:**
System creates new borrow record successfully.

**Impact:**
Violates core business rule of the library.

**Proposed Fix:**
Check number of borrowed books before creating new borrow record.

---

# BUG-02

| Attribute | Details |
| --- | --- |
| **Bug ID** | BUG-02 |
| **Related TC** | TC-19 |
| **Related REQ** | REQ-04 |
| **Severity** | Medium |
| **Found By** | Do Thanh Tung |
| **Found Date** | 11/06/2026 |
| **Status** | Open |

**Title:**
Rejection message for borrowing does not display correct reason.

**Steps to Reproduce:**

1. Login with suspended or expired member account.
2. Select available book.
3. Attempt to borrow book.

**Expected Result:**
System displays correct rejection reason (Suspended or Expired).

**Actual Result:**
System displays generic message or incorrect reason.

**Impact:**
User cannot identify exact cause of rejection.

**Proposed Fix:**
Separate error cases and display correct message according to SRS.

---

# BUG-03

| Attribute | Details |
| --- | --- |
| **Bug ID** | BUG-03 |
| **Related TC** | TC-22 |
| **Related REQ** | REQ-05 |
| **Severity** | Medium |
| **Found By** | Nguyen Ngoc Trung |
| **Found Date** | 11/06/2026 |
| **Status** | Open |

**Title:**
No warning displayed when returning overdue book.

**Steps to Reproduce:**

1. Login with account having overdue books.
2. Return overdue book.
3. Observe system message.

**Expected Result:**
Display warning that book is returned after due date.

**Actual Result:**
Book is returned successfully but no overdue warning appears.

**Impact:**
Librarian cannot identify late returns.

**Proposed Fix:**
Compare return date with dueDate and display corresponding warning.

---

# BUG-04

| Attribute | Details |
| --- | --- |
| **Bug ID** | BUG-04 |
| **Related TC** | TC-24 |
| **Related REQ** | REQ-06 |
| **Severity** | High |
| **Found By** | Nguyen The Anh |
| **Found Date** | 11/06/2026 |
| **Status** | Open |

**Title:**
"Check Overdue" function does not mark overdue borrow records.

**Steps to Reproduce:**

1. Login with Librarian account.
2. Select "Check Overdue" function.
3. Observe borrow list.

**Expected Result:**
Borrow records with dueDate ≤ today are changed to "Overdue" status.

**Actual Result:**
Record status does not change.

**Impact:**
Library cannot track overdue loans.

**Proposed Fix:**
Review logic for updating overdue status.

---

# BUG-05

| Attribute | Details |
| --- | --- |
| **Bug ID** | BUG-05 |
| **Related TC** | TC-26 |
| **Related REQ** | REQ-07 |
| **Severity** | High |
| **Found By** | Duong Dinh Phong |
| **Found Date** | 11/06/2026 |
| **Status** | Open |

**Title:**
Cannot add new member with valid data.

**Steps to Reproduce:**

1. Login with Librarian account.
2. Open Add Member screen.
3. Enter valid full name, email and phone number.
4. Click Save.

**Expected Result:**
New member is created successfully.

**Actual Result:**
System rejects or does not save data.

**Impact:**
Cannot manage new members.

**Proposed Fix:**
Review member data saving process.

---

# BUG-06

| Attribute | Details |
| --- | --- |
| **Bug ID** | BUG-06 |
| **Related TC** | TC-28 |
| **Related REQ** | REQ-07 |
| **Severity** | Medium |
| **Found By** | Duong Dinh Phong |
| **Found Date** | 11/06/2026 |
| **Status** | Open |

**Title:**
Invalid email (missing dot in domain) is still accepted.

**Steps to Reproduce:**

1. Login with Librarian account.
2. Add new member.
3. Enter email in format user@domain (without dot).
4. Click Save.

**Expected Result:**
Display email validation error.

**Actual Result:**
System accepts invalid email.

**Impact:**
Member data lacks accuracy guarantee.

**Proposed Fix:**
Add email format validation according to SRS.

---

# BUG-07

| Attribute | Details |
| --- | --- |
| **Bug ID** | BUG-07 |
| **Related TC** | TC-29 |
| **Related REQ** | REQ-07 |
| **Severity** | High |
| **Found By** | Do Thanh Tung |
| **Found Date** | 11/06/2026 |
| **Status** | Open |

**Title:**
Valid email is rejected when creating new member.

**Steps to Reproduce:**

1. Login with Librarian account.
2. Add new member.
3. Enter valid email.
4. Click Save.

**Expected Result:**
Create member successfully.

**Actual Result:**
System reports email is invalid.

**Impact:**
Prevents adding valid members.

**Proposed Fix:**
Review email format validation logic.

---

# BUG-08

| Attribute | Details |
| --- | --- |
| **Bug ID** | BUG-08 |
| **Related TC** | TC-30 |
| **Related REQ** | REQ-07 |
| **Severity** | Medium |
| **Found By** | Nguyen Ngoc Trung |
| **Found Date** | 11/06/2026 |
| **Status** | Open |

**Title:**
Error message for duplicate email is inaccurate.

**Steps to Reproduce:**

1. Login with Librarian account.
2. Add member with existing email.
3. Click Save.

**Expected Result:**
Display message indicating email already exists.

**Actual Result:**
Error message is unclear or does not match the cause.

**Impact:**
Confuses user.

**Proposed Fix:**
Display specific message according to SRS requirements.

---

# BUG-09

| Attribute | Details |
| --- | --- |
| **Bug ID** | BUG-09 |
| **Related TC** | TC-33 |
| **Related REQ** | REQ-08 |
| **Severity** | High |
| **Found By** | Nguyen The Anh |
| **Found Date** | 11/06/2026 |
| **Status** | Open |

**Title:**
Member can view borrow records of other members.

**Steps to Reproduce:**

1. Login with member account.
2. Access borrow record inquiry function.
3. Enter other member's ID or perform data access action.

**Expected Result:**
Only display borrow records of logged-in user.

**Actual Result:**
Display borrow records of other members.

**Impact:**
Violates access control and leaks user data.

**Proposed Fix:**
Verify user permissions before returning borrow record data.
