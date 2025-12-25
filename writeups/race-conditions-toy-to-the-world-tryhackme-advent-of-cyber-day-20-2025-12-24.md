# Race Conditions - Toy to The World

---
Race conditions emerge when concurrent execution of multiple processes or threads yields inconsistent system states, as the final 
outcome is determined by the specific sequence or timing of event completion. In web applications, this vulnerability typically 
manifests when shared resources, such as database records for inventory or financial balances, are accessed and modified by 
near-simultaneous requests without sufficient synchronization. If the application logic fails to maintain state integrity across 
these parallel operations, the system may allow unauthorized data modifications, such as the processing of duplicate transactions 
or the depletion of stock beyond zero.

A common manifestation is the Time-of-Check to Time-of-Use (TOCTOU) vulnerability, where a temporal gap exists between the 
verification of a condition and the execution of an action based on that condition. During this window, an attacker can modify 
the resource state, rendering the initial check obsolete. Similarly, shared resource conflicts occur when multiple entities 
attempt to update the same data point at once; without locking mechanisms, the final write operation may overwrite previous updates, 
leading to data corruption. Atomicity violations occur when a sequence of operations that should be treated as a single, 
indivisible unit is interrupted by concurrent requests, resulting in partial or inconsistent state transitions.

Exploitation of these flaws often involves capturing a legitimate transactional request and replaying it rapidly using specialized 
tooling. By organizing identical HTTP requests into a synchronized group and utilizing techniques like last-byte synchronization, 
an attacker can ensure that the server receives the bulk of the requests at virtually the same moment. This bypasses sequential 
logic checks, as multiple threads may read the same initial stock value before any single thread has successfully committed a 
decrement operation. Successful exploitation is confirmed when the system processes more requests than the resource constraints 
should allow, often resulting in negative inventory values or multiple successful order confirmations for a single-use entitlement.

---
| Description | Code/Command |
| --- | --- |
| Target Endpoint for Race Condition | `http://MACHINE_IP/process_checkout` |
| Application Credentials | `attacker` : `attacker@123` |
| Parallel Request Technique | `Send group in parallel (last-byte sync)` |

---
What is the flag value once the stocks are negative for SleighToy Limited Edition?
- `THM{WINNER_OF_R@CE007}`

Repeat the same steps as were done for ordering the SleighToy Limited Edition. What is the flag value once the stocks are negative for Bunny Plush (Blue)?
- `THM{WINNER_OF_Bunny_R@ce}`
  
---
### Key Takeaways* Use atomic database transactions to ensure that state changes, such as stock deduction and order creation, are executed as a single, indivisible operation.
* Implement final stock validation checks immediately prior to the database commit to prevent overselling.
* Utilize idempotency keys for sensitive transactional requests to prevent the processing of duplicate submissions.
* Enforce rate limiting and concurrency controls to restrict the frequency and volume of simultaneous requests from a single
  session or user.
---

To test REST APIs for race conditions using Burp Suite's "last-byte sync" (available in the Turbo Intruder extension or the 
native Repeater tab groups), you focus on minimizing network jitter. The goal is to send the headers of multiple requests and 
hold the final byte of each until they are all ready to be released simultaneously.

### Configuration for REST API TestingWhen targeting REST endpoints, ensure your requests are identical, including the 
`Content-Length` and authentication headers. In Burp Suite, you create a Tab Group for your captured API request (e.g., 
`POST /api/v1/orders`). By selecting **Send group in parallel (last-byte sync)**, Burp opens multiple TCP connections, sends 
the majority of each HTTP request, and waits. Once all connections are "primed," it dispatches the final byte of every request 
in a single burst. This forces the server-side application threads to compete for the same database row at the exact same microsecond.

---
###Technical Execution Steps| Description | Code/Command |
| --- | --- |
| Typical API JSON Payload | `{"item_id": "123", "quantity": 1}` |
| HTTP Method for State Changes | `POST` or `PATCH` |
| Burp Suite Synchronization Feature | `Repeater` > `Send group (parallel)` |

---
### Key Takeaways* Ensure the `Connection: keep-alive` header is present to maintain open channels during the synchronization phase.
* Use at least 15–20 parallel requests to increase the probability of hitting the race window, as some threads may be delayed by
  backend latency.
* Monitor response bodies for unique identifiers; if two different responses return the same "Order ID" or "Transaction ID," the
  race condition is confirmed.
* Test endpoints that involve "Limit-Increasing" or "Resource-Depleting" logic, such as coupon applications, credit transfers, or
  inventory reservations.

---
>[!Note]
>### You did it! Wareville is one step safer.
>The townsfolk are counting on you to keep Christmas secure.
>Head back to Wareville to continue your mission!


