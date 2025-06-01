# WebFlux

## Principles

When declaring a reactive chain with, let's say, `WebClient`, the whole expression gets evaluated, but only gets executed once it is subscribed to downstream. Then the whole flow is controlled by signals, where a `onNext` symbol denotes an emitted item which is ready to be handled by the next operator in the chain. This might get picked up by a different thread.  
In a reactive programming model (such as the one provided by Project Reactor), the declaration of the reactive chain is processed synchronously by a single thread, and only when the chain is subscribed to, the actual execution of each step in the chain can be handled by different threads depending on the operators used and the schedulers assigned.