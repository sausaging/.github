# Quark Layer

The quark layer is a decentralised proof verification and aggregation layer specifically built for verifying zero-knowledge proofs. Quark layer provides near-instant soft cryptoeconomic guarantees and delayed hard cryptographic guarantees on proof verification result(s). 


Read more about Quark layer here: [https://hackmd.io/@manojkogorle/quark-layer](https://hackmd.io/@manojkgorle/quark-layer)

## What verification systems are supported now?

- [Succinctlabs sp1](https://github.com/succinctlabs/sp1)
- [Risczero's zkvm](https://github.com/risc0/risc0)
- [0xpolygonmiden](https://github.com/0xpolygonmiden)
- [Jolt](https://github.com/a16z/jolt)
- [Gnark](https://github.com/Consensys/gnark) (under development)

Support for verifiers to be added:

- [ ] [Halo2](https://github.com/zcash/halo2)
- [ ] [Plonky2](https://github.com/0xPolygonZero/plonky2)
- [ ] [Plonky3](https://github.com/Plonky3/Plonky3) 
- [ ] [ZkLLVM](https://github.com/NilFoundation/zkLLVM)
- [ ] [KZG]() 
- [ ] [Arkworks]()
- [ ] [Nova]()
- [ ] [Spartan]()
- [ ] [Winterfell]()
and ...

## Quick Start

- Rust and Golang must be installed.
- Machine with >= 16 GiB of memory and cores >= 10.

### Installation:

- Clone all three repos in the same directory:

```shell
git clone https://github.com/sausaging/hyper-pvzk.git
git clone https://github.com/sausaging/jugalbandi.git
git clone https://github.com/sausaging/example-proofs.git
```

Hyper-PVZK contains the code behind the Quark layer's chain, while jugalbandi contains the hub and sausage instance that verifies proofs.

### Starting Devnet:

- Start hub and 5 sausage instances.

```shell
# Terminal 1: Rust Server Hub with port 8080
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

- Start hyper-pvzk, with 5 nodes.

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

i. Register proof-related metadata.

```shell
# Run in Terminal 2 of Hyper-pvzk

./build/morpheus-cli testing register
```
- The txID obtained will be the image ID for this proof instance.

ii. Register elf/proof with their hashes to their image ID.

- Image ID is the txID obtained in (i).
- Proof val type is 1 for ELF.
- Root hash is the root hash of elf. (feature not yet implemented, so any string works).

```shell
# Run in Terminal 2 of Hyper-pvzk

./build/morpheus-cli testing register-image
```

- Do the same for proof with the same image ID, but proof val type above 1.

iii. Broadcast elf/proof over the p2p network.

- File name is the path of the elf file. Here it will be `../example-proofs/sp1/riscv32im-succinct-zkvm-elf`
- Chunk index can be anything.(feature not yet implemented)

```shell
# Run in Terminal 2 of Hyper-pvzk

./build/morpheus-cli testing broadcast
```

- Do the same for proof, but with file path as `../example-proofs/sp1/proof-with-io.json`

iv. Verify the correctness of proofs.

- Verification type is 1 for SP1.
- Time-out blocks are the number of blocks in which a validator needs to cast their vote. (feature may change)

```shell
# Run in Terminal 2 of Hyper-pvzk

./build/morpheus-cli testing verify
```

v. Query if a proof is valid or invalid.

```shell
# Run in Terminal 2 of Hyper-pvzk

./build/morpheus-cli testing verify-status
```

- Follow the same procedure for JOLT, but with JOLT related data from `example-proofs`
- RiscZero & miden involves a few changes from the previous process.