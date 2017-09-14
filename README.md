# BrainDeadPool

A simple thread pool implementation with support for futures. Adding a task accepts a function with a return value, which is obtained through a Boost future, and whatever list of arguments you need to pass.

Rough Usage Example:
```
#include <iostream>
#include <boost/thread/future.hpp>
#include <braindead/thread_pool.hpp>

int main() {
    bdp::ThreadPool tp(1); // Instantiate with 1 worker thread.
    int a = 4;
    int b = 2;
    boost::future<int> future1 = tp.add_task(add_two_numbers, a, b); // Add function as task to queue.
    boost::future<void> future2 = future1.then([&]() { // Executes when the task returns.
        int c = future1.get(); // Retrieve the task result.
        std::cout << "The sum of " << a << " + " << b << " is " << c << std::endl;
    });

    return 0;
}
```

Requires Boost, but can be easily modified to use the C++ standard library if you don't need continuations (which is pretty much the only reason this is currently using Boost).
