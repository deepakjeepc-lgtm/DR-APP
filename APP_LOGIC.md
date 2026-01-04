# Detailed App Specifications & Rules

## 1. User Roles & Authentication
*   **Login:** EMAIL PASS.
*   **Roles:**
    *   **Admin (Operator):** Users 'Deepak'.
    *   **Member:** All others.
    *   **Constraint:** Admins must have their own personal dashboard (like a member) to pay their own EMI/Deposits.
*   **Multi-Account:** Users can have multiple IDs (e.g., Rupesh 01, Rupesh 02) and switch between them via the Profile Menu.

## 2. Navigation Structure
*   **Bottom Bar (Icons Only - No Text Labels):**
    1.  **Group Data:** Global stats (Total Fund, Medical Reserve, Active Loans).
    2.  **Home:** Personal User Dashboard.
    3.  **History:** Global Transaction Timeline.
    4.  **Admin Panel:** Visible **ONLY** to Admins.

## 3. Financial Logic (The Core)

### A. Contributions
*   **Amount:** ₹500/month mandatory.
*   **Late Fines:**
    *   1st-4th: ₹0
    *   5th-10th: ₹50
    *   11th-20th: ₹100
    *   21st-End: ₹150
*   **Payment Flow:** User clicks "Pay Now" (UPI Intent) -> Confirms in App -> Status 'Pending' -> Admin Approves -> Status 'Success'.

### B. Medical Emergency Fund (Article 7)
*   **Logic:** 20% of the `Total Collection` must be kept separate as `Medical Reserve`.
*   **Usage:** Loans marked as "Medical Emergency" are deducted from this reserve, not the main fund.

### C. Loan Logic (Admin Panel)
*   **Interest Rates:**
    *   Member: 1% (0-6 months), 1.5% (6-12m), 3% (12m+).
    *   Non-Member: 3% (Standard), requires a Guarantor from the group.
*   **EMI Types:**
    1.  **Standard EMI:** Principal + Interest divided by months.
    2.  **Interest Only:** User pays only interest every 30/60 days.
*   **Fines:**
    *   Missed EMI Date: ₹50 flat fine.
    *   Unpaid Month: Fine increases and is added to the total due.

### D. New Member Logic (Auto-Calculation)
*   When adding a member mid-year (e.g., Joining in June 2026):
    *   **Formula:** (Months Passed * ₹500) + (Months Passed * ₹150 Late Fee).
    *   This amount is calculated automatically and added to the fund immediately upon creation.

## 4. Database Structure (Firestore)
*   `users`: Stores Profile, Role, Total Deposit, Loan Status.
*   `loans`: Active loan details, EMI schedule, Type (Medical/Standard).
*   `transactions`: Holding area for payments waiting for Admin approval.
*   `global_activity`: Flattened history of all events (Money In/Out) for the History Tab.

## 5. UI Requirements
*   **Theme:** Dark/Light mode support.
*   **Transparency:** No Edit/Delete buttons for Members. Only "View" access.
*   **Profile Icon:** Top-right on AppBar (Switch Account, Help, Details).
