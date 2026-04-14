# Building Connected API Postman Collection

This repository contains a comprehensive Postman collection for interacting with the BuildingConnected APIs. It is organized in a clean, consistent, and scalable structure to support development, testing, and integration workflows.

---

## 📌 Overview

The collection includes endpoints for managing:

* Bids
* Bidding Stats
* Contacts & Certificates
* Invites & Invite Notes
* Offices & Preferred Contacts
* Opportunities
* Projects & NDAs
* Project Bid Forms
* Scope-Specific Bid Forms
* Project Team Members
* Users

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
* Select the exported collection file from this repository

### 2. Set Up Environment Variables

Create a Postman environment and configure the following variables:

| Variable            | Description                  |
| --------------------| ---------------------------- |      
| `APS_CLIENT_ID`     | Your APS Client Id           |
| `APS_CLIENT_SECRET` | Your APS Client Secret       |
| `Access_Token`         | Authorization token (Bearer) |

### 3. Authentication

Most endpoints require authentication. Add the following header:

```
Authorization: Bearer {{authToken}}
```

---

## 📂 Collection Structure

The collection is organized into the following folders:

* **Bids**
* **Contacts**
* **Invites**
* **Offices**
* **Opportunities**
* **Projects**
* **Project Bid Forms**
* **Scope-Specific Bid Forms**
* **Project Team Members**
* **Users**

Each folder contains logically grouped endpoints for better navigation and usability.

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

## 🧪 Beta Endpoints

Some endpoints may be marked as **(Beta)** or introduced as part of a **new** release.

Example:

* `Invite Bidders (Beta)`

---

## 📝 Notes

* Ensure all required IDs (e.g., `projectId`, `bidId`, `formId`) are provided
* Batch endpoints use request payloads for bulk operations
* Some endpoints return **signed URLs** for secure file downloads

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
