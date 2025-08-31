# GreenPolicy Ops

This repository provides a tutorial for GreenPolicy Ops, a concept that uses ansible-policy to enforce GreenOps principles in IT infrastructure automation.

For example, you can define and enforce policies to:

- Prevent over-provisioning of VMs in the cloud.

- Avoid unnecessary package installations to reduce storage usage.

- Prohibit enabling unnecessary services to improve security and reduce energy consumption.

## Example Usage

1. Restrict unnecessary package installations to save storage and maintain a secure system.

In this example, the policy allows installing only `mysql-server`, but the playbook attempts to install an unauthorized package `unauthorized-app`.

```
$ ansible-policy -p example_playbook.yml --policy-dir examples/check_project/policies
```

As a result, `ansible-policy` blocks the playbook execution because it violates the defined policy:

```
TASK [Install Unauthorized App] example_playbook.yml L10-15 ***************************************************************************
... Check_for_package_name Not Validated
    The package unauthorized-app is not allowed. Allowed packages are: ["mysql-server"]

-------------------------------------------------------------------------------------------------------------------------------------------------
SUMMARY
... Total files: 1, Validated: 0, Not Validated: 1

Violations detected in 1 task!
```