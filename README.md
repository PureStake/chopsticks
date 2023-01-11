# Chopsticks

Create parallel reality of your Substrate network.

## Quick Start

Fork Acala mainnet: `yarn dlx @acala-network/chopsticks dev --endpoint=wss://acala-rpc-2.aca-api.network/ws`

It is recommended to use config file. You can check [configs](configs/) for examples.

You can run a test node with config with `yarn dlx @acala-network/chopsticks dev --config=<config_file_path>`

## Install

Make sure you have setup Rust environment (>= 1.64).

- Clone repository with submodules ([smoldot](https://github.com/paritytech/smoldot))
  - `git clone --recurse-submodules https://github.com/AcalaNetwork/chopsticks.git && cd chopsticks`
- Install deps
  - `yarn`
- Build wasm
  - `yarn build-wasm`

## Run

- Replay latest block
  - `yarn start run-block --endpoint=wss://acala-rpc-2.aca-api.network/ws`
  - This will replay the last block and print out the changed storages
  - Use option `--block` to replay certain block hash
  - Use option `--output-path=<file_path>` to print out JSON file
  - Use option `--html` to generate storage diff preview (add `--open` to automatically open file)

- Dry run extrinsic, same as `run-block`, example:
  - `yarn start dry-run --config=configs/mandala.yml --html --open --extrinsic 0x51028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d01a4143d076116f80a4074ed7acc90f9e1f9e2db54603900be53145a6ef5faa333f2614e687a06a9c886a909d77f3115b1a9d989afdc9fd73e5dca941bc690ae8a0000000a00008eaf04151687736326c9fea17e25fc5287613693c912909cb226aa4794f26a481b000000a1edccce1bc2d3`

- Run a test node
  - `yarn start dev --endpoint=wss://acala-rpc-2.aca-api.network/ws`
  - You have a test node running at `ws://localhost:8000`
  - You can use [Polkadot.js Apps](https://polkadot.js.org/apps/) to connect to this node
  - Submit any transaction to produce a new block in the in parallel reality
  - (Optional) Pre-define/override storage using option `--import-storage=storage.[json/yaml]`. See example storage below.
  ```json5
  {
    "Sudo": {
      "Key": "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY"
    },
    "TechnicalCommittee": {
      "Members": ["5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY"]
    },
    "Tokens": {
      "Accounts": [
        [
          ["5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY", { "token": "KAR" }],
          {
            "free": 1000000000000000,
          }
        ]
      ]
    }
  }
  ```
- Run Kusama fork
  - Edit configs/kusama.yml if needed. (e.g. update the block number)
  - `yarn start dev --config=configs/kusama.yml`

- Setup XCM multichain (UpwardMessages not yet supported)
**_NOTE:_** You can also connect multiple parachains without a relaychain
```bash
yarn start xcm --relaychain=configs/kusama.yml --parachain=configs/karura.yml --parachain=configs/statemine.yml
```
