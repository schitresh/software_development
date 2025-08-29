## Lock
```py
from threading import Thread, Lock

thread_lock = Lock()

class CustomThread(Thread):
  def __init__(self, name):
    Thread.__init__(self)
    self.name = name

  # start() internally calls run() method
  def run(self):
    thread_lock.acquire()
    task(self.name)
    thread_lock.release()

def task(thread_name):
  for i in range(1, 4):
    time.sleep(1)
    print(f'Thread {thread_name}: {i}')

def run():
  threads = []

  for i in range(1, 3):
    thread = CustomThread(f't-{i}')
    threads.append(thread)

  for t in threads:
    t.start()

  for t in threads:
    t.join()
```

## Semaphore
```py
from threading import Thread, Semaphore
import time

lock = Semaphore(2)

def task(name):
  print(f'Thread {name}: waiting')
  lock.acquire()

  for i in range(3):
    print(f'Thread {name}: {i}')
    time.sleep(1)

  lock.release()

def run():
  t1 = Thread(target = task, args = ('t-1',))
  t2 = Thread(target = task, args = ('t-2',))
  t3 = Thread(target = task, args = ('t-3',))

  t1.start()
  t2.start()
  t3.start()
```
