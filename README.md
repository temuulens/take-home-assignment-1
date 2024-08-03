# User Registration System

This README provides an overview of the User Registration System, including its database schema, system architecture, and registration process flow.

## Table of Contents
1. [Database Schema](#database-schema)
2. [System Architecture](#system-architecture)
3. [User Registration Flow](#user-registration-flow)

## Database Schema

The database schema consists of the following tables:

- `Companies`: Stores information about companies.
- `Roles`: Defines various roles in the system.
- `Permissions`: Defines the types of permissions (View, Add, Edit, Delete).
- `Users`: Stores user information and their association with companies.
- `UserRoles`: Links users to their assigned roles.
- `UserPermissions`: Directly associates users with their permissions.

```sql
CREATE TABLE Companies (
    company_id INT PRIMARY KEY AUTO_INCREMENT,
    company_name VARCHAR(255) NOT NULL
);

CREATE TABLE Roles (
    role_id INT PRIMARY KEY AUTO_INCREMENT,
    role_name VARCHAR(100) NOT NULL
);

CREATE TABLE Permissions (
    permission_id INT PRIMARY KEY AUTO_INCREMENT,
    permission_name ENUM('View', 'Add', 'Edit', 'Delete') NOT NULL
);

CREATE TABLE Users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    company_id INT,
    FOREIGN KEY (company_id) REFERENCES Companies(company_id)
);

CREATE TABLE UserRoles (
    user_id INT,
    role_id INT,
    PRIMARY KEY (user_id, role_id),
    FOREIGN KEY (user_id) REFERENCES Users(user_id),
    FOREIGN KEY (role_id) REFERENCES Roles(role_id)
);

CREATE TABLE UserPermissions (
    user_id INT,
    permission_id INT,
    PRIMARY KEY (user_id, permission_id),
    FOREIGN KEY (user_id) REFERENCES Users(user_id),
    FOREIGN KEY (permission_id) REFERENCES Permissions(permission_id)
);
```

## System Architecture

The system follows a multi-layered architecture:

```mermaid 
graph TB
    subgraph Client["Client Layer"]
        A[Web Browser]
        B[Mobile App]
    end

    subgraph WebServer["Web Server Layer"]
        C[Web Server]
        D[Load Balancer]
    end

    subgraph ApplicationServer["Application Server Layer"]
        E[User Management Service]
        F[Authentication Service]
        G[Role Management Service]
        H[User Permission Management Service]
    end

    subgraph DataLayer["Data Layer"]
        I[(Database)]
        J[Cache]
    end

    A --> D
    B --> D
    D --> C
    C --> E
    C --> F
    C --> G
    C --> H
    E --> I
    F --> I
    G --> I
    H --> I
    E --> J
    F --> J
    G --> J
    H --> J

    style Client fill:#f9f,stroke:#333,stroke-width:2px
    style WebServer fill:#bbf,stroke:#333,stroke-width:2px
    style ApplicationServer fill:#bfb,stroke:#333,stroke-width:2px
    style DataLayer fill:#fbb,stroke:#333,stroke-width:2px
```

## User Registration Flow

```mermaid
graph TD
    A[Start] --> B[User Registration]
    B --> C{Company exists?}
    C -->|Yes| D[Assign to existing company]
    C -->|No| E[Create new company]
    E --> D
    D --> F[Select role]
    F --> G[Assign role to user]
    G --> H[Select highest-level permission for user]
    H --> I{Permission: Delete?}
    I -->|Yes| J[Grant Delete, Edit, Add, and View permissions]
    I -->|No| K{Permission: Edit?}
    K -->|Yes| L[Grant Edit, Add, and View permissions]
    K -->|No| M{Permission: Add?}
    M -->|Yes| N[Grant Add and View permissions]
    M -->|No| O{Permission: View?}
    O -->|Yes| P[Grant View permission]
    O -->|No| Q[No permissions granted]
    J --> R[Finalize user setup]
    L --> R
    N --> R
    P --> R
    Q --> R
    R --> S[End]
```

