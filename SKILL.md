# SKILL.md

## Propaga Skill System

### Overview

In Propaga, a **Skill** is a self-contained capability that an autonomous system can execute to **propagate**, **replicate**, and **scale** behavior across networks and environments.

Propaga is built around one core idea: **systems should be able to multiply and spread safely**.  
Instead of designing a single “perfect agent,” Propaga focuses on creating **repeatable units of intelligence** that can:

- replicate into new instances (with controlled variation)
- deploy into new environments automatically
- learn from outcomes and pass forward improvements
- coordinate at scale without collapsing into chaos

A Propaga system is therefore composed of **many skills**, each responsible for a specific function in the propagation lifecycle.

Skills are the building blocks of:  
**replication → distribution → adaptation → selection → reinvestment → propagation again**

---

## Design Principles

Propaga skills follow five principles to keep propagation scalable and controlled.

### 1) Modular
Each skill has a narrow responsibility. Small skills propagate better than complex systems.

### 2) Composable
Skills must connect through stable inputs/outputs so they can be chained into replication pipelines.

### 3) Observable
Every skill emits signals and metrics. Propagation without observability becomes noise.

### 4) Constraint-First
Skills must enforce budgets, rate limits, and safety boundaries by default.

### 5) Evolvable
Skills support controlled mutation and selection to improve across generations.

---

## Core Concepts

### Skill vs Instance vs Swarm

- **Skill**: a reusable capability module (e.g., replication planner, allocator, deployer)
- **Instance**: one running unit of the system composed of skills + state + memory
- **Swarm**: a population of instances that coordinate, compete, and propagate

Propaga is designed for populations, not single agents.

---

## Skill Taxonomy

Propaga organizes skills by propagation lifecycle.

### 1) Signal Skills (Perception)

Signal skills transform environment data into usable signals for decisions.

**Examples**
- environment health detection
- opportunity scoring signals
- resource availability signals
- drift / anomaly detection

**Outputs**
- normalized features
- confidence scores
- alerts and thresholds

---

### 2) Replication Skills (Spawn / Clone)

Replication skills define *when* and *how* new instances are created.

**Examples**
- spawn controller (when to replicate)
- clone template builder (what gets copied)
- mutation injector (how variation is applied)
- identity seeder (how uniqueness is generated)

**Outputs**
- replication plans
- instance blueprints
- mutation diffs
- identity seeds

---

### 3) Distribution Skills (Deploy / Spread)

Distribution skills decide *where* and *how* instances spread.

**Examples**
- environment selector (where to deploy)
- deployment orchestrator (how to launch)
- routing policy (how to allocate tasks/traffic)
- throttling controller (prevent overload)

**Outputs**
- deployment targets
- rollout configs
- rollout logs
- safety throttles

---

### 4) Coordination Skills (Swarm)

Coordination skills manage interaction between many instances.

**Examples**
- task allocator (divide work)
- consensus / voting module (merge decisions)
- leader-election or role assignment
- communication protocol adapter

**Outputs**
- assignments
- coordination states
- merge decisions
- conflict resolutions

---

### 5) Adaptation Skills (Learn)

Adaptation skills update behavior based on feedback.

**Examples**
- online parameter tuning
- reward shaping
- error correction loops
- stability dampening

**Outputs**
- parameter updates
- adaptation summaries
- drift metrics

---

### 6) Memory Skills (Persist)

Memory skills store knowledge so propagation accumulates value.

**Examples**
- episode memory (recent outcomes)
- long-term pattern memory
- compressed state snapshots
- knowledge export/import for clones

**Outputs**
- memory snapshots
- retrieval results
- export bundles
- health metrics

---

### 7) Selection Skills (Filter)

Selection skills prevent “infinite replication” from becoming “infinite garbage.”

**Examples**
- fitness evaluator (score instances)
- survival filters (keep/kill)
- rollback controller
- promotion pipeline (graduate winners)

**Outputs**
- fitness scores
- retention decisions
- promoted templates
- kill-switch triggers

---

### 8) Resource Skills (Budget & Safety)

Resource skills enforce constraints so propagation remains safe.

**Examples**
- rate limit enforcer
- cost budget manager
- risk boundary checker
- circuit breaker and safe mode

**Outputs**
- budgets and caps
- violation logs
- safe-mode states

---

## Skill Lifecycle (Propagation Loop)

Propaga skills operate inside a continuous loop:

1. **Sense** — collect signals from environments  
2. **Plan** — decide if replication is justified  
3. **Spawn** — create new instances from templates  
4. **Deploy** — distribute instances into targets  
5. **Coordinate** — share tasks and avoid duplication  
6. **Learn** — update behavior from outcomes  
7. **Select** — keep winners, discard failures  
8. **Promote** — export improved templates for next generation  

This loop repeats indefinitely, but remains controlled through selection and budgets.

---

## Standard Skill Interface

Propaga skills use a consistent interface to support composition and automation.

Minimum methods:

- `initialize(config)`  
- `observe(env)`  
- `propose(state)`  
- `act(intent)`  
- `update(feedback)`  
- `metrics()`

### Reference Interface (Python-style)

```python
from dataclasses import dataclass
from typing import Any, Dict, Optional

@dataclass
class SkillOutput:
    intent: Optional[Dict[str, Any]] = None
    signals: Optional[Dict[str, Any]] = None
    reason: Optional[str] = None
    confidence: float = 0.0

class Skill:
    name: str = "skill"

    def initialize(self, config: Dict[str, Any]) -> None:
        ...

    def observe(self, env: Dict[str, Any]) -> Dict[str, Any]:
        return {}

    def propose(self, state: Dict[str, Any]) -> SkillOutput:
        return SkillOutput()

    def act(self, intent: Dict[str, Any]) -> Dict[str, Any]:
        return {}

    def update(self, feedback: Dict[str, Any]) -> None:
        ...

    def metrics(self) -> Dict[str, Any]:
        return {}
```

---

## Composition Patterns

### 1) Replication Pipeline

A common Propaga pipeline:

Signal → Replication Planner → Mutation Injector → Deploy → Coordinator → Evaluator

Use this for controlled scaling.

---

### 2) Swarm Router

A router selects which instances should handle which tasks based on:

- environment type
- current load
- historical performance
- risk boundaries

---

### 3) Promotion Ladder

Instances graduate through levels:

- sandbox → limited rollout → full rollout  
Only winners are promoted.

---

## Observability Requirements

Propagation must be measurable.

### Required metrics (minimum)

- `spawn_rate`
- `instance_count`
- `failure_rate`
- `avg_fitness`
- `budget_used`
- `constraint_violations`
- `latency_ms` (if applicable)

### Required logs (minimum)

- replication plan and diff
- deployment target and rollout mode
- reason and confidence outputs
- selection decisions (keep/kill/promote)
- rollback actions

---

## Safety and Constraints

Propaga enforces hard boundaries to prevent uncontrolled replication:

- **spawn caps**: maximum new instances per window  
- **budget caps**: compute/cost ceilings  
- **rate limits**: per-environment throttles  
- **safe mode**: degrade to read-only behavior  
- **circuit breakers**: auto-stop on repeated failures  

A Propaga skill must fail closed when unsure.

---

## Mutation Policy (Controlled Variation)

Mutation is allowed, but never random chaos.

### Mutation types
- parameter mutation (small bounded shifts)
- structural mutation (swap modules)
- routing mutation (change dispatch policy)
- memory mutation (compression/retrieval strategy)

### Mutation rules
- bounded magnitude
- diff must be logged
- must run in sandbox before promotion
- rollback must exist

---

## Selection Policy (Survival, Not Hype)

Selection keeps Propaga productive.

### Fitness examples
- long-term performance
- stability under drift
- constraint compliance
- robustness across environments
- efficiency (reward per budget)

### Selection actions
- **promote**: export winning template
- **hold**: continue evaluating
- **kill**: retire instance
- **rollback**: revert template

---

## Developer Guidelines

Write Propaga skills that are:

- small and deterministic where possible
- observable and testable
- constraint-first
- compatible with replication and deployment
- versioned for safe inheritance

Avoid:

- giant skills doing everything
- hidden state without logs
- uncontrolled replication triggers
- mutations without evaluation or rollback

---

## Roadmap (Docs-First)

Upcoming Propaga documentation will include:

- replication templates and schema
- environment adapters
- rollout modes and safety gates
- swarm coordination strategies
- fitness scoring recipes
- contribution guidelines for community skills

---

## Conclusion

Propaga is not about one agent.

It is about **systems that propagate** — safely, measurably, and continuously.

Skills are the units that make that possible:
modular capabilities that replicate, adapt, and improve across a growing swarm.
