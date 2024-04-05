Still a draft.

# Quark Layer

- Don't trust blindly, verify cheaply. 
- As fundamental as a Quark.

## What is a Quark?

Here is what wikipedia has to answer:
<p align="center">
 <img  alt="quarks" src="assets/quarks_wikipedia.jpeg">
</p>

## Why do we need a proof verification and aggregation layer now?
 
With EIP-4844 in action, data publishing costs for rollups will decrease significantly. Ethereum has a limited gas per block, and not all (zero knowledge) verifying systems are optimized by Ethereum. Hence, proof verification on Ethereum is costly. With the increasing adoption of Zero-knowledge in web3, the significant cost would be the proof verification cost on Ethereum.

Aggregated proofs allow the verification of multiple proofs as a single proof instead of verifying them individually. With proof aggregation, multiple proofs can be verified as a single proof on Ethereum, reducing gas costs for proof verification.

Proof generation would require a lot of time and have high hardware requirements. But proof verification will take a few milliseconds. 

## What is a Quark Layer?

Quark layer is a fast decentralised Zero Knowledge proof(ZKP) verification & aggregation layer, leveraging [hypersdk](https://github.com/ava-labs/hypersdk), using snowman++ for consensus.

Zero Knowledge proofs will forever influence human progress, like cryptography securing the internet. ZKPs are going to secure the internet in the next decade or less. 

The quark layer will support verification of any ZKPs related to web3 or completely general purpose. All the proofs verified by the Quark layer are posted periodically on Ethereum as aggregated proof.

## What use cases are unlocked with Quark Layer?

- Cheap and fast verification for Zero-knowledge rollups/L2s and Zero-knowledge bridges.

- Generic proof verification system to build any zkDapp/zkRollup.

- Interoperability for Zero-knowledge rollups.

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

<div align="center">
    <img alt="Quark Layer" src="./assets/quark-layer-user-overview.png">  
   Simple overview of Quark Layer. 
</div>
<br>
<div align="center">
    <img alt="Quark Layer validator overview" src="./assets/quark-layer-validator-view.png">
    Validator overview of Quark Layer.
</div>
<br>
<div align="center">
   <img alt="verifiers" src="./assets/sausage-server.png">
   Verifiers to be supported by Quark Layer(non-exhasutive)
</div>

## MVP Quick Start

#### Must have:

- Rust and Go lang installed.

#### Recomended tooling:

- It is good to have SP1, RiscZero ZKVM installed in your machine. 

#### Installation:

- Clone all three repos in the same directory:

```shell
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

#### Verifing SP1 Proofs:

i) Register proof-related metadata.

```shell
# Run in Terminal 2 of Hyper-pvzk

./build/morpheus-cli testing register
```
- The txID obtained will be the image ID for this proof instance.

ii) Register elf/proof with their hashes to their image ID.

- Image ID is the txID obtained in (i).
- Proof val type is 1 for ELF.
- Root hash is the root hash of elf. (feature not yet implemented, so any string works).

```shell
# Run in Terminal 2 of Hyper-pvzk

./build/morpheus-cli testing register-image
```

- Do the same for proof with the same image ID, but proof val type above 1.

iii) Broadcast elf/proof over the p2p network.

- File name is the path of the elf file. Here it will be `../example-proofs/sp1/riscv32im-succinct-zkvm-elf`
- Chunk index can be anything.(feature not yet implemented)

```shell
# Run in Terminal 2 of Hyper-pvzk

./build/morpheus-cli testing broadcast
```

- Do the same for proof, but with file path as `../example-proofs/sp1/proof-with-io.json`

iv) Verify the correctness of proofs.

- Verification type is 1 for SP1.
- Time-out blocks are the number of blocks in which a validator needs to cast their vote. (feature may change)

```shell
# Run in Terminal 2 of Hyper-pvzk

./build/morpheus-cli testing verify
```

v) Query if a proof is valid or invalid.

```shell
# Run in Terminal 2 of Hyper-pvzk

./build/morpheus-cli testing verify-status
```

## TODO

- Add support for the planned verification system.
- Complete all the necessary checks in the vm and modify cli.
- Optimize on Miden Verifier integrations.
- Research on p2p data transfer methods. Broadcast list is cool, but what if some node is offline? Do we need to store proofs with all validators before verification or store them at a % of validators?
- Research on reward mechanisms for proper data transmission in p2p and for spamming p2p.
- Build example projects to demonstrate things using the proof verification layer.
- Test with various block times, block sizes, and timeouts.
- Improve DevX for better integrations.
- Noir??