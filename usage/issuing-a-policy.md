---
description: Before any Risk can be transferred a policy must be issued.
---

# Issuing a Policy

In the process of issuing a Fida Policy, there are three main actors involved: the policy broker, the policy owner (who have purchased the insurance), and the investors (who provide collateral for the policy). The policy broker registers a  policy on-chain and makes it discoverable.&#x20;

The process of issuing a policy (e.g. with the assistance of the provided `cli)` proceeds as follows: Clients initially approach the policy broker with the intention of purchasing a policy. They provide input parameters such as an insurance amount (which the broker translates into collateral amount), wallet public key hash, contract duration, and so forth. Subsequently, they must agree on the cost the client must pay for the policy. Once agreed upon, the policy broker (using the `cli`or `web app`) creates a policy and provides an address (Fida Policy Contract Address) where the client must send the agreed amount (premium amount). Upon receiving the payment at the correct address, the broker triggers a state transition of the policy contract, thereby making the client the policy owner, and the policy becomes available for the funding phase.

Investors, utilizing the `cli`or `web app`, can search the blockchain for a Fida Policy available for funding and contribute to the funding by purchasing a Fida Card Policy through the issuance of an appropriate transaction. On the other hand, the policy owner can monitor the progress of the funding phase using the `cli` or `web app` by querying the blockchain for specific policy information or obtaining a list of all policies in which they are the owner. Once the policy is fully funded, the policy broker, upon contacting the client, triggers a Fida Policy state transition, and the policy becomes "on risk" from that point onward.

You can continue reading in more detail about the [lifecycle](../fida-policy/policy-life-cycle/) of a [policy](../fida-policy/policy-contract.md), or the [lifecycle](../investors/investor-contract-life-cycle/) of an [investors commitment](../investors/investor-contract.md). Additionally you can [continue reading about the `cli`](command-line-interface.md)`.`
