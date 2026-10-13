# Research: Gestão de Processos Internos

## Decision 1: Focus on a documentation-first internal platform

**Decision**: Build the MVP as a documentation and analysis application for internal process management, not as a workflow-automation engine.

**Rationale**: The feature description emphasizes registration, description of stages, responsibilities, inputs/outputs, and AS-IS/TO-BE comparison. This is fundamentally a knowledge and analysis tool, and introducing workflow automation would exceed the stated scope and complexity budget.

**Alternatives considered**:
- Full BPM workflow automation engine: rejected because it requires process orchestration, approvals, and runtime execution logic beyond the requested MVP.
- Spreadsheet-based management: rejected because it lacks structured traceability, versioning, and comparison features required by the requirements.

## Decision 2: Use a relational model and versioned process snapshots

**Decision**: Store core entities in a relational model and represent AS-IS and TO-BE states as versioned process snapshots tied to the same process record.

**Rationale**: The system needs to compare current and future states while preserving the original data model and traceability to responsibilities and flows. A versioned snapshot pattern supports this without overcomplicating the architecture.

**Alternatives considered**:
- Single flat record with repeated fields for AS-IS and TO-BE: rejected because it weakens traceability and increases the chance of inconsistent data.
- Free-form documents only: rejected because the requirements need structured data, search, and comparison.

## Decision 3: Keep access simple in the MVP

**Decision**: the MVP will use shared access for authorized organization users without area-based segregation.

**Rationale**: The clarification confirmed that the priority is collaborative documentation and improvement, not role-based restriction by department. This keeps the first release simpler and easier to adopt.

**Alternatives considered**:
- Granular role management by area: postponed for a later iteration because it adds administrative complexity not required to achieve the primary user value.
- No user access layer: rejected because the organization needs controlled sharing of process documentation.

## Decision 4: Comparison should be structured and stage-based

**Decision**: Comparison between AS-IS and TO-BE will be built around differences in stages, responsibilities, entries, exits, and textual observations rather than complex analytics.

**Rationale**: The clarified requirement prefers a practical comparison model that supports analysis and improvement without creating a heavy reporting layer.

**Alternatives considered**:
- Advanced dashboard analytics: deferred because the current need is process comparison and documentation clarity rather than predictive insight.
- Pure text diff only: rejected because the requirements require identifying differences in structured process elements.

## Decision 5: Model validation before persistence

**Decision**: All process, stage, responsible, and flow records must validate required fields before saving.

**Rationale**: This reduces inconsistent and incomplete process documentation and aligns with the constitution’s emphasis on clarity and auditability.

**Alternatives considered**:
- Save without validation and clean up later: rejected because it would create poor-quality records and reduce trust in the system.
- Validation only on the UI side: rejected because domain validation must exist at the service or persistence layer as well.
