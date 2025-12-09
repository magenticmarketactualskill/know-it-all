# KnowItAll: Database Lifecycle Operations Feature List

## 1. Introduction

KnowItAll is a specialized graph database system designed for semantic data storage and retrieval. It is deployed as a self-contained Docker container and embeds the high-performance GraphLite graph database. KnowItAll ensures data integrity and consistency by exclusively storing data that conforms to SHACL (Shapes Constraint Language) constraints. It offers a flexible data ingestion interface supporting both standard JSON-RPC for unconstrained data and JSON-RPC-LD for semantically-rich, constrained data. This document outlines the comprehensive feature list for KnowItAll's database lifecycle operations, derived from an analysis of its core technologies: GraphLite, SHACL, and JSON-RPC-LD.

## 2. Core Technologies

The following table summarizes the core technologies that underpin KnowItAll's functionality:

| Technology | Role in KnowItAll |
|---|---|
| **GraphLite (Rust-based)** | Embedded graph database engine providing ISO GQL support, ACID transactions, and file-based storage. |
| **SHACL** | Constraint language used to define and validate the structure of all data stored in KnowItAll. |
| **JSON-RPC-LD** | Dual-purpose interface for data ingestion, supporting both unconstrained (JSON-RPC) and constrained (JSON-RPC-LD) data. |

## 3. Feature List

The following table details the features of KnowItAll's database lifecycle operations:

| Feature Category | Feature | Description | Implementation Notes |
|---|---|---|---|
| **Data Ingestion** | **Dual JSON-RPC/JSON-RPC-LD Interface** | KnowItAll provides a single endpoint that accepts both standard JSON-RPC 2.0 requests and JSON-RPC-LD requests. This allows for both unconstrained and semantically-rich data to be submitted. | The JSON-RPC-LD server will inspect incoming requests for the presence of a `@context` member to determine the request type. |
| **Data Validation** | **SHACL-based Input Validation** | For JSON-RPC-LD requests, the incoming data is validated against the SHACL shapes associated with the called method. If the data is invalid, a detailed validation report is returned. | The server will fetch the relevant SHACL shapes and use a SHACL validation engine to check the `params` object of the request. |
| **Data Validation** | **SHACL Inference for Unconstrained Data** | For standard JSON-RPC requests (unconstrained data), KnowItAll attempts to infer the appropriate SHACL shape for the data. This is a critical step before the data can be stored. | This will involve a multi-step process: searching for pre-existing SHACL shapes that match the data structure, and if none are found, generating a new SHACL shape. |
| **Data Validation** | **Pre-existing SHACL Shape Discovery** | When inferring SHACL for unconstrained data, KnowItAll will first search its internal repository of SHACL shapes to find a suitable match. | This can be implemented by comparing the structure of the incoming data with the properties and constraints of existing shapes. |
| **Data Validation** | **New SHACL Shape Generation with Similarity Linking** | If no pre-existing SHACL shape is found, a new one is generated based on the structure of the input data. The new shape will include links to the top 5 most similar existing shapes. | Similarity can be calculated based on the number of shared properties, data types, and other structural characteristics. |
| **Data Storage** | **SHACL-Constrained Storage** | KnowItAll's core principle is that all data persisted in the GraphLite database MUST be associated with and validated by a SHACL shape. Unconstrained data is never stored directly. | After successful validation (for JSON-RPC-LD) or inference and validation (for JSON-RPC), the data is transformed into RDF and stored in GraphLite. |
| **Data Storage** | **ISO GQL Compliant Graph Database** | The underlying storage is the Rust-based GraphLite, which is compliant with the new ISO GQL standard. This ensures a standardized and future-proof approach to graph data management. | Data is stored as nodes and edges, queryable via the GQL `MATCH` clause. |
| **Data Retrieval** | **Constrained Data Output** | All data retrieved from KnowItAll is guaranteed to be SHACL-constrained. The JSON-RPC-LD response includes the data and the corresponding JSON-LD `@context`. | When a query is executed, the results are formatted as JSON-LD, including the relevant context for semantic interpretation. |
| **Database Lifecycle** | **CRUD Operations via JSON-RPC-LD** | All fundamental database operations (Create, Read, Update, Delete) are exposed through the JSON-RPC-LD interface. | These operations are implemented as distinct JSON-RPC methods, each with its own set of SHACL shapes for requests and responses. |

## 4. Detailed Feature Descriptions

### 4.1. Dual Data Ingestion Interface

KnowItAll's support for both JSON-RPC and JSON-RPC-LD provides maximum flexibility for clients. Systems that are not Linked Data-aware can send plain JSON data, while more advanced clients can leverage the semantic richness of JSON-LD. This dual nature is central to KnowItAll's design, allowing it to bridge the gap between traditional and semantic web technologies.

### 4.2. SHACL-Only Storage

The commitment to storing only SHACL-constrained data is a key feature for ensuring data quality and consistency. This means that every piece of data within the GraphLite database has a corresponding SHACL shape that defines its structure, data types, and relationships. This approach eliminates data-related errors and makes the data more reliable and predictable.

### 4.3. SHACL Inference and Similarity Linking

For unconstrained data, the SHACL inference process is a powerful feature that brings unstructured data into the constrained world of KnowItAll. By first trying to match data to existing shapes, the system promotes reuse and standardization. When a new shape is created, the inclusion of links to similar shapes provides valuable context and aids in the discovery of related data structures.

## 5. Database Lifecycle Operations

The database lifecycle operations in KnowItAll are managed entirely through the JSON-RPC-LD interface. The following table maps the standard CRUD operations to their implementation in KnowItAll:

| Operation | Description | JSON-RPC-LD Method (Example) |
|---|---|---|
| **Create** | Add new, SHACL-constrained data to the database. | `createData` |
| **Read** | Retrieve SHACL-constrained data from the database using GQL queries. | `readData` |
| **Update** | Modify existing SHACL-constrained data. The update must also conform to the SHACL shape. | `updateData` |
| **Delete** | Remove data from the database. | `deleteData` |

Beyond these core operations, KnowItAll also supports:

*   **Schema Management:** Methods for creating, updating, and managing SHACL shapes themselves (e.g., `createShape`, `updateShape`).
*   **Validation:** A method to validate a piece of data against a given SHACL shape without persisting it (e.g., `validateData`).

## 6. References

[1] [GraphLite: ISO GQL Embedded Graph Database in Rust](https://byteiota.com/graphlite-iso-gql-embedded-graph-database-in-rust/)
[2] [Shapes Constraint Language (SHACL) - W3C Recommendation](https://www.w3.org/TR/shacl/)
[3] [magenticmarketactualskill/json-rpc-ld - GitHub](https://github.com/magenticmarketactualskill/json-rpc-ld)
[4] [CRUD (Create, Read, Update, Delete) - Wikipedia](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)
