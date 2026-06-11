<div align="center">

```
      ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
      ░                                                          ░
      ░    ██████╗    ███████╗   ████████╗  ██╗  ██╗   ███████╗ ░
      ░   ██╔══██╗   ██╔════╝   ╚══██╔══╝  ██║  ██║   ██╔════╝ ░
      ░   ███████║   █████╗        ██║     ███████║   █████╗    ░
      ░   ██╔══██║   ██╔══╝        ██║     ██╔══██║   ██╔══╝    ░
      ░   ██║  ██║   ███████╗      ██║     ██║  ██║   ███████╗  ░
      ░   ╚═╝  ╚═╝   ╚══════╝      ╚═╝     ╚═╝  ╚═╝   ╚══════╝  ░
      ░                                                          ░
      ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
```

**The backend language the internet should have been built with.**

[![License](https://img.shields.io/badge/license-Apache%202.0-00D9FF?style=flat-square)](LICENSE)
[![TypeScript](https://img.shields.io/badge/TypeScript-strict-7C3AED?style=flat-square)](https://typescriptlang.org)
[![Build](https://img.shields.io/badge/build-passing-10B981?style=flat-square)]()
[![Zero Dependencies](https://img.shields.io/badge/deps-zero-10B981?style=flat-square)]()
[![Status](https://img.shields.io/badge/status-alpha-F59E0B?style=flat-square)]()

[Docs](#language-reference) · [Quick Start](#quick-start) · [Examples](#examples) · [Community](#community)

</div>

---

## The problem no one wants to admit

Every backend system built today is, at its core, a stack of workarounds.

You write a schema in one language, a query in another, validation rules in a third, API logic in a fourth — then glue them together with config files, environment variables, and a prayer. You're not building a product. You're managing infrastructure.

The internet was built this way because it had to be. We were making it up as we went. But that was decades ago.

**AETHER is a complete restart.** Not a framework on top of what exists. Not an abstraction. A new model, designed for what backend systems actually need to do — built to outlast every tool you're currently using.

---

## What AETHER is

AETHER is a **hybrid programming language** and **autonomous runtime** for building backend systems.

It handles everything — data persistence, event routing, API contracts, security, and network communication — as unified, first-class language constructs. Not as libraries you import. Not as services you configure. As the language itself.

```
stream users quantum transparent {

  on register(req) {
    let user = validate req {
      email:    Email.format,
      password: Password.strong(min: 8),
      name:     String.min(2)
    }

    user.password = await hash(user.password)

    flow.users <- user

    emit signal.welcome(user.id)

    yield Response.created(token: sign(user.id))
  }

  on error(e) transparent {
    yield Response.error(e.trace)
  }

}
```

This is a complete user registration system.

No imports. No database client. No ORM. No HTTP framework. No middleware chain. No token library. **No dependencies at all.**

The `quantum` modifier enables post-quantum cryptographic security for the entire stream. `transparent` exposes full execution traces for debugging and auditing. `flow.users <- user` persists to the event stream. `emit signal.welcome` fires a real-time signal to any listener. Everything else is self-evident.

This is what backend code should look like.

---

## Three input modes. One language.

AETHER is the same language in three different forms. You choose how you think. The system handles the rest.

<table>
<tr>
<th>Code Mode</th>
<th>Visual Mode</th>
<th>Natural Mode</th>
</tr>
<tr>
<td>

```aether
stream auth {
  on login(req) {
    let user = validate req {
      email: Email.format,
      password: Password.strong
    }
    yield Response.ok(
      token: sign(user.id)
    )
  }
}
```

</td>
<td>

```
[HTTP Request]
     │
     ▼
[Validate: email + pass]
     │ ✓          │ ✗
     ▼             ▼
[sign(user.id)]  [400]
     │
     ▼
[Response.ok(token)]
```

</td>
<td>

```
"When a login request arrives,
validate the email and password.
If valid, return a signed token.
If invalid, return an error."
```

</td>
</tr>
</table>

All three compile to the same bytecode. Switch modes mid-session. Export any view of any system. The language is the spec, the diagram, and the documentation — simultaneously.

---

## Architecture

AETHER is built in seven independent layers. Each layer is a standalone module. Each module is fully functional on its own and replaceable without touching the others.

```
  ┌─────────────────────────────────────────────────────────┐
  │                   UNIVERSAL SHELL                        │
  │            Browser · Desktop · CLI · Embedded           │
  ├─────────────────────────────────────────────────────────┤
  │                   LANGUAGE CORE                          │
  │         Lexer · Parser · AST · Code Generators          │
  ├─────────────────────────────────────────────────────────┤
  │                   RUNTIME ENGINE                         │
  │      Executor · EventBus · Builtins · JIT Compiler      │
  ├─────────────────────────────────────────────────────────┤
  │                    FLOW DATA                             │
  │    Append-Only Streams · Time Travel · Reactive Queries  │
  ├─────────────────────────────────────────────────────────┤
  │                   NETWORK MESH                           │
  │       Auto-Routing · P2P · HTTP/3 · Custom Protocol     │
  ├─────────────────────────────────────────────────────────┤
  │                  QUANTUM GUARD                           │
  │     CRYSTALS-Kyber · Dilithium · Zero-Trust · DID       │
  ├─────────────────────────────────────────────────────────┤
  │                 SELF-EVOLUTION                           │
  │     Usage Learning · Pattern Mining · Auto-Optimize     │
  └─────────────────────────────────────────────────────────┘
```

No layer depends on anything above it. Data flows one direction: down. The result is a system where any layer can be upgraded, replaced, or extended without touching a single line of the layers around it.

---

## Data model

Traditional databases store state. AETHER stores **history**.

```aether
// Write — append an event to the stream
flow.users <- { name: "Alice", email: "alice@example.com" }

// Read current state
flow.users.latest(10)

// Query
flow.users.where(u => u.active == true)

// Time travel — what did the users stream look like last Tuesday?
flow.users.at("2024-06-01T00:00:00Z")

// React to changes
flow.users.subscribe(user => emit signal.newUser(user.id))
```

Nothing is ever deleted. Nothing is ever mutated. Every write is an immutable event appended to an ordered log. Current state is a reduction of that log — computed on read, not stored. This gives you:

- **Instant audit trail** — every change, who made it, when, and from what state
- **Time travel** — reconstruct any historical state without backups
- **Zero migration** — schema evolution is a query, not an ALTER TABLE
- **Reactive by default** — `.subscribe()` works on any stream

---

## Security model

The `quantum` modifier is not cosmetic. It activates CRYSTALS-Kyber (ML-KEM) for key encapsulation and CRYSTALS-Dilithium (ML-DSA) for digital signatures — both NIST-standardized post-quantum algorithms, implemented in pure TypeScript with zero native dependencies.

```aether
stream payments quantum {
  on transfer(req) {
    // Entire handler: encrypted in transit, signed at rest,
    // zero-trust verified, audit-logged — automatically.
    let transfer = validate req {
      amount:    Int.min(1),
      recipient: UUID.format
    }
    flow.transfers <- transfer
    emit signal.payment.processed(transfer.id)
    yield Response.created(transfer.id)
  }
}
```

Security in AETHER is not a library you add. It is a property of the code you write. The `quantum` modifier propagates through every data write, every signal, and every response in its scope. You cannot forget to encrypt something. The language makes it impossible.

---

## Performance

| Operation | Target | How |
|-----------|--------|-----|
| Event throughput | ≥ 10,000 events/sec per stream | Lock-free ring buffer, Worker pool |
| Flow write | ≥ 100,000 events/sec | Append-only log, no locking |
| Parser | ≥ 100,000 lines/sec | Single-pass, no backtracking |
| Local latency | < 1ms | In-process transport, zero serialization |
| LAN latency | < 10ms | QUIC / HTTP/3 transport |
| Cold start | < 50ms | No framework init, direct runtime boot |

---

## Quick Start

```bash
npm install -g @aether/language
```

Create `api.ae`:

```aether
stream api {
  on hello(req) {
    yield Response.ok(message: "AETHER is running")
  }
}
```

Run it:

```bash
aether run api.ae
```

```
▶ AETHER Runtime — api.ae
  stream api
    on hello(req)

── Listening ──
```

Check syntax:

```bash
aether check api.ae
# ✓ No errors found
#   stream api — 1 handler(s)
#     on hello(req) — 1 statement(s)
```

Compile to JavaScript:

```bash
aether build api.ae dist/api.js
# ✓ Compiled → dist/api.js
#   38 lines of JavaScript
```

Natural language:

```bash
aether natural "when a user registers, validate email and password, save them, send a welcome signal"
```

```aether
stream users quantum {
  on register(req) {
    let data = validate req {
      email:    Email.format,
      password: Password.strong
    }
    flow.users <- data
    emit signal.register_complete(data.id)
    yield Response.created(token: sign(data.id))
  }
}
```

Interactive REPL:

```bash
aether repl
# AETHER REPL v1.0
# Type AETHER code, press Enter twice to run.
aether> stream health { on ping(req) { yield Response.ok(status: "alive") } }
# ✓ Valid AETHER
```

---

## Language Reference

### Streams

The fundamental unit in AETHER. A stream is a named, long-lived process that listens for events and handles them.

```aether
stream <name> [modifiers] {
  on <event>(<params>) [modifiers] {
    <statements>
  }
}
```

### Modifiers

| Modifier | Effect |
|----------|--------|
| `quantum` | Enables post-quantum encryption for all I/O in scope |
| `transparent` | Exposes full execution trace (statements, values, timing) |
| `parallel` | Runs handlers concurrently instead of sequentially |
| `cached(ttl)` | Memoizes handler output for `ttl` seconds |

Modifiers compose. `quantum transparent` is valid. `quantum parallel cached(30)` is valid.

### Types

| Type | Constraints |
|------|-------------|
| `Email` | `.format` |
| `Password` | `.strong(min: N)` |
| `String` | `.min(N)` `.max(N)` `.pattern(regex)` |
| `Int` | `.min(N)` `.max(N)` `.positive` |
| `Float` | `.min(N)` `.max(N)` |
| `URL` | `.format` `.https` |
| `UUID` | `.format` |
| `Date` | `.future` `.past` `.after(date)` |
| `Bool` | — |

### Statements

```aether
// Bind a value
let user = validate req { email: Email.format }

// Write to a stream
flow.users <- user

// Assign to a property
user.password = await hash(user.password)

// Emit a signal
emit signal.welcome(user.id)

// Yield a response and end the handler
yield Response.created(token: sign(user.id))

// Branch
if (user.active == true) {
  yield Response.ok(user)
} else {
  yield Response.error("Account suspended")
}
```

### Flow queries

```aether
flow.users.latest(50)
flow.users.where(u => u.role == "admin")
flow.users.since("2024-01-01")
flow.users.at("2023-06-15T12:00:00Z")
flow.users.map(u => u.email)
flow.users.reduce((count, _) => count + 1, 0)
flow.users.subscribe(u => emit signal.newUser(u.id))
```

### Type declarations

```aether
type User = {
  email:    Email,
  password: String,
  name:     String,
  role:     String
}
```

---

## Examples

### Authentication system

```aether
stream auth quantum {

  on register(req) {
    let user = validate req {
      email:    Email.format,
      password: Password.strong(min: 12),
      name:     String.min(2)
    }
    user.password = await hash(user.password)
    flow.users <- user
    emit signal.user.created(user.id)
    yield Response.created(token: sign(user.id))
  }

  on login(req) {
    let creds = validate req {
      email:    Email.format,
      password: String.min(1)
    }
    yield Response.ok(token: sign(creds.email))
  }

  on error(e) transparent {
    yield Response.error(e.trace)
  }

}
```

### Real-time notifications

```aether
stream notifications parallel {

  on send(req) {
    let note = validate req {
      userId:  UUID.format,
      message: String.min(1).max(500),
      channel: String.min(1)
    }
    flow.notifications <- note
    emit signal.notification.sent(note.userId)
    yield Response.created(note.id)
  }

  on subscribe(req) {
    let sub = validate req { userId: UUID.format }
    flow.notifications
      .where(n => n.userId == sub.userId)
      .subscribe(n => emit signal.push(n))
    yield Response.ok(status: "subscribed")
  }

}
```

### Health monitoring

```aether
stream health transparent {

  on ping(req) {
    yield Response.ok(
      status:    "alive",
      timestamp: now(),
      version:   "1.0.0"
    )
  }

  on metrics(req) {
    yield Response.ok(
      events: flow.events.latest(1).at("now"),
      uptime: now()
    )
  }

}
```

---

## Monorepo structure

```
aether/
├── packages/
│   ├── language/     @aether/language    Lexer, Parser, AST, CLI
│   ├── runtime/      @aether/runtime     Executor, EventBus, Builtins
│   ├── flow/         @aether/flow        Event streams, Time travel
│   ├── network/      @aether/network     Protocol mesh, Transports
│   ├── guard/        @aether/guard       Post-quantum security
│   └── evolution/    @aether/evolution   Self-learning optimizer
├── apps/
│   ├── web-shell/    Browser IDE with Visual + Code + Natural modes
│   ├── desktop/      Native desktop app (Tauri)
│   └── cli/          aether command-line tool
├── examples/
├── docs/
├── package.json      pnpm workspaces
└── turbo.json        Build pipeline
```

---

## Compared to existing tools

| | AETHER | Express + Postgres | Fastify | tRPC | NestJS |
|--|--------|-------------------|---------|------|--------|
| Language | Custom hybrid | JavaScript | JavaScript | TypeScript | TypeScript |
| Database | Built-in (Flow) | External (ORM req.) | External | External | External |
| Security | Language-level | Library | Library | Library | Module |
| Real-time | Native | Socket.io | Plugin | Plugin | Gateway |
| Natural language | Native | ✗ | ✗ | ✗ | ✗ |
| Visual mode | Native | ✗ | ✗ | ✗ | ✗ |
| Dependencies | Zero | Hundreds | Dozens | Dozens | Hundreds |
| Post-quantum | Native | Manual | Manual | Manual | Manual |
| Self-evolving | Yes | No | No | No | No |

---

## Roadmap

**Phase 00 — Genesis** `current`
- [x] Language Core — Lexer, Parser, full AST
- [x] Runtime Engine — Executor, EventBus, Builtins
- [x] Flow Data Engine — Append-only streams, time travel
- [x] Natural Language Bridge — Arabic + English → AETHER
- [x] CLI — run, check, build, repl, natural

**Phase 01 — Emergence** `next`
- [ ] Network Mesh — full transport implementations
- [ ] Visual Editor — canvas-based flow builder in browser
- [ ] Browser IDE — Monaco with AETHER language support
- [ ] TypeScript definitions generator

**Phase 02 — Evolution**
- [ ] Quantum Guard — production-hardened security layer
- [ ] Desktop App — native via Tauri
- [ ] Self-Evolution Module — usage learning, auto-optimize
- [ ] Plugin architecture

**Phase 03 — Expansion**
- [ ] Distributed runtime — multi-node AETHER clusters
- [ ] Custom network protocol — AETHER-native binary protocol
- [ ] Package ecosystem — `aether install <package>`
- [ ] RFC process — community governance for language changes

---

## Contributing

AETHER is built in public. Everything is open. Nothing is proprietary.

```bash
git clone https://github.com/aether-lang/aether
cd aether
pnpm install
pnpm build
pnpm test
```

The codebase is TypeScript strict throughout. Zero `any`. Full test coverage enforced at the CI level. Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

**Good first issues** are labelled in the tracker. Start there.

**RFCs** — language changes, syntax decisions, and architectural shifts happen through the RFC process. Open a discussion before writing code for anything that changes observable behavior.

**Community** — we build in the open because we believe infrastructure should be owned by the people who depend on it. If you use AETHER in production, we want to know. If it breaks, we want to fix it. If you have a better idea, we want to hear it.

---

## Philosophy

The internet was built incrementally, under pressure, with whatever was available. The result is a massive pile of layers — each one a reasonable fix for the problem directly beneath it, none of them designed with knowledge of what would come after.

We are not trying to add another layer.

AETHER is a substrate. It sits beneath your product, not between your product and your framework. It handles the parts of backend development that have been re-solved by every engineering team since the web began — data persistence, authentication, real-time communication, API contracts, observability — and handles them once, correctly, at the language level.

The goal is a system that a developer in 2074 can read, understand, and extend without a migration guide. Languages that achieve this are rare. They share a common trait: they are built around ideas that are true, not around tools that are convenient.

AETHER is built around true things:
- Data is events. State is a view of those events.
- Security is not a feature. It is a property of correct code.
- Language and runtime should be the same thing.
- The best abstraction is one you never have to think about.

---

## License

Apache 2.0 — free to use, modify, and distribute. See [LICENSE](LICENSE) for the full terms.

AETHER is not owned by a company. It is not maintained by a startup that needs to monetize it. It is a language built to outlast everyone who builds it.

---

<div align="center">

**AETHER** · Apache 2.0 · Built in public · Owned by no one

*The universal substrate for backend systems.*

</div>
