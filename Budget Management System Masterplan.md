# Budget Management System (BMS) - Project Masterplan

## Overview
A three-tier GCP-based application for managing annual operating plans (AOPs), budgets, and purchase orders. The system supports organization hierarchy management and provides comprehensive reporting capabilities.

## Project Objectives
- Create a cost-effective budget management solution (GCP costs under $50)
- Support management of Annual Operating Plans (AOPs)
- Enable budget allocation and tracking
- Provide organizational hierarchy visualization
- Track purchase order details against budgets
- Generate hierarchical budget reports

## Target Audience
- Small team of 10 users
- Employees managing budgets
- Managers viewing organizational budgets
- Finance team managing AOPs

## Core Features

### Authentication
- Simple authentication using passphrase "IKnowYou241202"
- Future extensibility for more robust authentication

### User Interface
#### Dashboard (Landing Page)
- Welcome message with user information
- Header with user button, help, and logout options
- Key metrics display:
  - Active AOP status and utilization
  - Recent purchase orders
  - Quick access to reports
- Manual refresh functionality

#### Navigation
- Clear button-based navigation to main functions:
  - Cost Centers
  - Employees
  - AOPs
  - Budgets
  - Purchase Orders
  - Reports

### Core Modules

#### Cost Center Management
- CRUD operations for cost centers
- Display cost center code and name
- List view with search/filter capabilities

#### Employee Management
- CRUD operations for employees
- Employee attributes: LDAP, name, email, level (1-12), cost center
- Organization hierarchy management
- Traditional org chart visualization

#### AOP Management
- CRUD operations for AOP header and details
- States: Draft, Active, EOL
- Single active AOP enforcement
- Total amount tracking and validation
- Cost center budget allocation

#### Budget Management
- CRUD operations for budget lines
- Unique identifier per budget line
- Project details, description, amount tracking
- Employee assignment
- Budget vs. actual tracking

#### Purchase Order Integration
- Integration with external PO system
- PO details tracking: number, line items, amounts
- Budget utilization updates

### Reporting
- "Show My Organization" - Traditional org chart view
- "Show My Budget" - Hierarchical budget summary
- Export capabilities (if time permits)

## Technical Architecture

### Frontend (Presentation Layer)
- Angular JS
- Responsive design for web access
- Component-based architecture
- Manual refresh mechanism for data updates

### Backend (Business Logic Layer)
- Python
- RESTful APIs for all CRUD operations
- Business logic implementation:
  - AOP state management
  - Budget calculations
  - Organization hierarchy traversal
  - Report generation

### Database (Data Persistence Layer)
- Cloud Spanner
- Schema optimization for:
  - Organization hierarchy
  - Budget tracking
  - Purchase order details

## Development Phases

### Phase 1: Foundation
- Basic authentication
- Core database schema
- Employee and cost center management
- Organization hierarchy visualization

### Phase 2: Budget Management
- AOP management
- Budget line creation
- Purchase order integration
- Basic reporting

### Phase 3: Dashboard & Polish
- Dashboard implementation
- Advanced reporting
- UI refinements
- Performance optimization

## Technical Considerations

### Data Modeling
Key entities:
- Cost Centers
- Employees
- Organization Hierarchy
- AOP (Header & Detail)
- Budgets
- Purchase Orders

### Performance Optimizations
- Efficient hierarchy queries
- Materialized budget totals
- Cached organization structure

### Security Considerations
- Basic authentication for hackathon
- Future security enhancements noted
- Data access controls

## Future Enhancements
- Enhanced authentication system
- Approval workflows
- Real-time updates
- Advanced reporting features
- Audit logging
- Data export capabilities

## Constraints
- GCP costs under $50
- Support for 1000 budget lines
- 10 user capacity
- Single active AOP
