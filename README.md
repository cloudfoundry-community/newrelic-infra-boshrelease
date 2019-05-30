# BOSH Release for newrelic infrastructure monitoring agent

## Usage

To use this bosh release, add the release block from Releases to your manifest, or upload a version from the cloned repository.


Then add the newrelic license (you can find this at https://rpm.newrelic.com/accounts/YOUR_ACCOUNT_NUMBER) to the properties section of your manifest file and the newrelic release to the releases section:

```
properties:
  ...
  newrelic:
    license_key: foobar
    deployment_tag: my_deployment
releases:
- ...
- name: newrelic-infra
  version: latest
```

Finally add the `newrelic-infra` job to your manifest:

```
- instances: 1
  name: runner_z1
  ...
  jobs:
  - ...
  - name: newrelic-infra
    release: newrelic-infra
```


## This release is packaged with custom integrations for Redis, RabbitMQ, or Vault bosh releases


## [RabbitMQ](https://github.com/newrelic/nri-rabbitmq) - Haproxy Node
```
instance_groups:
- azs:
  ...
  instances: 1
  jobs:
  - name: rabbitmq-haproxy
    release: cf-rabbitmq
  - name: newrelic-infra-rabbitmq
    release: newrelic-infra
  - name: toolbelt
    release: toolbelt
  - name: toolbelt-everything
    release: toolbelt
  name: rabbitmq_haproxy
- ...
properties:
  azs:
  - az1
  newrelic:
    deployment_tag: ...
    hostname: ...-rabbitmq
    license_key: ...
    rabbitmq:
      env: name-of-rabbitmq-deployment 
      hostname: 192.168.1.150 # ip address of rmq haproxy
      interval: 60 # newrelic infra polling interval
      password: rabbit # rabbitmq management creds
      username: rabbit
```

## [Redis](https://docs.newrelic.com/docs/integrations/host-integrations/host-integrations-list/redis-monitoring-integration)
```
- azs:
  ...
  instances: 3
  jobs:
  - name: ...
    release: ...
  - name: newrelic-infra-redis
    release: newrelic-infra
  ...
```

## [Vault](https://github.com/jordanbcooper/newrelic-integration-vaultstatus)
```
- azs:
  ...
  instances: 3
  jobs:
  - name: ...
    release: ...
  - name: newrelic-infra-vault
    release: newrelic-infra
  ...
properties:
  newrelic:
    deployment_tag: env-vault
    license_key: ...
    vault_url: ((redirect_address))

```


