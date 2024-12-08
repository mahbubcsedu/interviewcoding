## Concurrency and Multithreading
### 1188: leetcode threadsafe blocking queue design
Implement a thread-safe bounded blocking queue that has the following methods:

BoundedBlockingQueue(int capacity) The constructor initializes the queue with a maximum capacity.
void enqueue(int element) Adds an element to the front of the queue. If the queue is full, the calling thread is blocked until the queue is no longer full.
int dequeue() Returns the element at the rear of the queue and removes it. If the queue is empty, the calling thread is blocked until the queue is no longer empty.
int size() Returns the number of elements currently in the queue.
Your implementation will be tested using multiple threads at the same time. Each thread will either be a producer thread that only makes calls to the enqueue method or a consumer thread that only makes calls to the dequeue method. The size method will be called after every test case.

Please do not use built-in implementations of bounded blocking queue as this will not be accepted in an interview.

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

