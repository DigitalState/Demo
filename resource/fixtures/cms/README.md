# Cms Microservice

## Fixtures Text Structure

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
├── data.yml                         The data entities.
├── texts.yml                        The text entities.
├── texts.yml                        The text entities.
└── configs.yml                      The config entities.
```

## Entities

### Access Entity

An access is assigned to a specific user and holds one or more permissions. Accesses are compiled and used by the security component to validate user actions by granting or denying them access based on whats configured for them.

The properties are:

- uuid: The access uuid.
- owner: The owner of the data. Possible values: `BusinessUnit`, `Anonymous`, `Individual`, `Organization`, `Staff`, `System`.
- owner_uuid: The owner uuid of the data.
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
- enabled: Whether the config is enabled or not. Sensible defaults of configs are stored at the text level, and overwritable at the database level. If a configuration is not enabled, the text based config value will be used.

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

### Data Entity

A data represents a means to store snippets of data of all kinds. For example: a list of possible values for a dropdown.

The properties are:

- uuid: The data uuid.
- owner: The owner of the data.
- owner_uuid: The owner uuid of the data.
- slug: The unique, machine name of the data.
- title: The title of the data. This field is translatable.
- data: The data object.

For example, a snippet of data regarding street classifications owned by a BusinessUnit:

```
items:
    -
        uuid: c859dbd2-2fc1-4b3b-b00a-00e15b1dbac0
        owner_uuid: b20a40d9-b95b-4462-b8f1-c7453b9b7067 # Portal
        slug: road-classifications
        title:
            en: Road Classifications
            fr: Classifications routières
        data:
            en:
                city-freeway: City Freeway
                arterial-roads: Arterial Roads
                major-collector-and-collector-roads: Major Collector and Collector Roads
                local-roads: Local Roads
            fr:
                city-freeway: Autoroute de ville
                arterial-roads: Artères
                collector-roads: Routes collectrices et routes collectrices principales
                local-roads: Rues locales
```

### Text Entity

A text represents a document. For example: a logo, an excel spreadsheet, a portrait.

The properties are:

- uuid: The data uuid.
- owner: The owner of the text.
- owner_uuid: The owner uuid of the text.
- slug: The unique, machine name of the text.
- title: The title of the text. This field is translatable.
- description: The description of the text. This field is translatable.
- presentation: The presentation of the text. This field is translatable.
- type: The text type.

For example, a logo text owned by a BusinessUnit:

```
items:
    -
        uuid: 3b94dc33-0db2-40e3-8916-b404a8b3fa41
        owner_uuid: b20a40d9-b95b-4462-b8f1-c7453b9b7067 # Portal
        slug: portal-logo
        title:
            en: Portal Logo
            fr: Logo Portal
        description:
            en: The Portal header logo.
            fr: Le logo de l'en-tête du portail.
        presentation:
            en: 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
            fr: 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
        type: image/png
```

### Text Entity

A text represents a snippet of text. For example: a paragraph of text, a title.

The properties are:

- uuid: The text uuid.
- owner: The owner of the text.
- owner_uuid: The owner uuid of the text.
- slug: The unique, machine name of the text.
- title: The title of the text. This field is translatable.
- value: The text value.

For example, the description of a web page owned by a BusinessUnit:

```
items:
    -
        uuid: e0bc9d13-3b37-4080-850a-d7b38756cd83
        owner_uuid: b20a40d9-b95b-4462-b8f1-c7453b9b7067 # Portal
        slug: portal-description
        title:
            en: Portal Description
            fr: Description du portail
        value:
            en: Digital Public Services Platform
            fr: Plate-forme de services publics numériques
```
