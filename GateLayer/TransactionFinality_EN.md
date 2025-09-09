# Transaction Finality

This guide explains when transactions on Gate Layer are considered "finalized".

## Basics of Finality

Transaction "finality" refers to the point at which a transaction becomes irreversible under certain assumptions. As a standard Rollup, Gate Layer delegates the ordering and finality of its transactions to its L1, **GateChain**.

## Steps to Finality

Transactions on Gate Layer go through the following steps to reach finality:

1.  **`unsafe` State**: 
    After a user submits a transaction, the Sequencer immediately processes it and includes it in an L2 block. At this point, the transaction data exists only on the Sequencer and has not been posted to GateChain.

2.  **`safe` State**: 
    The Sequencer posts the block containing the transaction data as a `blob` to GateChain. Once the data is successfully included in a GateChain block, the transaction reaches the `safe` state.

3.  **`finalized` State**: 
    The L2 transaction becomes final once the GateChain block containing its data is finalized.

> **Key Confirmation: Wait for 10 Blocks**
> In Gate Layer's design, a GateChain block that contains L2 transaction data is considered final after **10 new** GateChain blocks have been successfully built on top of it. This means your L2 transaction has become irreversible at this point.

## Conclusion

Gate Layer's transaction finality is tightly coupled with its L1, GateChain. Once the GateChain block containing your transaction data has received **10 block confirmations**, you can be confident that your L2 transaction is finalized and irreversible.
