<p align="center"><a href="http://digitalstate.ca" target="_blank">
    <img src="https://avatars3.githubusercontent.com/u/12055994?s=200&v=4">
</a></p>

The Demo repository is an example of a configured [Platform](https://github.com/DigitalState/Platform) repository.

The purpose of this repository is to highlight the configurations modifications required prior to deployment, demonstrate some features available to the developers and devops and just overall provide a deeper understanding of the Platform through a real world use case.

## Table of Contents

- [Context](#context)
- [Customizations](#customizations)
- [Features](#features)
- [Conclusion](#conclusion)
- [Contributing](#contributing)
- [Credits](#credits)

## Context

For the purpose of the demo, we will assume the following context:

- We have a laptop with Windows 10 Pro installed.
- We have a remote web server with Ubuntu 17.10 installed with ip 1.2.3.4.
- We wish to deploy the full DigitalState Platform on a remote dev server for sandbox purposes.

## Customizations

This repository is essentially a clone of [Platform](https://github.com/DigitalState/Platform) with the following amends:

### Ansible Inventory

> The DigitalState Platform contains an Ansible inventory file per environment. Each inventory files describes a wide range of configurations, from which servers Ansible is deploying to, to which microservices will be enabled, etc.

Prior to deploying the Platform on the dev server, the dev inventory file needs to be configured.

The following configurations have been modified:

- [ansible_host](https://github.com/DigitalState/Demo/blob/master/platform/ansible/env/dev/inventory.yml#L7): The ansible_host config has been set to our dev server ip.
- [encryption.secret](https://github.com/DigitalState/Demo/blob/master/platform/ansible/env/dev/inventory.yml#L16): The encryption.secret config has been set to secret, unique 32 characters long string. This value is used by various components for seeding, for example: Symfony CSRF token generation.
- [jwt.pass_phrase](https://github.com/DigitalState/Demo/blob/master/platform/ansible/env/dev/inventory.yml#L18): The jwt.pass_phrase config has been set to the secret pass phrase associated with the jwt private key used by the authentication component.

### Fixtures

> The purpose of fixtures is to document the known state of the Platform, and most importantly, being able to revert back to that known state on demand. The known state includes all Postgres database rows, Redis cache, Formio forms, Camunda BPMN workflows, etc.

Fixtures are often used for the purpose of setting up sample demo data during the first deployment of the Platform.

Custom fixtures have been added to the [fixtures directory](resource/fixtures). These custom fixtures enables the demo to have its own sample data and bypass the base fixtures provided by DigitalState, which are more developer centric.

For a better understanding on the structure of custom fixtures, [click here](resource/fixtures).

## Features

...

## Conclusion

Now that we have gone thru a quick overview of the Platform, it is time to download the [Platform](https://github.com/DigitalState/Platform) repository and setup an instance of your own.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Credits

This work has been developed by DigitalState.io
