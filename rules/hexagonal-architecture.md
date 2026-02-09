# Hexagonal Architecture (Ports and Adapters)

> **Intent**
> Create a system architecture that **isolates the domain core** from external concerns.
> Make the application **equally driven by users, programs, automated tests, or batch scripts**.
> Decouple the application from external services through **ports and adapters**.
> Enable **technology substitution** without affecting the core business logic.

---

## 1 Core Concepts

| Concept | Description |
|---------|-------------|
| **Domain/Application Core** | Central hexagon containing pure business logic, free from external dependencies. |
| **Ports** | Interfaces or APIs that define how the application core interacts with the outside world. |
| **Adapters** | Implementations that connect external systems to the application's ports. |
| **Inside/Outside Boundary** | Clear separation between domain logic and external concerns. |
| **Technology Independence** | Core business logic remains unaffected by UI, database, or external service changes. |
| **Dependency Rule** | Dependencies always point inward toward the domain core. |

---

## 2 Port Types

### 2.1 Primary/Driving Ports

* Interfaces that the application **exposes** to the outside world
* Define how external actors can use the application
* Implemented by the domain/application core
* Examples: service interfaces, REST API contracts, use cases

```c#
// Primary/Driving Port
interface IVersionService {
    Version FindById(string id):
    List<Version> FindAllByCandidate(string candidate)
    Version Register(VersionRequest version)
}
```

### 2.2 Secondary/Driven Ports

* Interfaces that the application **requires** from the outside world
* Define how the application uses external resources
* Implemented by adapters in the infrastructure layer
* Examples: repository interfaces, messaging interfaces, email sender interfaces

```c#
// Secondary/Driven Port
interface IVersionRepository {
    Version FindById(string id):
    List<Version> FindAllByCandidate(string candidate)
    Version Save(VersionRequest version)
}
```

---

## 3 Adapter Types

### 3.1 Primary/Driving Adapters

* Connect external actors to the application's primary ports
* Transform external requests into calls to the application core
* Examples: REST controllers, CLI applications, message consumers

```kotlin
// Primary/Driving Adapter
class VersionController{
    VersionService _versionService;
    public VersionController(VersionService versionService){
        _versionService = versionService;
    }
    HttpResponse getVersion(string id) => _versionService.findById(id);

    // Other controller methods
}
```

### 3.2 Secondary/Driven Adapters

* Implement the application's secondary ports
* Connect the application to external systems/infrastructure
* Examples: database repositories, HTTP clients, message publishers

```kotlin
// Secondary/Driven Adapter
class MongoVersionRepository(MongoDatabase database) : VersionRepository {
    MongoDatabase _database;
    // constructor omitted
    public override Version FindById(string id) {
        try {
            _database.Collection("versions")
                .Find(eq("_id", id))
                .FirstOrNull()
                .toVersion();
        } catch (Exception ex) {
            throw new RepositoryError(ex);
        }
    }

    // Other repository methods
}
```

---

## 4 Package Structure

### 4.1 Layered By Concept

// skip
---

## 5 Implementation Guidelines

### 5.1 Domain/Application Core

* Contains pure business logic with no external dependencies
* Uses domain entities and value objects
* Implements primary ports (service interfaces)
* Uses secondary ports (repository interfaces)
* Has no knowledge of specific technologies (HTTP, databases, etc.)

```kotlin
// Application Service implementing a primary port
class VersionServiceImpl(private val versionRepository: VersionRepository) : VersionService {
    override fun findById(id: String): Either<DomainError, Version> =
        versionRepository.findById(id)
            .mapLeft { it.toDomainError() }

    override fun register(request: VersionRequest): Either<DomainError, Version> =
        validateVersion(request)
            .flatMap { version -> versionRepository.save(version) }
            .mapLeft { it.toDomainError() }

    // Private methods for validation, etc.
}
```

### 5.2 Clean Dependencies

* Dependencies always point inward
* Domain core has no outward dependencies
* Adapters depend on ports, not directly on domain classes
* Use dependency injection to wire everything together

```kotlin
// Configuration showing dependency direction
val versionModule = module {
    // Domain/Application (core)
    single<VersionService> { VersionServiceImpl(get()) }

    // Secondary/Driven Adapters (infrastructure)
    single<VersionRepository> { MongoVersionRepository(get()) }

    // Primary/Driving Adapters (input)
    single { VersionController(get()) }
}
```

### 5.3 Error Handling

* Domain errors are defined in the domain layer
* Infrastructure errors are mapped to domain errors at the adapter boundary
* Use Either or similar constructs to make errors explicit

```kotlin
// Secondary port defines generic repository errors
sealed class RepositoryError {
    data class NotFound(val id: String) : RepositoryError()
    data class DatabaseError(val cause: Throwable) : RepositoryError()
}

// Domain errors are defined in the domain layer
sealed class DomainError {
    data class EntityNotFound(val id: String) : DomainError()
    data class ValidationError(val message: String) : DomainError()
    data class SystemError(val message: String) : DomainError()
}

// Extension function to map repository errors to domain errors
fun RepositoryError.toDomainError(): DomainError = when (this) {
    is RepositoryError.NotFound -> DomainError.EntityNotFound(id)
    is RepositoryError.DatabaseError -> DomainError.SystemError(cause.message ?: "Unknown error")
}
```

---

## 6 Testing Strategies

### 6.1 Domain/Application Testing

* Unit tests focused on business logic
* No mocking frameworks required for domain logic
* Mock/stub secondary ports (repositories) for application services

```kotlin
class VersionServiceSpec : ShouldSpec({
    val mockRepo = mockk<VersionRepository>()
    val service = VersionServiceImpl(mockRepo)

    should("return version when found") {
        // given
        val version = Version("gradle", "7.2")
        every { mockRepo.findById("gradle-7.2") } returns version.right()

        // when
        val result = service.findById("gradle-7.2")

        // then
        result shouldBeRight version
    }
})
```

### 6.2 Adapter Testing

* Test adapters in isolation
* Primary adapters: test conversion from external format to domain calls
* Secondary adapters: test against real external systems (databases, APIs)

```kotlin
class MongoVersionRepositorySpec : ShouldSpec({
    listener(MongoContainerListener)
    val mongo = MongoClient(MongoContainerListener.connectionString)
    val repo = MongoVersionRepository(mongo.getDatabase("test"))

    beforeTest {
        mongo.getDatabase("test").drop()
    }

    should("save and retrieve version") {
        // given
        val version = Version("gradle", "7.2")

        // when
        val saveResult = repo.save(version)
        val findResult = repo.findById("gradle-7.2")

        // then
        saveResult shouldBeRight version
        findResult shouldBeRight version
    }
})
```

### 6.3 End-to-End Testing

* Exercise the full hexagon through primary adapters
* Use real or test doubles for secondary adapters
* Focus on testing complete use cases

---

## 7 Common Anti-Patterns

### 7.1 Leaky Abstractions

| Anti-Pattern | Better Approach |
|--------------|-----------------|
| Exposing infrastructure details in the domain | Define domain-specific interfaces and convert at boundaries |
| Domain entities with ORM annotations | Keep entities clean; use mapping layers in adapters |
| Domain services with HTTP knowledge | Domain services should be completely agnostic of delivery mechanism |

### 7.2 Dependency Violations

| Anti-Pattern | Better Approach |
|--------------|-----------------|
| Domain importing infrastructure packages | Domain should have zero external dependencies |
| Circular dependencies between layers | Maintain strict layering with inward dependencies only |
| Concrete adapter implementations in core | Reference only port interfaces in the domain/application core |

---

## 8 Real-World Considerations

### 8.1 Balancing Purity and Pragmatism

* Be pragmatic about the depth of abstraction
* Not every external call needs its own port and adapter
* Focus on protecting the domain from external concerns
* Consider future change likelihood when deciding on abstractions

### 8.2 Migration Strategy

* Start by identifying the domain core
* Extract interfaces for existing infrastructure dependencies
* Gradually refactor toward the hexagonal model
* Apply more strictly to new features

### 8.3 Performance Considerations

* Avoid excessive mapping between layers
* Use efficient data structures at boundaries
* Consider bulk operations in repository interfaces

---

## 9 Implementation Considerations

* Focus on the principles of isolation and dependency direction
* Apply ports and adapters regardless of programming language
* Balance theoretical purity with practical needs of your specific system
* Consider testability when designing your ports and adapters

---

### TL;DR

1. **Isolate domain logic** in a central hexagon with no external dependencies.
2. Define **ports** (interfaces) for all interactions with the outside world.
3. Implement **adapters** that connect external systems to your ports.
4. Ensure all dependencies point **inward** toward the domain.
5. **Primary adapters** drive your application; **secondary adapters** are driven by it.
6. Map between **domain-specific** and **external** data models at the boundaries.
7. Test each component in **isolation** using the appropriate strategy.

