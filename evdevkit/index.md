# Evernode developer kit
Evernode uses HotPocket as its smart contract engine. Once you have developed a HotPocket smart contract on your PC, you can deploy it to Evernode using Evernode developer kit.

## Installation

### Prerequisites
Evernode developer kit requires you to install [NodeJs](https://nodejs.org/en/) on your development machine.

### Supports cross platform
This is an npm global package which supports both Linux and Windows
1. Install [prerequisites](#prerequisites).
2. Run the following command to install `evdevkit` on your machine.
    ```
    npm i evdevkit -g
    ```

_**NOTE:** In Linux platforms, installation requires root privileges. Hence, add `sudo` to the above command._

## Acquiring instance from Evernode
You can use the Evernode developer kit to acquire instances from Evernode. This will create you a fresh Evernode instance where you can deploy your smart contracts.
- You are required to set `EV_TENANT_SECRET` and `EV_USER_PRIVATE_KEY` [environment variables](#environment-variables) before acquiring the instance.
- To override Evernode instance's configuration You can create a [HotPocket configuration](../hotpocket/reference-configuration.md) file and set its path as `EV_INSTANCE_CONFIG_PATH` [environment variable](#environment-variables).
- Following command will create an instance in a random host. Given configurations will be populated if `EV_INSTANCE_CONFIG_PATH` is given.
```
evdevkit acquire
```
- This will return the acquired instance details.

## Deploying a contract to Evernode
You can deploy your already implemented [smart contract](../hotpocket/concepts.md#smart-contract) to the acquired Evernode instance.

### Creating the deployable contract package
You can package your contract using evdevkit.
- To override Evernode instance's contract configurations You can create a [Contract configuration](../hotpocket/reference-configuration.md#contract) file and set its path as `EV_CONTRACT_CONFIG_PATH` [environment variable](#environment-variables).
```
# evdevkit bundle <path to contract directory> <public key of the instance> <contract binary> -a <contract binary arguments>
evdevkit bundle $HOME/contract ed060a4aae0ec9183e4869e1490e908c9a9a3fd72816021c823ecd7d052e6e02d2 /usr/bin/node -a index.js
```
- This will return the path to the created contract bundle.

### Uploading a contract to Evernode
You can upload your contract bundle to the Evernode instance
- You are required to set `EV_USER_PRIVATE_KEY` [environment variable](#environment-variables) before upload the contract.
```
# evdevkit deploy <path to contract bundle> <IP of the instance> <User port of the instance>
evdevkit deploy $HOME/bundle/bundle.zip 45.76.238.97 26201
```

### Changing tenant info
In order to change the tenant info you need to override the [environment variables](#environment-variables):
1. Set the HP_CLUSTER_SIZE environment variable to the size you want.
    ```
    # Windows (command prompt)
    set EV_TENANT_SECRET=snmyH19JLWVaUJKtM4cNxTT6t38eA
    set EV_USER_PRIVATE_KEY=ed7b78ba4ffc9b7a55e427ff1ddb799ab1af59c6a9ab92e5f227815b04ab70e346831653e22c8293afac43694879c4083e1d7581b4326fcba423e3392e068028fe

    # Windows (powershell)
    $env:EV_TENANT_SECRET=snmyH19JLWVaUJKtM4cNxTT6t38eA
    $env:EV_USER_PRIVATE_KEY=ed7b78ba4ffc9b7a55e427ff1ddb799ab1af59c6a9ab92e5f227815b04ab70e346831653e22c8293afac43694879c4083e1d7581b4326fcba423e3392e068028fe

    # Linux (bash)
    export EV_TENANT_SECRET=snmyH19JLWVaUJKtM4cNxTT6t38eA
    export EV_USER_PRIVATE_KEY=ed7b78ba4ffc9b7a55e427ff1ddb799ab1af59c6a9ab92e5f227815b04ab70e346831653e22c8293afac43694879c4083e1d7581b4326fcba423e3392e068028fe
    ```
3. Now if you [acquire](#acquiring-instance-from-evernode) instance, Instance will be created using the from the new tenant.

## Advanced usage
```
# Do [acquire](#acquiring-instance-from-evernode), [bundle](#creating-the-deployable-contract-package) and [deploy](#uploading-a-contract-to-evernode) in one command
evdevkit acquire-and-deploy <path to contract directory> <contract binary> -a <contract binary arguments>

# List the active hosts in Evernode
evdevkit list

# See host info
evdevkit host <host XRPL address>

# Generate user key pair
evdevkit keygen

# Create Evernode cluster and deploy a smart contract
evdevkit cluster-create <cluster size> <path to contract directory> <contract binary> -a <contract binary arguments>
```

An example HotPocket configuration:
```json
{
    "contract": {
        "consensus": {
            "roundtime": 6000
        }
    },
    "mesh": {
        "peer_discovery": {
            "enabled": true,
            "interval": 10000
        }
    }
}
```

An example HotPocket contract configuration:
```json
{
    "consensus": {
        "roundtime": 2000
    }
}
```

_See [HotPocket configuration reference](/hotpocket/reference-configuration.md) for more details._

## Auditing hosts

You can use the Evernode developer kit to audit hosts. Auditing involves checking the instance acquisition, read request response and bootstrap status of hosts based on host addresses provided. A resultant matrix is generated at the end of an audit which consists of the audit status, bootstrap status, and response durations for each of the audited hosts.

- You are required to set `EV_TENANT_SECRET` and `EV_USER_PRIVATE_KEY` [environment variables](#environment-variables) before conducting an audit.

### Auditing multiple hosts

When auditing multiple hosts, you can use a text file containing a line-separated list of host XRPL addresses to define the hosts to be audited. The following command can be used to conduct an audit using an input file.

```
evdevkit audit -f <path to audit input file>
```

### Auditing a single host

When auditing a single host, you can use the following command to define the host address to be audited directly.
```
evdevkit audit -h <host XRPL address>
```

## Environment variables
`evdevkit` CLI supports the following environment variables:

| Name                    | Description                                                                    |
| ----------------------- | ------------------------------------------------------------------------------ |
| EV_TENANT_SECRET        | Tenant XRPL account secret.                                                    |
| EV_USER_PRIVATE_KEY     | Private key of the contract client (Can be generated using "evdevkit keygen"). |
| EV_INSTANCE_CONFIG_PATH | (Optional) Path of the locally created HotPocket instance configuration file.  |
| EV_CONTRACT_CONFIG_PATH | (Optional) Path of the locally created HotPocket contract configuration file.  |

## Updates
Run following command to update `evdevkit` to the latest version:
```
npm update evdevkit -g
```

## Uninstall
Run the following command to uninstall `evdevkit`:

```
npm uninstall evdevkit -g
```

_**NOTE:** In Linux platforms, you will need root privileges to execute updates and uninstallations. Hence, add `sudo` to the above commands._

## Reporting issues
Report issues [here](https://github.com/HotPocketDev/evernode-sdk/issues).
