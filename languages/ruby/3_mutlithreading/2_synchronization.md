## Mutex
```rb
$mutex = Mutex.new

class Counter
  def initialize(name)
    @name = name
  end

  def run
    $mutex.synchronize do
      task(@name)
    end
  end
end

def task(thread_name)
  (1..4).each do |i|
    sleep(1)
    p("Thread #{thread_name}: #{i}")
  end
end

def run
  threads = []

  (1..2).each do |i|
    thread = Counter.new("t-#{i}")
    threads.push(thread)
  end

  threads.map! { |t| Thread.new { t.run } }
  threads.each { |t| t.join }
end
```
