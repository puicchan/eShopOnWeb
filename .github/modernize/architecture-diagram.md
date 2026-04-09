# Architecture Diagram

eShopOnWeb is an ASP.NET Core reference application implementing a layered architecture with a web storefront, a Blazor-based admin panel, and a REST API, all backed by SQL Server via Entity Framework Core.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser / Client"]

    subgraph Presentation["Presentation Layer"]
        Web["Web\nASP.NET Core MVC\nRazor Pages, MediatR, AutoMapper\nJWT Bearer, Azure.Identity"]
        BlazorAdmin["BlazorAdmin\nBlazor WebAssembly\nAdmin UI"]
        PublicApi["PublicApi\nASP.NET Core Web API\nSwagger/OpenAPI, JWT Bearer\nAutoMapper, MinimalApi.Endpoint"]
    end

    subgraph Shared["Shared Models"]
        BlazorShared["BlazorShared\nShared DTOs and Models"]
    end

    subgraph BusinessLogic["Business Logic Layer"]
        AppCore["ApplicationCore\nDomain Entities, Services\nArdalis Specifications\nArdalis Result, MediatR"]
    end

    subgraph DataAccess["Data Access Layer"]
        Infra["Infrastructure\nEntity Framework Core\nArdalis Specification EF Core\nASP.NET Core Identity"]
    end

    subgraph DataStorage["Data Storage"]
        CatalogDb[("SQL Server\nCatalogDb")]
        IdentityDb[("SQL Server\nIdentityDb")]
    end

    subgraph ExternalServices["External Services"]
        AzureKeyVault["Azure Key Vault\nSecrets Configuration"]
        AzureIdentity["Azure Identity\nManaged Identity / MSAL"]
    end

    Browser -->|HTTP/HTTPS| Web
    Browser -->|HTTP/HTTPS| PublicApi
    Web --> BlazorAdmin
    BlazorAdmin -->|REST calls| PublicApi
    Web --> AppCore
    Web --> Infra
    PublicApi --> AppCore
    PublicApi --> Infra
    AppCore --> BlazorShared
    Infra --> AppCore
    Infra -->|EF Core| CatalogDb
    Infra -->|EF Core Identity| IdentityDb
    Web -->|Azure SDK| AzureKeyVault
    Web -->|Azure SDK| AzureIdentity
```
