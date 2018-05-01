# Identities Microservice

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
├── identity                         The identity folder contains all identity and persona entities.
│   │                                For organization purposes, identities have been categorized by identity types.
│   ├── anonymous
│   │   ├── identities.yml           The anonymous identity entities.
│   │   └── personas.yml             The anonymous personas entities.
│   ├── individual
│   │   ├── identities.yml           The individual identity entities.
│   │   └── personas.yml             The individual personas entities.
│   ├── organization
│   │   ├── identities.yml           The organization identity entities.
│   │   └── personas.yml             The organization personas entities.
│   ├── staff
│   │   ├── identities.yml           The staff identity entities.
│   │   └── personas.yml             The staff personas entities.
│   └── system
│       └── identities.yml           The system identity entities.
│
├── business_units.yml               The business unit entities.
├── roles.yml                        The role entities.
└── configs.yml                      The config entities.
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

### Anonymous Entity

An anonymous entity represents an identity that has yet to authenticate itself. It is typically used for non-authenticated interactions.

The properties are:

- uuid: The anonymous uuid.
- owner: The owner of the anonymous identity.
- owner_uuid: The owner uuid of the anonymous identity.

For example, an anonymous identity owned by a business unit:

```
items:
    -
        uuid: e3d95fa0-5661-4213-b5a4-36b7cded0207
        owner: BusinessUnit
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
```

### AnonymousPersona Entity

An anonymous persona entity represents a persona for an anonymous identity. It typically contains personal information such as first and last names, email, etc

The properties are:

- anonymous: The anonymous identity associated with the persona.
- uuid: The anonymous persona uuid.
- owner: The owner of the anonymous identity persona.
- owner_uuid: The owner uuid of the anonymous identity persona.
- title: The identity persona title. This field is translatable.
- data: The persona data.

For example, an anonymous identity persona owned by a business unit:

```
items:
    -
        anonymous: e3d95fa0-5661-4213-b5a4-36b7cded0207
        uuid: 28cd2b1f-c930-4f64-8e81-29c11ad71aee
        owner: BusinessUnit
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
        title:
            en: My Persona
            fr: Mon profil
        data:
            first_name: Morgan
            last_name: Cole
```

### Individual Entity

An individual entity represents an identity of type Individual. It is typically used for citizens.

The properties are:

- uuid: The individual uuid.
- owner: The owner of the individual identity.
- owner_uuid: The owner uuid of the individual identity.

For example, an individual identity owned by a business unit:

```
items:
    -
        uuid: 4c084c20-8ea6-45ff-b60c-68fc04098244
        owner: BusinessUnit
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
```

### IndividualPersona Entity

An individual persona entity represents a persona for an individual identity. It typically contains personal information such as first and last names, email, etc

The properties are:

- individual: The individual identity associated with the persona.
- uuid: The individual persona uuid.
- owner: The owner of the individual identity persona.
- owner_uuid: The owner uuid of the individual identity persona.
- title: The identity persona title. This field is translatable.
- data: The persona data.

For example, an individual identity persona owned by a business unit:

```
items:
    -
        individual: 4c084c20-8ea6-45ff-b60c-68fc04098244
        uuid: 28cd2b1f-c930-4f64-8e81-29c11ad71aee
        owner: BusinessUnit
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
        title:
            en: My Persona
            fr: Mon profil
        data:
            first_name: Morgan
            last_name: Cole
```

### Organization Entity

An organization entity represents an identity of type Organization. It is typically used for organizations.

The properties are:

- uuid: The organization uuid.
- owner: The owner of the organization identity.
- owner_uuid: The owner uuid of the organization identity.

For example, an organization identity owned by a business unit:

```
items:
    -
        uuid: 290d1d9a-07dd-416f-9327-a81eb02eb7dd
        owner: BusinessUnit
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
```

### OrganizationPersona Entity

An organization persona entity represents a persona for an organization identity. It typically contains personal information such as first and last names, email, etc

The properties are:

- organization: The organization identity associated with the persona.
- uuid: The organization persona uuid.
- owner: The owner of the organization identity persona.
- owner_uuid: The owner uuid of the organization identity persona.
- title: The identity persona title. This field is translatable.
- data: The persona data.

For example, an organization identity persona owned by a business unit:

```
items:
    -
        organization: 290d1d9a-07dd-416f-9327-a81eb02eb7dd
        uuid: 28cd2b1f-c930-4f64-8e81-29c11ad71aee
        owner: BusinessUnit
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
        title:
            en: My Persona
            fr: Mon profil
        data:
            name: Acme Corp
            number: 123456
```

### Staff Entity

A staff entity represents an identity of type Staff. It is typically used for backoffice staff members and administrators.

The properties are:

- uuid: The staff uuid.
- owner: The owner of the staff identity.
- owner_uuid: The owner uuid of the staff identity.
- business_units: The business units the staff is subscribed to.

For example, an staff identity owned by a business unit:

```
items:
    -
        uuid: 4c084c20-8ea6-45ff-b60c-68fc04098244
        owner: BusinessUnit
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
        business_units:
            - a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
```

### StaffPersona Entity

A staff persona entity represents a persona for an staff identity. It typically contains personal information such as first and last names, email, etc

The properties are:

- staff: The staff identity associated with the persona.
- uuid: The staff persona uuid.
- owner: The owner of the staff identity persona.
- owner_uuid: The owner uuid of the staff identity persona.
- title: The identity persona title. This field is translatable.
- data: The persona data.

For example, an staff identity persona owned by a business unit:

```
items:
    -
        staff: 4c084c20-8ea6-45ff-b60c-68fc04098244
        uuid: 28cd2b1f-c930-4f64-8e81-29c11ad71aee
        owner: BusinessUnit
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
        title:
            en: My Persona
            fr: Mon profil
        data:
            first_name: Morgan
            last_name: Cole
```

### System Entity

A system entity represents an identity of type System. It is typically used by the system to interact with other microservices.

The properties are:

- uuid: The system uuid.
- owner: The owner of the system identity.
- owner_uuid: The owner uuid of the system identity.

For example, an system identity owned by itself:

```
items:
    -
        uuid: 00dc1842-f6fa-4c5a-aada-71c97fd0e9ff
        owner: System
        owner_uuid: 00dc1842-f6fa-4c5a-aada-71c97fd0e9ff
```

### Business Unit Entity

A business unit entity represents a department within government.

The properties are:

- uuid: The business unit uuid.
- owner: The owner of the business unit.
- owner_uuid: The owner uuid of the business unit.
- title: The title of the business unit. This field is translatable.

For example, an staff identity owned by a business unit:

```
items:
    -
        uuid: ddec6583-f526-4397-99e1-cef658d3953a
        owner: BusinessUnit
        owner_uuid: c11c546e-bd01-47cf-97da-e25388357b5a # Administration
        title:
            en: Backoffice
            fr: Bureau
```

### Role Entity

A role entity represents a collection of access permissions accross all microservers.

The properties are:

- uuid: The role uuid.
- owner: The owner of the role.
- owner_uuid: The owner uuid of the role.
- title: The title of the role. This field is translatable.
- permissions: The list of permissions.

For example, an staff identity owned by a business unit:

```
items:
    -
        uuid: 6187de05-1550-4bd7-a71d-7237ecced34c
        owner: BusinessUnit
        owner_uuid: c11c546e-bd01-47cf-97da-e25388357b5a # Administration
        title:
            en: Administrator
            fr: Administrateur
        permissions:
            assets:
                ...
            authentication:
                ...
            cases:
                ...
```

