# EEAL Quick Start

## 5-minute overview

### What problem does EEAL solve?

When you use an AI assistant, you have no structured way to control what it can and cannot do. EEAL defines four permission levels that map AI behavior to explicit constraints.

### The four levels in plain language

1. **EMBRACE**: AI answers questions. That is it.
2. **EXPLORE**: AI can write, suggest, and analyze. But it asks before doing anything.
3. **ADAPT**: AI can act on its own. But it tells you what it did.
4. **LET_GO**: AI acts freely. Everything is logged.

### How does a user switch levels?

- **Upgrade**: AI requests permission when it needs a higher level. User approves.
- **Downgrade**: User can pull back to a lower level at any time.

### How to implement EEAL in your system

**Minimum viable implementation (30 minutes):**

1. Add a gear variable to your system (1-4)
2. Inject different System Prompts based on the current gear
3. Log every gear change as an event
4. On any error, reset gear to 1

**That is it.** You now have a basic EEAL-compliant system.

### What you need for full compliance

- Output validation layer (programmatic checks on AI output)
- Hash-chain audit log (immutable, cryptographically verifiable)
- Fault-safe defaults (auto-downgrade to EMBRACE on failure)
- AI upgrade request parsing (UPGRADE_REQUEST tags)

### Next steps

- Read the full protocol specification (EEAL-SPEC-v1.0.md)
- Try the reference implementation (EntropyGuard)
- Open an issue with feedback
