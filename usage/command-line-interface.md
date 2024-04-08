---
description: Using Fida from the Command Line
---

# Command Line Interface

For the purpose of deploying policy contracts on the network we plan to develop software in the form of a `cli` (command-line interface).  The fida `cli` has two parts: an `on-chain` component written in `Haskell` and an `off-chain` component written in `Purescript`.

Of the three main actors involved: the policy broker, policy owner, and investors;  the policy broker, possessing special permissions, can, for instance, trigger state transitions of the policy or mark it as discoverable on the chain. All actors, when using the `cli`, are required to provide access to their wallets in the form of script input parameters. The `cli` is capable of crafting a transaction, signing it using the provided key, and sending it to the chain.

The commands on the `cli` match the labeled transition methods which would move the policy contracts and investor contracts.

