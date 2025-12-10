# Product Overview

KnowItAll is a specialized graph database system designed for semantic data storage and retrieval with guaranteed data integrity.

## Core Concept

All data stored in KnowItAll must conform to SHACL (Shapes Constraint Language) constraints. The system never stores unconstrained data directly - it either validates against existing SHACL shapes or infers new shapes for incoming data.

## Key Features

- **Dual Interface**: Accepts both standard JSON-RPC (unconstrained) and JSON-RPC-LD (semantically-rich, constrained) requests
- **SHACL-Only Storage**: Every piece of data has a corresponding SHACL shape defining its structure, types, and relationships
- **Automatic Shape Inference**: For unconstrained data, the system searches for matching shapes or generates new ones with similarity links to existing shapes
- **ISO GQL Compliant**: Built on GraphLite, supporting the ISO/IEC 39075:2024 Graph Query Language standard
- **Docker Deployment**: Self-contained Docker container with embedded GraphLite database

## Data Flow

1. **Ingestion**: Data arrives via JSON-RPC or JSON-RPC-LD interface
2. **Validation**: JSON-RPC-LD data validated against SHACL shapes; JSON-RPC data matched to existing shapes or new shape generated
3. **Storage**: Validated data transformed to RDF and stored in GraphLite
4. **Retrieval**: All output guaranteed to be SHACL-constrained with JSON-LD context

## CRUD Operations

All database lifecycle operations (Create, Read, Update, Delete) are exposed through JSON-RPC-LD methods, with schema management and validation capabilities included.
