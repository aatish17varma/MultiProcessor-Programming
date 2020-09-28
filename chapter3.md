According to Amdahl's Law, concurrent objects whose methods hold exclusive locks are a lot slower than one's that do not. This is because more time is spent on synchronization between threads, which could be spent on pure parallelism

With concurrency, it is easier to understand what is going on if we can map concurrent executions to sequential ones, because in actuality, these concurrent executions are happening in a sequential order


3.2 Sequential Objects:
---------------------------

- Object: A container for data
- Methods: Way to manipulate the data in objects
- State: What an object's data is at a given time (for a FIFO queue, the state of the queue is simply the data in the queue and the order they are in)

- Precondition: the state of an object before invoking a method on it 
- Postcondition: The state of an object after invoking a method on it 
- Side Effect: The change in an object's state after a method is invoked on it

State A ----Side Effect----> State B 

Whenever data is changed, we can call that a side effect ( which makes everything we do in CS a side-effect)

However simply describing data manipulation as pre-conditions and post-conditions become difficult when concurrency is introduced. If two threads enqueue some number in a shared queue during the same time interval, we don't know which number was added first/second. Thus, what is the post-condition of the queue. What is the pre-condition and post-condition ater one number is queued. 

As said by Shavit/Herlihy - "Any method call must be prepared to encounter an object state that reflects the incomplete efefcts of other concurrent method calls, a problem that simply does not arise in single-threaded programs" 


3.3 Quescient Consistency: 
----------------------------

- Method Call: Interval that starts with 
	- invocation event (the function being called by some thread'hello()')
  and  ends with a 
	- response event (the code in the hello function being executed fully) 

** Method calls by multiple threads will possibly overlap. Not true with single threads ** 

Pending Method Call: Method has been invoked but has not responded. 

Register: Simply stated, a piece of shared memory where data is written to and read from

An Object is Quescient if it has no pending memory calls. 

Quescient Consistency: 
	Principle 1: method calls should appear to happen in a one-at-a-time sequential order
		- If I concurrently enqueue 3 and 4 to a queue, then when I check the state of the queue at some time after, the queue should either be [3,4] or [4,3]
		**NOTE**: This means that you cannot make any assumptions about the order of execution of function calls occuring at shared intervals (as shown above in the example)		  

	Principle 2: If a method call is separated by time with another method call (method calls are not concurrent), then the shared object should reflect the change in data in that sequential order
		- If one thread enqueues 3 in the time interval 1 - 6, and another thread enqueues 4 in the time interval 7 - 12, then at time 13, the queue = [3,4]

A correctness Property *P* is compositional if all smaller objects in a big object satisfy *P* then the entire systems satisfies *P*

Quesciently Consisteny objects are compositional. 
	- e.g. if you are forming a queue from smaller queues, and you add objects in a quesciently consistent way, then the arrangement of values in the entire queue will satisfy the requirements of quescient consistency.

3.4 Sequential Consistency:
-----------------------------

Program Order: The order in which a single thread issues method calls

Sequential Consistency:
	- Order of events being executed should obey program order
	- Execution of events should be done in a sequential "step-by-step" way

- Sequential Consistency can re-order events that occur in different threads, even if one event occurs at a time interval after the other one
 **NOTE** : This is not the case in Quiescient Consistency, as if two events are separated by time, then they should be executed in that order (Look at example 3.7 in the book)

Sequential Consistency is not the default setting in the modern multiprocessor architectures. If a programmer wants functions to be executed with sequential consistency, they must specifically ask for it. 

Sequential Consistency can completely reorder events occuring at different time stamps in different threads, which seems very unintuitive (If I deposit money on Monday, and withdraw on Tuesday, the bank shouldn't execute my withdrawl before my deposit). However, if methods are complete, then there should be **NO** exceptions thrown :) .

Is Sequential Consistency Compositional? - **NO**




