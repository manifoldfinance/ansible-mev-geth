# Ansible Role: MEV-Geth

### Description
Ansible role that will install, configure and runs MEV-Geth

### Table of Contents
  - [Supported Platforms](#supported-platforms)
  - [Dependencies](#dependencies)
  - [Role Variables](#role-variables)
  - [Example Playbook](#example-playbook)
  - [License](#license)
  - [Author Information](#author-information)

### Supported Platforms
```
* MacOS
* Debian
* Ubuntu
* Redhat(CentOS/Fedora)
* Amazon
```

### Dependencies

* Go 1.13.x or greater

### Role Variables:

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file. By and large these variables are configuration options.

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `geth_build_from_source` | ___unset___ |  When set to `true`, MEV-Geth is build from git sources. See also `geth_git_repo` and `geth_git_commit` |
| `geth_version` | ___unset___ |  __REQUIRED__ if `geth_build_from_source` is false. Version of MEV-Geth to install and run. All available versions are listed on our MEV-Geth [solutions](#) page |
| `geth_git_repo` | https://github.com/flashbots/mev-geth.git | The URL to use when cloning mev-geth sources. Only necessary when `geth_build_from_source` is `true`. |
| `geth_git_commit` | master | The git commit to use when building MEV-Geth from source. Can be a branchname, commit hash, or anything that's legal to be used as an argument to `git checkout`. Only used if `geth_build_from_source` is `true`. |
| `geth_user` | mev-geth | MEV-Geth user |
| `geth_group` | mev-geth | MEV-Geth group |
| `geth_download_url` | $<SOURCE_DOWNLOAD> | The download tar.gz file used. You can use this if you need to retrieve mev-geth from a custom location such as an internal repository. |
| `geth_install_dir` | /opt/mev-geth | Path to install to  |
| `geth_config_dir` | /etc/mev-geth | Path for default configuration |
| `geth_node_private_key_file` | "" | Path for node private key, if supplied. This needs to include the node key file name and path like so `/home/me/me_node/myPrivateKey`. If not supplied MEV-Geth will create one automatically |
| `geth_data_dir` | /opt/mev-geth/data | Path for data directory|
| `geth_log_dir` | /var/log/mev-geth | Path for logs |
| `geth_log4j_config_file` | "" | Absolute path for a custom log4j config file |
| `geth_profile_file` | /etc/profile.d/mev-geth-path.sh | Path to allow loading MEV-Geth into the system PATH |
| `geth_managed_service` | true | Enables a systemd service (or launchd if on Darwin) |
| `geth_launchd_dir` | /Library/LaunchAgents | The default launchd directory  |
| `geth_systemd_dir` | /etc/systemd/system/ | The default systemd directory |
| `geth_systemd_state` | restarted | The default option for the systemd service state |
| `geth_identity` | ___unset___  | Configuration of Identity in the Client ID |
| `geth_host_ip` | "" | The host IP that MEV-Geth uses for the P2P network. This specifies the host on which P2P listens |
| `geth_default_ip` | "{{ default(ansible_host) \| default('127.0.0.1') }}" | The fallback default for `geth_host_ip` |
| `geth_max_peers` | ___unset___ | The maximum number of P2P connections you can establish |
| `geth_network` | mainnet | The network that this node will join. Other values are 'ropsten', 'rinkeby', 'goerli', 'dev' and 'custom' |
| `geth_genesis_path` | ___unset___ | The path to the genesis file, only valid when `geth_network` is `custom` |
| `geth_sync_mode` | FAST | Specifies the synchronization mode. Other values are 'FULL' |
| `geth_log_level` | INFO | The log level to use. Other log levels are 'OFF', 'FATAL', 'WARN', 'INFO', 'DEBUG', 'TRACE', 'ALL' |
| `geth_p2p_port` | 30303 | Specifies the P2P listening ports (UDP and TCP). Ports must be exposed appropriately |
| `geth_min_gas` | 1000 | The minimum price that a transaction offers for it to be included in a mined block |
| `geth_miner_enabled` | false | Enables mining when the node is started |
| `geth_miner_coinbase` | 0x | Account to which mining rewards are paid |
| `geth_miner_extra_data` | "" | A hex string representing the 32 bytes to be included in the extra data field of a mined block. |
| `geth_rpc_http_enabled` | true | Enabled the HTTP JSON-RPC service |
| `geth_rpc_http_host` | 0.0.0.0 | Specifies the host on which HTTP JSON-RPC listens |
| `geth_rpc_http_port` | 8545 | Specifies the port on which HTTP JSON-RPC listens |
| `geth_rpc_http_api` | ["ADMIN","DEBUG","NET","ETH","MINER","WEB3"] | Comma-separated APIs to enable on the HTTP JSON-RPC channel. When you use this option, the `geth_rpc_http_enabled` option must also be enabled |
| `geth_rpc_http_cors_origins` | ["all"] | Comma separated origin domain URLs for CORS validation |
| `geth_rpc_ws_enabled` | true | Enabled the WebSockets service |
| `geth_rpc_ws_api` | ["NET", "ETH", "WEB3"] | Comma-separated APIs to enable on the HTTP JSON-RPC channel. When you use this option, the `geth_rpc_ws_enabled` option must also be enabled |
| `geth_rpc_ws_host` | 0.0.0.0 | Specifies the host on which WebSockets listens |
| `geth_rpc_ws_port` | 8546 | Specifies Websockets JSON-RPC listening port (TCP). Port must be exposed appropriately |
| `geth_graphql_http_enabled` | true | Enabled the HTTP JSON-RPC service |
| `geth_graphql_http_host` | 0.0.0.0 | Specifies the host on which HTTP JSON-RPC listens |
| `geth_graphql_http_port` | 8547 | Specifies the port on which HTTP JSON-RPC listens |
| `geth_graphql_http_cors_origins` | ["all"] | Comma separated origin domain URLs for CORS validation |
| `geth_metrics_host` | 0.0.0.0 | Specifies the host on which Prometheus accesses MEV-Geth metrics. The metrics server respects the `geth_whitelist` option |
| `geth_metrics_port` | 9545 | Specifies the port on which Prometheus accesses MEV-Geth metrics |
| `geth_bootnodes` | [] | List of comma-separated enode URLs for P2P discovery bootstrap. When connecting to MainNet or public testnets, the default is a predefined list of enode URLs |
| `geth_host_whitelist` | `["*"]` | Comma-separated list of hostnames to allow access to the JSON-RPC API. By default, access from localhost and 127.0.0.1 is accepted. |
| `geth_permissions_accounts_config_file` | ___unset___ | Path to the [local accounts permissioning file](#) |
| `geth_permissions_nodes_config_file` | ___unset___ | Path to the [local nodes permissioning file](#) |
| `geth_permissions_accounts_contract_address` | ___unset___ | The contract address for onchain accounts permissioning |
| `geth_permissions_nodes_contract_address` | ___unset___ | The contract address for onchain nodes permissioning |
| `geth_cmdline_args` | "" | Command line args that are passed in as overrides |
| `geth_env_opts` | [] | DEPRECIATED: Settings passed to the JVM through `JVM_OPTS` environment variable. eg: `[-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005]s` |
| `geth_privacy_enabled` | false | Enable privacy |
| `geth_privacy_url` | "" | URL to contact Orion on including port eg: `http://localhost:8888` |
| `geth_privacy_public_key_file` | ""| Path to Orion public key |
| `geth_privacy_marker_tx_signing_key_file` | "" | Path of the private key file used to sign Privacy Marker Transactions. If you do not specify this option, MEV-Geth signs each transaction with a different randomly generated key. |

### Example Playbook

1. Install via github

```
ansible-galaxy install git+https://github.com/manifoldfinance/ansible-role-mev-geth.git
```

Create a requirements.yml with the following:
Replace `x.y.z` below with the version you would like to use 
```
---
- hosts: localhost
  connection: local
  force_handlers: True

  roles:
  - role: ansible-role-mev-geth
    vars:
      geth_version: x.y.z

```

Run with ansible-playbook:
```
ansible-playbook -v /path/to/requirements.yml
```


### License

Apache


### License 

Apache-2.0
