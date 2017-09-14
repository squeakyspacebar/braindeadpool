# BrainDeadPool

A simple thread pool implementation with support for futures. Adding a task accepts a function with a return value, which is obtained through a Boost future, and whatever list of arguments you need to pass.

I've since cleaned up the implementation and the new style takes a lot of notes from [Jakob Progsch's ThreadPool](https://github.com/progschj/ThreadPool). Still supports Boost futures and continuations.

Trivial Usage Example:
```
#define BOOST_THREAD_PROVIDES_FUTURE
#define BOOST_THREAD_PROVIDES_FUTURE_CONTINUATION

#include <iostream>
#include <boost/thread/future.hpp>
#include <braindead/thread_pool.hpp>

int main() {
    bdp::ThreadPool tp(1); // Instantiate with 1 worker thread.
    int a = 4;
    int b = 2;

    // Queue a task and assign a handler function.
    auto f1 = tp.queue_task([a, b]() { return a + b; });
    auto f2 = f1.then([a, b](auto f) {
        int c = f.get();
        std::cout << "The sum of " << a << " + " << b << " is " << c << std::endl;
    });

    boost::wait_for_all(f2);

    return 0;
}
```

Requires Boost, but can be easily modified to use the C++ standard library if you don't need continuations (which is pretty much the only reason this is currently using Boost), or you can just use [Jakob Progsch's ThreadPool](https://github.com/progschj/ThreadPool).
