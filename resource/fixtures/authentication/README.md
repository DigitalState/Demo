# Authentication Microservice

## Fixtures File Structure

```
.
├── access                           The access folder contains all access and permission entities.
│   │                                For organization purposes, accesses have been categorized by assignee types.
│   ├── anonymous
│   │   ├── accesses.yml             The access entities assigned to anonymous identities.
│   │   └── permissions.yml          The permission entities assigned to anonymous identities.
│   ├── individual
│   │   ├── accesses.yml             The access entities assigned to individual identities.
│   │   └── permissions.yml          The permission entities assigned to individual identities.
│   ├── organization
│   │   ├── accesses.yml             The access entities assigned to organization identities.
│   │   └── permissions.yml          The permission entities assigned to organization identities.
│   ├── role
│   │   ├── accesses.yml             The access entities assigned to roles.
│   │   └── permissions.yml          The permission entities assigned to roles.
│   ├── staff
│   │   ├── accesses.yml             The access entities assigned to staff identities.
│   │   └── permissions.yml          The permission entities assigned to staff identities.
│   └── system
│       ├── accesses.yml             The access entities assigned to system identities.
│       └── permissions.yml          The permission entities assigned to system identities.
│
├── configs.yml                      The config entities.
├── registrations.yml                The registration entities.
│
└── user                             The user folder contains all user entities.
    │                                For organization purposes, users have been categorized by identity types.
    ├── anonymous
    │   └── users.yml                The user entities for anonymous identities.
    ├── individual
    │   └── users.yml                The user entities for individual identities.
    ├── organization
    │   └── users.yml                The user entities for organization identities.
    ├── staff
    │   └── users.yml                The user entities for staff identities.
    └── system
        └── users.yml                The user entities for system identities.
```

## Entities

### Access Entity

An access is assigned to a specific user and holds one or more permissions. Accesses are compiled and used by the security component to validate user actions by granting or denying them access based on whats configured for them.

The properties are:

- uuid: The access uuid.
- owner: The owner of the asset. Possible values: `BusinessUnit`, `Anonymous`, `Individual`, `Organization`, `Staff`, `System`.
- owner_uuid: The owner uuid of the asset.
- assignee: The assignee the access is created for.
- assignee_uuid: The assignee uuid the access is created for.

For example, an access card owned by a `BusinessUnit` and assigned to an `Individual`:

```
items:
    -
        uuid: 859e68e2-1c4c-4730-aab7-d51f5a399dec
        owner: BusinessUnit
        owner_uuid: c11c546e-bd01-47cf-97da-e25388357b5a # Administration
        assignee: Individual
        assignee_uuid: d0daa7e4-07d1-47e6-93f2-0629adaa3b49 # Morgan
```

### Permission Entity

A permission is a grant regarding a specific action. For example: "READ users", "EDIT users", "DELETE users".

The properties are:

- access: The access which holds this permission.
- scope: The permission scope. Possible values: `entity`, `object`, `owner`, `session`.
- entity: The entity this permission affects based on the scope.
- entity_uuid: The entity uuid this permission affects based on the scope.
- key: The key, or keys, from the list of defined keys this permission affects.
- attributes: The attributes granted by this permission.

For example, a permission held by an access card which grants `READ` action on `user` entities owned by a `BusinessUnit`:

```
items:
    -
        access: 859e68e2-1c4c-4730-aab7-d51f5a399dec
        scope: owner
        entity: BusinessUnit
        entity_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327
        key: user
        attributes: [READ]
```

### Config Entity

A config is a low-level microservice configuration. Each microservice contains it's own list of configurations.

The properties are:

- uuid: The config uuid.
- owner: The owner of the config.
- owner_uuid: The owner uuid of the config.
- key: The unique, machine-friendly name of the config.
- value: The value of the config.
- enabled: Whether the config is enabled or not. Sensible defaults of configs are stored at the file level, and overwritable at the database level. If a configuration is not enabled, the file based config value will be used.

For example, a config that sets the default pagination limit for cases listing:

```
items:
    -
        uuid: 8a457c70-9472-4428-9280-cd0ae92d0f09
        owner: BusinessUnit
        owner_uuid: c11c546e-bd01-47cf-97da-e25388357b5a # Administration
        key: case.pagination.default_limit
        value: 10
        enabled: true
```

### Registration Entity

A registration represents the submission data during user registration.

The properties are:

- uuid: The registration uuid.
- owner: The owner of the registration.
- owner_uuid: The owner uuid of the registration.
- username: The username.
- password: The password.
- data: The data of the registration. Typically contains first name, last name, various opt-ints and other registration related fields.

For example, a user registration for Morgan owned by a BusinessUnit:

```
items:
    -
        uuid: adca2b8a-b060-42f0-a1b7-69387633f375
        owner: BusinessUnit
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
        username: morgan@individual.ds
        password: 12345678
        data:
            first_name: Morgan
            last_name: Cole
            newsletter: true
```
