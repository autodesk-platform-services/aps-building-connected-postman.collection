# Building Connected APIs — Complete Postman Collection v1.4.0

This repository contains a comprehensive Postman collection for the **BuildingConnected** and **TradeTapp** APIs. It is organized in a clean, consistent, and scalable structure to support development, testing, and integration workflows, and it is built from and verified against every operation in the published API Reference: https://aps.autodesk.com/en/docs/buildingconnected/v2/reference/http/>

| File |
| ---- | 
| `Building Connected APIs v1.4.0.postman_collection.json` | 
| `Building Connected Environment v1.4.0.postman_environment.json` | 

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
