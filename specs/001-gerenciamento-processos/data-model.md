# Data Model: Gestão de Processos Internos

## Overview

The model is centered on a process record that can be documented in multiple states and compared between the current state (AS-IS) and the target state (TO-BE). Each process is broken into stages, each with a responsible owner and associated input/output flows.

## Entities

### Process

Represents a business process that the organization wants to document and improve.

| Field | Type | Description | Rules |
|-------|------|-------------|-------|
| id | string/UUID | Unique process identifier | Required |
| name | string | Process name | Required, non-empty |
| description | text | Description of the purpose of the process | Required |
| objective | text | Goal or result delivered by the process | Required |
| category | string | Business domain or classification | Required |
| status | enum | Documentation, analysis, improvement, final | Required |
| createdAt | datetime | Creation timestamp | Required |
| updatedAt | datetime | Last modification timestamp | Required |

### ProcessVersion

Represents a documented state of the process, either AS-IS or TO-BE.

| Field | Type | Description | Rules |
|-------|------|-------------|-------|
| id | string/UUID | Unique version record | Required |
| processId | string | Parent process | Required |
| versionType | enum | AS_IS or TO_BE | Required |
| title | string | Version label | Required |
| summary | text | Overview of the version | Optional |
| createdAt | datetime | Snapshot creation date | Required |
| createdBy | string | User or actor who documented it | Optional |

### ProcessStage

Represents one step in the process flow.

| Field | Type | Description | Rules |
|-------|------|-------------|-------|
| id | string/UUID | Unique stage identifier | Required |
| processId | string | Parent process | Required |
| versionId | string | Parent version snapshot | Required |
| orderIndex | integer | Sequence order within the process | Required, positive integer |
| name | string | Stage name | Required |
| description | text | Stage purpose/behavior | Required |
| startCondition | text | Conditions for stage activation | Optional |
| endCondition | text | Condition that indicates completion | Optional |
| responsibleId | string | Individual or unit assigned to the stage | Required |

### Responsible

Represents the person or unit accountable for a stage or process.

| Field | Type | Description | Rules |
|-------|------|-------------|-------|
| id | string/UUID | Unique identifier | Required |
| name | string | Full name or team name | Required |
| role | string | Role or function | Required |
| area | string | Department or area | Optional |
| email | string | Contact reference | Optional |

### FlowItem

Represents an input or output associated with a process or stage.

| Field | Type | Description | Rules |
|-------|------|-------------|-------|
| id | string/UUID | Unique item identifier | Required |
| processId | string | Parent process | Required |
| versionId | string | Parent version snapshot | Required |
| stageId | string | Related stage, if applicable | Optional |
| direction | enum | IN or OUT | Required |
| itemType | string | Document, material, data, service | Required |
| description | text | What enters or leaves the process | Required |
| source | string | Origin or destination | Optional |

### Observation

Captures commentary related to process improvement or version comparison.

| Field | Type | Description | Rules |
|-------|------|-------------|-------|
| id | string/UUID | Unique observation identifier | Required |
| processId | string | Parent process | Required |
| versionId | string | Related version | Required |
| text | text | Commentary or justification | Required |
| createdAt | datetime | Observation timestamp | Required |

## Relationships

- One process has many ProcessVersion records.
- One process has many ProcessStage records, each associated with a version snapshot.
- One responsible can be assigned to many stages.
- One process can have many FlowItem records across versions.
- One process can have many Observation records that explain AS-IS or TO-BE changes.

## Validation Rules

- A process must have a unique name within the organization context.
- Stage order must be unique within a version of a process.
- A stage must always have a responsible assignment.
- Flow items must specify direction and description.
- AS-IS and TO-BE versions must be distinguishable and linked to the same process.
- The system must prevent losing traceability between process, stages, and version records.

## State Transitions

- Draft -> In Review -> Approved -> Final
- A process may be edited while in documentation or analysis status.
- A TO-BE version may be created from an existing AS-IS version even when the final state is not yet approved.
