# Tasks: Gestão de Processos Internos

**Input**: Design documents from `/specs/001-gerenciamento-processos/`

**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and baseline structure for the internal process management system

- [ ] T001 Create backend and frontend project structure per implementation plan in backend/src/, backend/tests/, frontend/src/, and frontend/tests/
- [ ] T002 [P] Initialize dependency manifests and base configs in backend/package.json, frontend/package.json, backend/.env.example, and frontend/.env.example
- [ ] T003 [P] Configure linting and formatting rules in backend/eslint.config.js, frontend/eslint.config.js, and shared formatting configuration files

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Shared infrastructure that must be in place before any user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [ ] T004 Create the relational database schema and migration foundation in backend/migrations/001_create_process_tables.sql and backend/src/db/schema.ts
- [ ] T005 [P] Implement shared validation helpers in backend/src/validation/process_validation.ts for required fields, unique names, and stage ordering rules
- [ ] T006 [P] Create shared domain models for Process, ProcessVersion, ProcessStage, Responsible, FlowItem, and Observation in backend/src/models/
- [ ] T007 Create API routing and middleware structure in backend/src/api/index.ts and backend/src/api/process_routes.ts
- [ ] T008 Configure environment config and error handling in backend/src/config/config.ts and backend/src/api/error_handler.ts

**Checkpoint**: Foundation ready - user story implementation can now begin in parallel

---

## Phase 3: User Story 1 - Cadastrar e manter processos organizacionais (Priority: P1) 🎯 MVP

**Goal**: Allow the organization to register a process, describe its stages, assign owners, and capture inputs and outputs

**Independent Test**: Create a process with three stages, assign responsible owners, add input and output flows, and confirm the record can be saved and retrieved.

### Implementation for User Story 1

- [ ] T009 [P] [US1] Create Process model in backend/src/models/process.ts with fields name, description, objective, category, status, createdAt, and updatedAt
- [ ] T010 [P] [US1] Create ProcessStage, Responsible, and FlowItem models in backend/src/models/process_stage.ts, backend/src/models/responsible.ts, and backend/src/models/flow_item.ts
- [ ] T011 [US1] Implement process creation, update, and retrieval logic in backend/src/services/process_service.ts
- [ ] T012 [US1] Implement validation for required process, stage, responsible, and flow fields in backend/src/services/process_service.ts and backend/src/validation/process_validation.ts
- [ ] T013 [US1] Implement process CRUD endpoints in backend/src/api/process_routes.ts for create, read, and update operations
- [ ] T014 [US1] Build the process list and process form screens in frontend/src/features/processes/ProcessListPage.tsx and frontend/src/features/processes/ProcessForm.tsx
- [ ] T015 [US1] Build the stage details panel and responsible assignment UI in frontend/src/features/processes/StageEditor.tsx
- [ ] T016 [US1] Add process search and listing filters in frontend/src/features/processes/ProcessListPage.tsx and backend/src/services/process_query_service.ts

**Checkpoint**: At this point, User Story 1 should be fully functional and testable independently

---

## Phase 4: User Story 2 - Documentar o processo atual e o processo futuro (Priority: P1)

**Goal**: Support AS-IS and TO-BE documentation with clear separation and traceability between the current and future state of each process

**Independent Test**: Save a process as AS-IS, create a TO-BE version with changes in steps or responsibilities, and confirm both versions remain linked to the same process.

### Implementation for User Story 2

- [ ] T017 [P] [US2] Create ProcessVersion and Observation models in backend/src/models/process_version.ts and backend/src/models/observation.ts
- [ ] T018 [US2] Implement version creation and persistence logic in backend/src/services/version_service.ts for AS_IS and TO_BE snapshots
- [ ] T019 [US2] Implement comparison logic in backend/src/services/comparison_service.ts to detect differences in stage order, owners, inputs, and outputs
- [ ] T020 [US2] Add endpoints for version creation and comparison in backend/src/api/process_routes.ts and backend/src/api/comparison_routes.ts
- [ ] T021 [US2] Build the AS-IS/TO-BE comparison UI in frontend/src/features/processes/ProcessComparisonView.tsx
- [ ] T022 [US2] Add observation and improvement notes entry in frontend/src/features/processes/ProcessComparisonView.tsx and backend/src/models/observation.ts

**Checkpoint**: At this point, User Story 2 should be independently functional and comparable by process version

---

## Phase 5: User Story 3 - Monitorar e analisar melhorias de processo (Priority: P2)

**Goal**: Provide process visibility, search, and analysis support for managers and improvement teams

**Independent Test**: Query processes by category, responsible, or status, open the detail view, and confirm that stage sequence, owners, and flow items are exposed clearly.

### Implementation for User Story 3

- [ ] T023 [P] [US3] Implement dashboard and query service for process listing and filtering in backend/src/services/process_query_service.ts
- [ ] T024 [US3] Implement process detail retrieval with related stages, version snapshots, and flow items in backend/src/services/process_detail_service.ts
- [ ] T025 [US3] Build a process detail screen in frontend/src/features/processes/ProcessDetailPage.tsx showing status, stages, owner assignments, and flow records
- [ ] T026 [US3] Add search/filter controls in frontend/src/features/processes/ProcessDashboard.tsx for name, category, responsible, and status
- [ ] T027 [US3] Add status indicators and process lifecycle states in frontend/src/features/processes/ProcessStatusBadge.tsx and backend/src/models/process.ts

**Checkpoint**: At this point, User Story 3 should deliver monitoring and analysis value independently of the earlier stories

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Final consistency, documentation, and validation improvements across all stories

- [ ] T028 [P] Update project documentation and usage notes in docs/process-management.md and specs/001-gerenciamento-processos/quickstart.md
- [ ] T029 [P] Review authorization and access assumptions against the MVP scope in backend/src/api/auth_guard.ts and backend/src/config/access_policy.ts
- [ ] T030 [P] Run the end-to-end validation flow defined in specs/001-gerenciamento-processos/quickstart.md and confirm AS-IS/TO-BE comparison works as expected
- [ ] T031 Perform code cleanup, data consistency checks, and final traceability review across backend/src/ and frontend/src/

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3-5)**: Each depends on foundational completion; stories can proceed in parallel after the foundation is ready
- **Polish (Phase 6)**: Depends on all desired user stories being complete

### User Story Dependencies

- **US1 (P1)**: Can start after Foundational completion; no dependency on other stories
- **US2 (P1)**: Can start after Foundational completion; depends on the same core process model as US1
- **US3 (P2)**: Can start after Foundational completion and benefit from US1 and US2 data model completion

### Parallel Opportunities

- All Setup tasks are parallelizable
- Shared validation and model creation tasks in Phase 2 are parallelizable
- US1 phases can be implemented in parallel across backend and frontend streams after foundation is ready
- Versioning and comparison work in US2 can proceed alongside process list/detail work in US3 if the data model is stable

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational
3. Complete Phase 3: User Story 1
4. Validate the core process registration flow
5. Expand to AS-IS/TO-BE comparison and process analysis only after the process core is stable

### Incremental Delivery

- Deliver process registration and documentation first
- Add AS-IS/TO-BE versioning next
- Add monitoring and comparison analysis last

This sequence reduces the risk of building complex comparison logic before the core process model is stable.
