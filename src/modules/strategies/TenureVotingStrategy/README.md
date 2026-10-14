## Tenure Voting
An implementation of loyalty shares, where holding stock for set period results in enhanced voting rights.

### Scoring 
Where [Effective time]() is the variable of which duration is measured, which is indexed by tranch period for increased power multipliers.

### Configuration 
Tranches are defined by parameters `duration` and `value` to equate to y multiplier over x time period, for duration with `expiry`. Following the `Tranche` schema, the first entry of any schedule should contain a power multiplier of 1.00 to ensure no immediate power amplification. Schedules can updated via the `set()` function, admin ownership is at discretion of the governor. 
