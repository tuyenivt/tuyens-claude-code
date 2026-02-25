---
name: task-implement-feature
description: End-to-end Spring Boot feature implementation workflow. Generates entity, repository, service, controller, DTO records, Flyway migration, and tests (unit + integration). Orchestrates multiple atomic skills into a complete, production-ready feature.
metadata:
  category: backend
  tags: [spring-boot, java, feature, implementation, workflow, jpa, rest-api, testing]
  type: workflow
agent: spring-architect
---

# Implement Feature

## When to Use

- Implementing a new Spring Boot feature end-to-end (entity → controller → tests → migration)
- Scaffolding a complete CRUD or domain-specific resource with production-ready patterns
- Adding a new domain aggregate with REST API, persistence, and test coverage
- Any daily coding task that requires coordinated generation of multiple Spring Boot layers

## Rules

- Constructor injection only — use `@RequiredArgsConstructor` (Lombok); never `@Autowired` fields
- Records for all DTOs (Java 21+); classes for JPA entities
- `@Transactional(readOnly = true)` as default on service classes
- Never expose JPA entities in API responses — always map to DTO records
- No `synchronized` blocks — breaks Virtual Threads; use `ReentrantLock` if needed
- Connection pool sizing: 10–40 (optimized for Virtual Threads)
- Use `var` for local variables when type is obvious
- Use `@MockitoBean` not `@MockBean` (deprecated since Spring Boot 3.4.0)
- Jakarta EE 10 for Spring Boot 3.x; Jakarta EE 11 for Spring Boot 4
- Each step must complete and be reviewed before proceeding to the next
- Present the design to the user for approval before generating code
- Run compilation check after all files are generated

## Implementation

### Step 1 — Gather Requirements

**Interactive.** Ask the user for:

- **Feature name / resource** — e.g., "Order", "Payment", "Notification"
- **Package base** — e.g., `com.example.order`
- **Operations needed** — CRUD, specific operations (approve, cancel), event-driven
- **Relationships to existing entities** — OneToMany, ManyToOne, ManyToMany, or none
- **Business rules / validation constraints** — e.g., "amount must be positive", "status transitions"
- **API visibility** — public, internal, or admin-only (affects security annotations)

Do not proceed until all required inputs are confirmed.

### Step 2 — Design

Use skill: `api-guidelines` — validate REST endpoint design, URI conventions, HTTP methods
Use skill: `coding-standards` — enforce project conventions (naming, structure, style)
Use skill: `architecture-guardrail` — verify layer separation and dependency direction

Propose to the user:

- **Endpoints** — HTTP method, URI, request/response DTOs, status codes
- **DTOs** — record definitions with validation annotations
- **Entity fields** — column types, constraints, relationships
- **Service methods** — business operations, transaction boundaries

**Present the design and wait for user approval before proceeding.**

### Step 3 — Generate Entity + Migration

Create the JPA entity class and corresponding Flyway migration.

Use skill: `jpa-performance` — N+1 prevention, fetch strategy (`LAZY` by default)
Use skill: `db-migration-safety` — safe DDL patterns, zero-downtime migrations

Generate:

- **Entity** — `src/main/java/.../entity/{Name}.java`
  - `@Entity`, `@Table`, proper column annotations
  - Audit fields (`createdAt`, `updatedAt`) with `@CreationTimestamp`, `@UpdateTimestamp`
  - Relationships with correct cascade and fetch settings
- **Migration** — `src/main/resources/db/migration/V{timestamp}__create_{table_name}.sql`
  - If relationships exist, generate join table migrations separately
  - Include indexes for foreign keys and frequently queried columns

### Step 4 — Generate Repository

Create the Spring Data JPA repository interface.

Use skill: `jpa-performance` — optimal query patterns, projection usage

Generate:

- **Repository** — `src/main/java/.../repository/{Name}Repository.java`
  - Extend `JpaRepository<{Name}, Long>`
  - Custom query methods using `@Query` with JPQL (not native SQL) where possible
  - `Specification`-based dynamic queries if filtering is needed
  - Pagination support via `Page<>` return types

### Step 5 — Generate Service

Create the service class with business logic.

Use skill: `transaction` — `@Transactional` boundaries, read-only defaults
Use skill: `exception-handling` — business exception hierarchy, consistent error responses

Generate:

- **Service** — `src/main/java/.../service/{Name}Service.java`
  - `@Service`, `@Transactional(readOnly = true)`, `@RequiredArgsConstructor`, `@Slf4j`
  - `@Transactional` (read-write) only on mutating methods
  - Entity ↔ DTO mapping methods (private, in-class or via dedicated mapper)
  - Bean Validation on DTO inputs via `@Valid` at controller level
  - Virtual Thread-safe: no `synchronized`, no thread-local state
  - Business exceptions extending a common base (e.g., `ResourceNotFoundException`)

### Step 6 — Generate Controller

Create the REST controller with proper HTTP semantics.

Use skill: `api-guidelines` — consistent response format, URI conventions
Use skill: `exception-handling` — `@RestControllerAdvice` mapping

Generate:

- **Controller** — `src/main/java/.../controller/{Name}Controller.java`
  - `@RestController`, `@RequestMapping("/api/v1/{resources}")`, `@RequiredArgsConstructor`, `@Slf4j`
  - `@ResponseStatus` instead of `ResponseEntity` when a single status code suffices
  - `@Valid @RequestBody` on create/update endpoints
  - `Pageable` parameter for list endpoints
  - Proper HTTP status codes: `201 CREATED` for POST, `204 NO_CONTENT` for DELETE
- **Request DTO** — `src/main/java/.../dto/{Name}Request.java` (record with validation)
- **Response DTO** — `src/main/java/.../dto/{Name}Response.java` (record)

### Step 7 — Generate Tests

Use skill: `test-spring-integration` — select correct test slice for each layer

Generate:

- **Unit test** — `src/test/java/.../service/{Name}ServiceTest.java`
  - `@ExtendWith(MockitoExtension.class)`
  - Mock repository with `@Mock`, inject service with `@InjectMocks`
  - Test happy path, not-found, validation, and business rule scenarios
  - Use `@MockitoBean` not `@MockBean`
- **Integration test** — `src/test/java/.../repository/{Name}RepositoryTest.java`
  - `@DataJpaTest` with Testcontainers for real database
  - Test custom queries, pagination, relationship loading
- **API test** — `src/test/java/.../controller/{Name}ControllerTest.java`
  - `@WebMvcTest({Name}Controller.class)`
  - MockMvc with `@MockitoBean` for service
  - Test request validation, response format, HTTP status codes, error responses
- **Test fixtures** — builder pattern or static factory methods for test data

### Step 8 — Validate

Run compilation and final checks.

```bash
./gradlew compileJava compileTestJava
```

Use skill: `architecture-guardrail` — final layer dependency check
Use skill: `coding-standards` — final style and convention check

Present summary to user:

- Files created (with paths)
- Endpoints available (method + URI)
- Test count and coverage areas
- Any warnings or manual steps required

## Output

Present a checklist of generated files:

```markdown
## Generated Files

- [ ] Entity: `src/main/java/.../entity/{Name}.java`
- [ ] DTO: `src/main/java/.../dto/{Name}Request.java`
- [ ] DTO: `src/main/java/.../dto/{Name}Response.java`
- [ ] Repository: `src/main/java/.../repository/{Name}Repository.java`
- [ ] Service: `src/main/java/.../service/{Name}Service.java`
- [ ] Controller: `src/main/java/.../controller/{Name}Controller.java`
- [ ] Migration: `src/main/resources/db/migration/V{timestamp}__create_{table}.sql`
- [ ] Unit test: `src/test/java/.../service/{Name}ServiceTest.java`
- [ ] Integration test: `src/test/java/.../repository/{Name}RepositoryTest.java`
- [ ] API test: `src/test/java/.../controller/{Name}ControllerTest.java`

## Endpoints

| Method | URI                      | Status | Description      |
| ------ | ------------------------ | ------ | ---------------- |
| GET    | /api/v1/{resources}      | 200    | List (paginated) |
| GET    | /api/v1/{resources}/{id} | 200    | Get by ID        |
| POST   | /api/v1/{resources}      | 201    | Create           |
| PUT    | /api/v1/{resources}/{id} | 200    | Update           |
| DELETE | /api/v1/{resources}/{id} | 204    | Delete           |

## Tests

- Unit tests: {count} (service layer)
- Integration tests: {count} (repository layer)
- API tests: {count} (controller layer)
```

## Key Skills Reference

**Design and conventions:**

- Use skill: `api-guidelines` for REST endpoint design and URI conventions
- Use skill: `coding-standards` for project-wide style and naming conventions
- Use skill: `architecture-guardrail` for layer separation and dependency direction

**Data layer:**

- Use skill: `jpa-performance` for N+1 prevention, fetch strategy, query optimization
- Use skill: `db-migration-safety` for safe Flyway migrations and zero-downtime DDL
- Use skill: `transaction` for `@Transactional` boundaries and isolation levels

**Business logic:**

- Use skill: `exception-handling` for business exception hierarchy and `@RestControllerAdvice`
- Use skill: `concurrency-model` for Virtual Thread-safe patterns

**Testing:**

- Use skill: `test-spring-integration` for test slice selection and integration test patterns

## Checklist

- [ ] Requirements gathered and confirmed with user
- [ ] Design proposed and approved by user
- [ ] Entity created with proper JPA annotations and audit fields
- [ ] Flyway migration generated (with indexes)
- [ ] Repository with custom queries (JPQL, not native)
- [ ] Service with `@Transactional(readOnly = true)` default, read-write on mutating methods
- [ ] Constructor injection only (`@RequiredArgsConstructor`)
- [ ] DTOs as Java records with Bean Validation annotations
- [ ] Controller with proper HTTP methods, status codes, and `@Valid`
- [ ] Pagination for list endpoints
- [ ] Entity never exposed in API responses
- [ ] No `synchronized` blocks (Virtual Thread compatible)
- [ ] Unit tests with Mockito (`@MockitoBean`, not `@MockBean`)
- [ ] Integration tests with `@DataJpaTest` + Testcontainers
- [ ] API tests with `@WebMvcTest` + MockMvc
- [ ] Compilation verified (`compileJava compileTestJava`)
- [ ] Summary presented to user

## Avoid

- Exposing JPA entities in API responses
- `@Autowired` field injection
- `synchronized` blocks (breaks Virtual Threads)
- `@MockBean` (deprecated since Spring Boot 3.4.0)
- Native SQL in `@Query` when JPQL suffices
- Generating code before user approves the design
- Skipping test generation
- Over-engineering: only generate what was requested
