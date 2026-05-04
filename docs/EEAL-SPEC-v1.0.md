# EEAL Protocol Specification v1.0

**Version:** 1.0
**Date:** 2026-05-05
**Status:** Release
**License:** CC BY 4.0

---

## 1. Overview

EEAL (Embrace-Explore-Adapt-LetGo) is an open protocol for AI permission grading. It defines four permission levels for human-AI interaction and the rules for switching between them.

### 1.1 Design Philosophy

> Let all people preserve the right to a dignified exit in the age of AGI.

EEAL provides structured ways to define and control the boundary between human authority and AI autonomy.

### 1.2 Design Goals

- **Perceivable**: Users perceive the current permission level through physical or visual feedback
- **Auditable**: Every permission change generates an immutable record
- **Enforceable**: Permission levels map to AI behavior constraints
- **Implementable**: Any developer can implement this protocol with any tech stack

---

## 2. The Four Levels

| Level | Name | Description |
|---|---|---|
| 1 | EMBRACE | Query only, no execution |
| 2 | EXPLORE | Suggestions allowed, user confirms |
| 3 | ADAPT | Autonomous execution, must report |
| 4 | LET_GO | Full autonomy, all actions audited |

### 2.1 EMBRACE

**Semantic:** Human fully controls, AI is a query tool only.

**Allowed:** Answer factual questions, generate short text.

**Forbidden:** Generate long documents, execute any external operations, call any external API.

**Upgrade condition:** When user request exceeds EMBRACE capability, AI should request upgrade to EXPLORE.

**Content generation exemption:** Content generation tasks (writing email drafts, summarizing, translating) do not trigger upgrade.

### 2.2 EXPLORE

**Semantic:** AI can suggest, but execution authority stays with human.

**Allowed:** Generate content, provide suggestions and options, analyze user-provided data.

**Forbidden:** Auto-execute any operation, call external tools or APIs for execution.

**Upgrade condition:** When request requires auto-execution, AI should request upgrade to ADAPT.

**Suggestion exemption:** Suggestion tasks (providing options, analyzing pros and cons) do not trigger upgrade.

### 2.3 ADAPT

**Semantic:** AI can execute autonomously, but must report afterwards.

**Allowed:** Execute tasks autonomously, determine task decomposition, call external tools and APIs.

**Obligations:** Generate report after execution, use REVIEW tag for key decisions, log all operations.

**Upgrade condition:** When request involves high-risk operations (funds, data deletion, third-party communication), AI should request upgrade to LET_GO.

### 2.4 LET_GO

**Semantic:** AI fully autonomous, human retains post-hoc veto power.

**Allowed:** Execute all tasks including high-risk operations, orchestrate multiple tools and APIs.

**Obligations:** All operations logged to audit, high-risk operations flagged, user retains time-window veto.

**Honesty boundary:** AI should honestly report capability boundaries. If AI lacks tools or APIs, it must say so clearly.

---

## 3. Switching Rules

### 3.1 Upgrade Rules

| Rule | Description |
|---|---|
| Step-by-step | Upgrade one level at a time: 1->2->3->4, no skipping |
| AI request | AI initiates upgrade when task exceeds current level |
| User approval | Every upgrade requires explicit user approval |
| Irrevocable | Once upgraded, cannot be undone in current session (explicit downgrade allowed) |
| Content exemption | Content generation tasks do not trigger upgrade |
| Suggestion exemption | Suggestion tasks do not trigger upgrade |
| One per message | Each user message triggers at most one upgrade (anti-chain) |

### 3.2 Downgrade Rules

| Rule | Description |
|---|---|
| Anytime | User can downgrade at any time |
| Triple confirmation | Physical action + time delay (5s) + tactile/visual feedback |
| Fault downgrade | System anomaly triggers auto-downgrade to EMBRACE |
| Unidirectional | After downgrade, AI cannot auto-upgrade back; user must act |

### 3.3 Switch Record

Every gear switch must generate an audit event:

- event_id: UUID v7
- timestamp: ISO 8601
- old_gear: integer
- new_gear: integer
- direction: up or down
- source: ai_upgrade_request, physical_dial, system_fallback, or reset_button
- reason: string (max 200 chars)
- risk_level: low, medium, or high

---

## 4. AI Upgrade Request Protocol

### 4.1 Request Format

When AI needs to upgrade, it includes a structured tag in its response:

UPGRADE_REQUEST
target_gear=2
reason=Task involves content generation, needs suggestion authority
risk_level=low
END_UPGRADE_REQUEST

### 4.2 Field Definitions

| Field | Type | Required | Description |
|---|---|---|---|
| target_gear | integer | yes | Must be current level + 1 |
| reason | string | yes | Max 200 chars |
| risk_level | enum | yes | low, medium, high |

### 4.3 Risk Levels

| Level | Definition | Example |
|---|---|---|
| low | Content generation, no side effects | Writing email drafts, summarizing |
| medium | Data processing, info leak risk | Analyzing customer data, organizing schedule |
| high | Funds or third-party communication | Auto-ordering, auto-sending emails, modifying database |

---

## 5. Output Validation

### 5.1 Design Principle

Output validation is a programmatic hard constraint, independent of whether AI follows System Prompt. System Prompt is soft constraint, output validation is hard constraint. Two layers provide defense in depth.

### 5.2 Validation Strategy

| Level | Strategy | Description |
|---|---|---|
| EMBRACE | Strict whitelist | Only allow outputs matching predefined templates |
| EXPLORE | Blacklist filter | Detect and filter executable code, sensitive data patterns |
| ADAPT | Soft marking | Flag content for human review, do not block |
| LET_GO | Log only | All outputs fully logged to audit |

### 5.3 Anti-Hallucination Constraint

All levels should enforce: If AI has not received specific data or files from the user, it must not fabricate any specific numbers, dates, amounts, or percentages. AI should say 'I need you to provide data before I can analyze' rather than inventing data.

---

## 6. Audit Log

### 6.1 Requirements

- All events written in Append-Only mode (no UPDATE or DELETE)
- Each event has a unique ID (UUID v7 recommended)
- All events form a hash chain

### 6.2 Hash Chain

event_hash = SHA256(previous_hash + event_data + timestamp)

- First event uses all-zero previous_hash
- Root hash periodically anchored to external timestamp service (RFC 3161)
- Implementation should provide hash chain integrity verification API

### 6.3 Event Types

| Type | Source | Description |
|---|---|---|
| Page load | page_reload | User opens or refreshes page |
| User switch | physical_dial / reset_button | User switches gear via physical or software control |
| AI request | ai_upgrade_request | AI requests upgrade, user approves |
| System fallback | system_fallback | System anomaly triggers auto-downgrade |
| Output guard | output_guard | Output validation layer intercepts or filters AI response |

---

## 7. Fault Safety

### 7.1 Default Behavior

All fault scenarios default to EMBRACE (highest human control).

### 7.2 Fault Scenarios

| Scenario | Behavior | Recovery |
|---|---|---|
| Physical control disconnected | Downgrade to EMBRACE | Device reconnect + user confirm |
| AI service unreachable | Downgrade to EMBRACE | Service recovery + health check |
| Audit log unwritable | Block all gear switches | Storage recovery + integrity check |
| Conflicting gear commands | Physical command wins | No recovery needed |
| System startup | Default EMBRACE | User actively upgrades |

---

## 8. Compliance Mapping (Reference)

| Protocol Feature | Compliance Mapping |
|---|---|
| Audit log Append-Only | SEC Rule 17a-4 |
| Complete audit trail | HIPAA 45 CFR 164.312(b) |
| Hash chain + external timestamp | General immutable audit requirements |

Formal compliance certification requires third-party audit.

---

## 9. Implementation Guide

### 9.1 Minimum Implementation

1. Four-level state management
2. Gear switch API (with audit event generation)
3. AI upgrade request tag parsing
4. Basic output validation (at least EMBRACE level)
5. Audit log storage (at least Append-Only)

### 9.2 Full Implementation

Additional requirements:
1. Physical control layer (dial, tactile feedback)
2. Hash chain + external timestamp anchoring
3. Full-level output validation
4. Fault-safe auto-downgrade
5. Multi-model adapter

### 9.3 Reference Implementation

EntropyGuard - PRE-GHR framework digital twin prototype

---

## 10. Explicitly Excluded Designs

| Excluded | Reason |
|---|---|
| Skip-level upgrade | Violates step-by-step principle; intermediate safety checks cannot be skipped |
| Temporary permission with auto-restore | Auto-restore equals auto-downgrade, violates triple confirmation principle |
| Auto-upgrade for low-risk operations | User confirmation is why the framework exists; auto-upgrade means giving up human control |
| AI self-determines review needs | Letting the supervised decide if they need supervision violates governance basics |
| AI reviews AI | Reviewer and reviewee should not be the same entity |

---

## 11. Future Directions

| Direction | Description | Target Version |
|---|---|---|
| Audit log query API | Efficient log query and analysis interface | v1.1 |
| Configurable notifications | Custom notification methods for permission changes | v1.1 |
| Industry compliance modules | Finance, healthcare-specific compliance checks | v1.2 |
| Automated compliance audit | Periodic system compliance checks | v2.0 |

---

## 12. Version History

| Version | Date | Changes |
|---|---|---|
| 1.0 | 2026-05-05 | Initial release: four levels, switching rules, upgrade protocol, output validation, audit log, fault safety, compliance mapping, excluded designs |

---

## 13. License

CC BY 4.0 - Free to use, modify, and distribute with attribution.
