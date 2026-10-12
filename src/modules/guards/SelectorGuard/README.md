# Selector Guard
Arbitrary permissions for proposal top level calls, indexed by function selector signatures, best used in conjunction with the [WhitelistGuard](). 

## Behaviour 
Iterates through proposal actions before execution, verifying if any signatures are "blacklisted" and in the case of reverts.  

## Configuration
Parameters for `token` and `governor` are set on deployment and are immutable, initial selectors can be disabled through the `overwrite()` function. Instances at remove selector restrictions via `remove()` and introduce additional through `add()`.

## Limitations 
The module only adequately satisfies the intended behaviour when the target call implementation has been verified, malicious contracts can still satisfy the constraints simply by defining the same function schema. Additionally selectors which reference `uint`, need to define equivalent selector declarations for `uint256` given that both produce different selectors for the same target **otherwise selector guardrails can be bypassed**.      
