# Policy Initiated

Upon creation, a Policy is considered to be in an `Initiated` state. To create a policy, the broker must supply several parameters, including the collateral value, the number of share units to generate (referred to as `Fida Cards`), the duration for which the policy will remain at risk, the policy owner's information, and the premium amount, which denotes the cost the policy owner must pay for the policy.

Required parameters:

* collateral amount
* number of share units
* policy duration
* policy owner public key hash
* premium amount

{% hint style="info" %}
Please note that when creating a policy, we do not establish a start date indicating when the policy is at risk.
{% endhint %}

While the policy is in the `Initiated` state, it can be canceled without any consequences. All collateral will be returned to the investors, and the policy owner will not face any charges for the cancellation.
