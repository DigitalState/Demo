# Assets Microservice

## Fixtures File Structure

```
.
├── access                           The access folder contains all access and permissions entities, categorized by assignee types.
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
├── assets.yml                       The assets entities.
└── configs.yml                      The config entities.
```

## Entities

### Access Entity

An access is assigned to a specific user and holds one or more permissions. Accesses are used by the security component to validate user actions by granting or denying access.

The properties are:

-

### Permission Entity

A permission is a grant regarding a specific action. For example: "READ users", "EDIT users", "DELETE users".

The properties are:

-

### Asset Entity

A asset is a tangible documents issued by the government. For example: a deed to a land, a library card, a drivers license.

The properties are:

- uuid: The asset uuid. For example: `238fecd9-3911-4593-a5a5-b3f693248283`.
- owner: The owner of the asset. For example: `BusinessUnit`.
- owner_uuid: The owner uuid of the asset. For example: `a9d68bf7-5000-49fe-8b00-33dde235b327` (Backoffice)
- identity: The identity the asset is created for. For example `Individual`.
- identity_uuid: The identity uuid the asset is created for. For example `d0daa7e4-07d1-47e6-93f2-0629adaa3b49` (Morgan).
- title: The title of the asset. This field is translatable. For example `{ "fr": "Permis", "en": "Permit" }`.

### Config Entity

A config is a low-level microservice configuration. Each microservice contains it's own list of configurations.

The properties are:

-
