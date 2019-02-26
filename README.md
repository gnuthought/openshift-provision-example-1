# openshift-provision-example-1

This repository shows a simple declarative use of the
openshift-provision ansible role using the openshift-provision-manager
where the provision configuration is provide in a stand-alone ansible
vars file, `provision.yaml`. The config map for openshift-provision-manager
includes git repository configuration used to retrieve the `provision.yaml`.

## Installation

As a cluster administrator:

```
oc process -f install-template-admin.yaml \
 -p GIT_URL=https://github.com/gnuthought/openshift-provision-example-1.git \
 -p WEBHOOK_KEY=abcd1234 \
| oc create -f -
```

If installing as a non-administrator with the openshift-provision-manager
deployed in the namespace "example-provisioner":

```
PROVISIONER_NAMESPACE=example-provisioner
oc process  -f install-template-nonadmin.yaml \
 -p GIT_URL=https://github.com/gnuthought/openshift-provision-example-1.git \
 -p OPENSHIFT_PROVISION_NAMESPACE=$PROVISIONER_NAMESPACE \
 -p WEBHOOK_KEY=abcd1234 \
| oc create -f -
```

## Local Testing

An Ansible playbook, `playbook.yaml`, is provided for testing outside of
openshift-provision-manager. To use this playbook you will need to have the
[openshift-provision](https://github.com/gnuthought/ansible-role-openshift-provision)
ansible module installed locally. To run the playbook, first login to your
cluster with `oc login` and then run:

```
ansible-playbook playbook.yaml
```

To run in check mode and collect a change record:

```
ansible-playbook playbook.yaml --check -e openshift_provision_change_record=changes.yaml
```
