<p align="center"><a href="http://digitalstate.ca" target="_blank">
    <img src="https://avatars3.githubusercontent.com/u/12055994?s=200&v=4">
</a></p>

The Demo repository is an example of a configured [Platform](https://github.com/DigitalState/Platform) repository.

Its purpose is to highlight the configurations modifications required prior to deployment, demonstrate some available features and just overall provide a deeper understanding of the Platform through a real world use case.

## Table of Contents

- [Context](#context)
- [Customizations](#customizations)
- [Contributing](#contributing)
- [Credits](#credits)

## Context

For the purpose of the demo, we will assume the following context:

- We have a laptop with Windows 10 Pro installed.
- We have a remote web server with Ubuntu 17.10 installed with ip 1.2.3.4.
- We wish to deploy the full DigitalState Platform on a remote dev server for sandbox purposes.

## Customizations

### Ansible Inventory

The DigitalState Platform contains an Ansible inventory file per environment. Prior to deploying the Platform on the dev server, the dev inventory file needs to be configured.

The following configurations have been modified:

- [ansible_host](https://github.com/DigitalState/Demo/blob/master/platform/ansible/env/dev/inventory.yml#L7): The ansible_host config has been set to our dev server ip.
- [encryption.secret](https://github.com/DigitalState/Demo/blob/master/platform/ansible/env/dev/inventory.yml#L16): The encryption.secret config has been set to secret, unique 32 characters long string. This value is used by various components for seeding, for example: Symfony CSRF token generation.
- [jwt.pass_phrase](https://github.com/DigitalState/Demo/blob/master/platform/ansible/env/dev/inventory.yml#L18): The jwt.pass_phrase config has been set to the secret pass phrase associated with the jwt private key used by the authentication component.

### Fixtures

The purpose of fixtures is to document the known state of the Platform, and most importantly, being able to revert back to that known state on demand. The known state includes all Postgres database rows, Redis cache, Formio forms, Camunda bpmn workflows, etc.

Fixtures are often used for the purpose of setting up sample data during initialization of the Platform.

Custom fixtures have been added to the [fixtures directory](resource/fixtures). This enables the demo to have its own sample data and bypass the base fixtures provided by DigitalState.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Credits

This work has been developed by DigitalState.io
