Still a draft.

# Quark Layer

- Don't trust blindly, verify cheaply. 
- As fundamental as a Quark.

## What is a Quark?

Here is what wikipedia has to answer:
<p align="center">
 <img  alt="quarks" src="assets/quarks_wikipedia.jpeg">
</p>


## Why do we need a proof verification and aggregation layer, now?
 
With EIP-4844 in action, data publishing costs for rollups will decrease significantly. Ethereum has a limited gas per block and ethereum is not optimised for all (zero knowledge) verifying systems. Hence proof verification on ethereum is costly. With increasing adoption of Zero-knowledge in web3, the next significant cost would be proof verification cost on the Ethereum.

Aggregated proofs allow to verify multiple proofs as a single proof, instead of verifying them individually. With proof aggregation, multiple proofs can be verified as a single proof on Ethereum, reducing gas costs for proof verification.

Proof generation would require a lot of time and have high hardware requirements. But proof verification will take a few milli seconds. 

## What is Quark Layer?

Quark layer is a fast decentralised Zero Knowledge proof(ZKP) verification & aggregation layer, leveraging [hypersdk](https://github.com/ava-labs/hypersdk), using snowman++ for consensus.

Zero Knowledge proofs are going to stay forever and influence human progress, just like cryptography securing the current internet. ZKPs are going to secure the internet in the next decade or lesser. 

Quark layer is going to support verification of any ZKPs, whether they are releated to web3 or completely general purpose. All the proofs verifyied by Quark layer are posted onto ethereum as an aggregated proof periodically.

## What usecases are unlocked with Quark Layer?

- Cheap and fast verification for Zero knowledge rollups/L2s and Zero knowledge bridges.

- Generic proof verification system to build any zkDapp.

- Interoperability for Zero knowledge rollups.

## What verification systems are supported now?

- [Succinctlabs sp1](https://github.com/succinctlabs/sp1)
- [Risczero's zkvm](https://github.com/risc0/risc0)
- [0xpolygonmiden](https://github.com/0xpolygonmiden)
- [Gnark](https://github.com/Consensys/gnark) (under development)

Support for verifiers to be added:

- [ ] [Halo2](https://github.com/zcash/halo2)
- [ ] [Plonky2](https://github.com/0xPolygonZero/plonky2)
- [ ] [Plonky3](https://github.com/Plonky3/Plonky3) 
- [ ] [ZkLLVM](https://github.com/NilFoundation/zkLLVM)
- [ ] [KZG]() 
and ...

## How does Quark Layer work?


<p align="center">
A simple overview of Quark Layer.
    <img alt="Quark Layer" src="./assets/quark-layer-w.jpg">   
</p>


<p align="center">
Verifiers to be supported by Quark Layer(non-exhasutive)
   <img alt="verifiers" src="./assets/sausage-server.png">
</p>

## Quick Start(Devnet)

#### Must have:

- Rust and Go lang installed.

#### Recomended tooling:

- It is good to have SP1, RiscZero ZKVM installed in your machine. 

#### Installation:

- Clone all four repos in the same directory:

```shell
git clone https://github.com/sausaging/hypersdk.git
git clone https://github.com/sausaging/hyper-pvzk.git
git clone https://github.com/sausaging/jugalbandi.git
git clone https://github.com/sausaging/example-proofs.git
```

#### Starting Devnet:

- Start rust server(jugalbandi) in 6 different terminals:

```shell
# Terminal 1: Rust Server Hub
cargo run

# Terminal 2: Rust Server instance with rust port 8081 and unint port 8086
PORT=8081 cargo run

# Terminal 3: Rust Server instance with rust port 8082 and unint port 8087
PORT=8082 cargo run

# Terminal 4: Rust Server instance with rust port 8083 and unint port 8088
PORT=8083 cargo run

# Terminal 5: Rust Server instance with rust port 8084 and unint port 8089
PORT=8084 cargo run

# Terminal 6: Rust Server instance with rust port 8085 and unint port 8090
PORT=8085 cargo run
```

- Start hyper-pvzk:

```shell

# Terminal 1:
./scripts/build.sh
./scripts/run.sh

# Terminal 2:
./build/morpheus-cli key import ed25519 demo.pk
./build/morpheus-cli chain import-anr

# Terminal 3:
./build/morpheus-cli chain watch

```

#### Verifing Proofs:

i) Register: Register proof related metadata.

```shell
# Run in Terminal 2 of Hyper-pvzk

./build/morpheus-cli testing register
```

ii) Register image: Register proof(s)/elf(s) with their keccak hashes.
```shell
./build/morpheus-cli testing register-image
```

iii) Broadcast: Broadcast proof(s)/elf(s) over p2p network.
```shell
./build/morpheus-cli testing broadcast
```

iv) Verify: Verify correctness of proofs.
```shell
./build/morpheus-cli testing verify
```

v) Verify status: Query if proof is valid or invalid.
```shell
./build/morpheus-cli testing verify-status
```