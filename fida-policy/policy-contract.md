# Policy Contract

The Fida Policy Contract serves as a central entity within the system. It operates as a contract that is parameterized by a single element, namely `policyId`, which stands as a unique identifier.

Below you can find a definition of its validator function with `PolicyId` as a first parameter.

```haskell
mkFidaContractValidator ::
  PolicyId ->
  FidaContractDatum ->
  FidaContractRedeemer ->
  ScriptContext ->
  Bool
```

Technically `PolicyId` is a wrapper around the currency symbol generated by the Fida Contract Minting Policy. It's important to note that the `policyId` unequivocally dictates Fida Policy Contract validator address.

```haskell
newtype PolicyId = PolicyId CurrencySymbol
```

To ensure the uniqueness of each `policyId`, the Fida Contract Minting Policy (defined with `mkFidaContractMintingPolicy` validator function) is configured with an unspent UTXO (`TxOutRef`) that must be spent during the minting process.&#x20;

```haskell
mkFidaContractMintingPolicy ::
  TxOutRef ->
  () ->
  ScriptContext ->
  Bool
```

The Fida Contract Minting Policy produces two distinct types of tokens:

* The policy token, distinguished by the `POLICY_ID` token name.
* The Fida card token, identified with the `CARD` token name.

These tokens are exclusively minted during the policy creation process and can be considered as non-fungible tokens (NFTs). The first token is utilized to label the UTxO storing the datum - `FidaContractDatum`that describes the Fida Policy definition. On the other hand, the second token facilitates the creation of multiple Fida Card Tokens, with the quantity determined by a policy parameter. This mechanism serves to distribute shares among investors during the selling process.

```haskell
data FidaContractDatum = FidaContractDatum
  { collateralAmount :: Integer
  , fidaCardValue    :: Integer
  , premiumAmount    :: Integer
  , policyHolder     :: Address
  , policyAuthority  :: PolicyAuthority
  , startDate        :: Maybe POSIXTime
  , paymentIntervals :: Integer
  , contractState    :: FidaContractState
  }
```

When creating a Fida Policy, the broker designates the quantity of Fida Card Tokens to be generated, along with the collateral amount in Ada, which functions as insurance. This information is documented in a FidaContractDatum, directly in the `collateralAmount` field, and indirectly in the `fidaCardValue` (calculated as `collateralAmount / numberOfCards`).

To ensure easy discoverability of the Fida Policy, a beacon token is deployed to a contract address during its creation. The beacon token is minted by a System Minting Policy (`mkSystemIdMintingPolicy`), which is configured with two parameters:

* `SystemGovernance` - representing the public key hash of the authorized wallet permitted to mint tokens.
* `SystemMagic`- serving as the system seed enabling the generation of diverse system families.

```haskell
mkSystemIdMintingPolicy ::
  SystemGovernance ->
  SystemMagic ->
  () ->
  ScriptContext ->
  Bool
  
newtype SystemId = SystemId CurrencySymbol

newtype SystemGovernance = SystemGovernance PubKeyHash
```

The currency symbol, generated by the System Minting Policy and encapsulated with `SystemId`, acts as a system identifier. Multiple Fida systems can operate simultaneously in the same location, with each one distinguished by a unique `SystemId`.

### FidaContractDatum Fields

#### collateralAmount

The `collateralAmount` represents the amount required to cover insurance policy collateral. This collateral is mandatory for policy issuance and can be paid by acquiring Fida cards - units of shares. The more share units you possess, the greater your earnings from the premium pool. The collateral amount is calculated as the total value (in L`ovelace`) of all Fida cards created for a policy.

#### fidaCardValue

The `fidaCardValue` is in `Lovelace` and signifies the worth of a single share unit.

#### premiumAmount

The `premiumAmount`, represents the price of a policy in `Lovelace`. This sum must be met by the policy owner, who wants to purchase the policy. The premium amount must be paid in advance before the policy transitions to the founding phase.

#### policyHolder

The `policyHolder` is the public key hash of the policy owner. To keep the policy active and eligible for claim issuance, the owner must pay the premium each month.

#### policyAuthority

```haskell
data PolicyAuthority
  = SingleSign PubKeyHash
  | AtLeastOneSign [PubKeyHash]
  | AllMustSign [PubKeyHash]
  | MajorityMustSign [PubKeyHash]
```

The `policyAuthority` refers to the entity responsible for making decisions regarding the policy life cycle. Various options are available, each based on the number of signatures required to make a decision:

* `SingleSign`: A single signature is needed from a specified wallet.
* `AtLeastOneSign`: At least one signature is required from a designated list of wallets.
* `AllMustSign`: All wallets on the list must provide signatures.
* `MajorityMustSign`: A majority of the listed wallets must provide signatures.

#### startDate

The `startDate` indicates the date from which the policy is considered to be at risk.

#### paymentIntervals

The `paymentIntervals` parameter governs the number of payments made to investors since the policy was initiated. Together with the `startDate`, these intervals determine the policy end date, marking when the policy concludes. Payments are made on a monthly basis at the month's end.

#### contractState

```haskell
data FidaContractState = Initialized | OnRisk
```

The `contractState`, along with the `startDate` and `paymentIntervals`, defines the current state of the contract. There are three possible states:

* `Initialized`: The contract has recently been created.
* `OnRisk`: The entire collateral has been provided (all Fida cards have been sold), and the `startDate` has been correctly initialized.
* `Expired`: The policy has reached its end date, meaning that the `contractState` must be `OnRisk`, and the current date must be after `startDate + paymentInterval`.

### Contract actions

```haskell
data FidaContractRedeemer
  = BuyFidaCard Integer 
  | PayPremium Integer 
  | Activate
```

TODO
