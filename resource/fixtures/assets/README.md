# Assets Microservice

## Fixtures File Structure

```
.
├── access                           The access folder contains all access and permissions entities.
|   |                                For organization purposes, accesses have been categorized by assignee types.
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

An access is assigned to a specific user and holds one or more permissions. Accesses are compiled and used by the security component to validate user actions by granting or denying them access based on whats configured for them.

The properties are:

- uuid: The access uuid. For example: `859e68e2-1c4c-4730-aab7-d51f5a399dec`.
- owner: The owner of the asset. For example: `BusinessUnit`.
- owner_uuid: The owner uuid of the asset. For example: `c11c546e-bd01-47cf-97da-e25388357b5a` (Administration)
- assignee: The assignee the access is created for. For example `Individual`.
- assignee_uuid: The assignee uuid the access is created for. For example `d0daa7e4-07d1-47e6-93f2-0629adaa3b49` (Morgan).

### Permission Entity

A permission is a grant regarding a specific action. For example: "READ users", "EDIT users", "DELETE users".

The properties are:

- access: The access which holds this permission. For example: `859e68e2-1c4c-4730-aab7-d51f5a399dec`.
- scope: The permission scope. Available values: `entity`, `object`, `owner`, `session`. For example: `owner`.
- entity: The entity this permission affects based on the scope. For example: `BusinessUnit`.
- entity_uuid: The entity uuid this permission affects based on the scope. For example: `a9d68bf7-5000-49fe-8b00-33dde235b327`.
- key: The key, or keys, from the list of defined keys this permission affects. For example: `user`.
- attributes: The attributes granted by this permission. For example: `READ`.

Given the above examples, whoever is assigned this permission would grant them the `READ` action on `user` entities that are owned by the `BusinessUnit` with uuid `a9d68bf7-5000-49fe-8b00-33dde235b327`.

### Asset Entity

An asset is a tangible documents issued by the government. For example: a deed to a land, a library card, a drivers license.

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

- uuid: The config uuid. For example: `8a457c70-9472-4428-9280-cd0ae92d0f09`.
- owner: The owner of the config. For example: `BusinessUnit`.
- owner_uuid: The owner uuid of the config. For example: `c11c546e-bd01-47cf-97da-e25388357b5a` (Administration)
- key: The unique, machine-friendly name of the config. For example: `case.pagination.default_limit`.
- value: The value of the config. For example: `10`.
- enabled: Whether the config is enabled or not. If a config is disabled, it will be bypassed by the default value set in low level, file-based, configurations. For example: `true`.
