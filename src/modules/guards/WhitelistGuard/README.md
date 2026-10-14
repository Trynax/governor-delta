# Selector Guard
Arbitrary permissions for proposal target calls, indexed by address, best used in conjunction with the [SelectorGuard](../SelectorGuard). 

## Behaviour 
Iterates through proposal targets before execution, verifying that call targets are "whitelisted" and if not reverts.  

## Configuration
Parameters for `token` and `governor` are set on deployment and are immutable, initial targets can be disabled through the `overwrite()` function. Instances can remove targets via `remove()` and permit more targets through `add()`. Admin ownership and privileges is at discretion of the governor. 

## Limitations 
It should be stated there is no limitations with this module. 
