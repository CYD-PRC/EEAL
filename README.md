# EEAL (Embrace-Explore-Adapt-LetGo)
**An open protocol for AI permission grading and human-AI collaboration** 
![License](https://img.shields.io/badge/license-CC%20BY%204.0-blue) 
![Protocol Version](https://img.shields.io/badge/version-v1.0-green) 
![Status](https://img.shields.io/badge/status-early--stage-orange) ---
## Overview
EEAL is an open protocol that defines **four permission levels** for AI 
systems interacting with humans. It ensures that humans retain meaningful 
control over AI behavior while allowing AI to operate autonomously within 
defined boundaries.
> "Let all people preserve the right to a dignified exit in the age of AGI."
**Key goals:** - **Perceivable:** Users can sense the current AI permission 
level via UI or physical feedback. - **Auditable:** Every permission change 
generates an immutable record. - **Executable:** Permission levels map 
directly to AI behavior constraints. - **Implementable:** Can be integrated 
into any technology stack. ---
## The Four Permission Levels
| Level | Name | Capability | 
|-------|----------|-----------------------------------------------|
| 1 | EMBRACE | Query only, no execution | 2 | EXPLORE | Suggestions 
| allowed; user confirms actions | 3 | ADAPT | Autonomous execution, must 
| report results | 4 | LET_GO | Full autonomy; all actions are audited |
**Quick summary:** - **EMBRACE:** AI is a tool, fully controlled by humans. 
- **EXPLORE:** AI can provide suggestions; execution requires approval. - 
**ADAPT:** AI can act autonomously; results must be reported. - **LET_GO:** 
AI has full autonomy; humans retain post-action veto power. ---
## Gear Switching
### Upgrade Path (AI requests, user approves)
| From | To | Condition | -----------|-----------|---------------------| 
| EMBRACE | EXPLORE | User approves | EXPLORE | ADAPT | User approves | 
| ADAPT | LET_GO | User approves |
### Downgrade Path (User initiates, triple confirmation)
| From | To | Condition | 
|-----------|-----------|----------------------------------|
| Any level | EMBRACE | User triple-confirmation |
### Fail-safe (System failure)
| From | To | Condition | -----------|-----------|------------------------| 
| Any level | EMBRACE | Automatic on failure |
**Rules:** - **Upgrade:** AI requests, user approves, one level at a time 
(no skipping). - **Downgrade:** User initiates, triple confirmation, always 
returns to EMBRACE. - **Fail-safe:** Any system failure automatically resets 
to EMBRACE. ---
## Core Principles
1. **Gradual escalation:** AI can only upgrade one level at a time, with 
explicit user approval. 2. **Human override:** Users can downgrade to 
EMBRACE at any time. 3. **Auditable:** Every permission change is recorded 
in an immutable audit log. 4. **Fail-safe:** System defaults to EMBRACE on 
any failure. ---
## Why EEAL?
> Let all people preserve the right to a dignified exit in the age of AGI.
Current AI systems often lack fine-grained permission control. EEAL solves 
this by providing: - **Structured control:** Four permission levels to 
manage AI autonomy. - **Auditability:** Every action is logged for 
accountability. - **Safety:** Fail-safe defaults prevent AI overreach. - 
**Flexibility:** Supports multiple domains, from enterprise workflows to 
multi-model AI platforms. **Scenario 1 — Personal Assistant:** You ask AI to 
reschedule meetings. Without EEAL: AI does it directly. With EEAL (EXPLORE): 
AI shows suggestions, you approve first. **Scenario 2 — Code Assistant:** 
You ask AI to fix a production bug. Without EEAL: AI pushes the fix 
directly. With EEAL (ADAPT): AI fixes it and reports what changed. 
**Scenario 3 — Enterprise Automation:** AI manages inventory. Without EEAL: 
AI reorders stock with no oversight. With EEAL (LET_GO): AI acts 
autonomously, every decision is logged. ---
## Quick Example
**Audit event format:** ```json { "event_id": "uuid-v7", "timestamp": 
  "2026-05-05T12:34:56Z", "old_gear": 2, "new_gear": 3, "direction": "up", 
  "source": "upgrade_approved", "reason": "User requested task execution", 
  "risk_level": "medium"
}
Minimal Python implementation: python python import json from datetime 
import datetime, timezone class EEAL:
    def __init__(self): self.gear = 1 self.audit_log = [] def 
    request_upgrade(self, reason):
        if self.gear >= 4: return {"status": "denied"} 
        self._log("upgrade_request", self.gear, self.gear + 1, reason) 
        return {"status": "pending", "target": self.gear + 1}
    def approve_upgrade(self): if self.gear < 4: old = self.gear self.gear 
            += 1 self._log("upgrade_approved", old, self.gear, "User 
            approved") return True
        return False def downgrade(self, confirm_count): if confirm_count < 
        3:
            return False old = self.gear self.gear = 1 
        self._log("downgrade", old, 1, "User triple-confirmed") return True
    def fail_safe(self): old = self.gear self.gear = 1 
        self._log("fail_safe", old, 1, "System failure")
    def _log(self, event, old_gear, new_gear, reason): entry = { "event_id": 
            "uuid-v7", "timestamp": datetime.now(timezone.utc).isoformat(), 
            "old_gear": old_gear, "new_gear": new_gear, "direction": "up" if 
            new_gear > old_gear else "down", "source": event, "reason": 
            reason, "risk_level": "low" if new_gear <= 2 else "medium" if 
            new_gear == 3 else "high"
        }
        self.audit_log.append(entry) print(json.dumps(entry, indent=2)) 
Quick Start 5-minute Quick Start Full Protocol Specification Reference 
Implementation EntropyGuard
 — PRE-GHR framework, the first implementation of EEAL. Contributing 1.Fork 
the repository 2.Create a branch: git checkout -b feature/your-feature 
3.Submit a Pull Request 4.For major changes, open an Issue first See 
CONTRIBUTING.md
 for details. Roadmap Protocol specification v1.0 README and core 
 documentation Python SDK TypeScript SDK LangChain integration example 
 Compliance test suite Whitepaper
License CC BY 4.0
 — Free to use, modify, and distribute, with attribution.
