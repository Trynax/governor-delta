# Staking Guard
An explicit safeguard to restrict the transfer of voter canonical balances in context of [Relay Actions](../../../../SPEC.md#relay-actions), **recommended as a default for all instances**.  

## Behaviour 
Proposal actions authored for timelock execution face no constraints or checks. With relays, the module queries the governor's `totalStaked` state variable, representing the total locked voter balances. Ensuring the governor's balance after state changes is always greater than or equal to voter stakes. 

## Configuration
Parameters for `token` and `governor` are set on deployment and are immutable. 

## Limitations 
For the following constraints and module behaviour to hold, `governor` must be assigned a target address that adopts Delta's standard and that `token` is equal to the instance's parameter `canonicalToken`.
