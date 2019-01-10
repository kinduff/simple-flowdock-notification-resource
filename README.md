![concourse](https://user-images.githubusercontent.com/1270156/50938865-cc9f5500-143f-11e9-93c9-f594c4d51b31.jpg)

Simple Flowdock notification resource about the build status. Based on the official cloudfoundry one.

## Resource Type Configuration

```yml
resource_types:
- name: flowdock-notification-image
  type: docker-image
  source:
    repository: kinduff/better-flowdock-notification-resource
    tag: latest
```

## Resource Configuration

```yml
resources:
- name: flowdock-notify
  type: flowdock-notification-image
```

## Behavior

### `out`: Sends notification to Flowdock.

Send notification to Flowdock, with the configured parameters.

#### Parameters

Required:
- `flow_token`: Flow token where you want to post. Refer to [this documentation](https://www.flowdock.com/api/how-to-integrate).
- `status_value`: Custom message. Usually `Success` or `Failure`.
- `status_color`: Custom Flowdock color. Usually `green` or `red`.

## Examples

![example](https://user-images.githubusercontent.com/1270156/50939445-84356680-1442-11e9-9c34-a3ab2ba1e9bd.png)

```yml
---
jobs:
- name: flowdock-hello-world-test
  plan:
  - get: resource-tutorial
  - task: hello-world
    file: resource-tutorial/tutorials/basic/task-hello-world/task_hello_world.yml
    on_failure:
      put: flowdock-notify
      params:
        status_value: failure
        status_color: red
        flow_token: ((meta.flowdock.token))
    on_success:
      put: flowdock-notify
      params:
        status_value: success
        status_color: green
        flow_token: ((meta.flowdock.token))
resource_types:
- name: flowdock-notification-image
  type: docker-image
  source:
    repository: kinduff/better-flowdock-notification-resource
    tag: latest
resources:
- name: flowdock-notify
  type: flowdock-notification-image
- name: resource-tutorial
  type: git
  source:
    uri: https://github.com/starkandwayne/concourse-tutorial.git
```

![teams-main-pipelines-test-pipe](https://user-images.githubusercontent.com/1270156/50939532-e1311c80-1442-11e9-9064-408d6f3334b7.png)
