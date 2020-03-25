# Ansible Role RabbitMQ Cluster

## Description

Creates a RabbitMQ Cluster for use with n-node HA deployments of the Morpheus Cloud Management Platform.

## Supported Versions

Currently this is pinned to the 3.7 branch of RabbitMQ.  Ansible doesn't work properly with 3.8 as of yet.

## Role Variables

|Variable|Default|Description|
|--------|-------|-----------|
|rabbitmq_group|rabbitmq|Name of the inventory group containing the rabbit hosts|
|rabbitmq_plugins_enable_management|false|Flag to enable RabbitMQ Managment|
|rabbitmq_plugins_enable_tls_cert_auth|false|Flag to enable rabbitmq_auth_mechanism_ssl plugin for certificate auth|
|rabbitmq_vhost_name|defaultvhost|RabbitMQ vhost name|
|rabbitmq_mgmt_user|adminuser|RabbitMQ admin user|
|rabbitmq_mgmt_password|adminpass|RabbitMQ admin password|
|rabbitmq_user|rabbitmquser|RabbitMQ user|
|rabbitmq_password|rabbitmqpassword|RabbitMQ user password|
|rabbitmq_linux_user|rabbitmq|Linux user for rabbitmq|
|rabbitmq_centos_package|rabbitmq-server-3.7*|CentOS package variable.  Pinned to 3.7* for now until an Ansible bug is fixed|
|rabbitmq_tls_enable|false|Enable TLS|
|rabbitmq_tls_port|5671|TLS Port|
|rabbitmq_tls_verify|verify_none|TLS verify option|
|rabbitmq_tls_verify_cluster|verify_none|Inter-cluster TLS verify option|
|rabbitmq_tls_fail_if_no_peer_cert|true|Disallows connections to non-TLS cluster members|
|rabbitmq_tls_disable_non_tls|true|Disables non-TLS ports to RabbitMQ nodes|
|rabbitmq_tls_mgmt_nontls_localhost_only|true|Disable management plugin non-TLS access to any host but localhost|
|rabbitmq_tls_mgmt_ssl_port|15671|Management SSL Port|
|rabbitmq_tls_use_custom_cert|false|Use a custom SSL certificate|
|rabbitmq_tls_generate_self_signed_cert|false|Generate a self signed certificate|
|rabbitmq_tls_crt_path|/etc/pki/tls/certs|Certificate path on target|
|rabbitmq_tls_priv_path|/etc/pki/tls/private|Private Key path on target|
|rabbitmq_tls_self_signed_csr_path|/etc/pki/tls/certs|Self signed certificate CSR path on target|
|rabbitmq_tls_self_signed_basename|rabbitmq_selfsigned|Base name of self signed certificate files|
|rabbitmq_tls_self_signed_crt|"{{ rabbitmq_tls_crt_path }}/{{ rabbitmq_tls_self_signed_basename }}.crt"|Self signed certificate path on target|
|rabbitmq_tls_self_signed_csr|"{{ rabbitmq_tls_self_signed_csr_path }}/{{ rabbitmq_tls_self_signed_basename }}.csr"|Self signed CSR path on target|
|rabbitmq_tls_self_signed_priv|"{{ rabbitmq_tls_priv_path }}/{{ rabbitmq_tls_self_signed_basename }}.pem"|Self signed private key path on target|
|rabbitmq_tls_default_cabundle|/etc/pki/tls/certs/ca-bundle.crt|Default CA bundle location|
|rabbitmq_tls_self_signed_cabundle|"{{ rabbitmq_tls_default_cabundle }}"|Self Signed specific CA bundle|
|rabbitmq_tls_custom_cabundle_filename|""|Custom CA bundle filename|
|rabbitmq_tls_custom_cert_filename|""|Custom certificate filename|
|rabbitmq_tls_custom_priv_filename|""|Custom private key filename|
|rabbitmq_tls_cabundle|""|Default for generated fact within the role|
|rabbitmq_tls_cert|""|Default for generated fact within the role|
|rabbitmq_tls_private_key|""|Default for generated fact within the role|
|rabbitmq_tls_client_cert_only|false|Toggle for certificate authentication, if true it will disable PLAIN authentication and allow only EXTERNAL|

## RabbitMQ Installation

The intended purpose of this role is to create a RabbitMQ cluster for use with Morpheus.  As such, it is tailored to the needs of that platform.

The two use cases of RabbitMQ with HA deployments of Morpheus are: Internal and External.  Internal means running on the UI nodes themselves, and external means running on different instances altogether.

## TLS Configuration
### Self Signed Certificate
This role will generate a self-signed cert for each node if enabled using the following variables.

- `rabbitmq_enable_tls: true`
- `rabbitmq_generate_self_signed_cert: true`

### Custom TLS Certificate
This role will use a supplied certificate.  

### Custom TLS Certificate with Certificate Authentication
This role can configure RabbitMQ to use TLS certificate authentication to AMQP.  The following vars must be set:
* `rabbitmq_tls_verify: verify_peer`
* `rabbitmq_plugins_enable_tls_cert_auth: true`
* `rabbitmq_tls_client_cert_only: true`

You must supply CA, server certs and keys, see `molecule/tls_cluster_byo_cert` for an example.  This example used a self signed certificate with the supplied CA and client certs.

## Examples
Check out the examples in `molecule/*`.  The current examples, with example files and variables, are in the integrated tests.

## Morpheus Integration

Supply Morpheus with the credentials in `rabbitmq_user` and `rabbitmq_password` and the vhost `rabbitmq_vhost` in order to use the cluster.

## Testing

There are 3 different test scenarios in molecule.  Make sure you have molecule, docker and the `docker` python module and run `molecule test --all` to test.

