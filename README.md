# Dennis' K8S playbook

## Run the Ansible playbook directly

Create
- Run all roles: `ansible-playbook --extra-vars "@your-settings.yaml" create.yaml`
- Run sub-elements only (e.g., only keycloak): `ansible-playbook --extra-vars "@your-settings.yaml" --tags keycloak create.yaml`

Destroy
- Run `ansible-playbook --extra-vars "@your-settings.yaml" destroy.yaml`


## Open Issues

- Destroy does not check whether a component should have been installed in the first place (e.g., it tries to delete nginx ingress despite the fact that is shouldn't have been installed)
- Binder Tokens should be randomly generated

- Add option to configure a github access token in `files/binderhub.yaml`
	```GitHubRepoProvider:
		access_token: dsdfdsafasdfsdaffasdfassadfasdf7sdf
	```

## Prerequisites

- Ansible (tested with version 2.9.9)

## Some notes

- Ansible: [Using Variables - Variable precedence: Where should I put a variable?](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable)

# Authors

- Dennis Pfisterer <https://github.com/pfisterer>

