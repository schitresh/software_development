## Condition Variable
```rb
$numbers = []
$mutex = Mutex.new
$condition = ConditionVariable.new

def producer
  3.times do
    $mutex.synchronize do
      num = Random.rand(1..10)
      $numbers.push(num)
      p("P generated number: #{num}")

      $condition.signal
      p('P issued notification')
    end

    sleep(5)
  end
end

def consumer
  3.times do
    $mutex.synchronize do
      p('C waiting for update')
      $condition.wait($mutex) while $numbers.empty?

      num = $numbers.shift
      p("C obtained number: #{num}")
    end

    sleep(5)
  end
end

def run
  $numbers = []
  t1 = Thread.new { producer }
  t2 = Thread.new { consumer }
  t1.join
  t2.join
end
```
