# Pintos Priority Scheduler with Nested Priority Donation

## Project Description
This project implements a strict preemptive Priority Scheduler with nested Priority Donation in the Pintos operating system (x86). The scheduler ensures that the highest-priority ready thread always possesses CPU execution rights. To resolve unbounded priority inversion—where high-priority threads are blocked waiting for locks held by lower-priority threads—the system incorporates recursive (nested) priority donation across multiple lock wait-queues.

## Team Members
- **Mayur R Keni**
- **Hari Prannav**
- **Nitin Yajatkumar**

## Architecture Diagram

```mermaid
graph TD
    New[New Thread] -->|thread_create| Ready[Ready List<br/>Ordered by Priority: Descending]
    
    Ready -->|next_thread_to_run<br/>Pop highest priority| Running[Running Thread<br/>Executing on CPU]
    Running -->|thread_yield<br/>Preempted by higher priority| Ready
    Running -->|thread_exit| Dead[Dying / Terminated]
    
    Running --->|lock_acquire / sema_down<br/>Resource unavailable| Blocked[Lock Wait-Queue<br/>Blocked State]
    Blocked --->|lock_release / sema_up<br/>thread_unblock| Ready
    
    Blocked -.->|Priority Inversion Detected<br/>Waiter > Holder| Donation[Priority Donation<br/>Propagates to lock holder]
    Donation -.->|Boosts Effective Priority| Running
```
