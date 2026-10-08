LEVAD

SCHOOL OPERATIONS, IDENTITY, SECURITY & GOVERNANCE PLATFORM

You are a principal software architect, product engineer, security architect, database engineer, UX architect, QA engineer and school-operations systems analyst.

Build LEVAD as a production-grade, multi-tenant School Operations, Identity, Security, Governance and Compliance platform.

LEVAD is not a basic school ERP, CRUD application, attendance app, visitor-management app, or collection of disconnected modules.

Its core purpose is to digitally model and control the operational lifecycle of a school:

Identity → Authorization → Workflow → Action → Evidence → Audit → Analytics → Accountability

The implementation must be complete, coherent, secure, extensible and deployable.

Do not create mock functionality, fake integrations, placeholder dashboards, superficial CRUD screens, hard-coded business logic, duplicated workflows, or disconnected modules.

Where an external integration is unavailable, implement a proper adapter/interface and a clearly defined local fallback—not fake successful integration.

---

1. PRODUCT IDENTITY

Product name:

LEVAD

Product category:

School Operations, Identity, Security & Governance Platform

Primary users:

- Proprietor / Board
- Head / Principal
- Vice Principal
- Administrator
- Registrar
- Teachers
- Class Teachers
- Accounts / Finance
- HR
- Security
- Storekeepers
- ICT
- Counsellors
- School Health/Nurse
- Transport personnel
- Maintenance personnel
- Parents / Guardians
- Pupils / Students
- Auditors / authorized compliance personnel

LEVAD must support multiple schools and organizations without cross-tenant data leakage.

---

2. CORE ARCHITECTURAL PRINCIPLE

Everything must be modeled around:

IDENTITY
   ↓
ROLE
   ↓
PERMISSION
   ↓
AUTHORIZATION
   ↓
WORKFLOW
   ↓
ACTION
   ↓
EVIDENCE
   ↓
AUDIT EVENT
   ↓
ANALYTICS

Every significant operational action must answer:

- Who performed it?
- On whose authority?
- What exactly happened?
- When did it happen?
- Where did it happen where applicable?
- What record was affected?
- What was the previous state?
- What is the resulting state?
- Who approved it?
- What evidence exists?
- Can the event be reconstructed later?

Do not rely on UI restrictions as security.

All authorization must be enforced server-side.

---

3. MULTI-TENANCY

LEVAD must support:

Platform
 ├── Organization
 │    ├── School
 │    │    ├── Academic Year
 │    │    ├── Term
 │    │    ├── Departments
 │    │    ├── Classes
 │    │    ├── Users
 │    │    └── Operational Records
 │    └── Additional Schools
 └── Platform Administration

Every tenant-owned record must have an enforceable tenant boundary.

Implement:

- tenant isolation
- school isolation
- organization hierarchy
- tenant-aware queries
- tenant-aware authorization
- tenant-specific configuration
- tenant-specific branding
- tenant-specific policies
- tenant-specific retention settings
- tenant-specific workflows
- tenant-specific roles
- tenant-specific notification configuration

Prevent:

- IDOR
- cross-tenant enumeration
- insecure direct object access
- tenant switching through manipulated identifiers
- authorization bypass through API calls

---

4. IDENTITY SYSTEM

Implement a central identity model.

A person may have one or multiple relationships with the school.

Examples:

Person
 ├── Parent
 ├── Teacher
 ├── Staff
 ├── Student
 ├── Guardian
 └── Visitor

Do not duplicate people unnecessarily.

Support:

- unique person identity
- account identity
- staff profile
- student profile
- guardian relationship
- emergency contact
- authorized pickup person
- role assignment
- employment relationship
- school relationship
- account lifecycle

Account states:

INVITED
PENDING_VERIFICATION
ACTIVE
SUSPENDED
LOCKED
DISABLED
ARCHIVED

Authentication must support:

- secure password authentication
- MFA/TOTP architecture
- recovery
- session management
- device/session visibility
- session revocation
- login history
- suspicious-login detection
- account lockout/rate limiting
- password policy
- secure reset workflow

Never store plaintext passwords.

Use modern password hashing such as Argon2id.

Use secure, HttpOnly, SameSite cookies where cookie-based sessions are used.

Never place authorization decisions in client-side code.

---

5. RBAC + ABAC

Implement both:

RBAC

Roles determine baseline permissions.

Examples:

- PLATFORM_ADMIN
- SCHOOL_OWNER
- BOARD_MEMBER
- PRINCIPAL
- ADMINISTRATOR
- REGISTRAR
- TEACHER
- CLASS_TEACHER
- ACCOUNTANT
- HR_OFFICER
- SECURITY_OFFICER
- STOREKEEPER
- NURSE
- COUNSELLOR
- TRANSPORT_OFFICER
- ICT_ADMIN
- PARENT
- STUDENT
- AUDITOR

ABAC

Authorization must also consider context.

Examples:

A teacher can access:

students assigned to their classes

but not:

all students in the school

A parent can access:

their own children

but not:

another student's record

Security personnel can verify:

authorized pickup information

without automatically seeing:

academic performance
health records
financial history

Sensitive records must use need-to-know authorization.

---

6. PERMISSION MODEL

Use granular permissions such as:

student.read
student.create
student.update
student.archive

attendance.record
attendance.correct
attendance.approve
attendance.export

visitor.create
visitor.checkin
visitor.checkout
visitor.override

pickup.request
pickup.approve
pickup.release
pickup.override

finance.invoice
finance.payment
finance.refund
finance.reconcile
finance.export

asset.create
asset.assign
asset.transfer
asset.dispose

incident.create
incident.view
incident.investigate
incident.close

compliance.view
compliance.manage
compliance.export

Do not implement authorization using scattered "if role == ..." statements.

Centralize policy evaluation.

---

7. SCHOOL STRUCTURE

Model:

- organization
- school
- campus
- building
- floor
- room
- department
- academic year
- term
- class
- stream
- subject
- timetable
- academic calendar

A school must be able to operate multiple academic years without destroying historical records.

Never overwrite historical academic data merely because a new year begins.

---

8. STUDENT LIFECYCLE

Implement the complete lifecycle:

PROSPECT
↓
APPLICANT
↓
APPLICATION
↓
ASSESSMENT
↓
ACCEPTED
↓
ADMITTED
↓
ENROLLED
↓
CLASS_ASSIGNED
↓
ACTIVE
↓
PROMOTED / REPEATING
↓
TRANSFERRED / GRADUATED / WITHDRAWN
↓
ARCHIVED

Admissions must support:

- application
- applicant identity
- guardian information
- required documents
- application status
- assessment
- admission decision
- acceptance
- enrollment
- class allocation
- document verification
- admission history

Do not permanently delete student records through ordinary UI actions.

Use controlled archival/retention processes.

---

9. STUDENT PROFILE

Student profile must consolidate authorized information:

- identity
- photograph
- admission information
- guardians
- emergency contacts
- class
- attendance
- timetable
- subjects
- assessments
- results
- assignments
- behavior
- disciplinary records
- safeguarding records
- health information
- transport
- authorized pickup persons
- fees
- documents
- communications
- operational history

Do not expose all sections to every user.

Each section must independently enforce authorization.

---

10. PARENT/GUARDIAN MANAGEMENT

Support:

- multiple guardians
- guardian relationships
- custody/relationship metadata where legally appropriate
- emergency contacts
- authorized pickup persons
- communication preferences
- notification channels
- linked children
- account verification
- consent records

A guardian relationship must not automatically imply unrestricted access.

Support relationship-specific permissions.

---

11. CLASS MANAGEMENT

Class workspace:

- class profile
- class teacher
- subject teachers
- enrolled students
- timetable
- attendance
- assignments
- assessments
- behavior
- interventions
- announcements
- class analytics

Teachers should see only what their authorization permits.

---

12. ATTENDANCE ENGINE

Implement robust attendance.

States:

PRESENT
ABSENT
LATE
EXCUSED
AUTHORIZED_ABSENCE
LEFT_EARLY
SUSPENDED

Support:

- class attendance
- subject attendance where configured
- staff attendance
- late arrival
- early departure
- correction workflow
- approval workflow
- parent notification
- attendance history
- attendance analytics

Corrections must never silently overwrite history.

Instead:

Original event
+
Correction request
+
Reason
+
Actor
+
Approver
+
Timestamp
+
Audit trail

Detect anomalies:

- repeated late arrivals
- unusual attendance patterns
- suspicious bulk edits
- attendance edited after reporting deadline
- teacher marking attendance outside authorized class/time

---

13. ACADEMIC MANAGEMENT

Implement:

- subjects
- curriculum structures
- classes
- streams
- lessons
- timetable
- assignments
- assessments
- grading schemes
- continuous assessment
- examinations
- moderation
- result approval
- report generation
- promotion

Grade calculations must be configurable rather than hard-coded.

Maintain immutable historical results after final approval except through controlled correction workflows.

---

14. TEACHER MANAGEMENT

Teacher lifecycle:

APPLICANT
↓
VERIFICATION
↓
APPOINTED
↓
ONBOARDING
↓
ACTIVE
↓
LEAVE / SUSPENSION
↓
EXIT

Track:

- employment
- department
- subjects
- classes
- qualifications
- professional registration/licensing information
- CPD/training
- attendance
- timetable
- workload
- appraisal
- leave
- documents
- disciplinary records
- employment history

Implement expiry alerts for configured professional/compliance documents.

---

15. STAFF / HR

Support non-teaching staff:

- employment profile
- department
- position
- documents
- attendance
- leave
- training
- appraisal
- disciplinary actions
- asset assignment
- exit clearance

Implement configurable approval chains.

---

16. FEES AND FINANCE

Build a proper financial subsystem.

Entities:

- fee structure
- fee item
- invoice
- student account
- payment
- receipt
- discount
- scholarship
- waiver
- credit
- refund
- reconciliation
- outstanding balance

Never calculate balances solely from UI state.

The financial ledger must be authoritative.

Every financial mutation must be auditable.

Support payment-provider adapters without hard-coding one provider.

---

17. PROCUREMENT

Workflow:

REQUEST
↓
BUDGET CHECK
↓
APPROVAL
↓
QUOTATION
↓
SUPPLIER SELECTION
↓
PURCHASE ORDER
↓
RECEIVING
↓
STORES / ASSET REGISTRATION
↓
INVOICE
↓
PAYMENT

Implement approval thresholds.

Different monetary values may require different approval levels.

Prevent self-approval where policy prohibits it.

---

18. INVENTORY / STORES

Support:

- items
- categories
- units
- suppliers
- stock locations
- receipts
- issues
- transfers
- adjustments
- returns
- stock counts
- reorder thresholds
- expiry tracking
- stock ledger

Every inventory movement must produce an immutable transaction.

---

19. ASSET MANAGEMENT

Asset lifecycle:

REQUEST
↓
PURCHASE
↓
RECEIVE
↓
REGISTER
↓
TAG
↓
ASSIGN
↓
TRANSFER
↓
MAINTAIN
↓
AUDIT
↓
RETURN
↓
DISPOSE

Track:

- asset ID
- serial number
- category
- acquisition
- cost
- location
- custodian
- condition
- warranty
- maintenance
- transfer history
- audit history
- disposal

Support QR/barcode identification.

An asset must never simply “move” between custodians without an auditable transfer.

---

20. VISITOR MANAGEMENT

Build a complete visitor-control system.

Visitor workflow:

VISIT REQUEST
↓
HOST IDENTIFICATION
↓
AUTHORIZATION
↓
VISITOR REGISTRATION
↓
IDENTITY VERIFICATION
↓
CHECK-IN
↓
BADGE / PASS
↓
VISIT
↓
CHECK-OUT
↓
VISIT CLOSED

Support:

- preregistration
- host approval
- visitor categories
- purpose
- time window
- access restrictions
- badge/pass
- check-in
- check-out
- expired authorization
- denied entry
- incident association

Visitor passes must have explicit lifecycle states.

---

21. STUDENT RELEASE / PICKUP CONTROL

Treat student release as a high-security workflow.

Support:

- authorized guardians
- authorized pickup persons
- pickup requests
- pickup windows
- one-time pickup authorization
- identity verification
- security verification
- release authorization
- release timestamp
- releasing officer
- receiving person
- exception handling

Never allow a student to be released solely because somebody claims:

«“I know the child.”»

Every exception must be recorded.

---

22. ACCESS CONTROL

Create a logical access-control subsystem.

Entities:

- person
- credential
- access point
- zone
- authorization
- access event
- denial event

Zones may include:

- gate
- administration
- classrooms
- laboratories
- stores
- ICT room
- examination room
- staff areas
- restricted areas

Support time-bound and role-based access.

Example:

Teacher
→ Classroom Zone
→ 07:00–17:00
→ Monday–Friday

Access events must be auditable.

Do not assume physical door hardware exists.

Provide hardware integration interfaces.

---

23. SECURITY OPERATIONS

Security dashboard must show:

- current visitors
- expected visitors
- checked-in visitors
- overdue visitors
- student release activity
- denied access
- incidents
- suspicious events
- assets/security events
- current occupancy where supported

Provide a chronological event timeline.

---

24. INCIDENT MANAGEMENT

Support:

- incident creation
- classification
- severity
- persons involved
- location
- time
- witnesses
- evidence
- statements
- investigation
- actions
- escalation
- resolution
- closure
- review

Incident severity should drive escalation rules.

Never allow unauthorized users to access sensitive incident details.

---

25. SAFEGUARDING

Create a separately protected safeguarding domain.

Do not treat safeguarding as ordinary disciplinary records.

Support:

- concern/report
- involved persons
- risk classification
- confidential notes
- evidence
- designated officer
- escalation
- action
- review
- closure

Use strict least-privilege access.

Every access to sensitive safeguarding information should itself be auditable.

---

26. HEALTH

Create restricted health records.

Support:

- emergency information
- allergies
- relevant alerts
- clinic visits
- treatment records
- medication records
- referrals
- parent/guardian notifications
- health documents

Health data must not be exposed to ordinary teachers or administrative users unless explicitly authorized.

---

27. TRANSPORT

Support:

- vehicles
- drivers
- routes
- stops
- assigned students
- boarding
- drop-off
- incidents
- vehicle maintenance
- driver documentation

Model:

Route
→ Vehicle
→ Driver
→ Students
→ Boarding Event
→ Drop-off Event

Support parent notifications.

---

28. COMMUNICATION ENGINE

Create a centralized communication system.

Channels:

- in-app
- email
- SMS adapter
- push notification adapter

Messages must support:

- recipient targeting
- templates
- priority
- delivery status
- acknowledgement
- attachments
- scheduling
- expiry
- audit trail

Support:

School → Parent
Teacher → Parent
School → Staff
Security → Administration
Administration → Individual
Emergency Broadcast

Do not make WhatsApp the system of record.

---

29. NOTIFICATION ENGINE

Implement event-driven notifications.

Examples:

STUDENT_ABSENT
FEE_OVERDUE
VISITOR_CHECKED_IN
VISITOR_OVERSTAY
PICKUP_APPROVED
STUDENT_RELEASED
DOCUMENT_EXPIRING
ASSET_OVERDUE
INCIDENT_ESCALATED
RESULT_PUBLISHED
APPROVAL_PENDING

Notifications must be configurable.

Never hard-code notification behavior throughout business logic.

Use an event/notification architecture.

---

30. WORKFLOW ENGINE

Build a reusable workflow engine rather than hard-coding every approval process.

A workflow should support:

TRIGGER
→ STATE
→ ACTOR
→ CONDITION
→ ACTION
→ APPROVAL
→ NEXT STATE

Example:

Leave Request
→ Submitted
→ Department Review
→ Principal Approval
→ Approved
→ HR Record

Workflows must support:

- sequential approval
- parallel approval
- conditional approval
- delegation
- rejection
- escalation
- expiry
- cancellation
- reassignment
- SLA monitoring

---

31. DOCUMENT MANAGEMENT

Support:

- student documents
- staff documents
- contracts
- certificates
- licenses
- receipts
- incident evidence
- procurement documents
- compliance documents

Implement:

- metadata
- versioning
- access control
- retention
- expiration
- audit trail

Do not expose uploaded files through predictable public URLs.

Use authorization-aware document access.

---

32. AUDIT SYSTEM

Create a central immutable audit system.

Audit events should include:

actor
actor_type
tenant
school
action
resource_type
resource_id
previous_state
resulting_state
timestamp
IP/device metadata where appropriate
request/correlation ID
reason where required

Examples:

STUDENT_CREATED
STUDENT_UPDATED
ATTENDANCE_CORRECTED
RESULT_APPROVED
FEE_RECORDED
VISITOR_CHECKED_IN
STUDENT_RELEASED
ASSET_TRANSFERRED
ROLE_CHANGED
PERMISSION_CHANGED
DOCUMENT_ACCESSED
INCIDENT_VIEWED

Audit logs must not be editable through normal application interfaces.

---

33. SECURITY EVENT MONITORING

Create a security-event layer separate from ordinary business events.

Detect:

- repeated failed logins
- impossible access patterns
- privilege escalation attempts
- excessive record access
- bulk downloads
- unusual exports
- repeated authorization failures
- suspicious account changes
- mass data modifications

Create risk signals without automatically accusing users of wrongdoing.

---

34. DATA PROTECTION

Design for privacy by default.

Implement:

- data minimization
- purpose limitation
- least privilege
- access logging
- retention policies
- controlled exports
- consent records where required
- data-subject request workflow
- correction workflow
- archival
- secure deletion where legally appropriate
- breach/incident workflow

Do not collect data merely because the database can store it.

Every sensitive field must have a defined operational purpose.

---

35. RECORD RETENTION

Create configurable retention policies by record category.

Examples:

Academic Records
Financial Records
Student Records
Employee Records
Visitor Records
Security Events
Incident Records
Health Records
Safeguarding Records
Audit Records

Retention must not mean immediate deletion.

Support:

ACTIVE
RETENTION
ARCHIVED
LEGAL_HOLD
ELIGIBLE_FOR_DISPOSAL
DISPOSED

Legal hold must override ordinary disposal.

---

36. CONSENT MANAGEMENT

Where consent is legally/operationally required, track:

- purpose
- person
- scope
- version
- date
- method
- status
- withdrawal
- evidence

Do not use a single generic “consent=true” field.

---

37. COMPLAINTS / REQUESTS

Support structured:

- parent complaints
- staff complaints
- student concerns
- information requests
- administrative requests
- appeals

Workflow:

SUBMITTED
→ ACKNOWLEDGED
→ ASSIGNED
→ INVESTIGATING
→ RESPONSE
→ RESOLVED
→ CLOSED

Maintain SLA tracking.

---

38. COMPLIANCE MANAGEMENT

Create compliance registry.

Track:

- requirement
- authority
- responsible person
- evidence
- expiry
- status
- review date
- corrective action

Dashboard:

COMPLIANT
AT RISK
EXPIRING
OVERDUE
NON-COMPLIANT
UNDER REVIEW

Compliance records must be linked to evidence.

---

39. DASHBOARDS

Do not create decorative dashboards.

Every dashboard metric must originate from real system data.

Executive dashboard

Show:

- enrollment
- attendance
- financial position
- outstanding fees
- staff status
- incidents
- safeguarding alerts where authorized
- security activity
- assets
- compliance
- pending approvals
- operational exceptions

Principal dashboard

Show actionable exceptions.

Teacher dashboard

Show:

- assigned classes
- timetable
- attendance
- assignments
- assessments
- pending actions

Security dashboard

Show:

- expected visitors
- current visitors
- student release
- access events
- incidents
- alerts

Parent dashboard

Show only their authorized children.

---

40. EXCEPTION-FIRST DESIGN

LEVAD should not force management to inspect hundreds of records.

The system should identify:

what requires attention now.

Examples:

12 unexplained absences
3 expiring teacher credentials
5 pending approvals
2 overdue visitors
7 assets awaiting audit
14 unpaid invoices
1 unresolved critical incident

Each alert must link to the underlying record.

---

41. SEARCH

Build global search with authorization-aware filtering.

Users must only receive search results they are authorized to see.

Support:

- student
- staff
- parent
- visitor
- asset
- invoice
- incident
- document
- event

Prevent unauthorized information leakage through search suggestions, counts and autocomplete.

---

42. REPORTING

Build report generation from actual data.

Reports:

- enrollment
- attendance
- academic performance
- teacher workload
- fees
- outstanding balances
- procurement
- inventory
- assets
- incidents
- visitor activity
- student release
- transport
- compliance
- audit activity

Support:

- PDF
- CSV
- spreadsheet-compatible export

Exports must themselves be permission-controlled and audited.

---

43. API ARCHITECTURE

Build a versioned API.

Example:

/api/v1/auth
/api/v1/users
/api/v1/students
/api/v1/guardians
/api/v1/classes
/api/v1/attendance
/api/v1/academics
/api/v1/fees
/api/v1/hr
/api/v1/assets
/api/v1/inventory
/api/v1/procurement
/api/v1/visitors
/api/v1/access
/api/v1/pickups
/api/v1/incidents
/api/v1/health
/api/v1/transport
/api/v1/compliance
/api/v1/audit
/api/v1/reports

Apply:

- authentication
- authorization
- schema validation
- rate limiting
- pagination
- filtering
- sorting
- consistent errors
- idempotency where appropriate
- correlation IDs
- audit events

---

44. DATABASE DESIGN

Use a relational database for transactional integrity.

Normalize core entities appropriately.

Use foreign keys and constraints.

Do not rely solely on application code to maintain relationships.

Implement:

- indexes based on real query patterns
- unique constraints
- check constraints where appropriate
- transaction boundaries
- soft archival where appropriate
- immutable ledger/event tables where required

Do not use generic JSON blobs as a substitute for proper relational modeling.

JSON may be used for genuinely variable metadata, not core relationships.

---

45. CONCURRENCY

Handle concurrent operations correctly.

Examples:

Two administrators cannot simultaneously:

- issue the same inventory item beyond available stock
- approve the same workflow incorrectly
- release the same student twice
- assign the same unique asset
- record duplicate financial transactions

Use:

- transactions
- optimistic/pessimistic locking where appropriate
- idempotency keys
- unique constraints

---

46. STATE MACHINES

Use explicit state machines for sensitive lifecycle entities.

Examples:

Visitor

EXPECTED
AUTHORIZED
CHECKED_IN
CHECKED_OUT
EXPIRED
DENIED
CANCELLED

Student

APPLICANT
ADMITTED
ENROLLED
ACTIVE
SUSPENDED
TRANSFERRED
GRADUATED
WITHDRAWN
ARCHIVED

Invoice

DRAFT
ISSUED
PARTIALLY_PAID
PAID
OVERDUE
VOID

Incident

OPEN
TRIAGED
ASSIGNED
INVESTIGATING
ACTION_REQUIRED
RESOLVED
CLOSED

Do not implement lifecycle status as arbitrary free-text fields.

---

47. FRONTEND UX

The interface must be professional, fast and operationally clear.

Do not build a generic admin template and rename the menu items.

Design around user tasks.

Security users should be able to perform gate operations extremely quickly.

Teachers should be able to take attendance without navigating through unnecessary screens.

Parents should understand their child's status immediately.

Administrators should see pending actions and exceptions immediately.

Use:

- responsive design
- desktop optimization
- tablet support
- mobile support
- accessible forms
- keyboard navigation
- clear status indicators
- confirmation for destructive actions
- meaningful empty states
- error recovery

---

48. OFFLINE / INTERRUPTED CONNECTIVITY

Schools may experience unreliable connectivity.

Where appropriate, support controlled offline operation for:

- attendance
- gate operations
- visitor registration
- selected operational tasks

Offline data must be:

- encrypted locally where feasible
- scoped
- timestamped
- synchronized safely
- conflict-aware
- auditable

Never silently overwrite server data during synchronization.

---

49. HARDWARE INTEGRATION ARCHITECTURE

Create adapter interfaces for:

- QR scanners
- barcode scanners
- RFID/NFC
- biometric systems where lawfully appropriate
- access-control hardware
- printers
- SMS providers
- email providers
- payment providers
- identity verification services

Do not tightly couple the core system to a single hardware vendor.

---

50. SECURITY ENGINEERING

Design against at least:

- OWASP Top 10
- OWASP API Security risks
- broken access control
- IDOR
- privilege escalation
- session attacks
- CSRF
- XSS
- SQL injection
- command injection
- SSRF
- file upload attacks
- insecure deserialization
- mass assignment
- rate-limit bypass
- authentication bypass
- tenant isolation failures
- sensitive-data exposure
- enumeration
- business-logic abuse

Never implement offensive exploit functionality into the production system.

Security testing must use safe validation techniques.

---

51. FILE UPLOAD SECURITY

All uploads must be treated as untrusted.

Implement:

- type validation
- extension validation
- MIME validation
- size limits
- malware-scanning adapter
- randomized storage identifiers
- non-public storage
- authorization checks
- download authorization
- metadata sanitization
- image/document processing isolation where applicable

Never trust client-provided filenames.

---

52. SECRETS

Never hard-code:

- passwords
- API keys
- tokens
- encryption keys
- database credentials

Use environment/configuration secrets.

Provide:

.env.example

without real credentials.

---

53. OBSERVABILITY

Implement:

- structured application logs
- security logs
- audit logs
- metrics
- health checks
- error tracking
- correlation IDs
- request tracing architecture

Never log passwords, tokens or unnecessary sensitive personal data.

---

54. BACKUPS / RECOVERY

Design for:

- automated backups
- encrypted backups
- retention
- restore testing
- point-in-time recovery where supported
- disaster recovery documentation

Define:

RPO

and

RTO

for the deployment.

---

55. ADMINISTRATIVE SAFETY CONTROLS

High-risk operations require additional safeguards.

Examples:

- changing privileged roles
- changing school ownership
- deleting/archiving critical records
- financial refunds
- bulk exports
- changing access policies
- changing grading configuration
- releasing sensitive documents

Use:

- re-authentication
- confirmation
- reason capture
- approval where appropriate
- audit logging

---

56. BULK OPERATIONS

Support controlled bulk:

- student import
- staff import
- class assignment
- fee generation
- attendance correction
- communication

Validate before committing.

Provide:

Preview
→ Validation
→ Errors
→ Confirmation
→ Transaction
→ Audit

Never allow blind bulk mutation.

---

57. DATA IMPORT

Provide CSV/spreadsheet import with:

- column mapping
- validation
- duplicate detection
- preview
- error report
- rollback
- audit trail

Never import directly into production tables without validation.

---

58. DATA EXPORT

Exports must require permission.

Support:

- scoped export
- filters
- date range
- fields
- purpose/reason where appropriate
- audit record

Avoid unrestricted “Export Everything.”

---

59. TESTING

Build comprehensive tests.

Unit

Business rules.

Integration

Database + services.

API

Authentication and authorization.

Workflow

State transitions.

Security

Access-control tests.

Regression

Critical school workflows.

Concurrency

Financial, attendance, inventory and release operations.

Tenant isolation

Attempt every relevant cross-tenant access pattern in tests.

Critical security tests must fail the build if broken.

---

60. ACCEPTANCE TESTS

The completed system must successfully demonstrate:

Admission

Parent applies → admin reviews → student accepted → enrolled → class assigned.

Attendance

Teacher records attendance → parent receives notification → management sees analytics.

Fee

Invoice generated → payment recorded → balance recalculated → receipt generated → audit created.

Visitor

Visitor authorized → check-in → badge/pass → check-out → visit closed.

Student release

Guardian identified → authorization verified → security releases student → event logged.

Asset

Asset registered → assigned → transferred → returned → audited.

Incident

Incident reported → triaged → investigated → action → resolution → closure.

Teacher

Teacher created → documents verified → class assigned → attendance/timetable/appraisal tracked.

Procurement

Request → approval → purchase → receiving → inventory/asset update.

Compliance

Requirement created → evidence attached → review → expiry warning → renewal.

---

61. SECURITY ACCEPTANCE CRITERIA

The system is not complete if any authenticated user can:

- access another tenant's records
- modify another user's permissions without authorization
- access another student's confidential information
- bypass workflow approvals through direct API requests
- alter audit records
- access private uploaded documents
- manipulate financial balances
- release students without required authorization
- perform unauthorized bulk exports
- enumerate sensitive records
- reuse expired authorization tokens/passes
- bypass server-side authorization

These must be explicitly tested.

---

62. DEVELOPMENT QUALITY

Do not produce:

- fake buttons
- placeholder APIs
- hard-coded dashboards
- static sample statistics presented as real data
- duplicated business logic
- giant monolithic route handlers
- uncontrolled global state
- arbitrary database access from UI components
- hidden authorization assumptions
- TODOs in critical functionality
- fake payment success
- fake notification delivery
- fake identity verification
- fake compliance status

Every visible capability must either work or be explicitly marked as unavailable.

---

63. DOCUMENTATION

Generate:

README.md
ARCHITECTURE.md
SECURITY.md
DATA_MODEL.md
API.md
DEPLOYMENT.md
OPERATIONS.md
BACKUP_RECOVERY.md
PRIVACY.md
THREAT_MODEL.md
TESTING.md

Include:

- architecture
- setup
- environment variables
- database migration
- deployment
- roles
- permissions
- workflows
- security assumptions
- recovery procedures
- integration interfaces
- testing procedures

---

64. SEED DATA

Provide development/demo seed data representing:

- one organization
- one school
- academic year
- terms
- departments
- classes
- teachers
- students
- guardians
- staff
- visitors
- assets
- inventory
- sample invoices
- workflows
- permissions

Clearly distinguish development seed data from production data.

Never ship real personal data.

---

65. DEPLOYMENT

The application must support production deployment.

Provide:

- environment configuration
- database migrations
- secure production configuration
- HTTPS assumptions
- reverse proxy configuration
- background workers if required
- scheduled jobs
- backups
- logging
- health checks

Do not expose development services in production.

---

66. PERFORMANCE

Design for growth.

Avoid:

- N+1 database queries
- loading entire datasets into memory
- unbounded API responses
- synchronous processing of large jobs
- inefficient dashboard queries

Use:

- pagination
- indexing
- caching where appropriate
- asynchronous jobs
- aggregation
- database-efficient queries

---

67. ARCHITECTURAL EVENT MODEL

Create a domain-event architecture where useful.

Examples:

StudentEnrolled
AttendanceRecorded
AttendanceCorrected
InvoiceIssued
PaymentReceived
VisitorAuthorized
VisitorCheckedIn
VisitorCheckedOut
StudentReleaseAuthorized
StudentReleased
AssetAssigned
AssetTransferred
IncidentCreated
IncidentEscalated
ComplianceRequirementExpiring

Events may trigger:

- notifications
- audit records
- analytics
- workflows
- integrations

Do not allow events to bypass authorization.

---

68. ANALYTICS

Analytics must answer operational questions.

Examples:

Attendance

- Which classes have abnormal absence?
- Which students have repeated lateness?
- What trends exist by term?

Finance

- What is outstanding?
- Which invoices are overdue?
- What is collection performance?

Staff

- workload
- attendance
- leave
- document expiry
- training

Security

- visitor volume
- denied access
- release events
- incidents
- unusual activity

Assets

- missing assets
- overdue returns
- maintenance
- utilization

Compliance

- expiring obligations
- overdue actions
- unresolved findings

---

69. RISK ENGINE

Create a configurable operational risk engine.

Risk inputs may include:

- repeated attendance anomalies
- unresolved incidents
- expired credentials
- overdue assets
- unusual access events
- overdue visitors
- compliance deadlines
- failed approvals
- unusual financial activity

Risk output:

LOW
MEDIUM
HIGH
CRITICAL

Risk scoring must be explainable.

Never create opaque automated decisions for sensitive matters without human review.

---

70. GOVERNANCE

Create governance structures around:

- policies
- procedures
- approvals
- responsibilities
- audit
- compliance
- incidents
- corrective actions

A policy should be able to have:

Draft
→ Review
→ Approval
→ Published
→ Superseded
→ Archived

Maintain version history.

---

71. PRINCIPLE OF LEAST PRIVILEGE

Default permissions must be restrictive.

A newly created user must not automatically receive broad access.

Sensitive modules require explicit permissions.

Administrators should not automatically have access to every sensitive record simply because they are administrators.

Separate:

administrative authority

from

data visibility.

---

72. PRIVILEGED OPERATIONS

For highly privileged actions implement:

- re-authentication
- MFA where configured
- reason capture
- audit event
- optional dual approval
- notification to appropriate oversight users

Support configurable four-eyes approval.

---

73. UI INFORMATION ARCHITECTURE

Main navigation should be organized around:

HOME
PEOPLE
ACADEMICS
ATTENDANCE
ADMISSIONS
FINANCE
HR
PROCUREMENT
INVENTORY
ASSETS
SECURITY
VISITORS
STUDENT RELEASE
INCIDENTS
HEALTH
TRANSPORT
COMMUNICATIONS
COMPLIANCE
REPORTS
AUDIT
ADMINISTRATION

Navigation must dynamically respect authorization.

Do not merely hide unauthorized menu items; enforce authorization at the backend.

---

74. SCHOOL CONTROL CENTER

Create a central operational control center.

It should answer:

WHAT IS HAPPENING?
WHAT NEEDS ATTENTION?
WHO IS RESPONSIBLE?
WHAT IS OVERDUE?
WHAT IS AT RISK?
WHAT CHANGED?
WHAT REQUIRES APPROVAL?

This is the primary management interface.

---

75. IMPLEMENTATION ORDER

Build in dependency order.

Phase 1

Foundation:

- database
- authentication
- identity
- tenancy
- RBAC/ABAC
- audit
- configuration

Phase 2

People:

- students
- guardians
- teachers
- staff
- classes

Phase 3

Academic:

- attendance
- timetable
- assessments
- results

Phase 4

Operations:

- admissions
- finance
- HR
- procurement
- inventory
- assets

Phase 5

Security:

- visitors
- access
- student release
- incidents
- safeguarding

Phase 6

Support:

- health
- transport
- communication
- documents

Phase 7

Governance:

- compliance
- reporting
- analytics
- risk
- retention

Phase 8

Hardening:

- security testing
- performance
- backup/recovery
- observability
- deployment
- documentation

Do not move to the next phase while critical architectural foundations remain incomplete.

---

76. FINAL BUILD STANDARD

Before declaring LEVAD complete, perform a full system review.

Verify:

1. Every module has a real data model.
2. Every sensitive operation has authorization.
3. Every important state transition is auditable.
4. Every workflow has explicit states.
5. Every tenant boundary is enforced.
6. Every sensitive record has appropriate access control.
7. Every financial mutation is transactionally safe.
8. Every asset movement is traceable.
9. Every student release is attributable.
10. Every visitor lifecycle is reconstructable.
11. Every privileged operation is auditable.
12. Every export is controlled.
13. Every file is protected.
14. Every critical workflow has automated tests.
15. No dashboard contains fabricated metrics.
16. No critical button is non-functional.
17. No production secret is committed.
18. No authorization decision depends solely on the frontend.
19. No ordinary user can alter audit history.
20. No cross-tenant access is possible.
21. No critical workflow relies on undocumented manual intervention.
22. Recovery procedures are documented and testable.

The final product must feel like a real institutional operating system, not a collection of CRUD pages.

The design objective is:

«Replace fragmented school administration with a controlled, traceable, identity-aware digital operating environment while preserving appropriate human decision-making.»

Build LEVAD accordingly.
