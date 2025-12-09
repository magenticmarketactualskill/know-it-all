# KnowItAll Research Notes

## GraphLite Analysis

### Key Findings:
1. **Multiple GraphLite Projects Found**:
   - schencoding/GraphLite: A lightweight graph computation platform in C/C++ (version 0.20)
   - Newer GraphLite: Rust-based embedded graph database with ISO GQL support (Nov 2025)
   - haasdo95/graphlite: A lightweight C++ graph library
   - Graphlite (Python): Social graph datastore similar to FlockDB

2. **ISO GQL Support**:
   - The newer Rust-based GraphLite has full ISO GQL (ISO/IEC 39075:2024) support
   - This is the new Graph Query Language standard
   - Launched in November 2025

3. **schencoding/GraphLite (C/C++)**:
   - Version 0.20
   - Lightweight graph computation platform
   - Written in C++ (93.4%)
   - Limited documentation in the README
   - Focus on graph computation rather than database features

### URLs to Investigate:
- https://byteiota.com/graphlite-iso-gql-embedded-graph-database-in-rust/
- https://graphlite.readthedocs.io/
- https://www.w3.org/TR/shacl/
- https://github.com/magenticmarketactualskill/json-rpc-ld.git

### Next Steps:
1. Investigate the Rust-based GraphLite with ISO GQL support (more relevant)
2. Read SHACL specification
3. Analyze JSON-RPC-LD repository


## GraphLite (Rust-based) - ISO GQL Implementation

### Core Features:
- **Embedded Graph Database**: Single binary, no server required (like SQLite for graphs)
- **ISO GQL Standard**: Full support for ISO/IEC 39075:2024 (Graph Query Language)
- **Rust Foundation**: Memory safety, C/C++ performance, modern tooling
- **ACID Transactions**: Full ACID guarantees for local operations
- **File-based Storage**: Uses Sled embedded storage (Rust-native)
- **Cost-based Query Optimization**: Advanced query planning

### Database Operations:
1. **Installation/Initialization**:
   - `graphlite install --path ./my_db --admin-user admin --admin-password secret`
   
2. **Query Interface**:
   - `graphlite gql --path ./my_db -u admin -p secret`

3. **Schema Management**:
   - CREATE SCHEMA
   - CREATE GRAPH
   
4. **Data Operations**:
   - INSERT (nodes and relationships)
   - MATCH (graph pattern matching)
   - RETURN (result projection)

### ISO GQL Syntax Examples:
```gql
-- Schema creation
CREATE SCHEMA /social;
CREATE GRAPH /social/network;

-- Data insertion
INSERT (:User {name: 'Alice'})-[:FOLLOWS]->(:User {name: 'Bob'});

-- Querying
MATCH (u:User)-[:FOLLOWS]->(f:User)
RETURN u.name, f.name;
```

### Relevant for KnowItAll:
- Embedded design (can be embedded in Docker container)
- ISO GQL standard support
- ACID transactions
- Schema-based approach (aligns with SHACL constraints)
- File-based storage


## SHACL (Shapes Constraint Language) Analysis

### Core Concepts:
- **W3C Recommendation**: Published July 20, 2017 (stable standard)
- **Purpose**: Language for validating RDF graphs against a set of conditions
- **Shapes Graph**: RDF graph containing the constraint definitions
- **Data Graph**: RDF graph being validated against the shapes graph

### Key Components:

#### 1. Shapes
- **Node Shapes**: Constraints on RDF nodes
- **Property Shapes**: Constraints on property values
- **Targets**: Define which nodes to validate (sh:targetClass, sh:targetNode, etc.)

#### 2. Constraint Types
- **Value Type**: sh:class, sh:datatype, sh:nodeKind
- **Cardinality**: sh:minCount, sh:maxCount
- **Value Range**: sh:minExclusive, sh:maxInclusive, etc.
- **String-based**: sh:minLength, sh:maxLength, sh:pattern
- **Logical**: sh:not, sh:and, sh:or, sh:xone
- **Shape-based**: sh:node, sh:property
- **Other**: sh:closed, sh:hasValue, sh:in

#### 3. Validation Process
- **Input**: Shapes graph + Data graph
- **Output**: Validation Report (sh:ValidationReport)
- **Report Contains**:
  - sh:conforms (boolean)
  - sh:result (validation results)
  - sh:focusNode, sh:resultPath, sh:value
  - sh:resultMessage, sh:resultSeverity

#### 4. SPARQL-based Constraints
- Advanced feature for custom constraints
- SELECT-based and ASK-based validators
- Pre-bound variables: $this, $shapesGraph, $currentShape

### Example SHACL Shape:
```turtle
ex:PersonShape
    a sh:NodeShape ;
    sh:targetClass ex:Person ;
    sh:property [
        sh:path ex:ssn ;
        sh:maxCount 1 ;
        sh:datatype xsd:string ;
        sh:pattern "^\\d{3}-\\d{2}-\\d{4}$" ;
    ] ;
    sh:property [
        sh:path ex:worksFor ;
        sh:class ex:Company ;
        sh:nodeKind sh:IRI ;
    ] ;
    sh:closed true ;
    sh:ignoredProperties ( rdf:type ) .
```

### Relevant for KnowItAll:
- **Constraint Validation**: Ensures data quality before storage
- **Schema Definition**: SHACL shapes define the structure of valid data
- **Inference Support**: Can be used to infer SHACL from unconstrained data
- **Validation Reports**: Provides detailed feedback on constraint violations
- **RDF-based**: Works naturally with graph databases
- **Extensible**: SPARQL-based constraints allow custom validation logic


## JSON-RPC-LD Analysis

### Overview:
JSON-RPC-LD is a lightweight extension to JSON-RPC 2.0 that combines:
- **JSON-RPC 2.0**: Simple remote procedure call protocol
- **JSON-LD**: JSON for Linked Data (semantic expressiveness)
- **SHACL**: Shapes Constraint Language (validation capabilities)

### Key Features:

#### 1. @context Member
- **Optional** `@context` member in `params` or `result` objects
- Provides mapping from terms to IRIs (Internationalized Resource Identifiers)
- Enables data to be interpreted as Linked Data
- Follows JSON-LD 1.1 specification

#### 2. SHACL-based Validation
- Servers MAY provide SHACL shapes graph for each method
- Describes expected structure and types of `params` and `result`
- Used for validation by clients
- Used for documentation by developers

#### 3. Example Structure

**Request with @context:**
```json
{
  "jsonrpc": "2.0",
  "method": "getUser",
  "params": {
    "@context": {
      "id": "@id",
      "User": "http://example.org/User",
      "userId": "http://example.org/User#id"
    },
    "@type": "User",
    "userId": 123
  },
  "id": 1
}
```

**Response with @context:**
```json
{
  "jsonrpc": "2.0",
  "result": {
    "@context": {
      "id": "@id",
      "User": "http://example.org/User",
      "name": "http://xmlns.com/foaf/0.1/name",
      "email": "http://xmlns.com/foaf/0.1/mbox"
    },
    "@id": "http://example.org/users/123",
    "@type": "User",
    "name": "John Doe",
    "email": "john.doe@example.com"
  },
  "id": 1
}
```

### Relevant for KnowItAll:
- **Dual Interface**: Supports both unconstrained JSON-RPC and constrained JSON-RPC-LD
- **Semantic Richness**: @context enables semantic interpretation of data
- **Validation**: SHACL shapes provide validation for constrained data
- **Backward Compatibility**: JSON-RPC-LD is backward compatible with JSON-RPC 2.0
- **Flexible**: @context is optional, allowing unconstrained input
- **Documentation**: SHACL shapes serve as machine-readable API documentation
