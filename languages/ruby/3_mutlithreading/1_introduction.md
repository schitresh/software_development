## Introduction
```rb
require 'benchmark'

def task(thread_name, delay)
  (1..4).each do |count|
    sleep(delay)
    p("Thread #{thread_name} counting #{count}")
  end
end

def run_without_threads
  task('t-1', 1)
  task('t-2', 2)
end

def run_threads()
  begin
    t1 = Thread.new { task('t-1', 1) }
    t2 = Thread.new { task('t-2', 2) }

    # Program exits while the threads keep running
  rescue => e
    p('Unable to start thread: ', e.message)
  end
end

def run_and_join_threads()
  begin
    t1 = Thread.new { task('t-1', 1) }
    t2 = Thread.new { task('t-2', 2) }

    # Joins the thread to main thread
    # Waits for the threads to complete
    t1.join
    t2.join
  rescue => e
    p('Unable to start thread: ', e.message)
  end
end

Benchmark.measure { run_without_threads }
Benchmark.measure { run_and_join_threads }
```
