# Multithreaded Event Reservation System

## Overview
Developed a **multithreaded reservation system** for Nehru Centre to handle:
1. **Inquiries** about available seats.
2. **Booking** tickets.
3. **Cancellation** of booked tickets.

### Key Constraints
- Maximum of `MAX` concurrent active queries.
- **Mutexes** and **condition variables** ensure mutual exclusion, preventing race conditions.

## System Description

### Main Thread
- Initializes `e` events with an auditorium capacity of `c` seats.
- Spawns `s` worker threads to generate random queries.
- Worker threads periodically perform actions (inquiry, booking, cancellation) with random parameters.

### Query Types
1. **Inquire Seats:** Check available seats for a specified event.
2. **Book Tickets:** Book `k` tickets for a specified event.
3. **Cancel Ticket:** Cancel a previously booked ticket.

### Concurrency Management
- At most `MAX` queries can be active simultaneously.
- **Condition variables** block excess queries until active ones complete.
- **Write operations** (booking/cancelling):
    - No concurrent writes for the same event.
    - Writes are blocked if there are active reads for the event.
- **Read operations:** Concurrent reads allowed unless a write is active.

### Shared Table
- A shared table tracks active queries for mutual exclusion.
- Each entry logs the event number, query type, and thread ID.
- Threads update and remove entries upon starting and finishing a query.

### Thread Behavior
- Worker threads execute random queries in a loop.
- Threads sleep between queries to simulate real-world delays.
- If blocked, threads retry after a short interval.

### Shutdown Process
- After `T` minutes, the system shuts down.
- Worker threads finish, followed by the master thread.
- Final reservation status for all events is printed.

---

## Parameters
- `e` = 100 (Number of events)
- `c` = 500 (Capacity per event)
- `k` = 5-10 (Random ticket bookings per query)
- `s` = 20 (Number of worker threads)
- `MAX` = 5 (Max concurrent queries)
- `T` = 1-10 minutes (Total runtime)

## Synchronization Tools
- **Pthreads API** for threading.
- **Mutexes** for mutual exclusion.
- **Condition variables** for controlling query limits.
- **Barriers** for synchronizing thread termination and shutdown.

