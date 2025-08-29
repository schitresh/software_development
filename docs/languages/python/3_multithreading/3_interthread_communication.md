## Event
- Manages the state of an internal flag
- The flag is initially false and becomes true with set() method
- It resets to false with the clear() method
- To block the flow until the flag is true, wait() method is used
- To check if the flag is set or not, is_set() method is used
- Event and is_set() method can also be used to stop a thread

```py
from threading import Thread, Event
import time

def signal_state():
  while vehicle < 10:
    time.sleep(5)
    print('Signal turns Green', end='\n\n')
    event.set()

    time.sleep(10)
    print('Signal turns Red', end='\n\n')
    event.clear()

def traffic_flow():
  global vehicle
  while vehicle < 10:
    print('Waiting for Green signal')
    event.wait()
    print('Traffic can move')

    while event.is_set():
      vehicle = vehicle + 1
      print(f'Vehicle {vehicle} crossing')
      time.sleep(2)

    print('Traffic has to wait')

vehicle = 0
event = Event()

def run():
  global vehicle
  vehicle = 0
  t1 = Thread(target = signal_state)
  t2 = Thread(target = traffic_flow)

  t1.start()
  t2.start()
```

## Condition
- Forces one or more threads to wait until notified by another thread
- Thread A acquires the condition and thread B is in waiting state
- After execution, thread A notifies thread B and thread A releases the condition
- After condition is released, the waiting thread B proceeds execution

```py
from threading import Thread, Condition
import time
import random

numbers = []
condition = Condition()

def producer():
  for i in range(3):
    condition.acquire()

    num = random.randint(1, 10)
    numbers.append(num)
    print(f'A generated number: {num}')

    condition.notify()
    print('A issued notification')

    condition.release()
    time.sleep(5)

def consumer():
  for i in range(3):
    condition.acquire()

    print('B waiting for update')
    while len(numbers) == 0: condition.wait()
    num = numbers.pop()
    print(f'B obtained number: {num}')

    condition.release()
    time.sleep(5)

def run():
  global numbers
  numbers = []

  t1 = Thread(target = producer)
  t2 = Thread(target = consumer)
  t1.start()
  t2.start()
```
