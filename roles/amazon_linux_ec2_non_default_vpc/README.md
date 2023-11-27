<p align="center">
  <img src="https://github.com/intel/terraform-intel-aws-vm/blob/main/images/logo-classicblue-800px.png?raw=true" alt="Intel Logo" width="250"/>
</p>

# Intel® Optimized Cloud Modules for Ansible

© Copyright 2023, Intel Corporation

## Ansible Intel AWS VM - Linux VM in Non Default VPC


This example creates an AWS EC2 instance on 4th Generation Intel® Xeon® Scalable Processor (Sapphire Rapids) on Linux Operating System in a non-default VPC. The non-default VPC id is needed to be passed in this module as a variable. It is configured to create the EC2 instance in US-East-1 region. The region is provided in variables.tf in this example folder.

This example also creates an EC2 key pair. It associates the public key with the EC2 instance. The private key is created in the local system where terraform apply is done. It also creates a new scurity group to open up the SSH port 22 to a specific IP CIDR block.

In this example, the tags Name, Owner and Duration are added to the EC2 instance when it is created.

## Architecture Diagram
<p align="center">
  <img src="https://github.com/intel/terraform-intel-aws-vm/blob/main/images/amazon-ec2-non-default-vpc.png?raw=true" alt="amazon-ec2-non-default-vpc" width="750"/>
</p>

## Installation of `amazon_linux_ec2_non_default_vpc` role

### Below are ways to `How to install and use it`

1. **Case 1:-** When user's needs can be met with the default configuration, and they want to install a collection 
   from Ansible Galaxy to the default location (as a third-party collection), it is recommended to use the following command:
    ```commandline
        ansible-galaxy  collection install <intel.ansible-intel-aws-vm>
    ```
   
2. **Case 2:-** When user's needs can't be met with the default configuration, wants to extend/modify existing configuration and flow, They can install collection using Ansible Galaxy in user's define location
   Use below approaches

   1.
       ```commandline
       ansible-galaxy  collection install -p <local path> <intel.ansible-intel-aws-vm>
       ```
       Note: collection will download collection, you can remove as per need

   2. Download source and Copy role directory to your Ansible boilerplate  from GitHub (Used to extended behavior of role)  
       ```commandline
       git clone https://github.com/OTCShare2/ansible-intel-aws-vm.git
       cd ansible-intel-aws-vm
       cp -r role/amazon_linux_ec2_non_default_vpc /<your project path>/
      


## Usage
Use playbook to run amazon_linux_ec2_non_default_vpc role as below
```yaml
---
- name: playbook using amazon linux ec2 
  hosts: localhost
  tasks:
    - name: running a role amazon linux ec2 non default vpc
      ansible.builtin.import_role:
        name: amazon_linux_ec2_non_default_vpc
      vars:
        ec2_state: present
```
Use below Command:
```commandline
ansible-playbook intel_amazon_linux_ec2_non_default_vpc.yml
```

## Run Ansible with Different State
#### State - present (terraform apply)
```yaml
- name: playbook using amazon linux ec2 
  hosts: localhost
  tasks:
    - name: running a role amazon linux ec2 non default vpc
      ansible.builtin.import_role:
        name: amazon_linux_ec2_non_default_vpc
      vars:
        ec2_state: present
```
Use below Command:
```commandline
ansible-playbook intel_amazon_linux_ec2_non_default_vpc.yml
```

#### State - absent (terraform destroy)
```yaml
- name: playbook using amazon linux ec2 
  hosts: localhost
  tasks:
    - name: running a role amazon linux ec2 non default vpc
      ansible.builtin.import_role:
        name: amazon_linux_ec2_non_default_vpc
      vars:
        ec2_state: absent
```
Use below Command:
```commandline
ansible-playbook intel_amazon_linux_ec2_non_default_vpc.yml
```
Note: **It deletes ec2 vm instance and key pair**.

Requirements
------------
| Name                                                                                | Version  |
|-------------------------------------------------------------------------------------|----------|
| <a name="requirement_terraform"></a> [Terraform](#requirement\_terraform)           | =1.5.7   |
| <a name="requirement_aws"></a> [AWS](#requirement\_aws)                             | ~> 4.36.0 |
| <a name="requirement_random"></a> [Random](#requirement\_random)                    | ~>3.4.3  |
| <a name="requirement_ansible_core"></a> [Ansible Core](#requirement\_ansible\_core) | ~>2.14.2 |
| <a name="requirement_ansible"></a> [Ansible](#requirement\_ansible)                 | ~>7.2.0-1 |
| <a name="requirement_boto3"></a> [boto3](#requirement\_boto3)                     | ~>1.29.0 |
| <a name="requirement_botocore"></a> [botocore](#requirement\_botocore)               | ~>1.32.0 |
| <a name="requirement_cryptography"></a> [cryptography](#requirement\_cryptography)       | ~>41.0.5 |

Note: Above role requires `Terraform` as we are executing terraform module [terraform-intel-aws-vm](<https://github.com/intel/terraform-intel-aws-vm/tree/main>) using Ansible module called [community.general.terraform](<https://docs.ansible.com/ansible/latest/collections/community/general/terraform_module.html>)

### Terraform Modules
| Name                                                                                  |
|---------------------------------------------------------------------------------------|
| [terraform-intel-aws-vm](<https://github.com/intel/terraform-intel-aws-vm/blob/main>) |

# Ansible
## Module State Inputs

| Name                                                                                                                  | Description                                                                                | Type     | Default   | Required |
|-----------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|----------|-----------|:--------:|
| <a name="input_ec2_state"></a> [ec2_state](#input\_auto\_major\_version\_upgrades) | It specifices ec2 state of given stage, choies: "planned", "present" ← (default), "absent" | `string` | `present` | no |


## VM Exposed Inputs 

| Name                                                                                            | Description                       | Type     | Default                 | Required |
|-------------------------------------------------------------------------------------------------|-----------------------------------|----------|-------------------------|:--------:|
| <a name="input_ami"></a> [vpc_id](#input_ami)                                    | vpc id to use for the instance | `string` | `vpc-0bf95158fd23f4e2e` | no |
| <a name="input_duration_count_tag"></a> [duration_count_tag](#input_duration_count_tag)                                           | Tag: Duration count               | `int`    | `2`                     | no |

## Security group Exposed Inputs

| Name                                                                                               | Description                                                                                                         | Type        | Default                           | Required |
|----------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|-------------|-----------------------------------|:--------:|
| <a name="input_security_group_name"></a> [security_group_name](#input_security_group_name)                      | Security group name is a unique identifier for a security group within a VPC                                        | `string`    | `ssh_security_group` with prefix  | no |
| <a name="input_security_group_rules"></a> [security_group_rules](#input_security_group_rules)                            | Security group rules are a set of firewall rules that control the inbound and outbound traffic for an EC2 instance. | `list(map)` | `[{"from_port": 22, "to_port": 22, "proto": "tcp", "cidr_ip": "0.0.0.0/0"}]`                            | no |


## VM Terraform Extended Inputs
 Below Input variables can be used to extend variables in role, Add or update variable in vars/main.yml file
### Usage

roles/amazon_linux_ec2_non_default_vpc/vars/main.yml

```yaml
subnet_id: "<subnet-xxxxxxxxxxx>"
```

roles/amazon_ec2_rhel_default_vpc/tasks/ec2_vm.yml
```yaml
---
- name: ec2 creation
  community.general.terraform:
    project_path: '{{ ec2_tf_module_download_path }}'
    state: '{{ ec2_state }}'
    force_init: true 
    complex_vars: true 
    variables:
      key_name: '{{ keypair_name }}'
      subnet_id: '{{ subnet_ids[0] }}'
      tags:
        "Name": "my-test-vm-{{ random_id }}"       
        "Owner": "OwnerName-{{ random_id }}"       
        "Duration": "{{ duration_count_tag }}"
  register: ec2_output
```

Use `duration_count_tag` in playbook 
```yaml
---
- name: playbook using amazon linux ec2
  hosts: localhost
  tasks:
    - name: running a role amazon linux ec2 non default vpc
      ansible.builtin.import_role:
        name: amazon_linux_ec2_non_default_vpc
      vars:
        ec2_state: present
        subnet_id: <subnet-XXXXXXXXX>
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_ami"></a> [ami](#input\_ami) | ID of AMI to use for the instance | `string` | `null` | no |
| <a name="input_ami_ssm_parameter"></a> [ami\_ssm\_parameter](#input\_ami\_ssm\_parameter) | SSM parameter name for the AMI ID. For Amazon Linux AMI SSM parameters see [reference](https://docs.aws.amazon.com/systems-manager/latest/userguide/parameter-store-public-parameters-ami.html). To find the latest Windows AMI using Systems Manager, use this [reference](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/finding-an-ami.html#finding-an-ami-parameter-store) | `string` | `"/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2"` | no |
| <a name="input_associate_public_ip_address"></a> [associate\_public\_ip\_address](#input\_associate\_public\_ip\_address) | Whether to associate a public IP address with an instance in a VPC | `bool` | `null` | no |
| <a name="input_availability_zone"></a> [availability\_zone](#input\_availability\_zone) | AZ to start the instance in | `string` | `null` | no |
| <a name="input_capacity_reservation_specification"></a> [capacity\_reservation\_specification](#input\_capacity\_reservation\_specification) | Describes an instance's Capacity Reservation targeting option | `any` | `{}` | no |
| <a name="input_cpu_core_count"></a> [cpu\_core\_count](#input\_cpu\_core\_count) | Sets the number of CPU cores for an instance. | `number` | `null` | no |
| <a name="input_cpu_credits"></a> [cpu\_credits](#input\_cpu\_credits) | The credit option for CPU usage (unlimited or standard) | `string` | `null` | no |
| <a name="input_cpu_threads_per_core"></a> [cpu\_threads\_per\_core](#input\_cpu\_threads\_per\_core) | Sets the number of CPU threads per core for an instance (has no effect unless cpu\_core\_count is also set). | `number` | `null` | no |
| <a name="input_create"></a> [create](#input\_create) | Whether to create an instance | `bool` | `true` | no |
| <a name="input_create_iam_instance_profile"></a> [create\_iam\_instance\_profile](#input\_create\_iam\_instance\_profile) | Determines whether an IAM instance profile is created or to use an existing IAM instance profile | `bool` | `false` | no |
| <a name="input_create_spot_instance"></a> [create\_spot\_instance](#input\_create\_spot\_instance) | Depicts if the instance is a spot instance | `bool` | `false` | no |
| <a name="input_disable_api_stop"></a> [disable\_api\_stop](#input\_disable\_api\_stop) | If true, enables EC2 Instance Stop Protection. | `bool` | `null` | no |
| <a name="input_disable_api_termination"></a> [disable\_api\_termination](#input\_disable\_api\_termination) | If true, enables EC2 Instance Termination Protection | `bool` | `null` | no |
| <a name="input_ebs_block_device"></a> [ebs\_block\_device](#input\_ebs\_block\_device) | Additional EBS block devices to attach to the instance | `list(map(string))` | `[]` | no |
| <a name="input_ebs_optimized"></a> [ebs\_optimized](#input\_ebs\_optimized) | If true, the launched EC2 instance will be EBS-optimized | `bool` | `null` | no |
| <a name="input_enable_volume_tags"></a> [enable\_volume\_tags](#input\_enable\_volume\_tags) | Whether to enable volume tags (if enabled it conflicts with root\_block\_device tags) | `bool` | `true` | no |
| <a name="input_enclave_options_enabled"></a> [enclave\_options\_enabled](#input\_enclave\_options\_enabled) | Whether Nitro Enclaves will be enabled on the instance. Defaults to `false` | `bool` | `null` | no |
| <a name="input_ephemeral_block_device"></a> [ephemeral\_block\_device](#input\_ephemeral\_block\_device) | Customize Ephemeral (also known as Instance Store) volumes on the instance | `list(map(string))` | `[]` | no |
| <a name="input_get_password_data"></a> [get\_password\_data](#input\_get\_password\_data) | If true, wait for password data to become available and retrieve it. | `bool` | `null` | no |
| <a name="input_hibernation"></a> [hibernation](#input\_hibernation) | If true, the launched EC2 instance will support hibernation | `bool` | `null` | no |
| <a name="input_host_id"></a> [host\_id](#input\_host\_id) | ID of a dedicated host that the instance will be assigned to. Use when an instance is to be launched on a specific dedicated host | `string` | `null` | no |
| <a name="input_iam_instance_profile"></a> [iam\_instance\_profile](#input\_iam\_instance\_profile) | IAM Instance Profile to launch the instance with. Specified as the name of the Instance Profile | `string` | `null` | no |
| <a name="input_iam_role_description"></a> [iam\_role\_description](#input\_iam\_role\_description) | Description of the role | `string` | `null` | no |
| <a name="input_iam_role_name"></a> [iam\_role\_name](#input\_iam\_role\_name) | Name to use on IAM role created | `string` | `null` | no |
| <a name="input_iam_role_path"></a> [iam\_role\_path](#input\_iam\_role\_path) | IAM role path | `string` | `null` | no |
| <a name="input_iam_role_permissions_boundary"></a> [iam\_role\_permissions\_boundary](#input\_iam\_role\_permissions\_boundary) | ARN of the policy that is used to set the permissions boundary for the IAM role | `string` | `null` | no |
| <a name="input_iam_role_policies"></a> [iam\_role\_policies](#input\_iam\_role\_policies) | Policies attached to the IAM role | `map(string)` | `{}` | no |
| <a name="input_iam_role_tags"></a> [iam\_role\_tags](#input\_iam\_role\_tags) | A map of additional tags to add to the IAM role/profile created | `map(string)` | `{}` | no |
| <a name="input_iam_role_use_name_prefix"></a> [iam\_role\_use\_name\_prefix](#input\_iam\_role\_use\_name\_prefix) | Determines whether the IAM role name (`iam_role_name` or `name`) is used as a prefix | `bool` | `true` | no |
| <a name="input_instance_initiated_shutdown_behavior"></a> [instance\_initiated\_shutdown\_behavior](#input\_instance\_initiated\_shutdown\_behavior) | Shutdown behavior for the instance. Amazon defaults this to stop for EBS-backed instances and terminate for instance-store instances. Cannot be set on instance-store instance | `string` | `null` | no |
| <a name="input_instance_type"></a> [instance\_type](#input\_instance\_type) | Instance SKU, see comments above for guidance | `string` | `"m7i.large"` | no |
| <a name="input_ipv6_address_count"></a> [ipv6\_address\_count](#input\_ipv6\_address\_count) | A number of IPv6 addresses to associate with the primary network interface. Amazon EC2 chooses the IPv6 addresses from the range of your subnet | `number` | `null` | no |
| <a name="input_ipv6_addresses"></a> [ipv6\_addresses](#input\_ipv6\_addresses) | Specify one or more IPv6 addresses from the range of the subnet to associate with the primary network interface | `list(string)` | `null` | no |
| <a name="input_key_name"></a> [key\_name](#input\_key\_name) | Key name of the Key Pair to use for the instance; which can be managed using the `aws_key_pair` resource | `string` | `null` | no |
| <a name="input_launch_template"></a> [launch\_template](#input\_launch\_template) | Specifies a Launch Template to configure the instance. Parameters configured on this resource will override the corresponding parameters in the Launch Template | `map(string)` | `null` | no |
| <a name="input_metadata_options"></a> [metadata\_options](#input\_metadata\_options) | Customize the metadata options of the instance | `map(string)` | `{}` | no |
| <a name="input_monitoring"></a> [monitoring](#input\_monitoring) | If true, the launched EC2 instance will have detailed monitoring enabled | `bool` | `false` | no |
| <a name="input_name"></a> [name](#input\_name) | Name to be used on EC2 instance created | `string` | `""` | no |
| <a name="input_network_interface"></a> [network\_interface](#input\_network\_interface) | Customize network interfaces to be attached at instance boot time | `list(map(string))` | `[]` | no |
| <a name="input_placement_group"></a> [placement\_group](#input\_placement\_group) | The Placement Group to start the instance in | `string` | `null` | no |
| <a name="input_private_ip"></a> [private\_ip](#input\_private\_ip) | Private IP address to associate with the instance in a VPC | `string` | `null` | no |
| <a name="input_root_block_device"></a> [root\_block\_device](#input\_root\_block\_device) | Customize details about the root block device of the instance. See Block Devices below for details | `list(any)` | `[]` | no |
| <a name="input_secondary_private_ips"></a> [secondary\_private\_ips](#input\_secondary\_private\_ips) | A list of secondary private IPv4 addresses to assign to the instance's primary network interface (eth0) in a VPC. Can only be assigned to the primary network interface (eth0) attached at instance creation, not a pre-existing network interface i.e. referenced in a `network_interface block` | `list(string)` | `null` | no |
| <a name="input_source_dest_check"></a> [source\_dest\_check](#input\_source\_dest\_check) | Controls if traffic is routed to the instance when the destination address does not match the instance. Used for NAT or VPNs. | `bool` | `true` | no |
| <a name="input_spot_block_duration_minutes"></a> [spot\_block\_duration\_minutes](#input\_spot\_block\_duration\_minutes) | The required duration for the Spot instances, in minutes. This value must be a multiple of 60 (60, 120, 180, 240, 300, or 360) | `number` | `null` | no |
| <a name="input_spot_instance_interruption_behavior"></a> [spot\_instance\_interruption\_behavior](#input\_spot\_instance\_interruption\_behavior) | Indicates Spot instance behavior when it is interrupted. Valid values are `terminate`, `stop`, or `hibernate` | `string` | `"terminate"` | no |
| <a name="input_spot_instance_type"></a> [spot\_instance\_type](#input\_spot\_instance\_type) | If set to one-time, after the instance is terminated, the spot request will be closed. Default `persistent` | `string` | `"one-time"` | no |
| <a name="input_spot_launch_group"></a> [spot\_launch\_group](#input\_spot\_launch\_group) | A launch group is a group of spot instances that launch together and terminate together. If left empty instances are launched and terminated individually | `string` | `null` | no |
| <a name="input_spot_price"></a> [spot\_price](#input\_spot\_price) | The maximum price to request on the spot market. Defaults to on-demand price | `string` | `null` | no |
| <a name="input_spot_valid_from"></a> [spot\_valid\_from](#input\_spot\_valid\_from) | The start date and time of the request, in UTC RFC3339 format(for example, YYYY-MM-DDTHH:MM:SSZ) | `string` | `null` | no |
| <a name="input_spot_valid_until"></a> [spot\_valid\_until](#input\_spot\_valid\_until) | The end date and time of the request, in UTC RFC3339 format(for example, YYYY-MM-DDTHH:MM:SSZ) | `string` | `null` | no |
| <a name="input_spot_wait_for_fulfillment"></a> [spot\_wait\_for\_fulfillment](#input\_spot\_wait\_for\_fulfillment) | If set, Terraform will wait for the Spot Request to be fulfilled, and will throw an error if the timeout of 10m is reached | `bool` | `null` | no |
| <a name="input_subnet_id"></a> [subnet\_id](#input\_subnet\_id) | The VPC Subnet ID to launch in | `string` | `null` | no |
| <a name="input_tags"></a> [tags](#input\_tags) | A mapping of tags to assign to the resource | `map(string)` | `{}` | no |
| <a name="input_tenancy"></a> [tenancy](#input\_tenancy) | The tenancy of the instance (if the instance is running in a VPC). Available values: default, dedicated, host. | `string` | `null` | no |
| <a name="input_timeouts"></a> [timeouts](#input\_timeouts) | Define maximum timeout for creating, updating, and deleting EC2 instance resources | `map(string)` | `{}` | no |
| <a name="input_user_data"></a> [user\_data](#input\_user\_data) | The user data to provide when launching the instance. Do not pass gzip-compressed data via this argument; see user\_data\_base64 instead. | `string` | `null` | no |
| <a name="input_user_data_base64"></a> [user\_data\_base64](#input\_user\_data\_base64) | Can be used instead of user\_data to pass base64-encoded binary data directly. Use this instead of user\_data whenever the value is not a valid UTF-8 string. For example, gzip-encoded user data must be base64-encoded and passed via this argument to avoid corruption. | `string` | `null` | no |
| <a name="input_user_data_replace_on_change"></a> [user\_data\_replace\_on\_change](#input\_user\_data\_replace\_on\_change) | When used in combination with user\_data or user\_data\_base64 will trigger a destroy and recreate when set to true. Defaults to false if not set. | `bool` | `false` | no |
| <a name="input_volume_tags"></a> [volume\_tags](#input\_volume\_tags) | A mapping of tags to assign to the devices created by the instance at launch time | `map(string)` | `{}` | no |
| <a name="input_vpc_security_group_ids"></a> [vpc\_security\_group\_ids](#input\_vpc\_security\_group\_ids) | A list of security group IDs to associate with | `list(string)` | `null` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_arn"></a> [arn](#output\_arn) | The ARN of the instance |
| <a name="output_capacity_reservation_specification"></a> [capacity\_reservation\_specification](#output\_capacity\_reservation\_specification) | Capacity reservation specification of the instance |
| <a name="output_iam_instance_profile_arn"></a> [iam\_instance\_profile\_arn](#output\_iam\_instance\_profile\_arn) | ARN assigned by AWS to the instance profile |
| <a name="output_iam_instance_profile_id"></a> [iam\_instance\_profile\_id](#output\_iam\_instance\_profile\_id) | Instance profile's ID |
| <a name="output_iam_instance_profile_unique"></a> [iam\_instance\_profile\_unique](#output\_iam\_instance\_profile\_unique) | Stable and unique string identifying the IAM instance profile |
| <a name="output_iam_role_arn"></a> [iam\_role\_arn](#output\_iam\_role\_arn) | The Amazon Resource Name (ARN) specifying the IAM role |
| <a name="output_iam_role_name"></a> [iam\_role\_name](#output\_iam\_role\_name) | The name of the IAM role |
| <a name="output_iam_role_unique_id"></a> [iam\_role\_unique\_id](#output\_iam\_role\_unique\_id) | Stable and unique string identifying the IAM role |
| <a name="output_id"></a> [id](#output\_id) | The ID of the instance |
| <a name="output_instance_state"></a> [instance\_state](#output\_instance\_state) | The state of the instance. One of: `pending`, `running`, `shutting-down`, `terminated`, `stopping`, `stopped` |
| <a name="output_ipv6_addresses"></a> [ipv6\_addresses](#output\_ipv6\_addresses) | The IPv6 address assigned to the instance, if applicable. |
| <a name="output_outpost_arn"></a> [outpost\_arn](#output\_outpost\_arn) | The ARN of the Outpost the instance is assigned to |
| <a name="output_password_data"></a> [password\_data](#output\_password\_data) | Base-64 encoded encrypted password data for the instance. Useful for getting the administrator password for instances running Microsoft Windows. This attribute is only exported if `get_password_data` is true |
| <a name="output_primary_network_interface_id"></a> [primary\_network\_interface\_id](#output\_primary\_network\_interface\_id) | The ID of the instance's primary network interface |
| <a name="output_private_dns"></a> [private\_dns](#output\_private\_dns) | The private DNS name assigned to the instance. Can only be used inside the Amazon EC2, and only available if you've enabled DNS hostnames for your VPC |
| <a name="output_private_ip"></a> [private\_ip](#output\_private\_ip) | The private IP address assigned to the instance. |
| <a name="output_public_dns"></a> [public\_dns](#output\_public\_dns) | The public DNS name assigned to the instance. For EC2-VPC, this is only available if you've enabled DNS hostnames for your VPC |
| <a name="output_public_ip"></a> [public\_ip](#output\_public\_ip) | The public IP address assigned to the instance, if applicable. NOTE: If you are using an aws\_eip with your instance, you should refer to the EIP's address directly and not use `public_ip` as this field will change after the EIP is attached |
| <a name="output_spot_bid_status"></a> [spot\_bid\_status](#output\_spot\_bid\_status) | The current bid status of the Spot Instance Request |
| <a name="output_spot_instance_id"></a> [spot\_instance\_id](#output\_spot\_instance\_id) | The Instance ID (if any) that is currently fulfilling the Spot Instance request |
| <a name="output_spot_request_state"></a> [spot\_request\_state](#output\_spot\_request\_state) | The current request state of the Spot Instance Request |
| <a name="output_tags_all"></a> [tags\_all](#output\_tags\_all) | A map of tags assigned to the resource, including those inherited from the provider default\_tags configuration block |
<!-- END_TF_DOCS -->