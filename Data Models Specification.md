# Data Models Specification

## Core Entities

### CostCenter
```
- cost_center_code (STRING, Primary Key)
- cost_center_name (STRING)
- created_at (TIMESTAMP)
- updated_at (TIMESTAMP)
```

### Employee
```
- ldap (STRING, Primary Key)
- first_name (STRING)
- last_name (STRING)
- email (STRING)
- level (INT) # 1-12
- cost_center_code (STRING, Foreign Key -> CostCenter)
- created_at (TIMESTAMP)
- updated_at (TIMESTAMP)
```

### OrganizationHierarchy
```
- id (STRING, Primary Key)
- employee_ldap (STRING, Foreign Key -> Employee)
- manager_ldap (STRING, Foreign Key -> Employee)
- created_at (TIMESTAMP)
- updated_at (TIMESTAMP)
```

### AOPHeader
```
- id (STRING, Primary Key)
- name (STRING) # e.g., "AOP2024"
- total_amount (DECIMAL)
- state (ENUM) # ["DRAFT", "ACTIVE", "EOL"]
- created_at (TIMESTAMP)
- updated_at (TIMESTAMP)
```

### AOPDetail
```
- id (STRING, Primary Key)
- aop_header_id (STRING, Foreign Key -> AOPHeader)
- cost_center_code (STRING, Foreign Key -> CostCenter)
- amount (DECIMAL)
- created_at (TIMESTAMP)
- updated_at (TIMESTAMP)
```

### Budget
```
- id (STRING, Primary Key)
- aop_header_id (STRING, Foreign Key -> AOPHeader)
- employee_ldap (STRING, Foreign Key -> Employee)
- project (STRING)
- description (STRING)
- amount (DECIMAL)
- total_po_amount (DECIMAL) # Running total of all purchase orders
- created_at (TIMESTAMP)
- updated_at (TIMESTAMP)
```

### PurchaseOrder
```
- po_number (STRING)
- po_line_number (STRING)
- budget_id (STRING, Foreign Key -> Budget)
- requestor_ldap (STRING, Foreign Key -> Employee)
- purchase_item (STRING)
- amount (DECIMAL)
- date (DATE)
- created_at (TIMESTAMP)
- updated_at (TIMESTAMP)
Primary Key: (po_number, po_line_number)
```

## Key Relationships and Constraints

### Organization Hierarchy
- Each employee can have one manager
- One manager can have multiple reports
- No circular references allowed in hierarchy

### AOP Management
- Only one AOP can be in ACTIVE state at a time
- Total amount in AOPHeader must equal sum of amounts in related AOPDetail records
- When in ACTIVE state, modifications only allowed through special processes

### Budget Controls
- Total budget amounts for an AOP cannot exceed the AOP total_amount when AOP is ACTIVE
- Each budget is associated with one employee and inherits their cost center
- total_po_amount in Budget is maintained as a running total for efficient querying

### Purchase Orders
- Total PO amounts for a budget cannot exceed the budget amount
- Composite key on po_number and po_line_number for unique identification
- Each PO must reference a valid budget

## Indexes

### Primary Indexes
- CostCenter: cost_center_code
- Employee: ldap
- OrganizationHierarchy: id
- AOPHeader: id
- AOPDetail: id
- Budget: id
- PurchaseOrder: (po_number, po_line_number)

### Secondary Indexes
- Employee: cost_center_code, email
- OrganizationHierarchy: employee_ldap, manager_ldap
- AOPDetail: aop_header_id, cost_center_code
- Budget: aop_header_id, employee_ldap
- PurchaseOrder: budget_id, requestor_ldap

## Notes on Implementation

### Cloud Spanner Considerations
- Interleave PurchaseOrder records in parent Budget table for better performance
- Use strong consistency for financial data integrity
- Implement appropriate backup and recovery strategies

### Performance Optimizations
1. Materialized Views
   - Budget totals by cost center
   - Organization hierarchy paths
   - AOP summary statistics

2. Denormalized Fields
   - total_po_amount in Budget table
   - total_amount in AOPHeader

### Data Validation Rules
1. Employee
   - Level must be between 1 and 12
   - Email format validation
   - LDAP format validation

2. Financial Amounts
   - All amounts must be positive
   - Precision set to handle US dollars (2 decimal places)

3. State Transitions
   - Valid AOP states: DRAFT -> ACTIVE -> EOL
   - State transition validation in application logic

### Audit Trail
Consider adding for each table:
- created_by (STRING)
- updated_by (STRING)
- version (INT)

This would help with tracking changes and maintaining data integrity.
