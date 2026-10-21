# Pintos Priority Scheduler with Nested Priority Donation

## Project Description
This project implements a strict preemptive Priority Scheduler with nested Priority Donation in the Pintos operating system (x86). The scheduler ensures that the highest-priority ready thread always possesses CPU execution rights. To resolve unbounded priority inversion—where high-priority threads are blocked waiting for locks held by lower-priority threads—the system incorporates recursive (nested) priority donation across multiple lock wait-queues.

## Team Members
- **Mayur R Keni**
- **Hari Prannav**
- **Nitin Yajatkumar**

## Architecture Diagram

```mermaid
%%{init: {"flowchart": {"curve": "step"}}}%%
flowchart TD
    %% Custom Styles
    classDef default fill:#1f2937,stroke:#3b82f6,stroke-width:2px,color:#f9fafb;
    classDef special fill:#374151,stroke:#9ca3af,stroke-width:2px,color:#f3f4f6,stroke-dasharray: 5 5;

    %% Nodes with distinct shapes
    New([New Thread])
    Ready[[Ready List<br/>Ordered by Priority: Descending]]
    Running((Running Thread<br/>Executing on CPU))
    Blocked[(Lock Wait-Queue<br/>Blocked State)]
    Dead([Dying / Terminated])
    Donation{{Priority Donation<br/>Propagates to lock holder}}:::special

    %% Execution Flow
    New -- "thread_create()" --> Ready
    Ready -- "next_thread_to_run()<br/>Pop highest priority" --> Running
    Running -- "thread_yield()<br/>Preempted by higher priority" --> Ready
    Running -- "thread_exit()" --> Dead
    
    %% Synchronization Flow
    Running -- "lock_acquire() / sema_down()<br/>Resource unavailable" --> Blocked
    Blocked -- "lock_release() / sema_up()<br/>thread_unblock" --> Ready
    
    %% Priority Donation Flow
    Blocked -. "Priority Inversion Detected<br/>Waiter > Holder" .-> Donation
    Donation -. "Boosts Effective Priority" .-> Running
```
