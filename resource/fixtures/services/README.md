# Services Microservice

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
├── camunda
│   ├── bpmn
│   │   └── pothole-report.bpmn
│   └── deployments.yml              The camunda deployments.
│
├── categories.yml                   The categories entities.
├── configs.yml                      The config entities.
├── formio
│   └── forms.yml                    The formio forms.
│
├── scenarios.yml                    The scenario entities.
└── services.yml                     The services entities.
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

### Category Entity

A category represents a grouping of services.

The properties are:

- uuid: The category uuid.
- owner: The owner of the category.
- owner_uuid: The owner uuid of the category.
- slug: The unique, machine name of the category.
- title: The category title. This field is translatable.
- description: The category short description. This field is translatable.
- presentation: The category long description. This field is translatable.
- data: The category data. This field is used to store misc information outside the conceived schema.
- enabled: The category enabled status.
- weight: The category weight. This is used for custom ordering.
- services: The services inside the category.

For example, a category owned by a Business unit:

```
items:
    -
        uuid: ecc69524-0b12-4eae-a6f4-b6eb0f442e29
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
        slug: infrastructure
        title:
            en: Infrastructure
            fr: Infrastructure
        description:
            en: Description ...
            fr: Description ...
        presentation:
            en: Presentation ...
            fr: Presentation ...
        services:
            - 9ed44d24-b2e1-4882-82f9-7e4c2c8bf73d # Report a Pothole
            - 52d4797d-aeba-4933-8775-e44901cf5f2d # Report a Graffiti
```

### Service Entity

A service represents a digital service offered by the government.

The properties are:

- uuid: The service uuid.
- owner: The owner of the service.
- owner_uuid: The owner uuid of the service.
- slug: The unique, machine name of the service.
- title: The service title. This field is translatable.
- description: The service short description. This field is translatable.
- presentation: The service long description. This field is translatable.
- data: The service data. This field is used to store misc information outside the conceived schema.
- enabled: The service enabled status.
- weight: The service weight. This is used for custom ordering.

For example, a service owned by a Business unit:

```
items:
    -
        uuid: 9ed44d24-b2e1-4882-82f9-7e4c2c8bf73d
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
        slug: report-pothole
        title:
            en: Report a Pothole
            fr: Signaler un nids de poule
        description:
            en: Description ...
            fr: Description ...
        presentation:
            en: Presentation ...
            fr: Presentation ...
```

### Scenario Entity

A scenario represents a channel by which you can apply for a service offered by the government. For example, the online scenario, the by-phone scenario, etc.

The properties are:

- uuid: The scenario uuid.
- owner: The owner of the scenario.
- owner_uuid: The owner uuid of the scenario.
- service: The service associated with the scenario.
- type: The scenario type. Possible values are "bpm".
- config: The scenario configurations, which depends on the scenario type.
- slug: The unique, machine name of the scenario.
- title: The scenario title. This field is translatable.
- description: The scenario short description. This field is translatable.
- presentation: The scenario long description. This field is translatable.
- data: The scenario data. This field is used to store misc information outside the conceived schema.
- enabled: The scenario enabled status.
- weight: The scenario weight. This is used for custom ordering.

For example, a bpm scenario linked to a camunda workflow and owned by a Business unit:

```
items:
    -
        uuid: e049f2b4-b249-48c2-850c-64d4c4b39527
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
        service: 9ed44d24-b2e1-4882-82f9-7e4c2c8bf73d # Report a Pothole
        type: bpm
        config:
            bpm: camunda
            process_definition_key: pothole-report
        slug: online
        title:
            en: Report a Pothole Online
            fr: Signaler un nids de poule en-ligne
        description:
            en: Description ...
            fr: Description ...
        presentation:
            en: Presentation ...
            fr: Présentation ...
        data:
            en:
                button_text: Activate
            fr:
                button_text: Activer
        weight: 0
```
