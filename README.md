# Dennis' K8S playbook

## Open Issues

- Destroy does not check whether a component should have been installed in the first place (e.g., it tries to delete nginx ingress despite the fact that is shouldn't have been installed)
- Openstack resources are tried to be deleted despite the fact that enable_k8s_on_openstack is false
- Conditional import in "Create K8S deployment" does not work
- Binder Tokens should be randomly generated

- Add option to configure a github access token in `files/binderhub.yaml`
	```GitHubRepoProvider:
		access_token: dsdfdfasdfsdf978asdfasdfassadfasdf79
	```

## Getting started

### Running ansible directly

Set env var pointing to the settings: `export settings=private-settings.yaml`

Running all roles: `ansible-playbook --extra-vars "@$settings" create.yaml`

Run sub-elements only (e.g., only keycloak): `ansible-playbook --extra-vars "@$settings" --tags keycloak create.yaml`

### Using with k8s-on-openstack

1. Download/clone <https://github.com/infraly/k8s-on-openstack>.
2. Modify `run-demo-settings.yaml` to match your local setup.

Create: `./run-demo.sh create`

Destroy: `./run-demo.sh destroy`


## Prerequisites

- Ansible (tested with version 2.8.3)

# Authors

- Dennis Pfisterer <https://github.com/pfisterer>
