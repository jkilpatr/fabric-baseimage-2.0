# fabric-baseimage-2.0
An Ansible based build process for Fabric base image with platform agnostic design

## How it works
Like the current fabic-baseimage build process we kick off a docker container,
copy in some scripts in the dockerfile and then let it run it's course. The
difference is that we're using Ansible to help abstract individual distributions.
This script can use the same codepath to build Fabric on arbitrary architectures
and distributions using only a single config file per distro/arch to handle
the details.

`bootstrap.yml` templates out dockerfiles for every supported distribution, found
with simple bootstrapping scripts in `roles/bootstrap/defaults/main.yml` these
dockerfiles then bootstrap installing ansible and copy in this script folder.
This is done in parallel for all supported distributions, which then run the build
playbook locally.

The build playbook uses ansible facts to determine which config file to pull
from `group_vars` either distro-version-arch, distro-arch, distro-version, and
finally just disto, selected in that order depending on which matches first.

Right now we just build go and install package dependencies (using converged code
to actually do both) next up is protobuff stuff.

### To Run

Install Docker and Ansible on your host then run.

	ansible-playbook -i hosts bootstrap.yml


