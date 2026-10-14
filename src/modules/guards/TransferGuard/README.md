## Transfer Guard 
A module to budget and limit the flow of assets following the [ERC20 token standard]() with arbitrary spending context.

### Behaviour 
On set declaration of asset budget policies, the module iterates a snapshot of context balances before state changes. After proposal execution and state changes, it queries again and calculates the difference in balances. The parameter limit restricts the amount that be spent in one proposal and allowance represents the total budget available. Any expenditures that exceed these parameters results in the proposal to be reverted.

### Configuration
Assets can added with `push()` and removed with `pull()`, budget or policies then are declared by spender context following the `Policy` schema. Budgets can be created via `add()`, updated or modified with `overwrite` and redacted via `remove()`. Parameter `governor` is set on deployment and is immutable, with unequivocal admin privileges.

### Limitations
The behaviour and constraints defined by this module only hold in assumption that the assets declared fully satisfy the ERC20 standard.
