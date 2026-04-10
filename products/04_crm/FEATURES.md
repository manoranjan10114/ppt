# Application Features Document

> Comprehensive module-wise feature listing of the CRM (Customer Relationship Management) application.

---

## Module: Authentication

- Feature: Email/Mobile Login
  - Description: Users can log in using email or mobile number with password authentication.

- Feature: Password Validation
  - Description: Real-time validation of password length (minimum 4 characters) with helper text.

- Feature: Email/Mobile Validation
  - Description: Real-time validation of email format or mobile number format on the login form.

- Feature: Enter Key Submit
  - Description: Users can press Enter key to submit the login form without clicking the button.

- Feature: Token-Based Auto Login
  - Description: Supports URL-based login with company_id, hotel_id, admin_id, and auth_token parameters for seamless SSO.

- Feature: Browser Detection
  - Description: Detects the browser name and sends it as part of the login payload.

- Feature: Role-Based Routing
  - Description: Separate route sets for Admin (AppRoutes) and Agent (AgentRoutes) with different sidebar menus and permissions.

---

## Module: Dashboard

- Feature: Summary Cards
  - Description: Displays total counts for Contacts, Interactions (open/closed), Enquiries (open/closed), Tickets (open/closed), and Users.

- Feature: Card Navigation
  - Description: Clicking any dashboard card navigates to the respective module page.

- Feature: Open/Closed Breakdown
  - Description: Interactions, Enquiries, and Tickets cards show sub-values for open and closed counts.

- Feature: Loading Skeleton
  - Description: Shows a skeleton loader while dashboard data is being fetched from the API.

---

## Module: Contacts

- Feature: Contact List View
  - Description: Displays all contacts in a paginated table with name, source, email, mobile, affiliation, type, and created date.

- Feature: Graph View
  - Description: Toggle between List and Graph view to visualize contacts data graphically.

- Feature: Search Contacts
  - Description: Search contacts by name, email, or phone with throttled (1-second delay) API calls.

- Feature: Filter by Source
  - Description: Filter contacts by enquiry source using a searchable dropdown.

- Feature: Filter by Affiliation
  - Description: Filter contacts by affiliation using a searchable dropdown.

- Feature: Filter by Group
  - Description: Filter contacts by group membership using a searchable dropdown.

- Feature: Filter by Contact Type
  - Description: Filter contacts by contact type (e.g., Guest, Corporate, etc.) using a searchable dropdown.

- Feature: Add New Contact
  - Description: Sliding panel to add a new contact with name, phone, email, source, type, and other details.

- Feature: Import Contacts (XLS)
  - Description: Import contacts from an Excel/XLS file via a sliding panel.

- Feature: Bulk Update Contacts
  - Description: Bulk update existing contacts via file upload through a sliding panel.

- Feature: Download Contacts (CSV)
  - Description: Export filtered contacts list as a CSV file download.

- Feature: Pagination
  - Description: Server-side pagination with configurable page sizes (10, 50, 100) and page number navigation.

- Feature: Email Verification Badge
  - Description: Shows a green verified badge next to validated email addresses.

- Feature: Phone Verification Badge
  - Description: Shows a green verified badge next to validated phone numbers.

- Feature: IVR Click-to-Call
  - Description: If IVR is connected, clicking a phone number opens the IVR user list slider for initiating calls.

- Feature: Quick Notes
  - Description: Quick notes slider accessible from the contacts list for adding notes to a contact.

- Feature: Navigate to Contact Details
  - Description: Clicking a contact name navigates to the detailed contact profile page.

- Feature: Manage Groups
  - Description: Navigate to group management for creating, editing, and deleting contact groups.

- Feature: Total Contacts Count
  - Description: Displays the total number of contacts in the breadcrumb area.

---

## Module: Contact Details (Profile)

- Feature: Contact Profile View
  - Description: Detailed contact profile with tabs for Profile, Address, Notes, Documents, Memberships, Bookings, and Additional Info.

- Feature: Profile Picture Upload
  - Description: Upload and update contact profile picture with image validation (type and 5MB size limit).

- Feature: Edit Profile
  - Description: Edit contact details including name, type, source, phone, email, affiliation, gender, DOB, anniversary, language, pets, kids, preferences, and remarks.

- Feature: Edit Address
  - Description: Edit contact address with country/state/city cascading dropdowns, zipcode, and description. Also supports alternate phone numbers and email IDs.

- Feature: Contact Notes Tab
  - Description: View and add notes for a contact with note history display.

- Feature: Documents Tab
  - Description: View uploaded documents for the contact.

- Feature: Memberships Tab
  - Description: View active memberships linked to the contact with membership number and program details.

- Feature: Bookings Tab
  - Description: View booking history for the contact with total amount.

- Feature: Additional Info Tab
  - Description: Display additional custom fields and preferences for the contact.

- Feature: Audit Trail
  - Description: Shows created/updated timestamps with user information for the contact record.

- Feature: Membership Badge
  - Description: Displays a gold membership badge on the profile picture if the contact has an active membership.

---

## Module: Customer Data (Legacy View)

- Feature: Single Customer View
  - Description: Detailed view of a single customer with profile picture, personal info, and contact details.

- Feature: Profile Picture with Crop
  - Description: Upload profile picture with image cropping modal, compression, and aspect ratio control.

- Feature: Edit Customer
  - Description: Edit customer details via a sliding panel.

- Feature: Delete Customer
  - Description: Delete a customer with confirmation prompt.

- Feature: View Notes
  - Description: Navigate to view all notes for the customer with note count display.

- Feature: View Documents
  - Description: Navigate to view and manage documents for the customer.

- Feature: Additional Info Display
  - Description: Displays parsed additional info JSON with formatted key-value pairs and date formatting.

- Feature: Assigned Agent Display
  - Description: Shows the assigned agent name and allows removing the agent assignment.

- Feature: Created/Updated Audit Info
  - Description: Shows who created and last updated the customer record with timestamps.

---

## Module: Documents

- Feature: Document Upload
  - Description: Upload documents (images and PDFs) for a contact with drag-and-drop support.

- Feature: Image Cropping
  - Description: Crop uploaded images before saving with a modal cropping interface.

- Feature: Image Compression
  - Description: Automatically compresses uploaded images to max 1MB before upload.

- Feature: Document Preview
  - Description: Preview uploaded images in a sliding panel with carousel view.

- Feature: Document Download
  - Description: Download uploaded documents directly from the preview panel.

- Feature: Delete Document
  - Description: Delete uploaded documents with confirmation prompt.

- Feature: PDF Support
  - Description: Upload and view PDF documents, opening them in a new browser tab.

---

## Module: Manage Groups

- Feature: Group List
  - Description: View all contact groups in card layout with member count.

- Feature: Add Group
  - Description: Create a new contact group via a sliding panel.

- Feature: Edit Group
  - Description: Edit group name via a sliding panel.

- Feature: Delete Group
  - Description: Delete a group with confirmation prompt.

- Feature: View Group Members
  - Description: Navigate to view all members in a group with pagination.

- Feature: Add Members to Group
  - Description: Add contacts to a group from the contacts list.

- Feature: Remove Member from Group
  - Description: Remove individual members from a group with confirmation.

- Feature: Bulk Remove Members
  - Description: Select multiple members and remove them all at once.

- Feature: Select All / Deselect All
  - Description: Toggle selection of all members on the current page.

---

## Module: Interaction

- Feature: Interaction List
  - Description: View all interactions in a table with interaction ID, customer name, mobile, start/latest timestamps, follow-up info, and note count.

- Feature: Open/Closed Toggle
  - Description: Toggle between Open and Closed interactions.

- Feature: Category Filters (Open)
  - Description: Filter open interactions by category: All, Pending for Today, Pending Since Few Days, Not Scheduled, Scheduled — with counts.

- Feature: Agent Filter
  - Description: Filter interactions by assigned agent using a searchable dropdown.

- Feature: Search by Name/Phone
  - Description: Search interactions by customer name or phone number.

- Feature: View Interaction Notes
  - Description: Open a sliding panel to view and add notes for an interaction.

- Feature: Close Interaction
  - Description: Close an open interaction via a sliding panel with closing status (Converted/Not Converted).

- Feature: IVR Integration
  - Description: Click-to-call via IVR for contacts with IVR connection.

- Feature: New Interaction Badge
  - Description: Shows a "New" badge for recently created interactions.

- Feature: Closing Status Icons
  - Description: Shows green check for Converted and red X for Not Converted on closed interactions.

---

## Module: Enquiry / Lead Management

- Feature: Enquiry Dashboard
  - Description: Overview with stat cards for Unattended, In Conversation, Lost, Converted, and Junk enquiries with counts.

- Feature: In Conversation Leads Table
  - Description: Dashboard table showing leads in conversation with aging > 2 days, with customer details, assigned user, and aging indicator.

- Feature: Unattended Enquiries Tab
  - Description: List of new/unattended enquiries that haven't been picked up yet.

- Feature: In Conversation Tab
  - Description: List of active enquiries with search, source filter, type filter, and pagination.

- Feature: Closed Enquiries Tab
  - Description: List of closed enquiries (Converted, Lost, Junk).

- Feature: Add New Enquiry
  - Description: Sliding panel to add a new enquiry with customer name, phone, WhatsApp flag, email, source, type, and remark.

- Feature: Edit Enquiry
  - Description: Edit existing enquiry details via a sliding panel.

- Feature: Delete Enquiry
  - Description: Delete an enquiry with confirmation prompt.

- Feature: View Enquiry Details
  - Description: Sliding panel showing full enquiry details including Room Booking details (check-in/out, nights, adults, children, rooms) or Banquet details (event date, total pax, occasion).

- Feature: Start Interaction from Enquiry
  - Description: Create an interaction note from an unattended enquiry to move it to "In Conversation" status.

- Feature: Inline Enquiry Type Edit
  - Description: Click on enquiry type in the table to change it via an inline dropdown.

- Feature: Enquiry Notes
  - Description: View and add notes for an enquiry with note count display.

- Feature: Enquiry Age Indicator
  - Description: Color-coded age indicator (green < 4 days, yellow 4-7 days, red > 7 days).

- Feature: Follow-up Date Display
  - Description: Shows the next follow-up date for each enquiry.

- Feature: Created By Badge
  - Description: Shows whether the enquiry was created by a Guest or Staff member.

- Feature: Quotation Management
  - Description: Create and view quotations linked to an enquiry with quotation count display.

- Feature: Enquiry Analytics
  - Description: Visual analytics with charts for In Conversation by Type/User, Closed by Type/User/Status, Conversion by User, and Revenue by User.

- Feature: Analytics Date Range Presets
  - Description: Pre-defined date ranges (This Week, Last Week, Month Till Date, Last Month, Last 6 Months, Last 1 Year, Year Till Date, Custom).

- Feature: Revenue Summary Table
  - Description: Tabular summary of revenue by user with conversions, total revenue, and average revenue per conversion.

---

## Module: Tickets

- Feature: Ticket List View
  - Description: View all tickets in a paginated table with ticket ID, type, priority, subject, department, category, assigned to, raised by, aging, status, due date, and update count.

- Feature: Graph View
  - Description: Toggle between List and Graph view for ticket visualization.

- Feature: Add New Ticket
  - Description: Sliding panel to create a new ticket with department, category, priority, room number, guest name, subject, description, assigned staff, and due date.

- Feature: Raise Ticket from Form
  - Description: Create tickets from dynamic form submissions.

- Feature: View Ticket Details
  - Description: Sliding panel showing full ticket details.

- Feature: Ticket Timeline
  - Description: View the complete timeline/history of a ticket's status changes and updates.

- Feature: Update Ticket Status
  - Description: Change ticket status directly from the list view via dropdown.

- Feature: Filter by Status (Open/Closed)
  - Description: Toggle between open and closed tickets.

- Feature: Filter by Department
  - Description: Multi-select department filter for tickets.

- Feature: Filter by Priority
  - Description: Filter tickets by priority level.

- Feature: Filter by Ticket Type
  - Description: Filter tickets by type (Hotel/Guest).

- Feature: Search Tickets
  - Description: Search tickets by keyword with debounced input.

- Feature: Date Range Filter
  - Description: Filter tickets by date range (from/to).

- Feature: SSE Real-Time Updates
  - Description: Server-Sent Events (SSE) connection for real-time ticket updates without page refresh.

- Feature: Pagination
  - Description: Configurable pagination with page sizes (10, 50, 100).

---

## Module: Membership / Loyalty

- Feature: Membership Dashboard
  - Description: Overview cards showing Active Members (Paid/Complimentary breakdown), Pending Requests (Paid/Complimentary breakdown), and Expiring Soon count.

- Feature: Membership Requests
  - Description: List of pending membership requests with guest details, tier, type, amount, payment status, and approval actions.

- Feature: Approve Membership Request
  - Description: Approve a request with membership number, start date, card delivery status, and payment mode.

- Feature: Reject Membership Request
  - Description: Reject a request with mandatory remarks.

- Feature: Payment Details Modal
  - Description: View payment transaction details (date, amount, PG reference, bank reference) for paid memberships.

- Feature: Active Memberships List
  - Description: Paginated list of active memberships with search, program/tier/affiliation/type filters, and column sorting.

- Feature: Add New Membership
  - Description: Sliding panel to add a membership with guest details, geo-location (country/state/city), program/tier selection, membership type, payment details, and card delivery info.

- Feature: View Membership Details
  - Description: Sliding panel showing full membership details with editable fields (membership number, dates, card delivery, remarks).

- Feature: Edit Membership
  - Description: Update membership details including type, number, dates, card delivery status, and remarks.

- Feature: Membership Receipt Print
  - Description: Generate and print membership receipt using a utility function.

- Feature: Auto Expiry Date Calculation
  - Description: Automatically calculates expiry date based on start date and program validity years.

- Feature: Fee Calculation
  - Description: Automatically calculates base fee, 18% tax, and total amount based on selected tier.

- Feature: Expired Memberships List
  - Description: Paginated list of expired memberships with search and filters.

- Feature: Download Memberships (Excel)
  - Description: Export active or expired memberships as Excel file with applied filters.

- Feature: Membership Analytics
  - Description: Pie charts for membership by Type, Affiliation, and Age; Bar chart for membership timeline.

- Feature: Membership Reports
  - Description: Download collection report with date range selection as Excel file.

- Feature: Filter by Program/Tier/Affiliation/Type
  - Description: Multiple filter options across all membership tabs.

- Feature: Column Sorting
  - Description: Sort active memberships by various columns (ascending/descending).

- Feature: Navigate to Contact Profile
  - Description: Click guest name to navigate to the linked contact's profile page.

- Feature: Request Status Filter
  - Description: Filter membership requests by status (All, Pending, Rejected).

- Feature: Next Membership Number Auto-Fetch
  - Description: Automatically fetches the next available membership number when approving a request.

---

## Module: Guest Connect

- Feature: Engagements Tab
  - Description: View and manage guest engagement activities.

- Feature: Services Management
  - Description: Add, edit, and manage hotel services displayed to guests.

- Feature: Offers Management
  - Description: Add, edit, and manage promotional offers for guests.

- Feature: Activities Management
  - Description: Add, edit, and manage guest activities.

- Feature: Rooms Management
  - Description: Add, edit, and manage room listings with bulk upload support.

- Feature: Bulk Upload Rooms
  - Description: Upload multiple rooms at once via file upload.

- Feature: Guest Connect Public Page
  - Description: Public-facing page for guests with multiple template options (Template 1-6).

- Feature: Offer Carousel
  - Description: Display offers in a carousel format on the public page.

- Feature: Welcome Modal
  - Description: Welcome modal displayed to guests on the public page.

- Feature: Form Modal
  - Description: Dynamic form modal for guest data collection.

---

## Module: Marketing Tools

- Feature: QR Code List
  - Description: View all created QR codes in a table with name, title, target type, target value, logo status, scan count, and creation date.

- Feature: Create QR Code
  - Description: Generate QR codes for URL, WhatsApp, PDF, YouTube, or Image targets with optional logo inclusion.

- Feature: QR Code Preview
  - Description: Preview generated QR code before saving.

- Feature: Save QR Code
  - Description: Save generated QR code to the system.

- Feature: Download QR Code
  - Description: Download QR code as PNG image.

- Feature: View QR Code
  - Description: View a saved QR code in a sliding panel with download option.

- Feature: Edit QR Code
  - Description: Update QR code title, target value, and logo settings.

- Feature: Delete QR Code
  - Description: Delete a QR code with confirmation.

- Feature: QR Code Scan Analytics
  - Description: View scan details including scan time, IP address, device type, browser, OS, country, and city.

- Feature: File Upload for QR
  - Description: Upload PDF or Image files as QR code targets with type and size validation (max 5MB).

---

## Module: Campaigns

- Feature: Campaign List
  - Description: View all campaigns in a paginated table with name, subject/template, channel, status, provider, and creation date.

- Feature: Create Email Campaign
  - Description: Create and send email campaigns.

- Feature: Create SMS Campaign
  - Description: Create and send SMS campaigns.

- Feature: Create WhatsApp Campaign
  - Description: Create and send WhatsApp campaigns with template selection.

- Feature: Delete Campaign
  - Description: Delete a campaign with confirmation prompt.

- Feature: Campaign Analytics
  - Description: View campaign delivery analytics with date range filter (max 3 days) showing delivery logs.

- Feature: Template Management
  - Description: Create and manage campaign templates.

- Feature: WhatsApp Template Creation
  - Description: Create WhatsApp message templates via a dedicated modal.

- Feature: Contact Selection for Campaigns
  - Description: Select target contacts/groups for campaign delivery via a modal.

- Feature: View Campaign Contacts
  - Description: View the list of contacts targeted by a campaign.

---

## Module: IVR (Interactive Voice Response)

- Feature: IVR Dashboard
  - Description: View IVR system overview and manage IVR settings.

- Feature: Manage Calls
  - Description: View and manage IVR call records.

- Feature: IVR User List
  - Description: View IVR-connected users and initiate calls from contact lists.

---

## Module: Organisation

- Feature: Organisation List
  - Description: View all organisations in a paginated table with name, country, state, zip, and address.

- Feature: Add Organisation
  - Description: Add a new organisation via a sliding panel.

- Feature: Delete Organisation
  - Description: Delete an organisation with confirmation prompt.

- Feature: Pagination
  - Description: Configurable pagination with page sizes (10, 50, 100).

---

## Module: Users (Agent Management)

- Feature: User/Agent List
  - Description: View all users/agents in a paginated table with name, email, phone, department, status, and assigned customer count.

- Feature: Add User/Agent
  - Description: Add a new user/agent via a sliding panel with name, email, phone, department, IVR settings, and password.

- Feature: Edit User/Agent
  - Description: Edit existing user/agent details via a sliding panel.

- Feature: Toggle Active/Inactive Status
  - Description: Toggle user active/inactive status with a switch and confirmation prompt.

- Feature: View Active/Inactive Users
  - Description: Toggle between viewing active and inactive user lists.

- Feature: View Assigned Customers
  - Description: Navigate to view all customers assigned to a specific agent.

- Feature: Assign Customers to Agent
  - Description: Navigate to assign new customers to a specific agent.

- Feature: IVR Badge
  - Description: Shows an IVR badge for agents with IVR connection enabled.

- Feature: Agent Role Restrictions
  - Description: Agents cannot add/edit other agents or see admin-only features.

---

## Module: Calendar (Reminders)

- Feature: Calendar View
  - Description: Full calendar view (FullCalendar) showing reminders by date with monthly navigation.

- Feature: Reminder Events
  - Description: Displays reminder events on calendar dates with task type and count.

- Feature: Reminder Details Modal
  - Description: Click a date to view detailed reminder list with customer name, mobile, email, and remarks.

- Feature: Month Navigation
  - Description: Navigate between months with automatic data refresh.

- Feature: Dynamic Data Loading
  - Description: Fetches reminder data based on the currently visible month/year.

---

## Module: Quotation

- Feature: Create Quotation
  - Description: Create a quotation linked to an enquiry with customer details and line items.

- Feature: Edit Quotation
  - Description: Edit an existing quotation.

- Feature: View Quotation
  - Description: View quotation details in a sliding panel.

- Feature: Quotation List
  - Description: View all quotations for an enquiry with option to create new ones.

- Feature: Send Quotation
  - Description: Send a quotation to the customer.

- Feature: Quotation PDF Generation
  - Description: Generate quotation as a PDF document using a utility function.

---

## Module: Integration

- Feature: Mailer Setup
  - Description: Configure email mailer settings for campaign delivery.

- Feature: IVR Setup (TeleCMI)
  - Description: Configure TeleCMI IVR integration settings.

- Feature: Messaging Setup
  - Description: Configure messaging provider settings (MSG91/Interakt) for SMS and WhatsApp.

---

## Module: Setup / Configuration

- Feature: Contact Type Setup
  - Description: Create and manage contact type categories.

- Feature: Enquiry Source Setup
  - Description: Create and manage enquiry source options.

- Feature: Custom Field Setup
  - Description: Create and manage custom fields for contacts.

- Feature: Department Setup
  - Description: Create and manage departments for ticket assignment.

- Feature: Ticket Priority Setup
  - Description: Create and manage ticket priority levels.

- Feature: Ticket Categories Setup
  - Description: Create and manage ticket category options.

- Feature: Membership Programs Setup
  - Description: Create and manage membership programs with tiers, fees, validity, benefits, and coupon codes.

- Feature: Membership Form Configuration
  - Description: Configure the public membership application form.

- Feature: Preview Membership Form
  - Description: Preview the public-facing membership form.

- Feature: Public Membership Form
  - Description: Public form for guests to apply for membership with online payment support.

- Feature: Membership Payment Success/Failure Pages
  - Description: Payment result pages for online membership payments.

- Feature: Guest Connect App Setup
  - Description: Configure the Guest Connect public-facing application settings.

- Feature: Dynamic Form Builder
  - Description: Create custom forms with a drag-and-drop form builder.

- Feature: Edit Dynamic Form
  - Description: Edit existing dynamic forms.

- Feature: Form Submissions Modal
  - Description: View submissions received for dynamic forms.

- Feature: Media Library
  - Description: Upload and manage media assets (images) for use across the application.

---

## Module: Public Pages

- Feature: HRAO Membership Form
  - Description: Public membership application form for HRAO with custom styling.

- Feature: Support Ticket Submission
  - Description: Public page for raising support tickets.

- Feature: Ticket Status Check
  - Description: Public page for checking the status of a submitted support ticket.

- Feature: Thank You Page
  - Description: Confirmation page displayed after successful ticket submission.

---

## Module: Layout & Navigation

- Feature: Sidebar Navigation
  - Description: Collapsible sidebar with module icons and labels for all main modules.

- Feature: Agent Sidebar (Restricted)
  - Description: Restricted sidebar for agent role showing only Dashboard, Contacts, Membership, Interaction, Enquiry, Tickets, and Calendar.

- Feature: Breadcrumb Navigation
  - Description: Hierarchical breadcrumb navigation across all pages.

- Feature: Property Switcher
  - Description: Switch between different properties/hotels from the header.

- Feature: User Profile
  - Description: View user profile information in the header area.

- Feature: Confirmation Prompt
  - Description: Global confirmation dialog for delete and destructive actions.

- Feature: Toast Notifications
  - Description: Toast notifications for success, error, and info messages across the application.

---

## Module: Firebase / Push Notifications

- Feature: Firebase Cloud Messaging
  - Description: FCM integration for push notifications with service worker registration.

- Feature: FCM Test Button
  - Description: Test button to verify FCM push notification delivery.

- Feature: Device Token Management
  - Description: Register and manage device tokens for push notification delivery.

---

## Module: Shared / Cross-Cutting Features

- Feature: Pagination Component
  - Description: Reusable pagination component with page size selection, page numbers, and arrow navigation.

- Feature: Date Picker Component
  - Description: Reusable date picker component for date selection across modules.

- Feature: Time Picker Component
  - Description: Custom time picker component for time selection.

- Feature: Pay Now Button
  - Description: Reusable payment button component for online payment integration.

- Feature: Loading Skeletons
  - Description: Skeleton loading states across all data-fetching pages.

- Feature: Empty State Messages
  - Description: Consistent empty state messages when no data is available.

- Feature: Axios Interceptors
  - Description: API request/response interceptors for authentication token injection and error handling.

- Feature: Redux State Management
  - Description: Centralized state management for auth, customer data, filters, agents, groups, organisations, and UI state.

- Feature: Persistent Customer Filters
  - Description: Customer list filters (source, affiliation, group, type, search, page size, page number) are persisted in Redux state.

---
