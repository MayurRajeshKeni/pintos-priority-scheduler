# Pintos Priority Scheduler with Nested Priority Donation

## Project Description
This project implements a strict preemptive Priority Scheduler with nested Priority Donation in the Pintos operating system (x86). The scheduler ensures that the highest-priority ready thread always possesses CPU execution rights. To resolve unbounded priority inversion—where high-priority threads are blocked waiting for locks held by lower-priority threads—the system incorporates recursive (nested) priority donation across multiple lock wait-queues.

## Team Members
- **Mayur R Keni**
- **Hari Prannav**
- **Nitin Yajatkumar**

## Architecture Diagram

```mermaid
flowchart TD
    NEW["New Thread"] -->|"thread_create()"| READY["Ready List\n(Ordered by Priority: Descending)"]
    
    READY -->|"next_thread_to_run()\n(Pop highest priority front)"| RUNNING["Running Thread\n(Executing on CPU)"]
    
    RUNNING -->|"thread_yield()\n(Preempted by higher priority thread or priority lowered)"| READY
    
    RUNNING -->|"lock_acquire() / sema_down()\n(Resource unavailable)"| BLOCKED["Lock Wait-Queue\n(Blocked State)"]
    
    BLOCKED -->|"lock_release() / sema_up()\n(thread_unblock)"| READY
    
    BLOCKED -.->|"Priority Inversion Detected\n(Waiter priority > Holder priority)"| DONATION["Priority Donation\n(Propagates nested to current lock holder)"]
    
    DONATION -.->|"Boosts Effective Priority"| RUNNING
    
    RUNNING -->|"thread_exit()"| DYING["Dying / Terminated"]
```
