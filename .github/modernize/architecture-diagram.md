# Architecture Diagram

Spring PetClinic is a layered Spring Boot web application managing pet clinic operations including owners, pets, veterinarians, and visit scheduling.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser\nHTTP Client"]

    subgraph App["Spring Boot Application (Java 17)"]
        subgraph Web["Presentation Layer\nSpring Web MVC + Thymeleaf"]
            WC["WelcomeController\nHome Page"]
            OC["OwnerController\nOwner Search and CRUD"]
            PC["PetController\nPet Management"]
            VC["VisitController\nVisit Scheduling"]
            VetC["VetController\nVeterinarian List"]
            CC["CrashController\nError Handling"]
        end

        subgraph Domain["Domain Layer\nJPA Entities and Repositories"]
            OwnerRepo["OwnerRepository\nSpring Data JPA"]
            VetRepo["VetRepository\nSpring Data JPA"]
            PetTypeRepo["PetTypeRepository\nSpring Data JPA"]
        end

        subgraph Infra["Infrastructure Layer"]
            Cache["Caffeine Cache\nJCache API - vets list"]
            Actuator["Spring Boot Actuator\nHealth and Metrics"]
            Validation["Bean Validation\nJakarta Validation"]
        end
    end

    subgraph DataStores["Data Storage"]
        H2["H2 Database\nEmbedded - default profile"]
        MySQL["MySQL\nmysql profile"]
        Postgres["PostgreSQL\npostgres profile"]
    end

    Browser -->|"HTTP requests"| Web
    Web -->|"queries and updates"| Domain
    Domain -->|"JPA / Hibernate ORM"| DataStores
    Domain -->|"cached reads"| Cache
    Actuator -->|"exposes"| Browser
    Validation --> Web
```
