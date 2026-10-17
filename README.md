# FoBV - Tour Ansible

David Williamson @ Varilink Computing Ltd

------

Ansible for deploying the Friends of Bennerley Viaduct virtual 3D tour.

## Contents

| File or Folder                 | Description                                                                                                                            |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| `group_vars/`<br>`host_vars/`  | Project specific Ansible variables.                                                                                                    |
| `playbooks/`                   | Playbooks provided by this repository - see under [Usage](#usage) below.                                                               |
| `roles/`                       | The Varilink [Libraries - Ansible Roles](https://github.com/varilink/libraries_ansible-roles) repository included here as a submodule. |
| `tools/`                       | Any of the Varilink Docker Compose based tools included here as submodules.                                                            |
| `.env`<br>`docker-compose.yml` | This repository's master Docker Compose configuration.                                                                                 |

## Usage

In order to use this repository, you must place the HTML files provided by the vendor who created the 3D tour for the Friends of Bennerley Viaduct in the `playbooks/html/` directory.

Then, to install the tour website on a host:

```sh
docker-compose run --rm ansible-playbook ./playbooks/deploy-site.yml
```

Note the process of synchronising the vendors HTML files to the host is time consuming, have patience and it will complete eventually.

As well as the `deploy-site.yml` playbook, this repository provides the following other playbooks all of which can be run as above:

`add-dns-mask.yml`

Adds a DNS entry for an instance of the 3D tour website to the office Dnsmasq service. Useful for testing out the deployment to a new host prior to migrating from an old host.

`copy-certificate.yml`

Copies the LetsEncrypt certificate for the 3D tour website over from an old host to a new host. Again, useful when testing on a new host ahead of migrating from an old host.

`get-certificate.yml`

Uses LetsEncrypt to issue a new certificate for the 3D tour website on a enw host. Usually follows switching over from old host to new host to setup automated certificate renewals on the new host. Must be followed by running `deploy-site.yml` on the new host to update the Nginx site configuration.

`remove-dns-mask.yml`

Removes the DNS entry for an instance of the 3D tour website that was added using `add-dns-mask.yml`. This reverts the office DNS so that what it resolves for that instance of the 3D tour website is the same as what everybody else gets.
