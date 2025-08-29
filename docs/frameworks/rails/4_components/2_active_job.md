## Active Job
- Framework for declaring jobs and run them on queuing backends
- These jobs can be for scheduled clean-ups, billing charges, mailings, etc.
- To create a job, run 'rails g job guests_cleanup'
  - Can pass also pass queue name like '--queue urgent'
  - Creates the file in 'app/jobs' and inherits 'ApplicationJob'
- For enqueuing and executing jobs in product, a queuing backend needs to be set up
  - Rails itself only provides an in-process queuing system (kept in RAM)
  - If the process crashes or the machine is reset, all the jobs are lost
  - ActiveJob has built-in adapters for Sidekiq, Resque, DelayedJob, etc.
  - So specify a third party queuing library in config/application.rb
  - And setup its queuing service since jobs run in parallel

```rb
class GuestsCleanupJob < ApplicationJob
  queue_as :default

  def perform(*guests)
    # Do something later
  end
end

# Enqueue a job to be performed as soon as the queuing system is free
GuestsCleanupJob.perform_later(guest)
# Enqueue a job to be performed tomorrow at noon
GuestsCleanupJob.set(wait_until: Date.tomorrow.noon).perform_later(guest)
# Enqueue a job to be performed 1 week from now
GuestsCleanupJob.set(wait: 1.week).perform_later(guest)
# `perform_now` and `perform_later` will call `perform` under the hood
GuestsCleanupJob.perform_later(guest1, guest2, filter: 'some_filter')
# Specify queue dynamically
GuestsCleanupJob.set(queue: :another_queue).perform_later(record)
```

## Callbacks
- (before, around, after)_enqueue
- (before, around, after)_perform

```rb
class GuestsCleanupJob < ApplicationJob
  queue_as :default

  around_perform :around_cleanup

  def perform
    # Do something later
  end

  private
    def around_cleanup
      # Do something before perform
      yield
      # Do something after perform
    end
end
```
