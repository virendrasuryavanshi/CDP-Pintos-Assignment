# CDP Pintos Assignment

This is our submission for the following Pintos [Assignment](https://drive.google.com/file/d/1eGXkml_AY-WhnkMKVebFXoOibMcjexhO/view?usp=drivesdk)


## Contributors

Suryavanshi Virendrasingh (B16037)  
Sujetth Rangannath (B16036)  
Amrendra Singh (B16010)  

## Changes 

Changes in timer.c:
Only the timer_sleep function was modified as specified in the assignment. 
The while loop causing busy wait was removed and all the functions specified in task 1, 2 and 3 were called in the given order.

Changes in thread.c:
Following functions were added:

1. void thread_priority_temporarily_up()
- Temporarily increase thread priority so that no other thread interrupts its execution. (Since the operations are not necessarily atomic).

2. void thread_priority_restore()
- Restore thread priority to original value so that other threads can get executed.

3. void thread_block_till(int64_t wakeup_at, list_less_func *before)
- Add current thread to sleepers list with the given wakeup time. Block the current thread.

4. bool before(const struct list_elem *a, const struct list_elem *b, void*aux UNUSED)
- Comparator function for sleeper list (Works on wakeup times)

5. bool cmp(const struct list_elem *a, const struct list_elem *b, void*aux UNUSED)
- Comparator function for ready list (Works on priority)

6. void thread_wakeup(int64_t current_time)
- Checks if the first thread on sleepers list should be woken up. Unblocks it if the condition evaluates to true, removes thread from sleepers list.
  Called by thread_tick() function with timer_ticks() as a parameter.
  
7. void thread_set_next_wakeup()
- Called after a thread is woken up from sleep. This is primarily used to unblock any threads with the same wakeup time as itself 
  (This makes it so that the thread_tick() function doesn't take too many ticks to execute
  
Following functions were modified:

1. In thread_unblock(), the unblocked thread was added into the ready list using list_insert_ordered() instead of the default list_push_back() so that the ready list is in the sorted order.

## Result 

```
suryavanshi@MSI-GE63VR-7RE:~/pintos/src/threads/build$ make check
pass tests/threads/alarm-single
pass tests/threads/alarm-multiple
pass tests/threads/alarm-simultaneous
pass tests/threads/alarm-priority
pass tests/threads/alarm-zero
pass tests/threads/alarm-negative
FAIL tests/threads/priority-change
FAIL tests/threads/priority-donate-one
FAIL tests/threads/priority-donate-multiple
FAIL tests/threads/priority-donate-multiple2
FAIL tests/threads/priority-donate-nest
FAIL tests/threads/priority-donate-sema
FAIL tests/threads/priority-donate-lower
FAIL tests/threads/priority-fifo
FAIL tests/threads/priority-preempt
FAIL tests/threads/priority-sema
FAIL tests/threads/priority-condvar
FAIL tests/threads/priority-donate-chain
FAIL tests/threads/mlfqs-load-1
FAIL tests/threads/mlfqs-load-60
FAIL tests/threads/mlfqs-load-avg
FAIL tests/threads/mlfqs-recent-1
pass tests/threads/mlfqs-fair-2
pass tests/threads/mlfqs-fair-20
FAIL tests/threads/mlfqs-nice-2
FAIL tests/threads/mlfqs-nice-10
FAIL tests/threads/mlfqs-block
19 of 27 tests failed.
../../tests/Make.tests:26: recipe for target 'check' failed
make: *** [check] Error 1

```

## Note

This is our solution for the assignment, not the standard solution. The Algorithm used may not be the best!

