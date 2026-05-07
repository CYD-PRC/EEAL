# EEAL Protocol (Embrace-Explore-Adapt-LetGo)

**License:** CC BY 4.0
**Organization:** EESCF

An open protocol for AI permission grading and human-AI collaboration.

---

## Table of Contents

* Overview
* Four-Level Permissions
* Gear Shifts (Upgrade/Downgrade)
* Quick Example
* Motivation
* Reference Implementation
* Contributing
* Roadmap
* License

---

## Overview

EEAL defines four AI permission levels to structure human-AI interaction and ensure humans retain meaningful control while allowing AI autonomy.

*"Preserve a dignified exit for everyone in the era of advanced AI systems."*

### Main Objectives:

* **Perceivable:** Users can sense the AI permission level via UI or physical feedback.
* **Auditable:** Every permission change generates an immutable record.
* **Executable:** Permission levels directly map to AI behavior constraints.
* **Implementable:** Can be implemented across any tech stack.

### Conceptual Operation Cost:

* Each gear shift has a conceptual operation cost, measured in steps, logs, or approval time.
* Emphasizes careful decision-making and conceptual irreversibility, not energy consumption.

---

## Four-Level Permissions

| Level | Name    | Capability                                      |
| ----- | ------- | ----------------------------------------------- |
| 1     | Embrace | Read-only queries, no execution                 |
| 2     | Explore | Suggestions allowed, user confirmation required |
| 3     | Adapt   | Autonomous execution, must report results       |
| 4     | LET_GO  | Fully autonomous, all actions auditable         |

Each gear shift records a related conceptual operation cost.

---

## Gear Shifts (Upgrade/Downgrade)

**Rules:**

* Upgrade: AI requests → User approval → One-level increase
* Downgrade: User can downgrade anytime → Recommended triple confirmation
* Fail-Safe: Defaults to EMBRACE if any operation fails
* Each shift increases the conceptual operation cost

---

## Quick Example

**Audit Event Example (JSON):**

```json
{
  "event_id": "uuid-v7",
  "timestamp": "2026-05-05T12:34:56Z",
  "old_gear": 2,
  "new_gear": 3,
  "direction": "up",
  "source": "ai_upgrade_request",
  "reason": "User requested task execution",
  "operation_cost": 3,
  "risk_level": "medium"
}
```

**Minimal Python Example:**

```python
class EEAL:
    def __init__(self):
        self.gear = 1

    def upgrade(self):
        if self.gear < 4:
            self.gear += 1
            print(f"Gear upgraded to {self.gear}, operation_cost=2")
        else:
            print("Already at LET_GO")

    def downgrade(self):
        if self.gear > 1:
            self.gear -= 1
            print(f"Gear downgraded to {self.gear}, operation_cost=1")
        else:
            print("Already at EMBRACE")

ai = EEAL()
ai.upgrade()   # Gear upgraded to 2, operation_cost=2
ai.downgrade() # Gear downgraded to 1, operation_cost=1
```

---

## Motivation

* Structured control: Four levels manage AI autonomy
* Auditable: All actions are logged
* Safe: Fail-safe defaults to EMBRACE
* Flexible: Supports enterprise workflows and multi-model AI platforms
* Conceptual operation cost: Emphasizes irreversibility of permission changes

---

## Reference Implementation

* Python + FastAPI + WebSocket prototype
* Supports device management, AI calls, upgrade requests, and audit logs
* Every gear shift records a conceptual operation cost

---

## Contributing

* Fork the repository
* Create a feature branch (`feature/your-feature`)
* Submit a pull request
* Discuss major changes via Issues or Discussions
* See `CONTRIBUTING.md` for full guidelines

---

## Roadmap

* v1.1: Audit log query API, customizable notifications
* v1.2: Industry compliance modules (Finance, Healthcare)
* v2.0: Automated compliance auditing, multi-model support

---

## License

EEAL is licensed under the **Creative Commons Attribution 4.0 International License (CC BY 4.0)**.
You are free to share, copy, redistribute, remix, transform, and build upon the material for any purpose, even commercially, provided you give appropriate credit.

© 2026 EESCF

Full license text: [https://creativecommons.org/licenses/by/4.0/legalcode](https://creativecommons.org/licenses/by/4.0/legalcode)
