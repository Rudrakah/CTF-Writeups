# Failure Failure
| **Platform** | picoCTF 2026 |
| **Category** | General Skills |
| **Difficulty** | Medium |
| **Author** | Darkraicg492 |
| **Topic** | Load Balancing / Failover |

## Challenge
The challenge simulates a real-world failover scenario where one server is prioritized over another.

A load balancer stands between us and the flag, and it will not hand over the flag until we force it to switch to the backup server.

The challenge description says:

```text
This challenge simulates a real-world failover scenario where one server is prioritized over the other.

A load balancer stands between you and the truth — and it won't hand over the flag until you force its hand.
```

The hint is:

```text
How does a load balancer decide which server should get the traffic?
```

This points toward understanding how a load balancer handles healthy and failed backend servers.

## Step 1 – Understand the Concept

A load balancer distributes incoming requests between backend servers.

Normally, it sends traffic to the primary or healthy server.

If the primary server becomes unavailable, the load balancer can detect the failure and move traffic to another available server.

This process is called:

```text
Failover
```

The challenge is designed so that we need to generate enough requests to make the primary server fail.

## Step 2 – Generate Requests

The terminal shows repeated requests being sent to the server.

The output contains many HTTP 503 responses:

```text
[+] Got 503 at request 322
[+] Got 503 at request 323
[+] Got 503 at request 324
...
[+] Got 503 at request 350
```

HTTP status:

```text
503 Service Unavailable
```

indicates that the server is unavailable.

The repeated requests are causing the primary backend to fail.

## Step 3 – Wait for the Failover

After enough failed requests, the load balancer detects that the primary server has failed.

The important terminal output is:

```text
[*] Waiting for HAProxy to detect the failed primary...
```

HAProxy is the load balancer used by the challenge.

Once HAProxy detects the failed primary server, the challenge checks whether the backup server is now being used.

The terminal then shows:

```text
[*] Checking for the flag...
```

## Step 4 – Backup Server Takes Over

After the primary server fails, HAProxy switches the traffic to the backup server.

The output changes from repeated HTTP 503 responses to:

```text
[*] Attempt 1: HTTP 200
```

HTTP 200 means the request was successfully handled.

This confirms that the failover occurred successfully.

## Step 5 – Retrieve the Flag

The challenge automatically checks the backup server after the failover.

The terminal displays:

```text
FLAG: picoCTF{f4110v3r_f0r_7h3_w1n_b2954edb}
```

The challenge page also confirms:

```text
Correct flag!
```

## Flag

```text
picoCTF{f4110v3r_f0r_7h3_w1n_b2954edb}
```

## Important Concept – Load Balancer Failover

The main concept in this challenge is **failover**.

A typical setup looks like:

```text
             User
               |
               v
          Load Balancer
           /         \
          /           \
         v             v
   Primary Server   Backup Server
      Healthy          Standby
```

When the primary server fails:

```text
             User
               |
               v
          Load Balancer
               |
               v
        Backup Server
          Healthy
```

The load balancer detects that the primary server is unavailable and redirects traffic to the backup server.

## HTTP Status Codes

During the attack, the server repeatedly returned:

```text
503
```

which means:

```text
Service Unavailable
```

After failover, the response became:

```text
200
```

which means:

```text
OK
```

The change from:

```text
503 → 200
```

is the indication that the backup server has taken over.

## HAProxy

The challenge specifically mentions:

```text
HAProxy
```

HAProxy is commonly used as a:

- Load balancer
- Reverse proxy
- High-availability server

In this challenge, HAProxy is responsible for detecting the failed primary server and switching traffic to the backup server.

## Commands / Actions Used

The challenge's request generator sends repeated requests to the target.

The important part of the process is:

```text
Send many requests
        ↓
Primary server starts returning 503
        ↓
Continue until the primary fails
        ↓
HAProxy detects the failure
        ↓
HAProxy switches to the backup
        ↓
Backup returns HTTP 200
        ↓
Flag is revealed
```

## Tools Used

- Linux Terminal
- HTTP
- HAProxy
- Load Balancer
- Failover
- HTTP status codes

## Key Learning

- Load balancers distribute traffic between backend servers.
- Health checks allow a load balancer to detect failed servers.
- A `503` response indicates that a service is unavailable.
- When the primary backend fails, a load balancer can switch traffic to a backup server.
- A `200` response indicates that the backup server is responding successfully.
- HAProxy is commonly used for load balancing and high availability.
- Failover provides availability when the primary server becomes unavailable.

## Final Solution

```text
Start the challenge
        ↓
Send repeated requests
        ↓
Observe HTTP 503 responses
        ↓
Primary server becomes unavailable
        ↓
Wait for HAProxy to detect the failure
        ↓
HAProxy performs failover
        ↓
Traffic moves to the backup server
        ↓
Receive HTTP 200
        ↓
Flag is revealed
        ↓
picoCTF{f4110v3r_f0r_7h3_w1n_b2954edb}
```

## Flag

```text
picoCTF{f4110v3r_f0r_7h3_w1n_b2954edb}
```
