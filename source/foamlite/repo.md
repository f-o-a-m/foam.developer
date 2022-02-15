# FOAM Lite Repo

The Real World Example has been implemented as a set of Solidity smart contracts, and powered by FOAM’s PureScript Web3 tooling, deployed with [Chanterelle](https://github.com/f-o-a-m/chanterelle/). The [GitHub repository](https://github.com/f-o-a-m/foam.lite/) contains the smart contracts, and various utilities written in PureScript to aid you in interacting with the smart contracts and exploring the FOAM Lite approach. A quickstart example is available in the README. We hope it serves as a useful reference to show you how the concepts demonstrated here come together to enable an Ethereum-of-Things application.

## System Requirements

- Either an x86/64 Linux OS, or an Intel or M1-based Mac
    - The build hasn’t been tested on Windows, let us know if it works for you!
    - The PureScript compiler and Spago package manager unfortunately do not have precompiled binaries for all ARM distributions, and as such NPM has trouble executing our build scripts on these platforms
- GNU Make
- Bash shell
- Node.js ≥ v10.
    - We recommend using [NVM](https://nvm.sh). Various libraries in the PureScript ecosystem occasionally have trouble keeping up with newest versions of Node, so we would recommend that use the `lts/dubnium` release.
- Docker
    - This is to allow the quickstart example to spin up an ephemeral blockchain with our [Cliquebait](https://hub.docker.com/r/foamspace/cliquebait) image for development purposes.

## Components and Configuration

Most of the software components can be configured with environment variables outlined here.

### FOAM Lite Explorer

A Purescript frontend showing RelayableNFT minting and transfer events queried from the blockchain. It serves as a demo of the kind of applications that are possible with the purescript-web3 libraries and tooling.

### Helper Scripts

A command line utility is built as part of `make bundle` and resides in `dist/helper.js`. Running `node dist/helper.js` will show all of the available options and flags, all of which are thoroughly documented in the script and most subcommands have a `--help` option that can be passed in. These serve to allow you to perform the specific steps needed to replicate the setup outlined in the Real World example.

### Relayer Emulation Server

A basic REST API server is build as part of `make bundle` and resides in `dist/server.js`. It is used in the quickstart to allow one to simulate the radio broadcast between a relayer and end node. Encoded messages can be submitted to the `/relay/validate` and `/relay/submit` endpoint.

Environment variables include:

- `NODE_URL`: The URL of the Ethereum node to use for blockchain interactions. This defaults to `http://localhost:8545`, which is convenient when running a local node or Cliquebait
- `FUNGIBLETOKEN_ADDRESS`: The contract address of the ERC20 token used for transaction fees. If not specified,  will attempt to read it from the Chanterelle deployment artifacts.
    - An alternative path to the deployment artifact can be specified with `FUNGIBLETOKEN_ARTIFACT`
- `RELAYABLENFT_ADDRESS`: The address of the RelayableNFT contract to interact with. If not specified, will attempt to read it from the Chanterelle deployment artifacts
    - An alternative path to the deployment artifact can be specified with  `RELAYABLENFT_ARTIFACT`
- `RELAYER_PRIVATE_KEY`: If the Ethereum node specified in `NODE_URL` does not have any unlocked accounts, you can specify the private key of the relayer account to submit relayed transactions with.
- `SERVER_HOST`: The address to listen for connections on
- `SERVER_PORT`: The port to listen for connections on

### LoRa Packet Forwarder

There is a small server meant to interface with the LoRa packet forwarder present in most off-the-shelf gateways built with `make bundle` and resides in `dist/lora.js`. This allows one to use real LoRa hardware to relay FOAM Lite messages to the blockchain. While more detailed documentation is coming soon, you can read a quickstart in `PI.md`.

Environment variables include:

- `NODE_URL`: The URL of the Ethereum node to use for blockchain interactions. This defaults to `http://localhost:8545`, which is convenient when running a local node or Cliquebait
- `FUNGIBLETOKEN_ADDRESS`: The contract address of the ERC20 token used for transaction fees. If not specified,  will attempt to read it from the Chanterelle deployment artifacts.
    - An alternative path to the deployment artifact can be specified with `FUNGIBLETOKEN_ARTIFACT`
- `RELAYABLENFT_ADDRESS`: The address of the RelayableNFT contract to interact with. If not specified, will attempt to read it from the Chanterelle deployment artifacts
    - An alternative path to the deployment artifact can be specified with  `RELAYABLENFT_ARTIFACT`
- `RELAYER_PRIVATE_KEY`: If the Ethereum node specified in `NODE_URL` does not have any unlocked accounts, you can specify the private key of the relayer account to submit relayed transactions with.
- `PACKET_FORWARDER_HOST`: The address to listen for connections from the LoRa gateway driver on.
- `PACKET_FORWARDER_PORT`: The address to listen for connections from the LoRa gateway driver on.

### End Node Firmware

Firmware for an end node that can transmit RelayableNFT messages implemented on an STM32F405 Feather Express and RFM95W LoRa Radio FeatherWing. This firmware can be considered the embedded equivalent of the helper scripts in some respects, and is intended to be studied and modified for other applications. See the `End Node` section for more details.