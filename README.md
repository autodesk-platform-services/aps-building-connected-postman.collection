# Building Connected APIs — Complete (v1.5.0)

A complete Postman collection for the **BuildingConnected API**, generated from and verified against every operation in the published API Reference: https://aps.autodesk.com/en/docs/buildingconnected/v2/reference/http/

This collection supersedes `Building Connected APIs v1.4.0.postman_collection.json`, which is left unchanged. It preserves that collection's folder structure, request names, request bodies and `Authorization: Bearer {{Access_Token}}` header convention, and refreshes every operation, query parameter, request-body schema and response example against the BuildingConnected API **v1.5.0** reference (released 8 September 2026).

---

<a id="overview"></a>
## 📌 Overview

The collection includes endpoints for managing:

* Bids — plus Plugs, Highlights and Notes
* Bid Packages — plus Bid Leveling, Other Cost Questions and Other Cost Responses
* Bid Package Organizational Attributes *(new in v1.5.0)*
* Bid Package Event History & Bid Package Stats
* Bidding Stats
* Contacts & Certification
* Invites & Invite Notes
* Offices, Preferred Contacts & Primary Contacts
* Opportunities & Opportunity-Project Pairs
* Projects, NDAs, Cost Items & Sealed Bidding
* Project Bid Forms
* Scope-Specific Bid Forms
* Project Team Members
* Users

All endpoints follow one of two naming patterns carried over from earlier releases — see [⚙️ Naming Conventions](#naming-conventions) — built from a consistent set of verbs:

* **Get** – Retrieve resources
* **Create** – Create new resources
* **Update** – Modify existing resources
* **Delete / Remove** – Delete or detach resources
* **Batch** – Bulk operations

---

<a id="whats-new"></a>
## 🆕 What's New in v1.5.0 (8 September 2026)

* **Bid Package Organizational Attributes API** — new folder `Bid Package Organizational Attributes (New)`. Maps a bid package to a classification or location tree (up to 5 classification mappings and 1 location mapping per bid package).
* **Batch retrieval by ID** — `POST /bid-packages:batch-get` (`Bid Packages`) and `POST /projects:batch-get` (`Projects ▸ BC Projects`). Both return `207 Multi-Status` with a partial `results` array plus an `errors` array when some IDs cannot be resolved.
* **Bid leveling** — `POST /bids/{bidId}/apply-previous-plugs` copies plugs, highlights and notes from an earlier revision (replacing all existing ones), and `GET`/`PATCH /bid-packages/{bidPackageId}/line-item-toggle-states` read and write the toggle states used when levelling a bid package.
* **Private bid attachments** — `POST /bids/{bidId}/private-attachments` and `DELETE /bids/{bidId}/private-attachments/{attachmentId}`. Private attachments are visible to project owners only, not to the bidder. `GET /bids` and `GET /bids/{bidId}` gained the `includePrivateAttachments` query parameter that surfaces the `privateAttachments` field.
* **Quarantine-aware v3 file contracts** — `GET /v3/bids/{bidId}/attachments/{attachmentId}` and `GET /v3/projects/{projectId}/nda` return `200 OK` with `signedUrl: null` and `fileStatus: QUARANTINED` when Autodesk OSS has quarantined a file after malware scanning. The matching v2 operations keep the previous contract and still return an error for those files.
* **Projects** — new `filter[createdBy]` and `filter[isTemplate]` query parameters on `GET /projects` (v2 and v3); new `aiProposalExtraction` request/response field on `POST /projects` and `PATCH /projects/{projectId}` (v2 and v3); new `isBidFormTemplate` response field.
* **Invites** — new `searchText` and `filter[createdAt]` query parameters on `GET /invites`.

> New and changed requests are labelled **(New)** or **(New - Beta)** in the collection so they're easy to spot alongside the existing operations.

---

<a id="migration-note"></a>
## 🔀 Migration Note (v2 → v3)

`Projects` and `Project Team Members` exist in both v2 and v3. The deprecated v2 operations stay grouped in the `Deprecated (v2)` and `BC Projects - Deprecated (v2)` folders so they remain runnable until removal. Behavioural differences in v3:

* `projectSize` is returned in the unit given by `projectSizeUnits` instead of always square feet.
* `bidFormId` is a new nullable field on project responses.
* `PATCH /projects/{projectId}` no longer supports the `DRAFT → CLOSED` or `CLOSED → DRAFT` transitions.
* `outcome.notificationMessage` grew from 500,000 to 5,242,880 characters.
* `user.bidBoardPermissions` has been removed from Project Team Member responses.

See the full guide: https://aps.autodesk.com/en/docs/buildingconnected/v2/developers_guide/migration-guides/buildingconnected-v2-to-v3/

---

<a id="getting-started"></a>
## 🚀 Getting Started

### 0. Prerequisites

* A BuildingConnected account with API access.
* An APS (Autodesk Platform Services) app. Create one at https://aps.autodesk.com/myapps with the **BuildingConnected API** enabled, and note its **Client ID** and **Client Secret**.
* A way to complete the OAuth **Authorization Code** flow (or a **Secure Service Account**, for headless/server-to-server use) to obtain a three-legged access token — see [Authentication](#3-authentication) below.

### 1. Import the Collection

* Open Postman
* Click **Import**
* Drag in both JSON files from this repository (collection + environment)

### 2. Select the Environment

Select the `Building Connected Environment v1.5.0` environment, then fill in the variables you need — see [🔑 Environment Variables](#environment-variables) for the full reference.

> Request bodies carry **no hard-coded IDs** — every account, project, bid package, office, user or resource ID in a payload is a reusable variable named after the field it fills (e.g. `{{projectId}}`, `{{bidPackageId}}`). Fill them in once in the environment and every request picks them up.

### 3. Authentication

Every operation requires a **three-legged (user context)** access token, obtained via the OAuth Authorization Code flow or a Secure Service Account (SSA) flow. Each request sends:

```
Authorization: Bearer {{Access_Token}}
```

Scopes: `data:read` for reads and `data:write` for create/update/delete. Requests with a body also send `Content-Type: application/json`.

> Three-legged tokens expire (typically within an hour). When requests start failing with `401`, re-run your Authorization Code or SSA flow and paste the new value into `Access_Token` — the collection does not refresh tokens automatically.

### 4. Send a Request

* Fill in path variables per request via the **Path Variables** table (e.g. `bidPackageId`)
* Query parameters ship **disabled** — enable only the ones you need

---

<a id="environment-variables"></a>
## 🔑 Environment Variables

### Authentication

| Variable | Description |
| -------- | ----------- |
| `APS_CLIENT_ID` | Your APS app's client ID. |
| `APS_CLIENT_SECRET` | Your APS app's client secret. |
| `Access_Token` | Three-legged bearer token, sent as `Authorization: Bearer {{Access_Token}}` on every request. |

<details>
<summary>Resource ID variables (32) — expand for the full reference</summary>

**Bid packages, bids & line items**

| Variable | Description |
| -------- | ----------- |
| `bidPackageId` | The ID of a bid package. |
| `templateBidPackageId` | The ID of an existing bid package to clone the form, files and settings from when creating a new one. |
| `bidAdminUserId` | The BuildingConnected user ID of the bid administrator on a sealed project; only they (or a current administrator) can unseal bids after the due date. |
| `bidderCompanyId` | The ID of a bidder's (subcontractor's) company. |
| `lineItemId` | The ID of a bid-form or cost line item. |
| `questionId` | The ID of an Other Cost Question that a response answers. |
| `treeId` | The ID of a bid package's organizational-attribute classification or location tree. |
| `nodeId` | The ID of a node within that classification or location tree. |
| `attachmentId` | The ID of a file attached to a bid. |
| `costId` | The ID of an indirect cost item on a project. |

**Certification**

| Variable | Description |
| -------- | ----------- |
| `certificateAgencyId` | The ID of a certifying agency, used in a project's `relevantCertificates` entries. |
| `certificateTypeId` | The ID of a certificate type, paired with `certificateAgencyId` in the same entry. |

**Invites**

| Variable | Description |
| -------- | ----------- |
| `inviteId` | The ID of an invite. |
| `inviteId1`, `inviteId2` | Two distinct invite IDs, used together by the batch-create-invite-notes example body. |
| `inviteeUserId` | The BuildingConnected user ID of a bidder being invited to a bid package. |
| `selectedOfficeId` | The ID of the office the invitee is being invited through (a user can belong to more than one). |

**Offices**

| Variable | Description |
| -------- | ----------- |
| `owningOfficeId` | The ID of the office that owns a project, opportunity or bid package. |
| `clientOfficeId` | The ID of the client's office on an opportunity. |

**Opportunities**

| Variable | Description |
| -------- | ----------- |
| `opportunityId` | The ID of an opportunity (or, on Opportunity-Project Pair operations, the opportunity half of the pair). |
| `parentOpportunityId` | The ID of the parent opportunity when creating or updating a grouped opportunity. |
| `groupChildOpportunityId` | The ID of a child opportunity added to a group. |
| `clientLeadUserId` | The BuildingConnected user ID of the client contact leading the opportunity. |
| `memberUserId` | The BuildingConnected user ID of a team member assigned to the opportunity. |

**Projects**

| Variable | Description |
| -------- | ----------- |
| `projectId` | The ID of a project. |
| `originalProjectId` | The ID of the template or project a new project is cloned from. |
| `currentAccLinkedHubId` | The ID of the ACC/Forma hub that contains the linked project. |
| `currentAccLinkedProjectId` | The ID of the ACC/Forma project linked to this BuildingConnected project. |
| `currentAccDocsFolderId` | The ID of the ACC/Forma Docs folder shared with all bidders. |

**Companies & users**

| Variable | Description |
| -------- | ----------- |
| `companyId` | The ID of a BuildingConnected company. |
| `userId` | The ID of a user. |
| `externalId` | A caller-defined identifier on a bid form or line item, used to cross-reference an external estimating system. |

</details>

---

<a id="naming-conventions"></a>
## ⚙️ Naming Conventions

The collection carries forward two request-naming patterns from earlier releases; both remain in active use side by side and neither is being retired in v1.5.0:

* **`Resource - Action`** — used by `Bid Packages`, `Bids`, `Contacts`, `Certificates`, `Offices` and most of `Invites`.
  Example: `Bid Packages - Create`, `Bid Packages - Get by ID`, `Bid Packages - Update by ID`, `Bid Packages - Delete by ID`, `Bid Packages - Batch Create`.
* **`Action Resource`** — used by `Projects`, `Opportunities`, `Opportunity-Project Pairs`, `Users`, `Project Bid Forms`, `Scope-Specific Bid Forms` and `Project Team Members`.
  Example: `Get All Projects`, `Create Project`, `Get Project by ID`, `Update Project by ID`, `Delete Project by ID`.

Within both patterns, the same verbs carry the same meaning everywhere:

* **Get** – Retrieve resources (`Get All …` / `… - List` for collections, `Get … by ID` for a single resource)
* **Create** – Create new resources
* **Update** – Modify existing resources
* **Delete / Remove** – Delete or detach resources
* **Batch** – Bulk create / update / delete / get

If you're scripting against request names (e.g. in a Postman/Newman collection runner filter), check which pattern the target folder uses rather than assuming one convention collection-wide.

---

<a id="beta-and-new-endpoints"></a>
## 🧪 Beta and New Endpoints

Endpoints are labelled **(Beta)** where the API itself is in beta, and **(New)** / **(New - Beta)** where they were introduced in this release. The `Plugs`, `Highlights`, `Notes`, `Other Cost Questions`, `Other Cost Responses`, `Bid Leveling` and `Sealed Bidding` folders arrived in v1.4.0; the items listed under [🆕 What's New in v1.5.0](#whats-new) above arrived in v1.5.0.

Examples:

* `Bid Package Organizational Attributes (New)`
* `Bid Packages - Batch Get (New)`
* `Bids - Apply Previous Plugs (New)`
* `Bids - Get Attachment by ID (New - Beta)`

---

<a id="collection-structure"></a>
## 🗂 Collection Structure

<details>
<summary>Expand full folder and request tree (31 folders, 155 requests)</summary>

```
Bid Package Event History (Beta)
  Bid Package Activities - List
Bid Package Organizational Attributes (New)
  Get All Bid Package Organizational Attributes
  Create Bid Package Organizational Attributes
  Update Bid Package Organizational Attribute by ID
  Delete Bid Package Organizational Attribute by ID
Bid Package Stats (Beta)
  Bid Package Stats - Get by ID
  Bid Package Stats - Batch Get
Bid Packages
  Bid Leveling (New)
    Bid Packages - Get Bid Leveling Settings
    Bid Packages - Update Bid Leveling Settings
    Bid Packages - Get All Line Item Toggle States (New)
    Bid Packages - Update Line Item Toggle States (New)
    Bid Packages - Get All Plugs
    Bid Packages - Get All Highlights
    Bid Packages - Get All Notes
  Other Cost Questions (New)
    Bid Packages - Get Other Cost Questions
    Bid Packages - Upsert Other Cost Questions
    Bid Packages - Update Other Cost Question by ID
    Bid Packages - Delete Other Cost Question by ID
  Other Cost Responses (New)
    Bid Packages - Get All Other Cost Responses
    Bid Packages - Create Other Cost Response
    Bid Packages - Get Other Cost Response by ID
    Bid Packages - Update Other Cost Response by ID
    Bid Packages - Delete Other Cost Response by ID
  Bid Packages - List
  Bid Packages - Create
  Bid Packages - Batch Create
  Bid Packages - Batch Delete
  Bid Packages - Batch Update
  Bid Packages - Batch Get (New)
  Bid Packages - Get by ID
  Bid Packages - Update by ID
  Bid Packages - Delete by ID
Bidding Stats (Beta)
  Get Bidding Stats by Company ID
  Batch Get Bidding Stats
Bids
  Plugs (New)
    Bids - Get Plugs
    Bids - Create Plug
    Bids - Get Plug by ID
    Bids - Update Plug by ID
    Bids - Delete Plug by ID
    Bids - Delete All Plugs
  Highlights (New)
    Bids - Get Highlights
    Bids - Create Highlight
    Bids - Get Highlight by ID
    Bids - Update Highlight by ID
    Bids - Delete Highlight by ID
    Bids - Delete All Highlights
  Notes (New)
    Bids - Get Notes
    Bids - Create Note
    Bids - Get Note by ID
    Bids - Update Note by ID
    Bids - Delete Note by ID
    Bids - Delete All Notes
  Bids - Get All
  Bids - Get by ID
  Bids - Create
  Bids - Create a Bid Attachment
  Bids - Delete Bid by Bid ID (Beta)
  Bids - Delete Bid Attachment by Attachment ID
  Bids - Get Attachment by ID
  Bids - Get Attachment by ID (New - Beta)
  Bids - Create Private Attachments (New)
  Bids - Delete Private Attachment by Attachment ID (New)
  Bids - Get Line Items
  Bids - Apply Previous Plugs (New)
Contacts
  Contacts - Get All
  Contacts - Get by ID
  Contacts - Get Certificate Download URL
Certification
  Certificates - Get Types
  Certificates - Get Agencies
Invites
  Invites - Get All Bid Package Invites
  Invites - Get Invite by ID
  Invites - Batch Invite Bidders (Beta)
  Invites - Import Bidder Emails (Deprecated)
  Invites - Batch Import Bidder Emails (Deprecated)
  Invites - Update Invite by ID (Beta)
  Invites - Get All Invite Notes (Beta)
  Invites - Create Invite Note (Beta)
  Invites - Batch Create Invite Notes (Beta)
  Invites - Get Invite Note by ID (Beta)
  Invites - Delete Invite Note by ID (Beta)
  Invites - Remove Invitee from Invite (Beta)
  Invites - Get Invite Certificate Download URL
Offices
  Offices - Get All Offices (Requesting User's Company)
  Offices - Get Office by ID (Requesting User's Company)
Opportunity-Project Pairs
  Get All Opportunity Project Pairs
  Get Opportunity Project Pair by ID
  Create Opportunity Project Pair
  Update Opportunity Project Pair by ID
Opportunities
  Get All Opportunities
  Create Opportunity
  Get Opportunity by ID
  Update Opportunity by ID
  Delete Opportunity by ID
  Get Opportunity Comments
  Get Opportunity Comment by Comment ID
Preferred Contacts (Beta)
  Get Preferred Contacts for Requesting User's Office
Primary Contacts (Beta)
  Get Primary Contacts list for Office
Project Bid Forms
  Get All Project Bid Forms
  Get Project Bid Form by ID
  Create Project Bid Form
  Update Project Bid Form by ID (Beta)
  Get Project Bid Form Line Items
  Create Line Item for Project Bid Form
  Batch Create Line Items
  Batch Update Line Items
  Batch Delete Line Items
  Update Line Item by ID
  Delete Line Item by ID
Project Team Members
  Deprecated (v2)
    Get All Project Team Members (Deprecated)
    Create Project Team Member (Deprecated)
    Get Project Team Member by ID (Deprecated)
    Update Project Team Member by ID (Deprecated)
  Get All Project Team Members (New-Beta)
  Create Project Team Member (New-Beta)
  Batch Create Project Team Members (Beta)
  Get Project Team Member by ID (New - Beta)
  Update Project Team Member by ID (New - Beta)
  Remove Project Team Member from Project
Projects
  NDA
    Create Project NDA (Beta)
    Delete Project NDA by ID (Beta)
    Get Project NDA (Beta)
    Get Project NDA (New - Beta)
    Sign Project NDA (Beta)
  Cost Items
    Get Project Cost Items
    Create Project Cost Item
    Batch Create Project Cost Items
    Batch Update Project Cost Items
    Batch Delete Project Cost Items
    Update Project Cost Item by Cost ID
    Delete Project Cost Item by Cost ID
  BC Projects
    Get All Projects (New - Beta)
    Create Project (New - Beta)
    Get Project by ID (New - Beta)
    Update Project by ID (New - Beta)
    Delete Project by ID
    Batch Get Projects (New)
  BC Projects - Deprecated (v2)
    Get All Projects (Deprecated)
    Create Project (Deprecated)
    Get Project by ID (Deprecated)
    Update Project by ID (Deprecated)
  Sealed Bidding (New)
    Unseal Bid Packages in Project
Scope-Specific Bid Forms
  Get All Scope-Specific Bid Forms
  Create Scope-Specific Bid Form
  Get Scope-Specific Bid Form by ID
  Update Scope-Specific Bid Form by ID (Beta)
  Get Scope-Specific Bid Form Line Items
  Create Line Item for Scope-Specific Bid Form
  Batch Create Line Items
  Batch Update Line Items
  Batch Delete Line Items
  Update Line Item by ID
  Delete Line Item by ID
Users
  Get All Users
  Get User by UserID
  Get Current User
```

</details>

---

<a id="conventions"></a>
## 📐 Conventions

* Path parameters use Postman path variables (`:bidPackageId`) with empty values — fill them in the request's **Path Variables** table.
* Request bodies carry no hard-coded IDs; every resource ID is a reusable Postman variable declared in the environment. Non-identifying example values (names, dates, amounts, enum values) are kept as documented.
* Every request carries a description with the operation ID, endpoint, authentication context, required scopes, headers, path/query parameters, request-body schema and HTTP status codes.
* Every request carries a saved example response taken from the API Reference, with one exception: `Create Project (New - Beta)` currently ships without one, as no example is published for that operation in the reference at time of writing.
* Deprecated operations are grouped in `Deprecated (v2)` folders and labelled in their names.

---

<a id="contribution"></a>
## 🤝 Contribution

Feel free to:

* Raise issues
* Suggest improvements
* Submit pull requests

---

<a id="license"></a>
## License

This sample is licensed under the terms of the MIT License. Please see the LICENSE file for full details.

## Written by

Naveen Kumar Thalaivirichan, Developer Advocate and Support

---

Happy Testing! 🚀
