# Building Connected APIs — Complete Postman Collection v1.4.0

This repository contains a comprehensive Postman collection for the **BuildingConnected** and **TradeTapp** APIs. It is organized in a clean, consistent, and scalable structure to support development, testing, and integration workflows, and it is built from and verified against every operation in the published API Reference:

<https://aps.autodesk.com/en/docs/buildingconnected/v2/reference/http/>

| File | Purpose |
| ---- | ------- |
| `Building Connected APIs v1.4.0.postman_collection.json` | The collection (156 requests) |
| `Building Connected Environment v1.4.0.postman_environment.json` | Environment with every variable the collection references |

---

## 📌 Overview

The collection includes endpoints for managing:

* Bids — plus Plugs, Highlights and Notes
* Bid Packages — plus Bid Leveling, Other Cost Questions and Other Cost Responses
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
* TradeTapp — Users, Financials, Qualifications & Flags

All endpoints follow a consistent naming convention using:

* **Get** – Retrieve resources
* **Create** – Create new resources
* **Update** – Modify existing resources
* **Delete / Remove** – Delete or detach resources
* **Batch** – Bulk operations

### Coverage

| | Count |
| --- | --- |
| Documented API Reference operations | **155** |
| Extra request preserved for backward compatibility (not in the current API Reference) | 1 |
| **Total requests** | **156** |
| Requests with a full description (endpoint, auth, scopes, headers, params, body schema, status codes) | 156 |
| Requests with a saved example response | 154 |

Breakdown by base path: 135 x `buildingconnected/v2`, 8 x `buildingconnected/v3`, 13 x `tradetapp/v2`.
Methods: 71 GET, 39 POST, 24 PATCH, 21 DELETE, 1 PUT.

---

## 🚀 Getting Started

### 1. Import the Collection

* Open Postman
* Click **Import**
* Drag in both JSON files from this repository (collection + environment)

### 2. Select the Environment

Select the `Building Connected Environment v1.4.0` environment, then configure the following variables:

| Variable | Description |
| -------- | ----------- |
| `APS_CLIENT_ID` | Your APS app client ID |
| `APS_CLIENT_SECRET` | Your APS app client secret |
| `Access_Token` | Three-legged bearer token (used by every request) |
| `inviteId`, `inviteId1`, `inviteId2` | Used by the invite-note request bodies |

### 3. Authentication

Every operation requires a **three-legged (user context)** access token, obtained via the OAuth Authorization Code flow or a Secure Service Account (SSA) flow. Each request sends:

```
Authorization: Bearer {{Access_Token}}
```

Scopes: `data:read` for reads, `data:write` for create/update/delete, and `data:create` for `POST /construction/tradetapp/v2/flags`. Requests with a body also send `Content-Type: application/json`.

TradeTapp requests include an optional, pre-disabled `general-contractor-company-id` header. Enable it only if the calling user belongs to a parent enterprise company in TradeTapp.

### 4. Send a Request

* Fill in path variables per request via the **Path Variables** table (e.g. `bidPackageId`)
* Query parameters ship **disabled** — enable only the ones you need

---

## 📂 Collection Structure

The collection is organized into the following folders, each containing logically grouped endpoints for better navigation and usability:

```
Bid Package Event History (Beta)      1
Bid Package Stats (Beta)              2
Bid Packages                          8
  Bid Leveling (New)                  5
  Other Cost Questions (New)          4
  Other Cost Responses (New)          5
Bidding Stats (Beta)                  2
Bids                                  8
  Plugs (New)                         6
  Highlights (New)                    6
  Notes (New)                         6
Contacts                              3
Certification                         2
Invites                              14
Offices                               2
Opportunity-Project Pairs             4
Opportunities                         7
Preferred Contacts (Beta)             1
Primary Contacts (Beta)               1
Project Bid Forms                    11
Project Team Members                  6
  Deprecated (v2)                     4
Projects
  NDA                                 4
  Cost Items                          7
  BC Projects                         5
  BC Projects - Deprecated (v2)       4
  Sealed Bidding (New)                1
Scope-Specific Bid Forms             11
Users                                 3
TradeTapp APIs
  Users                               1
  Financials                          2
  Qualifications                      4
  Flags                               6
                                   ----
                                    156
```

---

## ⚙️ Naming Conventions

All requests follow a standardized naming pattern:

* `Get All Resources`
* `Get Resource by ID`
* `Create Resource`
* `Update Resource by ID`
* `Delete Resource by ID`
* `Batch Create / Update / Delete`

Example:

* **Get All Projects**
* **Create Project**
* **Get Project by ID**
* **Update Project by ID**
* **Delete Project by ID**

---

## 🧪 Beta and New Endpoints

Endpoints are labelled **(Beta)** where the API itself is in beta, and **(New)** where they were introduced in this release.

Examples:

* `Bid Package Stats (Beta)`
* `Invite Bidders (Beta)`
* `Bids > Plugs (New)`

---

## 🆕 What is New in BuildingConnected API v1.4.0 (Public Beta release 2)

| Capability | Where in this collection |
| ---------- | ------------------------ |
| **Bid Plugs API** | `Bids > Plugs (New)` (6) and `Bid Packages > Bid Leveling (New)` |
| **Bid Highlights API** | `Bids > Highlights (New)` (6) |
| **Bid Notes API** | `Bids > Notes (New)` (6) |
| **Bid Leveling Settings API** | `Bid Packages > Bid Leveling (New)` (GET + PATCH) |
| **Other Cost Questions API** | `Bid Packages > Other Cost Questions (New)` (4) |
| **Other Cost Responses API** | `Bid Packages > Other Cost Responses (New)` (5) |
| **Bid Package Unsealing** | `Projects > Sealed Bidding (New)` |
| **Clear ACC Docs folder association** | `Projects > BC Projects`, `Bid Packages` |
| **Multi-value filtering** | 7 list operations; noted in each description |
| **ACC-linked project visibility for bid forms** | `Project Bid Forms`, `Scope-Specific Bid Forms` |

Notes and limits captured in the request descriptions:

* Other-cost questions are capped at **200 per bid package**, so the list endpoint is not paginated.
* `PUT .../other-cost-questions` **replaces the entire list**; omitted questions (and any responses referencing them) are deleted. Send `id` to update an existing question, omit it to create one.
* Deleting a question also deletes every other-cost response that references it.
* **Unsealing is one-way** and cannot be undone. It applies only to bid packages in a project with sealed bidding already enabled, and only after that bid package's due date has passed. Send either `{"bidPackageIds": [...]}` (max 100) or `{"unsealAll": true}` — never both.
* `bid-packages/{id}/plugs|highlights|notes` paginate over bids (default 100), with each bid capped at 100 child records; use the per-bid list endpoints to page through the remainder.

---

## 🤝 Contribution

Feel free to:

* Raise issues
* Suggest improvements
* Submit pull requests

---

## License

This sample is licensed under the terms of the MIT License. Please see the LICENSE file for full details.

## Written by

Naveen Kumar Thalaivirichan, Developer Advocate and Support

---

Happy Testing! 🚀
</content>
</invoke>
