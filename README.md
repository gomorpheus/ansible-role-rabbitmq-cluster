# Ansible Role RabbitMQ Cluster

## Description

Creates a RabbitMQ Cluster for use with n-node HA deployments of the Morpheus Cloud Management Platform.

## Supported Versions

Currently this is pinned to the 3.7 branch of RabbitMQ.  Ansible doesn't work properly with 3.8 as of yet.

## Role Variables

|Variable|Required|Default|Description|
|--------|--------|-------|-----------|
|rabbitmq_group|Y|rabbitmq|Name of the inventory group containing the rabbit hosts|
|rabbitmq_plugins|Y|rabbitmq_stomp, rabbitmq_management|List of plugins to enable.  Morpheus needs management and stomp.|
|rabbitmq_vhost_name|Y|defaultvhost|RabbitMQ vhost name|
|rabbitmq_mgmt_user|Y|adminuser|RabbitMQ admin user|
|rabbitmq_mgmt_password|Y|adminpass|RabbitMQ admin password|
|rabbitmq_user|Y|rabbitmquser|RabbitMQ user|
|rabbitmq_password|Y|rabbitmqpassword|RabbitMQ user password|
