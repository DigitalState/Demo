# Fixtures

It is possible to override DigitalState fixtures by providing your own fixtures in this directory.

Ansible will bypass the base fixtures for each microservices, if a directory of the same name is present here for that microservice.

Each directory contains a group of YML files, basically describing the known state of that microservices.



