# Fixtures

The purpose of fixtures is to document the known state of the Platform, and most importantly, being able to revert back to that snapshot on demand. The snapshot includes all Postgres database rows, Redis cache, Formio forms, Camunda BPMN workflows, etc.

It is possible to override fixtures on a per-microservice level using this directory. By default, this directory comes empty, meaning the base fixtures provided by DigitalState will be used. However, the Platform allows you to override base fixtures by creating your own here.

## Structure

The first level in this directory will contain folders, which will override a particular microservice's fixtures.

The available microservices names are:

- assets
- authentication
- cases
- cms
- identties
- records
- services


