# SAINA

Straggler-Aware In-Network Aggregation (SAINA) mitigates the straggler problem in distributed deep learning while preventing accuracy degradation. It aggregates local gradients of the fastest $k$ workers to exclude stragglers
and changes $k$ adaptively to balance the tradeoff between training speed and accuracy.

The source code of SAINA was implemented on top of [SwitchML](https://github.com/p4lang/p4app-switchML).
