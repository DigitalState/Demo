# Fixtures

The purpose of fixtures is to document the known state of the Platform, and most importantly, being able to revert back to that known state on demand. The known state includes all **Postgres database rows**, **Redis cache**, **Formio forms**, **Camunda BPMN workflows**, etc.

By default, each microservice comes with its own set of fixtures, which are decentralized, very minimal and developer centric. However, it is possible to override these fixtures at the Platform level by creating your own here.

Fixtures tend to be very useful for developers during development and debugging. They are also very convenient when wanting to showcase demos with specific sample data.

## Microservices

The list of folders in this directory represents each microservice fixtures you wish to override. By default, this directory is empty, meaning the Platform will use the decentralized base fixtures found in each microservice. However, by creating folders in this directory, this will instruct the Platform to use these fixtures, instead of the base ones.

The available microservices names are:

- assets
- authentication
- cases
- cms
- identties
- records
- services

For demo purposes, we have created fixtures overrides for each microservice, which are exact duplicates of the base decentralized fixtures found in each microservice.

> Note: when overriding fixtures for a microservice, it is required to include all fixtures files. It is not possible to just override specific files.

## Structure

Fixtures are implemented as [YML configuration](https://en.wikipedia.org/wiki/YAML) files. Each files represents a list of items (such as entities, forms, workflows), that will be loaded into the microservice.

Each YML configuration files contains two attributes `items` and `prototype`.

The `items` attribute describes the array of items wanted to be loaded into the known state.

The `prototype` attribute is a convenient feature that allows you to define sensible defaults for each `items` specified.

Take for example, the `Individual` entity, which has 3 properties: `uuid`, `owner` and `owner_uuid`. An [individuals fixtures](identities/identity/individual/identities.yml) could resemble the following:

```
items:
    -
        uuid: d0daa7e4-07d1-47e6-93f2-0629adaa3b49 # Morgan

    -
        uuid: f82f2ccb-818b-44d6-b425-823c9899f846 # Taylor

prototype:
    uuid: ~
    owner: BusinessUnit
    owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
```

> Note: tilda (`~`) means null.

In the example above, we are wanting to load 2 individual identities in the known state, **Morgan** and **Taylor**. Both identities are owned by the same BusinessUnit. A convenient way to optimize this is to use the prototype attribute to define sensible defaults.

The Platform would interpret the above as followed:

```
items:
    -
        uuid: d0daa7e4-07d1-47e6-93f2-0629adaa3b49 # Morgan
        owner: BusinessUnit
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice

    -
        uuid: f82f2ccb-818b-44d6-b425-823c9899f846 # Taylor
        owner: BusinessUnit
        owner_uuid: a9d68bf7-5000-49fe-8b00-33dde235b327 # Backoffice
```

## Creating fixtures from database rows

1. In Postman, login as the system identity.

2. In Postman, do a GET request on the entity you wish to generate fixtures for. For example: GET /cases:

```
[
    {
        "id": 15,
        "uuid": "82184661-3aa4-4c68-a130-e07dc2e0d974",
        "customId": "GOV-82184661",
        "createdAt": "2018-04-27T19:17:10+00:00",
        "updatedAt": "2018-04-27T19:17:10+00:00",
        "deletedAt": null,
        "owner": "BusinessUnit",
        "ownerUuid": "a9d68bf7-5000-49fe-8b00-33dde235b327",
        "identity": "Individual",
        "identityUuid": "d0daa7e4-07d1-47e6-93f2-0629adaa3b49",
        "title": {
            "en": "Pothole - 123 Street - Urgent",
            "fr": "Nid-de-poule - 123 rue - urgent"
        },
        "data": {
            "en": [],
            "fr": []
        },
        "state": "open",
        "priority": 0,
        "statuses": [
            "/app_dev.php/case-statuses/21b3b72a-bd1b-4d87-9b28-7768aaea8ae6",
            "/app_dev.php/case-statuses/1739b92f-9ce9-4f7d-92ff-6314171794b4",
            "/app_dev.php/case-statuses/5a438bff-afa9-489d-953f-4524ca0905cb",
            "/app_dev.php/case-statuses/0a42016c-c625-4025-becf-b7dc98db44f8",
            "/app_dev.php/case-statuses/02a74181-c59d-459a-acaf-64c7a597f6b7",
            "/app_dev.php/case-statuses/40f6af60-b55b-4eee-a8db-71eec55fc8f5"
        ],
        "version": 1
    },
    {
        "id": 16,
        "uuid": "788e4c03-b97b-4f5c-8024-cd128121ca11",
        "customId": "GOV-788e4c03",
        "createdAt": "2018-04-27T19:17:10+00:00",
        "updatedAt": "2018-04-27T19:17:10+00:00",
        "deletedAt": null,
        "owner": "BusinessUnit",
        "ownerUuid": "a9d68bf7-5000-49fe-8b00-33dde235b327",
        "identity": "Staff",
        "identityUuid": "5449ce65-5df0-440d-aead-21a77907e59a",
        "title": {
            "en": "Pothole - Quality Review",
            "fr": "Nid-de-poule - Examen de la qualité"
        },
        "data": {
            "en": [],
            "fr": []
        },
        "state": "open",
        "priority": 0,
        "statuses": [],
        "version": 1
    }
]
```

3. Convert the JSON formatted response to YAML format using an [online tool](https://www.json2yaml.com):

```
- id: 15
  uuid: 82184661-3aa4-4c68-a130-e07dc2e0d974
  customId: GOV-82184661
  createdAt: '2018-04-27T19:17:10+00:00'
  updatedAt: '2018-04-27T19:17:10+00:00'
  deletedAt:
  owner: BusinessUnit
  ownerUuid: a9d68bf7-5000-49fe-8b00-33dde235b327
  identity: Individual
  identityUuid: d0daa7e4-07d1-47e6-93f2-0629adaa3b49
  title:
    en: Pothole - 123 Street - Urgent
    fr: Nid-de-poule - 123 rue - urgent
  data:
    en: []
    fr: []
  state: open
  priority: 0
  statuses:
  - "/app_dev.php/case-statuses/21b3b72a-bd1b-4d87-9b28-7768aaea8ae6"
  - "/app_dev.php/case-statuses/1739b92f-9ce9-4f7d-92ff-6314171794b4"
  - "/app_dev.php/case-statuses/5a438bff-afa9-489d-953f-4524ca0905cb"
  - "/app_dev.php/case-statuses/0a42016c-c625-4025-becf-b7dc98db44f8"
  - "/app_dev.php/case-statuses/02a74181-c59d-459a-acaf-64c7a597f6b7"
  - "/app_dev.php/case-statuses/40f6af60-b55b-4eee-a8db-71eec55fc8f5"
  version: 1
- id: 16
  uuid: 788e4c03-b97b-4f5c-8024-cd128121ca11
  customId: GOV-788e4c03
  createdAt: '2018-04-27T19:17:10+00:00'
  updatedAt: '2018-04-27T19:17:10+00:00'
  deletedAt:
  owner: BusinessUnit
  ownerUuid: a9d68bf7-5000-49fe-8b00-33dde235b327
  identity: Staff
  identityUuid: 5449ce65-5df0-440d-aead-21a77907e59a
  title:
    en: Pothole - Quality Review
    fr: Nid-de-poule - Examen de la qualité
  data:
    en: []
    fr: []
  state: open
  priority: 0
  statuses: []
  version: 1
```

4. Remove unneeded properties, such as id's and created/updated dates, the for yml to be compatible with the examples provided:

```
- uuid: 82184661-3aa4-4c68-a130-e07dc2e0d974
  customId: GOV-82184661
  owner: BusinessUnit
  ownerUuid: a9d68bf7-5000-49fe-8b00-33dde235b327
  identity: Individual
  identityUuid: d0daa7e4-07d1-47e6-93f2-0629adaa3b49
  title:
    en: Pothole - 123 Street - Urgent
    fr: Nid-de-poule - 123 rue - urgent
  data:
    en: []
    fr: []
  state: open
  priority: 0
- uuid: 788e4c03-b97b-4f5c-8024-cd128121ca11
  customId: GOV-788e4c03
  owner: BusinessUnit
  ownerUuid: a9d68bf7-5000-49fe-8b00-33dde235b327
  identity: Staff
  identityUuid: 5449ce65-5df0-440d-aead-21a77907e59a
  title:
    en: Pothole - Quality Review
    fr: Nid-de-poule - Examen de la qualité
  data:
    en: []
    fr: []
  state: open
  priority: 0
```

5. Adjust the value of foreign keys from `entity-name/uuid` to just `uuid`.

6. Envelop the fixtures with a `fixtures` property

```
fixtures:
  - uuid: 82184661-3aa4-4c68-a130-e07dc2e0d974
    customId: GOV-82184661
    owner: BusinessUnit
    ownerUuid: a9d68bf7-5000-49fe-8b00-33dde235b327
    identity: Individual
    identityUuid: d0daa7e4-07d1-47e6-93f2-0629adaa3b49
    title:
      en: Pothole - 123 Street - Urgent
      fr: Nid-de-poule - 123 rue - urgent
    data:
      en: []
      fr: []
    state: open
    priority: 0
  - uuid: 788e4c03-b97b-4f5c-8024-cd128121ca11
    customId: GOV-788e4c03
    owner: BusinessUnit
    ownerUuid: a9d68bf7-5000-49fe-8b00-33dde235b327
    identity: Staff
    identityUuid: 5449ce65-5df0-440d-aead-21a77907e59a
    title:
      en: Pothole - Quality Review
      fr: Nid-de-poule - Examen de la qualité
    data:
      en: []
      fr: []
    state: open
    priority: 0
```