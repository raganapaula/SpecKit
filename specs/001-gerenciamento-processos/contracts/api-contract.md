# Process Management API Contract

## Overview

This contract defines the core API expectations for an internal process management application. The focus is on process documentation and comparison between AS-IS and TO-BE states.

## Resources

### Process

#### GET /processes
Returns all visible processes.

Response:
- list of process records
- each record contains id, name, description, objective, category, status, createdAt, updatedAt

#### POST /processes
Creates a new process.

Request body:
- name
- description
- objective
- category
- status

Validation:
- required fields cannot be empty
- name must be unique within the organization context

#### GET /processes/{id}
Returns the full process with related stages, version snapshots, and flow items.

#### PATCH /processes/{id}
Updates editable process metadata.

### Version snapshots

#### POST /processes/{id}/versions
Creates a new AS-IS or TO-BE version.

Request body:
- versionType
- title
- summary

### Stage management

#### POST /processes/{id}/stages
Adds a stage to a process version.

Request body:
- versionId
- orderIndex
- name
- description
- responsibleId
- startCondition
- endCondition

### Flow management

#### POST /processes/{id}/flows
Adds an input or output item to a process or stage.

Request body:
- versionId
- stageId (optional)
- direction
- itemType
- description
- source

### Comparison

#### GET /processes/{id}/compare
Returns the structural differences between AS-IS and TO-BE versions.

Response fields:
- processId
- asIsVersionId
- toBeVersionId
- differencesByStage
- differencesByResponsible
- differencesByFlow
- observations

## Notes

- The MVP is documentation-first and does not require orchestration automation.
- Comparison is built around structured differences, not advanced predictive metrics.
- Authentication and authorization should be enforced at the service layer even if access rules remain simple for the first release.
