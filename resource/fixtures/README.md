# Fixtures

The purpose of fixtures is to document the known state of the Platform, and most importantly, being able to revert back to that known state on demand. The known state includes all Postgres database rows, Redis cache, Formio forms, Camunda BPMN workflows, etc.

By default, the Platform comes with its own set of fixtures, which are very minimal and developer centric. However, it is possible to override these fixtures by creating your own here.

Essentially, this override mechanism enables you to create your own fixtures and sample data to demo specific use cases.

## Structure

The list of folders in this directory represents each microservices fixtures you wish to override. By default, this directory is empty, meaning the Platform will use the base fixtures. However, by creating folders in this directory, will instruct the Platform to use these fixtures here, instead of the base ones.

The available microservices names are:

- assets
- authentication
- cases
- cms
- identties
- records
- services

For demo purposes, we have created fixture overrides for all microservices available.

