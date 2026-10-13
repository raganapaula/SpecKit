# Quickstart: Validação do Sistema de Gestão de Processos

## Objective

Validate that the project can document a process, structure its stages, assign responsibilities, register inputs/outputs, and compare AS-IS and TO-BE versions.

## Prerequisites

- Access to the application in an internal organizational environment
- At least one authenticated user with permission to create and edit process records
- A database or persistence layer configured for process records and version snapshots

## Validation Flow

1. Open the process management module.
2. Create a process with the following information:
   - name
   - description
   - objective
   - category
   - status
3. Add a minimum of three stages in sequence.
4. Assign a responsible person or unit to each stage.
5. Register input and output flow items for the process and for at least one stage.
6. Save the current version as AS-IS.
7. Create a TO-BE version with proposed changes to stage order, responsibilities, or flows.
8. Compare AS-IS and TO-BE.
9. Record a textual observation explaining the improvement rationale.

## Expected Outcomes

- The process is stored with all required fields.
- The stage sequence is visible and ordered correctly.
- Each stage has a responsible assignment.
- Inputs and outputs are traceable to the defined process or stage.
- AS-IS and TO-BE versions can be compared side by side.
- The justification for improvement is visible in the observation or version notes.

## Success Criteria for the Validation

- A user can complete the process documentation flow without external assistance.
- Comparison between AS-IS and TO-BE clearly highlights differences in flow, responsibility, and process description.
- The system preserves the traceability between process, stages, and version records.
