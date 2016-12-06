
# Requirements Explanations

## FUN 1

> The bank deals with clients and accounts.

The bank deals with accounts and clients, which is translated in Event-B as two finite sets `ACCOUNT` and `CLIENT`, as defined in the `bankAccount_C0` context.

## FUN 2

> Opening an account permanently associates it with a single client, the owner of the account.

We used the `owner` variable, defined as a partial function, that maps a `CLIENT` owner to each `ACCOUNT` from its domain. The set starts empty, and ownership is set during the `accountCreation` event, which is executed only once for every account, hence the property holds.

## FUN 3

 > Every account has a balance.

We used the `balance` variable, defined as a partial function, that maps a natural number to each `ACCOUNT` from its domain.

## FUN 4

> The balance of any newly opened account is zero.

The balance of each `ACCOUNT` is set to **0** when its created in the `accountCreation` event.

## FUN 5

> The balance of an account cannot be negative at any moment.

The range of the balance partial function is *NAT*, hence balance can never be negative.

## FUN 6

> Bank clients can deposit money in any existing account, thereby increasing its balance.

The `deposit` event, introduced in refinement `bankAccount_M2_deposit` effectively increases the balance of `ACCOUNT` *a* by amount *x*, no matter who the owner is.

## FUN 7

> Money can be withdrawn from an existing account, and its balance is decreased by that amount.

The `withdraw` event, introduced in refinement `bankAccount_M3_withdraw` effectively decreases the balance of `ACCOUNT` *a* by amount *x*, if `CLIENT` *c* is its owner.

## FUN 8

> Money can be transfered from between two existing accounts. The balance of the account from which money is transferred is reduced by the amount being transferred, and the balance of the account to which money is transferred is increased by the amount being transferred.

The `transfer` event, introduced in refinement `bankAccount_M4_transfer` effectively decreases the balance of account aFrom, and increases the balance of account *aTo* by *x*, if an only if the owner of account aFrom is `CLIENT` *c*.


## FUN 9

> Only the owner of an account can withdraw or transfer money from it.

As stated in explanations for **FUN 7** and **FUN 8**, the guards from events `withdraw` and `transfer` prevent any client who is not owner of the account to debit to perform the operation.

## FUN 10

> The decrease of the balance of any account caused by two consecutive opera- tions has to be less than or equal to `K` (for some constant `K`).

First, we added a constant `K` to the `bankAccount_C0` context.  
Then, we declared a new partial function `lastMovement` that keeps track of the amount of the last debit operation for each account. If a credit operation occurs on `ACCOUNT` *a*, `lastMovement(a)` is set to **0**.  
Additionally, among the guards of every debit operation event, we added `authorizedByBank`, which only triggers the event if the sum of the last movement for the current account and the amount of the operation is lesser or equal to `K`.
