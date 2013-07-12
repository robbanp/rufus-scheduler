
# rufus-scheduler

[![Build Status](https://secure.travis-ci.org/jmettraux/rufus-scheduler.png)](http://travis-ci.org/jmettraux/rufus-scheduler)


## note about the 3.0 line

It's a complete rewrite of rufus-scheduler.

There is no EventMachine-based scheduler anymore.


## Notables changes:

* As said, no more EventMachine-based scheduler
* ```scheduler.every('100') {``` will schedule every 100 seconds (previously, it would have been 0.1s). This aligns rufus-scheduler on Ruby's ```sleep(100)```
* The scheduler isn't catching the whole of Exception anymore, only StandardException


## scheduling

TODO: in/at/cron/every


## job options

### :blocking => true

By default, jobs are triggered in their own, new thread. When :blocking => true, the job is triggered in the scheduler thread (a new thread is not created). Yes, while the job triggers, the scheduler is not scheduling.

### :overlap => false

Since, by default, jobs are triggered in their own new thread, job instances might overlap. For example, a job that takes 10 minutes and is scheduled every 7 minutes will have overlaps.

To prevent overlap, one can set :overlap => false. Such a job will not trigger if one of its instance is already running.

### :mutex => mutex_instance / mutex_name / array of mutexes

When a job with a mutex triggers, the job's block is executed with the mutex around it, preventing other jobs with the same mutex to enter (it makes the other jobs wait until it exits the mutex).

This is different from :overlap => false, which is, first, limited to instances of the same job, and, second, doesn't make the incoming job instance block/wait but give up.

:mutex accepts a mutex instance or a mutex name (String). It also accept an array of mutex names / mutex instances. It allows for complex relations between jobs.

Array of mutexes: original idea and implementation by [Rainux Luo](https://github.com/rainux)


## parsing cronlines

TODO


## license

MIT, see LICENSE.txt
