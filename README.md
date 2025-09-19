

# README – Event Rewards Smart Contract

## Overview

This smart contract enables **decentralized event management on Stacks blockchain**.
It allows organizers to create events, attendees to check in/out, and verifiers to confirm participation. Rewards can then be distributed based on verified attendance and duration.

The system is designed to ensure fairness, transparency, and immutability of event data while enforcing strict validations for event setup and participation.

---

## ✨ Key Features

* **Event Creation** – Organizers can create events with:

  * Name & description (validated for length and format).
  * Start and end block height.
  * Base and bonus reward amounts.
  * Minimum attendance duration requirement.

* **Attendance Tracking** –

  * Attendees **check in** at the start of the event.
  * Attendees **check out** to log their duration.
  * Each attendee can only register once per event.

* **Reward System** –
  Rewards are distributed based on participation duration and verification.
  Claims are recorded to prevent double-claiming.

* **Verification Layer** –
  Authorized verifiers can confirm attendance records for authenticity.

* **Admin Controls** –
  The contract owner has exclusive authority to:

  * Create new events.
  * Register verifier addresses.

---

## 📂 Data Structures

### Events

```clarity
{ 
  name: string-ascii(50),
  description: string-ascii(200),
  start-height: uint,
  end-height: uint,
  base-reward: uint,
  bonus-reward: uint,
  min-attendance-duration: uint,
  organizer: principal,
  is-active: bool
}
```

### Attendance

```clarity
{
  check-in-height: uint,
  check-out-height: uint,
  duration: uint,
  verified: bool
}
```

### Verification

```clarity
{
  verified-by: principal,
  verified-at: uint
}
```

### Rewards Claimed

```clarity
{
  amount: uint,
  claimed-at: uint,
  reward-tier: uint
}
```

---

## 🚀 Public Functions

### `create-event`

Creates a new event with validation checks.
Only the contract owner can call this function.

### `check-in`

Registers attendance at the start of an event.
Fails if the event hasn’t started, already ended, or user already registered.

### `check-out`

Logs check-out height and calculates total attendance duration.
Fails if the event has ended or if duration is invalid.

---

## 🔍 Read-Only Functions

* `get-owner` → Returns the contract owner.
* `get-event (id)` → Fetches event details.
* `get-attendance-record (id, attendee)` → Retrieves attendance record.
* `get-reward-claim (id, attendee)` → Fetches claimed reward details.
* `is-verifier (address)` → Checks if a principal is an authorized verifier.
* `event-exists (id)` → Returns true if an event exists.

---

## ⚠️ Error Codes

| Code    | Error                       |
| ------- | --------------------------- |
| `u100`  | Not authorized              |
| `u101`  | Already claimed             |
| `u102`  | Event not started           |
| `u103`  | Event ended                 |
| `u104`  | No reward available         |
| `u105`  | Event not found             |
| `u106`  | Insufficient funds          |
| `u107`  | Invalid duration            |
| `u108`  | Already registered          |
| `u2000` | Invalid name                |
| `u2001` | Invalid description         |
| `u2002` | Contains invalid characters |
| `u110`  | Invalid start height        |
| `u111`  | Invalid reward              |
| `u112`  | Invalid min attendance      |

---

## 🛠️ Example Flow

1. **Owner creates event**

   ```clarity
   (create-event "Blockchain Meetup" "Learn Clarity" 20000 500 1000 200 100)
   ```

2. **User checks in**

   ```clarity
   (check-in u1)
   ```

3. **User checks out**

   ```clarity
   (check-out u1)
   ```

4. **Verifier confirms attendance (future implementation)**

   ```clarity
   ;; pseudo example
   (verify-attendance u1 'ST...123)
   ```

5. **User claims reward**

   ```clarity
   (claim-reward u1)
   ```

---

## 🔮 Future Improvements

* Reward distribution mechanism with treasury deductions.
* Multi-tier rewards based on duration thresholds.
* Penalty for false attendance claims.
* Organizer-defined verifier assignment.

---

## 📜 License

This project is licensed under the MIT License.

---
