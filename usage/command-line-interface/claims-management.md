---
description: Stuff
---

# Claims Management

# Table of Contents

1.  [Environment](#orgad82e5a)
    1.  [testnet      : preview](#orge606235)
    2.  [cardano-node : 10.1.2](#orgc443b1c)
    3.  [kupo         : 2.9.0](#orgff64af5)
    4.  [ogmios       : 6.7.0](#orgaa4770c)
2.  [Fida cli](#orgf7273c6)
3.  [Wallets](#org99ca505)
    1.  [Public key hash-es](#org749ccda)
        1.  [broker-1](#org33de268)
        2.  [policy-holder](#orgd884c62)
4.  [Insurance policies examples](#orgbb264f0)
    1.  [policy-1 (claim accepted)](#org8d0b91b)
        1.  [create, pay for policy, buy fida cards, complete funding & issue a claim](#org3915d42)
    2.  [policy-2 (claim issued)](#orgbe74c70)
        1.  [create, pay for policy, buy fida cards, complete funding & issue a claim](#org65d5bcf)
    3.  [policy-3 (short lived)](#org9e2d53f)
        1.  [create, pay for policy, buy fida cards, complete funding & issue a claim](#orga5ef24c)
5.  [New Functionalities in Milestone 3](#org9ecb840)
    1.  [fail-claim               Fail claim (policy authority)](#orgbc45d23)
    2.  [claim-premium            Claim premium (investor)](#org90a70fe)
    3.  [pay-for-claim            Pay for claim optionally using own funds (investor)](#org2d436cc)
    4.  [expire-policy            Expire policy (everyone)](#org0262dfa)
    5.  [unlock-collateral        Unlock investor collateral (investor)](#org7c684c2)
6.  [Transactions](#org6d431c6)



<a id="orgad82e5a"></a>

# Environment


<a id="orge606235"></a>

## testnet      : preview


<a id="orgc443b1c"></a>

## cardano-node : 10.1.2


<a id="orgff64af5"></a>

## kupo         : 2.9.0


<a id="orgaa4770c"></a>

## ogmios       : 6.7.0


<a id="orgf7273c6"></a>

# Fida cli

    bash-5.2$ node ./dist/cli-index.js --help
    
    Version: v0.0.3
    
    Usage: cli-index.js COMMAND
    
    Available options:
      -h,--help                Show this help text
    
    Available commands:
      make-new-policy          Create new Fida Policy
      get-policy               Get policy by id
      buy-fida-cards           Buy Fida cards
      pay-for-a-policy         Pay for a insurance policy with a given wallet
      sell-fida-cards          Sell Fida cards
      complete-funding         Complete funding (going OnRisk)
      issue-claim              Issue Fida insurance policy claim
      accept-claim             Accept active claim (broker)
      close-claim              Close claim (policy holder)
      fail-claim               Fail claim (policy authority)
      claim-premium            Claim premium (investor)
      pay-for-claim            Pay for claim optionally using own funds
                               (investor/everyone after deadline)
      unlock-collateral        Unlock investor collateral (investor)
      expire-policy            Expire policy (everyone)
    
    Fida https://fida.finance


<a id="org99ca505"></a>

# Wallets

    ~/git/fida/dev-tools/cli-toolbox ./wallet.sh -l
    
    Id                                      Name            Desc
    ==============================================================
    19f589a1-091d-44a4-b4d2-c82fb4e4c9ab    broker-1
    9557307d-8930-423c-a2c3-750dd058435f    investor-1
    e06ec2bf-2da8-4157-8825-34f547180941    policy-holder


<a id="org749ccda"></a>

## Public key hash-es


<a id="org33de268"></a>

### broker-1

    cardano-cli latest address key-hash --payment-verification-key-file /home/ssledz/etc/.cardano-wallets/preview/19f589a1-091d-44a4-b4d2-c82fb4e4c9ab/payment.vkey
    
    81e5819399c73eeef29db727fe36a2a0a2d4555955af7f3f51fc9a05


<a id="orgd884c62"></a>

### policy-holder

    cardano-cli latest address key-hash --payment-verification-key-file /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/payment.vkey
    
    fac649cff91290b8222b0594c121daf1f7d8d1ae3f8026f8dc15626f


<a id="orgbb264f0"></a>

# Insurance policies examples


<a id="org8d0b91b"></a>

## policy-1 (claim accepted)

    node dist/cli-index.js get-policy --insurance-id 16d097c673e7dc956baa45be6f8778e79a0bef4df1d44cbd98da7f93
    
    Insurance Id : 16d097c673e7dc956baa45be6f8778e79a0bef4df1d44cbd98da7f93
    Address      : addr_test1wzkwkselegljkwgayeh9xk0t6ml6tdg9jzsy8yln0eqqqrqu7k6vh
    Payment      : Paid
    Insurance    :
        - Collateral:                   100000000
        - Fida card value:              50000000
        - Fida card quantity:           2
        - Premium amount:               20000000
        - Installments:
              - interval: 1 minutes  | amount: 2500000
              - interval: 5 minutes  | amount: 2500000
              - interval: 10 minutes | amount: 2500000
              - interval: end        | amount: 2500000
        - Insurance period:             4 hours
        - Policy holder:                (PubKeyHash (Ed25519KeyHash (unsafePartial $ fromJust $ fromBytes (hexToByteArrayUnsafe "fac649cff91290b8222b0594c121daf1f7d8d1ae3f8026f8dc15626f"))))
        - Policy authority:             (SingleSign (PubKeyHash (Ed25519KeyHash (unsafePartial $ fromJust $ fromBytes (hexToByteArrayUnsafe "81e5819399c73eeef29db727fe36a2a0a2d4555955af7f3f51fc9a05")))))
        - State:                        OnRisk
        - Funding deadline:             11-13-2024 12:00
        - Claim accepted amount:        0
        - Claim time to live:           7 days
        - Claim time to pay:            5 days
        - Start date:                   11-11-2024 08:30
        - Current claim:
             - id:       (UUID a5f4ca66-d5a4-4425-a337-9105854484f1)
             - created:  11-11-2024 08:30
             - amount:   10000000
             - reason:   I need money !
             - accepted: true
    Piggy banks  :
         - (1)     addr_test1wrnf4f349ptqyh2a20dt0xx67x4de0hmlmgtuxeel92eepcql9ahp | collateral: 51180940 / 50000000 | premium: 10000000 / 10000000 r: 0 | claims: []
         - (2)     addr_test1wpck3xvmfdw6vj8068adgm9arxaqlqpvukxwz7te2zcqa5slugyea | collateral: 51180940 / 50000000 | premium: 10000000 / 10000000 r: 0 | claims: []


<a id="org3915d42"></a>

### create, pay for policy, buy fida cards, complete funding & issue a claim

    node dist/cli-index.js make-new-policy \
       --input-utxo '4b5425dd6fccc608015c86eb0d986d39da5d64f164655b4a4c16248d1bc4e1ef#1' \
       --collateral-amount=100000000 \
       --insurance-price 20000000 \
       --insurance-period "4 hours" \
       --funding-deadline 2024-11-13 \
       --policy-holder-pkh 'fac649cff91290b8222b0594c121daf1f7d8d1ae3f8026f8dc15626f' \
       --fida-card-quantity 2 \
       --single-sign-governance-authority '81e5819399c73eeef29db727fe36a2a0a2d4555955af7f3f51fc9a05' \
       --installment-intervals '1 minutes,5 minutes,10 minutes'
    
    node dist/cli-index.js pay-for-a-policy \
      --insurance-id 16d097c673e7dc956baa45be6f8778e79a0bef4df1d44cbd98da7f93 \
      -k /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/stake.skey
    
    node dist/cli-index.js buy-fida-cards \
      --insurance-id 16d097c673e7dc956baa45be6f8778e79a0bef4df1d44cbd98da7f93 \
      -k /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/stake.skey \
      --fida-cards 1,2
    
    node dist/cli-index.js complete-funding \
      --insurance-id 16d097c673e7dc956baa45be6f8778e79a0bef4df1d44cbd98da7f93 \
      -k /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/stake.skey
    
    node dist/cli-index.js issue-claim \
      --insurance-id 16d097c673e7dc956baa45be6f8778e79a0bef4df1d44cbd98da7f93 \
      --claim-amount 10000000 \
      --claim-reason 'I need money !' \
      -k /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/stake.skey
    
    node dist/cli-index.js accept-claim \
      --insurance-id 16d097c673e7dc956baa45be6f8778e79a0bef4df1d44cbd98da7f93


<a id="orgbe74c70"></a>

## policy-2 (claim issued)

    node dist/cli-index.js get-policy --insurance-id 6e7694000d40f34634ed8a31353adf4c6206d64a9167f9a6cb5f3ceb
    
    Insurance Id : 6e7694000d40f34634ed8a31353adf4c6206d64a9167f9a6cb5f3ceb
    Address      : addr_test1wpmck4ph3dkngvu5u9nna68a70u0sdhv6avp8es6rqugcjqtmhkqt
    Payment      : Paid
    Insurance    :
        - Collateral:                   100000000
        - Fida card value:              50000000
        - Fida card quantity:           2
        - Premium amount:               20000000
        - Installments:
              - interval: 1 minutes  | amount: 2500000
              - interval: 5 minutes  | amount: 2500000
              - interval: 10 minutes | amount: 2500000
              - interval: end        | amount: 2500000
        - Insurance period:             4 hours
        - Policy holder:                (PubKeyHash (Ed25519KeyHash (unsafePartial $ fromJust $ fromBytes (hexToByteArrayUnsafe "fac649cff91290b8222b0594c121daf1f7d8d1ae3f8026f8dc15626f"))))
        - Policy authority:             (SingleSign (PubKeyHash (Ed25519KeyHash (unsafePartial $ fromJust $ fromBytes (hexToByteArrayUnsafe "81e5819399c73eeef29db727fe36a2a0a2d4555955af7f3f51fc9a05")))))
        - State:                        OnRisk
        - Funding deadline:             11-13-2024 12:00
        - Claim accepted amount:        0
        - Claim time to live:           7 days
        - Claim time to pay:            5 days
        - Start date:                   11-11-2024 08:39
        - Current claim:
             - id:       (UUID 004189b8-856a-4e50-b311-ff59dfe9720f)
             - created:  11-11-2024 08:41
             - amount:   10000000
             - reason:   I need money !
             - accepted: false
    Piggy banks  :
         - (1)     addr_test1wqju8awhepp3apu6kjjvfupf050y30sf5wmnyqprz7w7ktghf2g33 | collateral: 51180940 / 50000000 | premium: 10000000 / 10000000 r: 0 | claims: []
         - (2)     addr_test1wrfyqwrslg6g4m5quxvqsat9ql9j9zsc0et7rfne9wsfj5qtnp2v0 | collateral: 51180940 / 50000000 | premium: 10000000 / 10000000 r: 0 | claims: []


<a id="org65d5bcf"></a>

### create, pay for policy, buy fida cards, complete funding & issue a claim

    node dist/cli-index.js make-new-policy \
       --input-utxo '2e84cd86194ee4331e8581541fcfeb8c47b7a347d406a2dbf576ff212a10ca93#1' \
       --collateral-amount=100000000 \
       --insurance-price 20000000 \
       --insurance-period "4 hours" \
       --funding-deadline 2024-11-13 \
       --policy-holder-pkh 'fac649cff91290b8222b0594c121daf1f7d8d1ae3f8026f8dc15626f' \
       --fida-card-quantity 2 \
       --single-sign-governance-authority '81e5819399c73eeef29db727fe36a2a0a2d4555955af7f3f51fc9a05' \
       --installment-intervals '1 minutes,5 minutes,10 minutes'
    
    node dist/cli-index.js pay-for-a-policy \
      --insurance-id 6e7694000d40f34634ed8a31353adf4c6206d64a9167f9a6cb5f3ceb \
      -k /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/stake.skey
    
    node dist/cli-index.js buy-fida-cards \
      --insurance-id 6e7694000d40f34634ed8a31353adf4c6206d64a9167f9a6cb5f3ceb \
      -k /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/stake.skey \
      --fida-cards 1,2
    
    node dist/cli-index.js complete-funding \
      --insurance-id 6e7694000d40f34634ed8a31353adf4c6206d64a9167f9a6cb5f3ceb \
      -k /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/stake.skey
    
    node dist/cli-index.js issue-claim \
      --insurance-id 6e7694000d40f34634ed8a31353adf4c6206d64a9167f9a6cb5f3ceb \
      --claim-amount 10000000 \
      --claim-reason 'I need money !' \
      -k /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/stake.skey


<a id="org9e2d53f"></a>

## policy-3 (short lived)

    node dist/cli-index.js get-policy --insurance-id 945af5bec28946abf49b1fe646135a7bcad6c83e5b699ca78b0a630f
    
    Insurance Id : 945af5bec28946abf49b1fe646135a7bcad6c83e5b699ca78b0a630f
    Address      : addr_test1wzgukz4qrve4fvt7epp62lzwaepareynmfffn55kvvhk46suyfhx7
    Payment      : Paid
    Insurance    :
        - Collateral:                   100000000
        - Fida card value:              50000000
        - Fida card quantity:           2
        - Premium amount:               20000000
        - Installments:
              - interval: 90 days    | amount: 2500000
              - interval: 90 days    | amount: 2500000
              - interval: 90 days    | amount: 2500000
              - interval: end        | amount: 2500000
        - Insurance period:             15 minutes
        - Policy holder:                (PubKeyHash (Ed25519KeyHash (unsafePartial $ fromJust $ fromBytes (hexToByteArrayUnsafe "fac649cff91290b8222b0594c121daf1f7d8d1ae3f8026f8dc15626f"))))
        - Policy authority:             (SingleSign (PubKeyHash (Ed25519KeyHash (unsafePartial $ fromJust $ fromBytes (hexToByteArrayUnsafe "81e5819399c73eeef29db727fe36a2a0a2d4555955af7f3f51fc9a05")))))
        - State:                        OnRisk
        - Funding deadline:             11-12-2024 12:00
        - Claim accepted amount:        0
        - Claim time to live:           7 days
        - Claim time to pay:            5 days
        - Start date:                   11-11-2024 08:22
        - Current claim:                No claim
    Piggy banks  :
         - (1)     addr_test1wrleu372t320vcpe2x65x52c0nyv3l7cyl564her9rayg0c8kvsr9 | collateral: 51180940 / 50000000 | premium: 10000000 / 10000000 r: 0 | claims: []
         - (2)     addr_test1wpazhc7gtmglgh00ygh23jk54wrrr0n5ezy7dd68t3f2l7cfak7cw | collateral: 51180940 / 50000000 | premium: 10000000 / 10000000 r: 0 | claims: []


<a id="orga5ef24c"></a>

### create, pay for policy, buy fida cards, complete funding & issue a claim

    node dist/cli-index.js make-new-policy \
       --input-utxo '9612a86dd61b68dc5097e22216b2cee6c6cddeb274ad9ccc7cf33e0e101d824e#1' \
       --collateral-amount=100000000 \
       --insurance-price 20000000 \
       --insurance-period "15 minutes" \
       --funding-deadline 2024-11-12 \
       --policy-holder-pkh 'fac649cff91290b8222b0594c121daf1f7d8d1ae3f8026f8dc15626f' \
       --fida-card-quantity 2 \
       --single-sign-governance-authority '81e5819399c73eeef29db727fe36a2a0a2d4555955af7f3f51fc9a05'
    
    node dist/cli-index.js pay-for-a-policy \
      --insurance-id 945af5bec28946abf49b1fe646135a7bcad6c83e5b699ca78b0a630f \
      -k /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/stake.skey
    
    node dist/cli-index.js buy-fida-cards \
      --insurance-id 945af5bec28946abf49b1fe646135a7bcad6c83e5b699ca78b0a630f \
      -k /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/stake.skey \
      --fida-cards 1,2
    
    node dist/cli-index.js complete-funding \
      --insurance-id 945af5bec28946abf49b1fe646135a7bcad6c83e5b699ca78b0a630f \
      -k /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/e06ec2bf-2da8-4157-8825-34f547180941/stake.skey


<a id="org9ecb840"></a>

# New Functionalities in Milestone 3


<a id="orgbc45d23"></a>

## fail-claim               Fail claim (policy authority)

    node dist/cli-index.js fail-claim \
      --insurance-id 6e7694000d40f34634ed8a31353adf4c6206d64a9167f9a6cb5f3ceb
    
    node dist/cli-index.js get-policy --insurance-id 6e7694000d40f34634ed8a31353adf4c6206d64a9167f9a6cb5f3ceb


<a id="org90a70fe"></a>

## claim-premium            Claim premium (investor)

    node dist/cli-index.js claim-premium \
      --insurance-id 6e7694000d40f34634ed8a31353adf4c6206d64a9167f9a6cb5f3ceb \
      -k /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/stake.skey \
      --fida-cards 1,2 \
      --premium-amount 2500000
    
    node dist/cli-index.js get-policy --insurance-id 6e7694000d40f34634ed8a31353adf4c6206d64a9167f9a6cb5f3ceb


<a id="org2d436cc"></a>

## pay-for-claim            Pay for claim optionally using own funds (investor)

    node dist/cli-index.js pay-for-claim \
      --insurance-id 16d097c673e7dc956baa45be6f8778e79a0bef4df1d44cbd98da7f93 \
      -k /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/stake.skey \
      --fida-cards 1,2 \
      --own-funds-amount 1000000
    
    node dist/cli-index.js get-policy --insurance-id 16d097c673e7dc956baa45be6f8778e79a0bef4df1d44cbd98da7f93


<a id="org0262dfa"></a>

## expire-policy            Expire policy (everyone)

    node dist/cli-index.js expire-policy \
      --insurance-id 945af5bec28946abf49b1fe646135a7bcad6c83e5b699ca78b0a630f \
      -k /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/stake.skey
    
    node dist/cli-index.js get-policy --insurance-id 945af5bec28946abf49b1fe646135a7bcad6c83e5b699ca78b0a630f


<a id="org7c684c2"></a>

## unlock-collateral        Unlock investor collateral (investor)

    node dist/cli-index.js unlock-collateral \
      --insurance-id 945af5bec28946abf49b1fe646135a7bcad6c83e5b699ca78b0a630f \
      -k /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/payment.skey \
      -K /home/ssledz/etc/.cardano-wallets/preview/9557307d-8930-423c-a2c3-750dd058435f/stake.skey \
      --fida-cards 1,2
    
    node dist/cli-index.js get-policy --insurance-id 945af5bec28946abf49b1fe646135a7bcad6c83e5b699ca78b0a630f


<a id="org6d431c6"></a>

# Transactions

<table border="2" cellspacing="0" cellpadding="6" rules="groups" frame="hsides">


<colgroup>
<col  class="org-left" />

<col  class="org-left" />
</colgroup>
<thead>
<tr>
<th scope="col" class="org-left"><b>Action</b></th>
<th scope="col" class="org-left"><b>Transaction</b></th>
</tr>
</thead>

<tbody>
<tr>
<td class="org-left">fail-claim</td>
<td class="org-left"><a href="https://preview.cardanoscan.io/transaction/9612a86dd61b68dc5097e22216b2cee6c6cddeb274ad9ccc7cf33e0e101d824e">9612a86dd61</a></td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">claim-premium</td>
<td class="org-left"><a href="https://preview.cardanoscan.io/transaction/bc537db8d4d3c4bd831ba1f8b0317b3f9dfcb716133073032263d8a96042f5c1">bc537db8d4d</a></td>
</tr>


<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left"><a href="https://preview.cardanoscan.io/transaction/9ed730db98a90361d26674db6c3f5d626b37f802c4eebf20f42ce12baa3fb5d9">9ed730db98a</a></td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">pay-for-claim</td>
<td class="org-left"><a href="https://preview.cardanoscan.io/transaction/0aea116de2fbfad161fd4350e622e48f427121a1d6b3f127a20251d03c4eb2b3">0aea116de2f</a></td>
</tr>


<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left"><a href="https://preview.cardanoscan.io/transaction/127a5af1dfc09615da2721a72a7bdaee4a7aa385e5e9a874502042aeaa659336">127a5af1dfc</a></td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">expire-policy</td>
<td class="org-left"><a href="https://preview.cardanoscan.io/transaction/7680acc7b8fa3e7169dc8d3363d2678c1f7c4110cb3e03c0c950252dce7e1b2e">7680acc7b8f</a></td>
</tr>
</tbody>

<tbody>
<tr>
<td class="org-left">unlock-collateral</td>
<td class="org-left"><a href="https://preview.cardanoscan.io/transaction/3ebc5bf692973ad1a56bc5c9fc6576979a1c9f34ac47d4f6370ad784d0cebca6">3ebc5bf6929</a></td>
</tr>


<tr>
<td class="org-left">&#xa0;</td>
<td class="org-left"><a href="https://preview.cardanoscan.io/transaction/3c48434a3ad858141ed50a39d7f28a0acb274927f67d0ccd66eb9a4944b832ab">3c48434a3ad</a></td>
</tr>
</tbody>
</table>


{% embed url="https://drive.google.com/file/d/1-5X0Kx88IMN3sJ5veg8LbPxAhtjfKKhN/view?usp=drive_link" %}
Video Walkthrough
{% endembed %}
