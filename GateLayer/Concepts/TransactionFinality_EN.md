# Transaction Finality

This guide explains when transactions on Gate Layer are considered "finalized".

## Basics of Finality

Transaction "finality" refers to the point at which a transaction becomes irreversible under certain assumptions. As a standard Rollup, Gate Layer delegates the ordering and finality of its transactions to its L1, **GateChain**.

## Steps to Finality

Transactions on Gate Layer go through the following steps to reach finality:

1.  **`unsafe` State**: 
    After a user submits a transaction, the Sequencer immediately processes it and includes it in an L2 block. At this point, the transaction data exists only on the Sequencer and has not been posted to GateChain.

2.  **`safe` state**: 
    The Sequencer posts a block containing the transaction data as a `blob` to GateChain. Once this data is included in a GateChain block, the transaction reaches the `safe` state. Since GateChain blocks have instant finality, the data is theoretically irreversible at this point.

3.  **`finalized` state**: 
    To provide an additional layer of security and align with industry best practices, Gate Layer's design requires that after a GateChain block containing L2 transaction data is confirmed, an additional **10** new GateChain blocks must be successfully produced. Only then is the L2 transaction officially considered `finalized`.

> **Key Confirmation: A 10-Block Safety Buffer**
> This design ensures the highest level of irreversibility for L2 transactions under any extreme network conditions, providing a robust security foundation for high-value applications like cross-chain bridge withdrawals.

## Conclusion

The transaction finality model of Gate Layer is tightly coupled with its L1, GateChain. Once the GateChain block containing your transaction data has received **10 block confirmations**, you can be confident that the L2 transaction is finalized and irreversible.
