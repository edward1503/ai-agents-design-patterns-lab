# Design review

Team: Individual lab.
AI tool/model: OpenAI Codex (AI-assisted).

1. **Architecture:** A modular monolith fits one small team, release and database.
   SRP separates assignment policy from storage and delivery; DIP uses injected
   interfaces. Microservices could scale delivery independently, but add deployment,
   network and consistency costs.

2. **Patterns:** Strategy varies the delivery channel; registration belongs in
   composition code, preserving OCP. Decorator wraps a channel, delegates once and
   records acceptance only after success. Observer becomes useful for independent
   subscribers; one notification reaction does not require it.

3. **R6:** Commit the assignment and notification intent in a durable outbox in
   one database transaction. A relay reads pending intents, sends and marks
   completion. Crashing after provider acceptance but before marking completion
   can cause duplicate sends. A stable message ID with provider deduplication can
   control duplicates. Eventual acceptance requires durable pending work, relay
   retries, worker/provider recovery and a valid destination. This is proposed,
   not implemented.

4. **Independent evidence:** I inspected `assertEqual(email.attempts, 0)` after
   `SaveError`; it matches the diagram's no-send branch. `assertRaises(RuntimeError)`
   checks that unexpected bugs remain visible. Offline tests establish neither
   architectural quality nor real database durability, crash recovery or actual
   provider delivery. Audit events are held only in memory.
