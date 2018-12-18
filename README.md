# openstack-provider

To create a new __Openstack stack__, run the playbook `create.yml`
		
This playbook needs 2 parameters:

- **input_dir**: folder which contains yaml files with Openstack parameters
- **output_dir**: folder which contains yaml files with Openstack parameters


> If **proxy** is needed to connect to AWS, export **proxy** parameters in environment variables (http\_proxy, https\_proxy, no\_proxy).


__Example__:

```
ansible-playbook create.yml -e "input_dir=/opt/input output_dir=/opt/output" -i inventory/
```

__Openstack configuration file example__:

```yaml
instances: 3
params:
  prefix_names: test
  image: myImage
  private_network: private_net
  public_network: public_net
  security_groups: 
  - sec01
  - sec02
  flavor: docker.small
  availability_zone: region-1
  dns_zone: myzone.com.
  metadata:
    param1: test
    param2: value2
    param3:
      ssparam: value
  user_data: |
    #cloud-config    
    chpasswd:
      list: |
        ubuntu:test01
      expire: False
    system_info:
      default_user:
        name: ubuntu
        plain_text_passwd: 'test01'
        home: /home/ubuntu
        shell: /bin/bash
        lock_passwd: false
        sudo: ALL=(ALL) NOPASSWD:ALL
volumes:
- path: /data
  params:
    volume_type: CEPH
    volume_size: 10
labels:
  label1: value1
```

## Openstack Parameters for Instances

All variables which can be overridden as in table below (under **params** tag).

| Name           | Required | Default Value | Description                        |
| -------------- | -------- | ------------- | -----------------------------------|
| `image` |  True |   | Instance type to use for the instance |
| `private_network` |  True |  | Private network to use for the instance |
| `public_network` |   |  | Public network to use for the instance |
| `flavor` |   |  | Flavor id to use for the instance |
| `security_groups` |  |  | List of existing security groups |
| `availability_zone` |  |  | Availability zone to use |
| `dns_zone` |  |  | Dns zone to use to create a Designate Record|
| `metadata` |  |  | Metadata for each instance |
| `user_data` |  |  | User config for each instance |
| `prefix_names` |  |  | Prefix name for each instance |


## Openstack parameters for Volumes (Cinder)

| Name           | Required | Default Value | Description                        |
| -------------- | -------- | ------------- | -----------------------------------|
| `name` |  True |   | Mount point's name |
| `params/volume_type` |  True |  | Volume type |
| `params/volume_size` |  True |  | Volume size |


## Openstack parameters for Labels (metadata) 

| Name           | Required | Default Value | Description                        |
| -------------- | -------- | ------------- | -----------------------------------|
| `key` |  True |   | Label's key / value |
