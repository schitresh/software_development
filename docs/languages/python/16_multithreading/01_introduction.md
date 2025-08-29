## Multithreading
- A computer program by default executes instructions in a sequential manner
- Multi-threading is a mechanism where the main task is divided into threads
  - And executed in an overlapping manner
  - This makes the execution faster
- Threads
  - Light-weight sub-processes in a single program
  - Threads of a single program share the same memory space
  - So they have less overhead than multiple processes
- A process always start with a single thread (main thread)
  - A new thread can be started and sub task is delegated to it
  - When the task assigned to a secondary thread is over
    - It merges with the main thread
  - A thread can be paused for a event or for a specified interval

```py
from threading import Thread
import time

def task(thread_name, delay):
  for count in range(1, 4):
    time.sleep(delay)
    print(f'Thread {thread_name} counting {count}')

def run_without_threads():
  task('t-1', 1)
  task('t-2', 2)

def run_threads():
  t1 = Thread(target = task, args = ('t-1', 1))
  t2 = Thread(target = task, args = ('t-2', 2))

  try:
    t1.start()
    t2.start()

    # Program exits while the threads keep running
  except Exception as e:
    print('Unable to start thread: ', e)

def run_and_join_threads():
  t1 = Thread(target = task, args = ('t-1', 1))
  t2 = Thread(target = task, args = ('t-2', 2))

  try:
    t1.start()
    t2.start()

    # Joins the thread to main thread
    # Waits for the threads to complete
    t1.join()
    t2.join()
  except Exception as e:
    print('Unable to start thread: ', e)

def test(method):
  print(f'Running {method.__name__}:')
  start_time = time.perf_counter()
  method()
  end_time = time.perf_counter()
  print(f'Time taken: {end_time - start_time}')
  print()

test(run_without_threads)
test(run_and_join_threads)
```

## Custom Thread Class
```py
from threading import Thread

class CustomThread(Thread):
  def __init__(self, name):
    Thread.__init__(self)
    self.name = name

  # start() internally calls run() method
  def run(self):
    pass
```

## Thread Priority
```py
from threading import Thread
from queue import PriorityQueue
import time
import random

def producer(queue):
  print('Producer Started')

  for i in range(5):
    value = random.random()
    priority = random.randint(0, 5)
    item = (priority, value)
    queue.put(item)
    print(f'P: {item}')

  print('Producer Finished')

def consumer(queue):
  print('Consumer Started')

  while not queue.empty():
    item = queue.get()
    time.sleep(item[1])
    queue.task_done()
    print(f'C: {item}')

  print('Consumer Finished')

def run():
  queue = PriorityQueue()
  pro = Thread(target = producer, args = (queue,))
  con = Thread(target = consumer, args = (queue,))

  pro.start()
  con.start()

  pro.join()
  con.join()
```
