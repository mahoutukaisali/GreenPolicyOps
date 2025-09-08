# GreenPolicy Ops

This repository provides a tutorial for **GreenPolicy Ops**, a concept that uses `ansible-policy` to enforce GreenOps principles in IT infrastructure automation.

With GreenPolicy Ops, you can define and enforce policies to:

- Prevent over-provisioning of VMs in the cloud
- Avoid unnecessary package installations to reduce storage usage
- Prohibit enabling unnecessary services to improve security and reduce energy consumption

---

## Example Usage

### 1. Restrict Unauthorized Package Installations

**Playbook:** [`install_packages.yml`](./install_packages.yml)  
This playbook tries to install an unauthorized package (`unauthorized-app`).  

**PolicyBook:** [`policies/check_package_policy.yml`](./policies/check_package_policy.yml)  
This policy allows installing only `mysql-server` and blocks other packages.  

Run the policy check:

```bash
ansible-policy -p install_packages.yml --policy-dir ./policies
```

Execution result: As you can see, Ansible Policy blocks to execute a playbook since it vaiolates the Policy `Check_for_package_name`.
```
TASK [Install Unauthorized App] install_packages.yml L10-15 ***********************************
... Check_for_package_name Not Validated
    The package unauthorized-app is not allowed. Allowed packages are one of ["mysql-server"]

-----------------------------------------------------------------------------------------------
SUMMARY
... Total files: 1, Validated: 0, Not Validated: 1

Violations detected in 1 task!
```

### 2. Restrict Inefficient EC2 Instance Types

**Playbook:** [`deploy_ec2_instance.yml`](./deploy_ec2_instance.yml)  
This playbook provisions an EC2 instance with a deprecated instance type (t1.micro).

**PolicyBook:** [`policies/check_package_policy.yml`](./policies/check_ec2_instance_policy.yml)  
This policy blocks provisioning of legacy/inefficient instance types (t1.micro, m1.small, m1.medium).

Run the policy check:
```
ansible-policy -p deploy_ec2_instance.yml --policy-dir ./policies
```

Execution result: As you can see, Ansible Policy blocks to execute a playbook since it vaiolates the Policy `Deny_inefficient_instance_types`.
```
TASK [Create EC2 instance] deploy_ec2_instance.yml L13-26 *****************************************************************************
... Deny_inefficient_instance_types Not Validated
    input.amazon.aws.ec2_instance

-------------------------------------------------------------------------------------------------------------------------------------------------
SUMMARY
... Total files: 1, Validated: 0, Not Validated: 1

Violations are detected! in 1 task
```