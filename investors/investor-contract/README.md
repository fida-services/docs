# Investor Contract

The Investor contract serves as the gateway for investors into the Fida ecosystem. It acts as a repository for all the collateral required by investors when purchasing Fida cards, as well as a storage for their premium pool. All interactions between Fida and investors are facilitated through this component. Investors can utilize this contract to buy or sell Fida cards and collect premiums. Investor have the opportunity to earn Ada passively by staking all the Ada deposited on his script.&#x20;

Technically, the Investor contract is parameterized by two elements: the investor's public key hash (`PubKeyHash`) and the Fida `SystemId`. The Fida `SystemId` serves as an identifier for the Fida system where the investor has initialized their Investor contract. It is presumed that multiple Fida systems may operate concurrently on the blockchain, whether for technical or business reasons.

```haskell
mkInvestorContractValidator ::
  PubKeyHash ->
  SystemId ->
  InvestorDatum ->
  InvestorRedeemer ->
  ScriptContext ->
  Bool
```
