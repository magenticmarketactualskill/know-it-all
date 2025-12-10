# Technology Stack

## Core Technologies

### GraphLite (Rust-based)
- **Role**: Embedded graph database engine
- **Features**: ISO GQL support, ACID transactions, file-based storage using Sled
- **Performance**: Memory-safe Rust with C/C++ performance
- **Standard**: ISO/IEC 39075:2024 (Graph Query Language)

### SHACL (Shapes Constraint Language)
- **Role**: Constraint validation and schema definition
- **Standard**: W3C Recommendation (July 2017)
- **Purpose**: Validates RDF graphs against defined shapes
- **Components**: Node shapes, property shapes, validation reports

### JSON-RPC-LD
- **Role**: Dual-purpose data ingestion interface
- **Base**: JSON-RPC 2.0 with JSON-LD extensions
- **Features**: Optional @context for semantic data, SHACL-based validation
- **Compatibility**: Backward compatible with standard JSON-RPC

## Deployment

- **Container**: Docker-based deployment
- **Database**: Embedded GraphLite (no separate server required)
- **Storage**: File-based (similar to SQLite for graphs)

## Query Language

### ISO GQL Syntax
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

## Common Commands

### GraphLite Operations
```bash
# Initialize database
graphlite install --path ./my_db --admin-user admin --admin-password secret

# Query interface
graphlite gql --path ./my_db -u admin -p secret
```

### Docker Operations
```bash
# Build container
docker build -t knowitall .

# Run container
docker run -d -p 8080:8080 knowitall

# View logs
docker logs <container-id>
```

## Data Formats

- **Input**: JSON-RPC 2.0 or JSON-RPC-LD
- **Storage**: RDF in GraphLite
- **Output**: JSON-RPC-LD with @context
- **Validation**: SHACL shapes in Turtle/RDF format
