# Fixtures

The purpose of fixtures is to document the known state of the Platform, and most importantly, being able to revert back to that known state on demand. The known state includes all Postgres database rows, Redis cache, Formio forms, Camunda BPMN workflows, etc.

By default, the Platform comes with its own set of fixtures, which are very minimal and developer centric. However, it is possible to override these fixtures by creating your own here.

Although fixtures tend to be very useful for developers during development, they are also very convenient when wanting to showcase demonstrations with pre-built sample data.

## Microservices

The list of folders in this directory represents each microservices fixtures you wish to override. By default, this directory is empty, meaning the Platform will use the decentralized base fixtures found in each microservice. However, by creating folders in this directory, this will instruct the Platform to use these fixtures found here, instead of the base ones.

The available microservices names are:

- assets
- authentication
- cases
- cms
- identties
- records
- services

For demo purposes, we have created above fixtures overrides for each microservices.

## Structure

Fixtures are implemented as YML configuration files. Each files represents a specific type of data and contains a list of items of said type of data that will be loaded into the microservice.

Each YML configuration files contains two attributes `items` and `prototype`.

The `items` attribute describes the array of items wanted to be loaded into the known state. A `prototype` attribute is also present.

The `prototype` attribute is a convenient feature that allows you to define sensible defaults for each `items` specified, while enabling you not having to redefine all attributes for each items.

Consider the following [individuals fixtures](identities/identity/individual/identities.yml):

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

In the example above, we are wanting to load 2 individual identities in the known state, Morgan and Taylor. Both identities are owned by the same BusinessUnit. A convenient way to optimize this is to use the prototype attribute to define sensible defaults.

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
