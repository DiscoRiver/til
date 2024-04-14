Scaling code is a lengthy discussion, but essentially I see three avenues;

1. Code scaling

This is the scaling of our code to the surrounding code/products. In a monolith it's easy to understand; our application grows, and our code must be able to effortlessly grow alongside it.

In microservices, our code still needs to grow with the code around it, however we need to have the appropriate documentation and runbooks in place that describe the implementation.

2. Concurrent scaling

Go does most of the work for us with cheap goroutines, as long as we're implementing them "correctly". We need to be mindful of locking data structures and goroutine synchronisation. Each request/event to our software should be handled independantly and not be blocked by any execution from another request. Also we should be mindful of the processing time for requests so we reduce the risk of getting bunched up. 

We can also scale horizontally if needed. 

3. Sequencial scaling

Sequential code may require scaling outside of the software itself, and a supporting infrastructure designed around this fact. Horizontal scaling of containers, for example. 

In this case, concurrency by way to multiple instances? I'm not sure if that's the correct approach.
