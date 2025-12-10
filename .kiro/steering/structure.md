# Project Structure

## Current State

This is an early-stage project in the planning and research phase. The repository currently contains design documentation and research notes.

## Repository Organization

```
.
├── README.md                                          # Project overview
├── KnowItAll: Database Lifecycle Operations Feature List.md  # Comprehensive feature specification
├── research_notes.md                                  # Technical research on GraphLite, SHACL, JSON-RPC-LD
└── .kiro/
    └── steering/                                      # AI assistant guidance documents
```

## Expected Future Structure

Based on the feature specification, the project will likely evolve to include:

```
.
├── src/                                               # Source code
│   ├── api/                                          # JSON-RPC-LD interface implementation
│   ├── validation/                                   # SHACL validation engine
│   ├── inference/                                    # SHACL shape inference logic
│   ├── storage/                                      # GraphLite integration layer
│   └── main.rs                                       # Application entry point
├── schemas/                                          # SHACL shape definitions
├── tests/                                            # Test suites
├── docker/                                           # Docker configuration
│   └── Dockerfile
├── docs/                                             # Additional documentation
└── examples/                                         # Usage examples
```

## Key Components

### API Layer
- JSON-RPC 2.0 and JSON-RPC-LD request handling
- @context detection and processing
- Method routing and response formatting

### Validation Layer
- SHACL shape repository management
- Validation engine integration
- Validation report generation

### Inference Layer
- Shape discovery and matching
- New shape generation
- Similarity calculation and linking

### Storage Layer
- GraphLite database interface
- RDF transformation
- ISO GQL query execution
- ACID transaction management

## Documentation Files

- **Feature List**: Comprehensive specification of all database lifecycle operations
- **Research Notes**: Technical analysis of core technologies (GraphLite, SHACL, JSON-RPC-LD)
- **Steering Rules**: AI assistant guidance for project conventions and patterns
