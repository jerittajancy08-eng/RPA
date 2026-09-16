<screenshot_instructions>
## Screenshot Analysis

Screenshot shows application with target element highlighted in green. Use it to identify visual context for choosing reliable attributes.

**What to look for:**
    **Labels and headers near the target** - Often map to `name`, `aria-label`, or `labeledby` attributes
    **Named parent containers** - Dialogs, panels, sections with stable titles/names providing anchoring context
    **Table structure** - If in table, use visible column/row headers instead of numeric indices

**What to avoid:**
    Visible dynamic content (numbers, dates, user names, status messages) - Use wildcards or avoid these attributes
    Content in editable fields (placeholders, data) - Current value will change

**Examples:**

  1. **Table cell scenario**
     - Screenshot shows: Cell with ""$75,000"" in row with ""John Doe"" under column ""Salary""
     - ❌ Bad: Visually counted row/column numbers → `tableRow='3' tableCol='2'` (breaks when data changes)
     - ✓ Good: Visible headers → `rowName='John Doe' colName='Salary'`

  2. **Input field with placeholder**
     - Screenshot shows: Empty text field with grayed-out text ""Enter your email address""
     - ❌ Bad: Placeholder text → `visibleinnertext='Enter your email address'` (placeholders change frequently)
     - ✓ Good: Visible label above/beside it → `aria-label='Email'` or `labeledby` attribute

  3. **Editable field with current value**
     - Screenshot shows: Text field containing ""John Smith"" with label ""Full Name:""
     - ❌ Bad: Current content → `name='John Smith'` (user input changes)
     - ✓ Good: Stable identifier → `automationid='*fullName*'` or `labeledby='fullNameLabel'`

  4. **Button inside a dialog**
     - Screenshot shows: ""Save"" button in dialog titled ""Edit Employee Details""
     - ❌ Bad: Button alone → `<webctrl tag='BUTTON' visibleinnertext='Save' />` (many ""Save"" buttons exist)
     - ✓ Good: Anchor to dialog → `<webctrl role='dialog' aria-label='Edit Employee Details' /><webctrl tag='BUTTON' visibleinnertext='Save' />`

</screenshot_instructions>