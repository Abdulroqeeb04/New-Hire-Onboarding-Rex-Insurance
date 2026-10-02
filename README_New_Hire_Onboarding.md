# New Hire Onboarding Process

A digital employee onboarding workflow built to streamline the end-to-end onboarding of new hires — from HR initiation and employee data collection to approvals, IT provisioning, document signing, automated document generation, and onboarding completion.

> **Project stage:** Implemented in development and prepared for User Acceptance Testing (UAT).

---

## Overview

The **New Hire Onboarding Process** was designed to replace a largely manual, fragmented onboarding experience with a structured digital workflow.

The solution connects an external new-hire onboarding portal with **Kissflow**, allowing Human Capital Management (HCM), departmental stakeholders, IT, supervisors, and the new employee to complete their respective onboarding activities within one coordinated process.

The workflow also creates an audit trail of approvals, provisioning activities, signatures, and generated onboarding documents.

---

## Problem Statement

New employee onboarding often requires information and actions from several teams, including:

- Human Capital Management
- Head of HCM
- Staff Welfare
- BCC
- Facilities
- CDIO
- Supervisors / Heads of Department
- IT
- The new employee

Managing these activities manually can result in:

- Delayed onboarding
- Repeated data collection
- Poor visibility into onboarding progress
- Missing approvals
- Inconsistent employee records
- Difficulty tracking responsibility
- Manual document preparation
- Weak audit traceability

This project centralizes those activities into a controlled workflow.

---

## Objectives

The solution was designed to:

- Digitize the new-hire onboarding process.
- Allow HR to initiate onboarding from Kissflow.
- Send the new hire a unique onboarding link automatically.
- Capture employee information through an external multi-step onboarding form.
- Synchronize submitted information back into Kissflow.
- Route approvals and provisioning tasks to the correct stakeholders.
- Dynamically assign supervisor approval using the employee's department.
- Support conditional routing based on employee requirements and office location.
- Capture approval signatures and employee document signatures.
- Automatically generate completed onboarding documents.
- Provide HR with visibility into onboarding status.
- Create a traceable record for audit and compliance review.

---

## Solution Architecture

```text
HR / HCM
   |
   | Initiates onboarding request
   v
Kissflow New Hire Onboarding Process
   |
   | Generates unique onboarding link
   v
Automated Email
   |
   v
External New Hire Portal
   |
   | Employee completes onboarding information
   v
Webhook / Integration
   |
   v
Kissflow Employee Record
   |
   +-------------------------------+
   |                               |
   v                               v
HR Employee Details          Head HCM Approval
                                   |
                                   v
                         Parallel Department Tasks
                    /        |        |        \
                 Welfare     BCC   Facilities   CDIO
                                                |
                                                v
                                      Supervisor Approval
                                                |
                                                v
                                           IT Provisioning
                                                |
                                                v
                                         HR Final Review
                                                |
                                                v
                                      New Hire Document Signing
                                                |
                                                v
                                  Generated Documents + Completion
```

---

## End-to-End Workflow

### 1. HR Initiates the Onboarding Request

HR starts a new onboarding request in the Human Capital Management application.

Requester information is populated where applicable, including fields such as:

- Staff ID
- Requester
- Designation
- Department
- Unit
- Year
- Date Created

HR indicates whether the new hire's information is already available.

When the information is not yet available, HR provides the new hire's email address.

---

### 2. Automated New Hire Email

An integration automatically sends the new hire a welcome email containing a unique onboarding link.

The generated link follows the pattern:

```text
/new-hire/{process-instance-id}
```

The process instance ID ensures that the employee's submission is linked to the correct Kissflow onboarding request.

No production credentials or internal configuration values are stored in this public documentation.

---

### 3. External 11-Step Onboarding Form

The new hire completes a multi-step onboarding form outside Kissflow.

The form captures information across areas such as:

- Personal information
- Dependents
- Parents / guardians
- Emergency contact
- Secondary education
- Tertiary education / certificate verification
- Personal references
- Previous employment
- Payee / banking information
- Beneficiary information
- Review and submission

The form includes required-field validation and conditional logic.

Examples include:

- Disability details displayed only when applicable.
- Spouse information displayed when the employee is married.
- Previous-employer details displayed only when previous employment exists.
- State of Origin captured where applicable.
- Beneficiary allocation validated to ensure the total equals 100%.

---

## Data Synchronization

After the external form is submitted, a webhook-based integration sends the employee information back to Kissflow.

The integration updates the original onboarding process instance instead of creating an unrelated record.

Mapped information includes relevant:

- Personal information
- Contact details
- Dependents
- Parent / guardian information
- Emergency contacts
- Education history
- References
- Employment history
- Banking / payee information
- Beneficiary information

This keeps the onboarding process tied to one employee record throughout the workflow.

---

## HR Employee Setup

After employee information is received, HR completes employment-related information such as:

- Resumption date
- Department
- Unit
- Job title
- Buddy staff
- Office location
- Employment / staff type
- Required system access
- Other specifications

The employee's access requirements can include items such as:

- Network access
- Microsoft Office
- Office email
- Biometrics
- Other required applications or services

---

## Dynamic Supervisor Assignment

Supervisor approval is assigned dynamically using a **Head of Department lookup**.

The logic follows:

```text
New Hire Department
        |
        v
Head of Departments Data Source
        |
        v
Matching Department
        |
        v
HOD / Manager
        |
        v
Supervisor Approval Task
```

The lookup returns the HOD/Manager associated with the new hire's department, and the workflow uses that user as the assignee for the supervisor approval step.

This avoids manually maintaining a supervisor on every onboarding request.

---

## Approval Workflow

### Head HCM

Head HCM reviews the employee and job information, provides concurrence, signs, and approves the onboarding request.

After approval, the workflow launches multiple onboarding activities.

### Parallel Departmental Activities

The following activities can run concurrently:

| Role | Responsibility |
|---|---|
| Staff Welfare Officer | Employee / welfare profile setup |
| BCC Officer | ID card and welcome-pack readiness |
| Facilities Officer | Workstation, facility and vehicle allocation where applicable |
| CDIO | Technology / access concurrence |

Running these tasks in parallel helps reduce onboarding turnaround time.

---

## IT and Access Provisioning

Following technology approval, the workflow supports:

1. Supervisor review of required roles and permissions.
2. IT profile / directory setup.
3. Employee credential creation where applicable.
4. Laptop setup based on onboarding requirements.
5. Conditional routing for Head Office and Branch Office employees.

IT-related fields are intentionally separated from employee financial and dependent information to support least-privilege access.

---

## Role-Based Access Model

The process is designed around functional roles rather than specific individuals.

| Role | Primary Responsibility |
|---|---|
| HCM Process Administrator | Application configuration and administration |
| HR / HCM Onboarding Officer | Initiation, employee setup and final review |
| New Hire | Information submission and document signing |
| Head HCM | Formal concurrence and approval |
| Staff Welfare Officer | Welfare / employee profile setup |
| BCC Officer | ID card and welcome-pack preparation |
| Facilities Officer | Physical workplace allocation |
| CDIO | Technology concurrence |
| Supervisor / HOD | Roles and permissions review |
| IT Officer | Account, credential and device provisioning |
| Buddy | Post-onboarding employee support |
| Auditor / Internal Control | Read-only oversight and control review |

The design supports segregation of duties so that one role does not perform every onboarding action.

---

## Final HR Review

Once the required departmental and IT activities are complete, HR performs a final review of the onboarding record.

HR can verify:

- Required approvals
- Department confirmations
- Technology provisioning
- Access requirements
- Signatures
- Completion status

The process then advances to the employee document-signing stage.

---

## Document Signing

The new hire completes the required onboarding documents within the workflow.

The solution supports employee signatures, approval signatures, dates, and conditional document fields where applicable.

Examples include:

- Personal History
- Personal Data / Information Consent
- Conflict of Interest
- Assumption of Duty
- IT Policy
- Code of Business

---

## Automated Document Generation

After completion, Kissflow integrations generate the final onboarding documents.

The completed employee record contains the generated files, including:

1. IT Policy Document
2. Assumption of Duty Certificate
3. Conflict of Interest Document
4. Personal Data / Information Consent Document
5. Personal History Form
6. Code of Business Document

This reduces the need for manual document preparation and maintains the completed documents alongside the onboarding record.

---

## Buddy Notification

The workflow also supports an automated **Buddy Staff Information** email.

The email can dynamically include:

- New hire's name
- Assigned buddy
- Buddy office location
- Relevant contact information
- Description of the buddy's onboarding support role

---

## Dashboard and Tracking

The HR onboarding page provides visibility into the onboarding pipeline.

Examples include:

- Completed onboarding count
- In-progress onboarding count
- My Items
- My Tasks
- Completed requests
- Draft requests
- Rejected / withdrawn items where applicable

This allows HR to monitor onboarding progress rather than following up manually with every department.

---

## Automations and Integrations

The solution uses several integrations to support the workflow.

### Initial New Hire Email

Triggered when an onboarding request requiring employee information is submitted.

Responsibilities:

- Retrieve the process instance ID
- Generate the external onboarding URL
- Send the new hire the onboarding email

### Receive Onboarding Details

Triggered by the external portal.

Responsibilities:

- Catch the webhook request
- Update the corresponding Kissflow process instance
- Map submitted employee information
- Advance the onboarding process

### Workflow Advancement

Used to update/advance process stages after required actions or approvals.

### Document Generation

Triggered at the relevant completion point.

Responsibilities:

- Generate onboarding documents from document templates
- Populate employee/process information
- Attach generated documents to the onboarding record

### Buddy Email Notification

Triggered at the configured stage to introduce the assigned buddy to the employee.

---

## Technology Used

| Technology | Purpose |
|---|---|
| Kissflow | Workflow, forms, approvals, permissions and integrations |
| JavaScript | Dynamic link / email preparation and integration logic |
| Webhooks | Transfer of onboarding information from the external portal |
| Microsoft Outlook Integration | Automated employee notifications |
| Kissflow Document Templates | Automated onboarding document generation |
| External Web Portal | New-hire self-service data collection |
| Lookup / Data Sources | Dynamic HOD / supervisor assignment |

---

## Key Controls

The workflow incorporates controls intended to improve accountability and data integrity.

These include:

- Role-based access.
- Segregation of duties.
- Required-field validation.
- Conditional field visibility.
- Approval signatures.
- Process-specific instance IDs.
- Controlled workflow progression.
- Department-based supervisor assignment.
- Completion status tracking.
- Generated document retention.
- Read-only / hidden field permissions at selected workflow stages.
- Separate departmental approvals.
- Audit history through Kissflow process activity.

---

## Testing

A dedicated **User Acceptance Testing (UAT)** document was created for the process.

The UAT covers:

- Request initiation
- Dashboard behavior
- Initial onboarding email
- Dynamic onboarding URL
- External form validation
- Conditional form logic
- Webhook / data synchronization
- Bank mapping
- HR employee setup
- Head HCM approval
- Departmental approvals
- Supervisor assignment
- IT provisioning
- Office-location routing
- Laptop setup
- Final HR review
- Document signing
- Document generation
- Buddy notification
- Permissions
- Request isolation and data integrity

The UAT workbook contains **78 detailed test cases**.

---

## Audit Documentation

Formal audit and control documentation was also prepared for the process.

The documentation covers:

- Process overview
- Workflow stages
- Roles and responsibilities
- System architecture
- Integration inventory
- Data flow
- Access controls
- Segregation of duties
- Audit trail
- Document generation
- Exception handling
- Security considerations
- Control matrix
- Production-readiness checks
- Evidence register

---

## Production Readiness

Before production deployment, the following should be validated in the production configuration:

- Alternative bank mapping
- Branch-office credential routing
- Head Office vs Branch Office conditions
- Laptop requirement routing
- In-progress dashboard count
- Buddy-email dynamic values
- External onboarding-link security
- Webhook authentication and error monitoring
- Data-retention requirements
- Backup / recovery requirements
- Production role and permission assignments
- Kissflow employee-access provisioning

---

## Potential Enhancement: Automatic Employee Access Provisioning

A future enhancement can extend the workflow after onboarding completion.

```text
Corporate Email Created
        |
        v
Kissflow Access Required?
       / \
     Yes  No
      |    |
      v    +----> Complete
Create / Provision User
      |
      v
Add to Employee Group
      |
      v
Employee Role Inherited
      |
      v
Verify Access
      |
      v
Complete Onboarding
```

A recommended model is:

- Create an **Employee** application role.
- Maintain an **Employees** group.
- Associate the Employee role with that group.
- Add newly onboarded staff to the group after their corporate identity has been provisioned.

This keeps application access separate from the new hire's temporary onboarding access.

---

## Repository Structure

A public portfolio repository can be structured as follows:

```text
new-hire-onboarding/
│
├── README.md
│
├── docs/
│   ├── workflow-overview/
│   ├── uat/
│   └── audit/
│
├── screenshots/
│   ├── workflow/
│   ├── integrations/
│   ├── external-form/
│   └── completed-process/
│
└── diagrams/
    └── onboarding-workflow.png
```

> **Important:** Do not commit confidential employee data, real credentials, private company URLs, webhook secrets, temporary passwords, production screenshots containing personal information, or internal configuration secrets to a public repository.

---

## Suggested Screenshots

For a portfolio version of this project, useful screenshots include:

1. New Hire Request dashboard
2. High-level Kissflow workflow
3. Parallel departmental workflow branches
4. HOD / Supervisor lookup configuration
5. External onboarding portal landing page
6. Integration flow overview
7. Document-generation integration
8. Completed onboarding status
9. Generated document section
10. Sanitized automated email example

Any screenshot used publicly should have personal data and internal identifiers removed or replaced.

---

## Key Skills Demonstrated

This project demonstrates practical experience in:

- Business process automation
- Workflow design
- Process engineering
- User acceptance testing
- Role-based access design
- Conditional workflow routing
- Data mapping
- Webhook integration
- JavaScript integration logic
- Automated email notifications
- Document generation
- Approval workflows
- Audit documentation
- Requirements analysis
- Process control design
- HCM process digitization

---

## Future Improvements

Potential future enhancements include:

- Automated Kissflow user provisioning after onboarding.
- Integration with an enterprise identity provider.
- Automated employee group / role assignment.
- Stronger integration retry and monitoring logic.
- SLA tracking and escalation.
- HR onboarding analytics.
- Automated reminder notifications for pending approvers.
- Centralized onboarding exception dashboard.
- Additional audit reporting.
- Automated offboarding linkage.

---

## Author

**Yekini Abdulroqeeb Ademola**  
Data Analyst | Process Engineer | Graphics Designer

GitHub: **[Abdulroqeeb04](https://github.com/Abdulroqeeb04)**

---

## Confidentiality

This repository is intended to demonstrate the **design and engineering approach** used to build an employee onboarding workflow.

Company-sensitive configuration, personal employee information, credentials, internal endpoints, and confidential business data should not be included in a public repository.

