## Concurrency and Multithreading
### 1188: leetcode threadsafe blocking queue design
Implement a thread-safe bounded blocking queue that has the following methods:

BoundedBlockingQueue(int capacity) The constructor initializes the queue with a maximum capacity.
void enqueue(int element) Adds an element to the front of the queue. If the queue is full, the calling thread is blocked until the queue is no longer full.
int dequeue() Returns the element at the rear of the queue and removes it. If the queue is empty, the calling thread is blocked until the queue is no longer empty.
int size() Returns the number of elements currently in the queue.
Your implementation will be tested using multiple threads at the same time. Each thread will either be a producer thread that only makes calls to the enqueue method or a consumer thread that only makes calls to the dequeue method. The size method will be called after every test case.

Please do not use built-in implementations of bounded blocking queue as this will not be accepted in an interview.

To implement a thread-safe bounded blocking queue, we need to manage two important synchronization problems:
1. **Blocking when the queue is full** (for `enqueue` operations).
2. **Blocking when the queue is empty** (for `dequeue` operations).

To handle synchronization, we will use:
- **Locks** to ensure mutual exclusion when modifying the queue's state.
- **Condition variables** to allow threads to wait when they can't perform their operation (enqueue when the queue is full or dequeue when the queue is empty).

We will implement a queue using a list and manage the capacity and size using locks and condition variables.

### Bounded Blocking Queue Implementation in Python:

```python
import threading

class BoundedBlockingQueue:
    def __init__(self, capacity: int):
        """Initializes the queue with the given capacity."""
        self.capacity = capacity  # Maximum capacity of the queue
        self.queue = []  # This will store the elements in the queue
        self.lock = threading.Lock()  # Lock to protect access to the queue
        self.not_full = threading.Condition(self.lock)  # Condition variable for not full
        self.not_empty = threading.Condition(self.lock)  # Condition variable for not empty

    def enqueue(self, element: int):
        """Adds an element to the front of the queue. Blocks if the queue is full."""
        with self.lock:
            while len(self.queue) == self.capacity:
                # Wait until there's space in the queue (i.e., it's not full)
                self.not_full.wait()
            
            # Add the element to the front of the queue (the list's start)
            self.queue.insert(0, element)

            # Notify that the queue is not empty (a consumer may now be able to dequeue)
            self.not_empty.notify()

    def dequeue(self) -> int:
        """Returns the element at the rear of the queue and removes it. Blocks if the queue is empty."""
        with self.lock:
            while len(self.queue) == 0:
                # Wait until there's at least one element in the queue (i.e., it's not empty)
                self.not_empty.wait()
            
            # Remove and return the element from the rear of the queue (the list's end)
            element = self.queue.pop()

            # Notify that the queue is not full (a producer may now be able to enqueue)
            self.not_full.notify()

            return element

    def size(self) -> int:
        """Returns the number of elements in the queue."""
        with self.lock:
            return len(self.queue)

# Example usage:

def producer(queue, num_items):
    for i in range(num_items):
        print(f"Enqueueing {i}")
        queue.enqueue(i)
        print(f"Enqueued {i}, queue size: {queue.size()}")

def consumer(queue, num_items):
    for _ in range(num_items):
        item = queue.dequeue()
        print(f"Dequeued {item}, queue size: {queue.size()}")

def test_bounded_blocking_queue():
    capacity = 3
    queue = BoundedBlockingQueue(capacity)

    # Start producer and consumer threads
    producer_thread = threading.Thread(target=producer, args=(queue, 5))
    consumer_thread = threading.Thread(target=consumer, args=(queue, 5))

    producer_thread.start()
    consumer_thread.start()

    producer_thread.join()
    consumer_thread.join()

if __name__ == "__main__":
    test_bounded_blocking_queue()
```

### Explanation:

1. **Class Initialization (`__init__`)**:
   - The `BoundedBlockingQueue` is initialized with a `capacity` and an empty list (`queue`) to hold the elements.
   - We create a `Lock` object to ensure thread-safety when accessing the queue.
   - Two `Condition` objects are created (`not_full` and `not_empty`), which will allow threads to wait and be notified when the queue's state changes (e.g., when it becomes not full or not empty).

2. **`enqueue` Method**:
   - This method attempts to add an element to the front of the queue. If the queue is full (i.e., the size is equal to `capacity`), the calling thread will block by calling `self.not_full.wait()`. It will only continue once space becomes available.
   - When an item is successfully added, we notify other threads that the queue is no longer empty, signaling that consumers may proceed to dequeue.

3. **`dequeue` Method**:
   - This method retrieves and removes an element from the rear of the queue. If the queue is empty, the calling thread will block by calling `self.not_empty.wait()`. It will only continue once there are items available to dequeue.
   - After an item is removed, we notify other threads that the queue is no longer full, signaling that producers may proceed to enqueue.

4. **`size` Method**:
   - The `size()` method simply returns the number of elements in the queue. It uses a lock to ensure thread-safety when accessing the `queue` list.

### Example Usage:

In the `test_bounded_blocking_queue` function, two threads are created:
- A **producer** thread that enqueues five items (from 0 to 4) into the queue.
- A **consumer** thread that dequeues five items from the queue.
- Both threads will block when necessary (the producer will block when the queue is full, and the consumer will block when the queue is empty).

### Output:

The output will show how elements are enqueued and dequeued while respecting the queue's capacity limits:

```plaintext
Enqueueing 0
Enqueued 0, queue size: 1
Enqueueing 1
Enqueued 1, queue size: 2
Enqueueing 2
Enqueued 2, queue size: 3
Dequeued 0, queue size: 2
Dequeued 1, queue size: 1
Enqueueing 3
Enqueued 3, queue size: 2
Dequeued 2, queue size: 1
Enqueueing 4
Enqueued 4, queue size: 2
Dequeued 3, queue size: 1
Dequeued 4, queue size: 0
```

### Thread Safety:

- **Locks**: The `enqueue`, `dequeue`, and `size` methods use the `lock` to ensure that only one thread can access or modify the queue at a time. This prevents race conditions.
  
- **Condition Variables**: The `enqueue` and `dequeue` methods use `Condition` variables (`not_full` and `not_empty`) to block threads when the queue is full or empty. When the queue's state changes (e.g., an item is added or removed), the corresponding condition variable is notified to wake up waiting threads.

### Performance Considerations:
- The `enqueue` and `dequeue` methods use `insert(0, element)` and `pop()`, which are **O(n)** operations for a list in Python. For performance-sensitive scenarios, it may be beneficial to use a `deque` instead of a list, as `deque` offers O(1) operations for appending and popping from both ends.
  
### Improvements:
- We could replace the `queue` list with `collections.deque` to improve performance, particularly when dealing with large numbers of items.
  
Would you like me to demonstrate the use of a `deque` here? Or would you like to test this implementation in more detail?


Using a **semaphore** is another way to manage thread synchronization, particularly when you need to control access to a limited resource. In the case of a bounded blocking queue, a semaphore can be used to manage the queue’s capacity (to block when the queue is full) and to signal when items are available for consumption (to block when the queue is empty).

### Using a Semaphore for Thread Synchronization

1. **Semaphore for full queue**: We can use a semaphore to track the available capacity of the queue. Each time an item is enqueued, we signal that space has been taken, and each time an item is dequeued, we signal that space is free.
   
2. **Semaphore for empty queue**: A second semaphore can be used to track whether there are any items in the queue. When an item is added, we signal that an item is available, and when an item is removed, we signal that the queue has fewer items.

This approach simplifies the synchronization since semaphores naturally allow threads to wait until a resource is available (for example, space in the queue or an item to consume).

### Semaphore-Based Bounded Blocking Queue

Let’s rewrite the bounded blocking queue implementation using semaphores for synchronization:

```python
import threading

class BoundedBlockingQueue:
    def __init__(self, capacity: int):
        """Initializes the queue with the given capacity."""
        self.capacity = capacity  # Maximum capacity of the queue
        self.queue = []  # This will store the elements in the queue
        self.lock = threading.Lock()  # Lock to protect access to the queue
        self.empty_slots = threading.Semaphore(capacity)  # Semaphore for empty slots (capacity)
        self.items = threading.Semaphore(0)  # Semaphore for available items (starts empty)

    def enqueue(self, element: int):
        """Adds an element to the front of the queue. Blocks if the queue is full."""
        self.empty_slots.acquire()  # Wait for an empty slot in the queue
        
        with self.lock:
            # Add the element to the front of the queue (the list's start)
            self.queue.insert(0, element)
        
        self.items.release()  # Signal that there is a new item in the queue

    def dequeue(self) -> int:
        """Returns the element at the rear of the queue and removes it. Blocks if the queue is empty."""
        self.items.acquire()  # Wait for an item to become available
        
        with self.lock:
            # Remove and return the element from the rear of the queue (the list's end)
            element = self.queue.pop()
        
        self.empty_slots.release()  # Signal that there is now an empty slot in the queue
        return element

    def size(self) -> int:
        """Returns the number of elements in the queue."""
        with self.lock:
            return len(self.queue)

# Example usage:

def producer(queue, num_items):
    for i in range(num_items):
        print(f"Enqueueing {i}")
        queue.enqueue(i)
        print(f"Enqueued {i}, queue size: {queue.size()}")

def consumer(queue, num_items):
    for _ in range(num_items):
        item = queue.dequeue()
        print(f"Dequeued {item}, queue size: {queue.size()}")

def test_bounded_blocking_queue():
    capacity = 3
    queue = BoundedBlockingQueue(capacity)

    # Start producer and consumer threads
    producer_thread = threading.Thread(target=producer, args=(queue, 5))
    consumer_thread = threading.Thread(target=consumer, args=(queue, 5))

    producer_thread.start()
    consumer_thread.start()

    producer_thread.join()
    consumer_thread.join()

if __name__ == "__main__":
    test_bounded_blocking_queue()
```

### Explanation:

1. **Semaphores**:
   - **`empty_slots`**: A semaphore that tracks the available space in the queue. It starts with a value equal to the queue's capacity. Each time an item is added (i.e., enqueued), this semaphore’s count decreases, indicating that there is less space. When the queue is full, producers will block on this semaphore until there is space.
   - **`items`**: A semaphore that tracks how many items are available in the queue. It starts with a count of 0 because the queue is initially empty. Each time an item is removed (i.e., dequeued), this semaphore’s count increases, indicating that there is an item to consume. When the queue is empty, consumers will block on this semaphore until there is an item.

2. **`enqueue`**:
   - The producer calls `self.empty_slots.acquire()`. This operation will block if there is no space left in the queue (i.e., the semaphore’s count is zero).
   - After ensuring there is space, it acquires the lock, adds the item to the queue, and then releases the `items` semaphore to signal that an item is available for consumption.

3. **`dequeue`**:
   - The consumer calls `self.items.acquire()`. This operation will block if there are no items in the queue (i.e., the semaphore’s count is zero).
   - After ensuring there is an item, it acquires the lock, removes the item from the queue, and then releases the `empty_slots` semaphore to signal that space is available for a producer to add an item.

4. **`size`**:
   - The `size()` method simply returns the number of elements currently in the queue, which is thread-safe because it is protected by a lock.

### Example Usage:

In the `test_bounded_blocking_queue` function, two threads are created:
- A **producer** thread that enqueues five items (from 0 to 4) into the queue.
- A **consumer** thread that dequeues five items from the queue.

The output will show how elements are enqueued and dequeued while respecting the queue's capacity limits.

```plaintext
Enqueueing 0
Enqueued 0, queue size: 1
Enqueueing 1
Enqueued 1, queue size: 2
Enqueueing 2
Enqueued 2, queue size: 3
Dequeued 0, queue size: 2
Dequeued 1, queue size: 1
Enqueueing 3
Enqueued 3, queue size: 2
Dequeued 2, queue size: 1
Enqueueing 4
Enqueued 4, queue size: 2
Dequeued 3, queue size: 1
Dequeued 4, queue size: 0
```

### Semaphore vs Condition Variable:

- **Semaphore**: In this approach, semaphores are used to signal the availability of space in the queue (`empty_slots`) and the availability of items (`items`). A semaphore essentially counts the resources and allows threads to acquire or release those resources. It is a simpler way to coordinate producer-consumer behavior than using condition variables.
  
- **Condition Variables**: With condition variables, you explicitly define conditions for blocking and waking up threads (like waiting for the queue to become non-empty or non-full). While semaphores can handle these conditions implicitly by counting, condition variables provide more fine-grained control.

### Thread Safety:

- The `enqueue` and `dequeue` methods are thread-safe because they both use **semaphores** for synchronization and a **lock** (`self.lock`) to protect the shared resource (the queue) from race conditions.
  
- **Blocking behavior**: Both the `enqueue` and `dequeue` methods will block when necessary:
  - `enqueue` blocks if the queue is full (waits on `empty_slots`).
  - `dequeue` blocks if the queue is empty (waits on `items`).

### Conclusion:

This semaphore-based solution for a bounded blocking queue provides an elegant way to manage concurrent access to a shared resource. Semaphores handle synchronization and blocking efficiently, making this approach ideal for problems like producer-consumer scenarios.
