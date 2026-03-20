# ♻️ Prototype: KALPATARU (Digital Waste Management & Circular Economy)

## 📌 Project Overview
**KALPATARU** is a digital transformation initiative designed to bridge the gap between waste management and the circular economy. This prototype demonstrates a robust backend architecture for managing a community-driven waste collection system, where waste is treated as a tradeable commodity through a transparent transaction ledger.

The project aligns with the **Sovereign Systems** narrative by empowering local communities to manage their resources and sustainability data independently, ensuring economic value is returned directly to the participants.

---

## 🏗️ System Architecture (Circular Economy Flow)

```mermaid
flowchart TD
    %% Global Styling
    classDef default fill:#f9f9f9,stroke:#333,stroke-width:1px,color:#333,font-family:Inter,sans-serif;
    classDef user fill:#e1f5fe,stroke:#01579b,stroke-width:2px;
    classDef core fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;
    classDef logic fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    classDef data fill:#efebe9,stroke:#5d4037,stroke-width:2px;
    classDef infra fill:#eceff1,stroke:#455a64,stroke-width:1px;

    %% Nodes & Structure
    Member((👤 Member/User))
    Operator((👷 Operator))
    
    subgraph API_GATEWAY [Restful API Layer]
        direction LR
        AUTH[JWT Authentication] --- ROUTE[Express Router]
    end

    subgraph BUSINESS_LOGIC [Waste Transaction Engine]
        direction TB
        TRASH[Trash Catalog] --- CART[Collection Cart]
        TX[Transaction Manager]
    end

    subgraph DATA_PERSISTENCE [Relational State]
        VDB[(MySQL Database)]
        ORM[Sequelize ORM]
    end

    %% Connections
    Member ==>|Request Pick-up| AUTH
    Operator ==>|Validate Collection| AUTH
    AUTH ==> ROUTE
    ROUTE ==> TX
    
    TX -->|Query Price| TRASH
    TX -->|Update| CART
    TX ==>|Commit| ORM
    ORM ==> VDB
    
    %% Assign Classes
    class Member,Operator user;
    class AUTH,ROUTE core;
    class TRASH,CART,TX logic;
    class VDB,ORM data;
    class API_GATEWAY,BUSINESS_LOGIC,DATA_PERSISTENCE infra;
```

### 📋 Diagram Legend (System Functional Mapping)
| Symbol/Style | Description | Classification |
| :--- | :--- | :--- |
| **Double Circle (( ))** | System Actors (Members & Operators) | **Actors** |
| **Rectangle [ ]** | API Endpoints and Functional Modules | **Component** |
| **Cylinder [( )]** | Persistent Relational Data (MySQL) | **Data Store** |
| **Bold Line (==>)** | Transactional & Financial Data Flow | **Primary Relation** |
| **Thin Line (---)** | Informational or Catalog Lookups | **Association** |
| **Blue Box** | User Interaction & Security Layer | **Access Layer** |
| **Purple Box** | Core API & Routing Middleware | **Orchestration** |
| **Green Box** | Waste Processing & Financial Logic | **Business Layer** |

---

## 🚀 Key Components & Logic

### 1. The Economy: Trash Catalog & Pricing
Waste is categorized (e.g., Plastic, Paper, Metal) with dynamic pricing. The system treats each collection as a financial transaction, converting physical waste weight into digital credits for the user.

### 2. The Ledger: Transaction Manager
A strict relational model handles the lifecycle of a waste transaction:
- **Carts:** Temporary storage for pending collections.
- **Transactions:** Finalized records linking members, operators, and specific trash items.
- **Details:** Granular logging of weights and individual item valuations.

### 3. Security: Role-Based Access Control (RBAC)
Using JWT (JSON Web Tokens) and Bcrypt, the system distinguishes between **Members** (who submit waste) and **Operators** (who verify and process waste). This ensures data integrity and prevents unauthorized financial claims.

---

## 🛡️ Sustainability Workflow

1.  **Collection:** A Member gathers waste and requests a pickup/drop-off.
2.  **Valuation:** An Operator weighs the waste and inputs it into the system via the "Cart".
3.  **Settlement:** The system calculates the total value based on the current Trash Catalog.
4.  **Commitment:** A Transaction is generated, updating the Member's balance and the Operator's ledger.
5.  **Audit:** All historical transactions are available for reporting and environmental impact analysis.

---

## 🛠️ Tech Stack Employed

| Layer | Technologies |
| :--- | :--- |
| **Backend Framework** | ![](https://img.shields.io/badge/Node.js-339933?style=flat&logo=node.js&logoColor=white) ![](https://img.shields.io/badge/Express-000000?style=flat&logo=express&logoColor=white) |
| **Database & ORM** | ![](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white) ![](https://img.shields.io/badge/Sequelize-52B0E7?style=flat&logo=sequelize&logoColor=white) |
| **Authentication** | ![](https://img.shields.io/badge/JWT-000000?style=flat&logo=jsonwebtokens&logoColor=white) ![](https://img.shields.io/badge/Bcrypt-4F5D95?style=flat&logo=pypi&logoColor=white) |
| **Infrastructure** | ![](https://img.shields.io/badge/Nodemon-76D04B?style=flat&logo=nodemon&logoColor=white) ![](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white) |
