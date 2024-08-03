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

## System Architecture

The system follows a multi-layered architecture:

1. **Client Layer**
   - Web Browser
   - Mobile App

2. **Web Server Layer**
   - Load Balancer
   - Web Server

3. **Application Server Layer**
   - User Management Service
   - Authentication Service
   - Role Management Service
   - User Permission Management Service

4. **Data Layer**
   - Database
   - Cache

## User Registration Flow

