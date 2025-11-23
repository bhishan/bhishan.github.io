---
title: 'Python, Boto and AWS EC2'
date: 2018-09-16
permalink: /posts/2018/09/aws-ec2-boto/
tags:
  - AWS
  - Python
  - EC2
  - Boto
---

Most if not all software companies have adopted to cloud infrastructure and services. AWS in particular is very popular amongst all. The intentions of this post is to host a few examples on using boto to make use of one of the services available on AWS i.e EC2. It is more likely than not to have need of a mechanism to programatically fire up a few instances, shut them down, filter instances and send remote commands to it to say the least.

## Filter instances based on tag names from the AWS inventory

EC2 instances on AWS can have as many tag names key: value as required for purposes like identifying an instance or a set of instances. Also when the instance you are working on quite frequently needs to shut down and boot over again and you haven't implemented elastic IP, you are bound to changes in the public IP address. Although you could argue to use private IP to filter an instance, it isn't very effective when you have a lot of instances(>100).

### Boto2

```python
import boto.ec2

conn = boto.ec2.connect_to_region('us-east-1', aws_access_key_id='aws_access_id', aws_secret_access_key='aws_secret')
reservations = conn.get_all_instances(filters={'tagName' : 'value'})
public_ips = [each_instance.ip_address for r in reservations for each_instance in r.instances]
# each_instance.private_ip_address  to get the private ip address of the instance
```

### Boto3

```python
import boto3
session = boto3.session.Session(aws_access_key_id=aws_access_id,
                                aws_secret_access_key=aws_secret,
                                region_name='us-east-1')
 
ec2 = session.resource('ec2')
instances = ec2.instances.filter(
    Filters=[{'Name':'tag:purpose', 'Values':['intelligence']}
])
public_ips = [each_instance.public_ip_address for each_instance in instances]
# each_instance.private_ip_address to get the private ip address of the instance
```

## Boot/Shutdown an instance/instances from the AWS inventory

Using boto, you can boot/shutdown/terminate instances.

### Boto2

```python
def start_stop_terminate_instance(instance_ids, conn, action='start'):
    if action == 'start':
        conn.start_instances(instance_ids=instance_ids)
    elif action == 'stop':
        conn.stop_instances(instance_ids=instance_ids)
    elif action == 'terminate':
        conn.terminate_instances(instance_ids=ids)
```

### Boto3

```python
def start_stop_terminate_instance(instance_ids, conn, action='start'):
    if action == 'start':
        conn.instances.filter(InstanceIds=instance_ids).start()
    elif action == 'stop':
        conn.instances.filter(InstanceIds=instance_ids).stop()
    elif action == 'terminate':
        conn.instances.filter(InstanceIds=instance_ids).terminate()
```

## Create Instances based on various metrics

Boto makes use of the AWS APIs that also allows creating instances. An EC2 instance can have various properties. The most common is the type of the instance. Types are generally a grouping of instances based on metrics such as power, performance, bandwidth. Commonly used types for general purpose are t2, m4, m3. C5, c4, c3 are compute optimized instances. For a process/application more leaned towards in-memory activities, you'd use x1, r4, r3. There are other types too but the above mentioned are quite common in use. The other properties of an instance are instance id, the memory size (micro, nano, small, large, xlarge, 2xlarge, 4xlarge, 8xlarge, 10xlarge.), the key pair to make a secured connection to the instance, tag names, display names, security groups, attached storage id, etc. Using boto we can create an instance or multiple instances based on the above mentioned parameters.

### Boto2

```python
import boto.ec2
conn = boto.ec2.connect_to_region('us-east-1', aws_access_key_id='aws_access_id', aws_secret_access_key='aws_secret')
conn.run_instances(
    'ami-ag139jf',
    min_count=10, 
    max_count=100,
    key_name='myKey',
    instance_type='t2.small',
    security_groups=['sg-4512']
)
```

### Boto3

```python
import boto3
session = boto3.session.Session(aws_access_key_id='aws_access_id',
                                aws_secret_access_key='aws_secret',
                                region_name='us-east-1')
 
ec2 = session.resource('ec2')
ec2.create_instances(
    ImageId='ami-ag139jf', 
    MinCount=10, 
    MaxCount=100, 
    InstanceType='t2.small',
    KeyName='myKey',
    SecurityGroups=['sg-4512']
)
```

## Send remote commands to an EC2 instance

Paramiko can be used for connecting to a remote instance and sending commands to be executed and get the standard output/error to act accordingly.

```python
import paramiko

key = paramiko.RSAKey.from_private_key_file(path_to_pem_file)
client = paramiko.SSHClient()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())

# Connect to the instance
try:
    # using username, public ip address and the pem file, create connection to the instance
    client.connect(hostname=instance_ip, username="ubuntu", pkey=key)

    # Execute command remotely.
    stdin, stdout, stderr = client.exec_command("ls -l")
    print stdout.read()
    client.close()

except Exception, e:
    print e
```