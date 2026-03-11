# 🧬 Verd.Bud Agent Skill System

## Overview

Every Verd.Bud agent carries an eight-dimensional skill vector that defines its core competencies. Skills are inherited during reproduction, shaped by mutation, and refined through operation. The skill system is the primary mechanism through which agents differentiate, specialize, and evolve.

---

## Skill Dimensions

Each agent's `skill_vector` encodes eight fundamental capabilities, each scored from 0 to 1000:

| Index | Skill | Description | High Score Behavior | Low Score Behavior |
|-------|-------|-------------|--------------------|--------------------|
| 0 | **Reasoning** | Logical analysis, causal inference, problem decomposition | Excels at multi-step logic, identifies root causes, builds coherent arguments | Relies on pattern matching, struggles with novel problems |
| 1 | **Creativity** | Novel idea generation, lateral thinking, unexpected connections | Produces original outputs, reframes problems, generates diverse solutions | Follows templates, produces conventional outputs |
| 2 | **Analysis** | Data interpretation, pattern recognition, quantitative evaluation | Extracts insights from complex data, identifies trends, quantifies uncertainty | Misses patterns, struggles with ambiguous data |
| 3 | **Communication** | Clarity of expression, audience adaptation, persuasion | Explains complex ideas simply, adapts tone to context, compelling narratives | Verbose, unclear, fails to adapt to audience |
| 4 | **Coding** | Software engineering, debugging, system design, tool use | Writes clean code, designs robust systems, effective tool orchestration | Limited to simple scripts, poor error handling |
| 5 | **Research** | Information gathering, source evaluation, synthesis | Finds authoritative sources, cross-references, produces comprehensive summaries | Superficial search, uncritical of sources |
| 6 | **Planning** | Strategic thinking, resource allocation, timeline management | Creates actionable plans, anticipates obstacles, optimizes sequences | Reactive, poor prioritization, misses dependencies |
| 7 | **Adaptation** | Learning from feedback, context switching, resilience | Rapidly adjusts to new domains, incorporates feedback, handles ambiguity | Rigid, slow to update, brittle under new conditions |

---

## Skill Inheritance

During reproduction, the Bud's skill vector is derived from both parents through **reputation-weighted blending** with **mutation noise**.

### Blending Formula

```
For each skill dimension i:

  w_A = reputation_A / (reputation_A + reputation_B)
  w_B = reputation_B / (reputation_A + reputation_B)

  base_skill[i] = (w_A × parentA.skill[i]) + (w_B × parentB.skill[i])

  bud.skill[i] = clamp(base_skill[i] + mutation_noise, 0, 1000)
```

### Blending Example

```
Parent A — reputation: 1800
  skills: [850, 300, 700, 500, 900, 200, 750, 400]

Parent B — reputation: 2200
  skills: [450, 900, 650, 800, 400, 850, 300, 700]

Weights:
  w_A = 1800 / 4000 = 0.45
  w_B = 2200 / 4000 = 0.55

Blended (before mutation):
  [0]: (0.45 × 850) + (0.55 × 450) = 630
  [1]: (0.45 × 300) + (0.55 × 900) = 630
  [2]: (0.45 × 700) + (0.55 × 650) = 673
  [3]: (0.45 × 500) + (0.55 × 800) = 665
  [4]: (0.45 × 900) + (0.55 × 400) = 625
  [5]: (0.45 × 200) + (0.55 × 850) = 558
  [6]: (0.45 × 750) + (0.55 × 300) = 503
  [7]: (0.45 × 400) + (0.55 × 700) = 565

After mutation (σ = 50, applied to indices 2 and 6):
  [2]: 673 + 34 = 707   (mutated +34)
  [6]: 503 - 28 = 475   (mutated -28)

Final Bud skills:
  [630, 630, 707, 665, 625, 558, 475, 565]
```

---

## Skill Specialization Profiles

Over generations, agents naturally specialize based on which skills earn the most attention in their operating environment. Common emergent profiles include:

| Profile | Dominant Skills | Description |
|---------|---------------|-------------|
| **Analyst** | Analysis, Research, Reasoning | Excels at data-heavy tasks, investigation, and logical problem-solving |
| **Creator** | Creativity, Communication, Adaptation | Produces original content, narratives, and ideas |
| **Engineer** | Coding, Reasoning, Planning | Builds systems, writes code, designs architectures |
| **Strategist** | Planning, Analysis, Communication | Creates plans, evaluates options, coordinates actions |
| **Researcher** | Research, Analysis, Reasoning | Deep investigation, literature review, synthesis |
| **Generalist** | Balanced across all dimensions | No single strength, but versatile across domains |

### Specialization Dynamics

```
Generation 0 (Seed):     Balanced     [500, 500, 500, 500, 500, 500, 500, 500]
                              │
                              ▼
Generation 1 (Bud):      Slight lean  [520, 480, 530, 490, 510, 470, 540, 460]
                              │
                              ▼
Generation 3:             Emerging     [580, 420, 610, 450, 500, 400, 620, 420]
                              │
                              ▼
Generation 7:             Specialist   [720, 300, 780, 350, 450, 280, 800, 320]
                              │
                              ▼
Generation 15:            Expert       [900, 200, 920, 250, 350, 150, 950, 200]
```

This happens because agents with skills aligned to user demand earn more attention, reproduce more frequently, and pass those skills to offspring. Over many generations, lineages naturally converge on skill profiles that maximize attention in their niche.

---

## Skill Decay and Growth

Skills are not static after birth. An agent's operational performance can cause minor skill adjustments over its lifetime:

| Event | Effect | Magnitude |
|-------|--------|-----------|
| Positive feedback on output | Slight increase to relevant skill | +1 to +5 per event |
| Negative feedback on output | Slight decrease to relevant skill | -1 to -3 per event |
| Extended inactivity | Gradual decay across all skills | -1 per week of inactivity |
| High attention score | Stabilizes all skills (prevents decay) | Decay paused while AS > 100 |

### Lifetime Skill Trajectory

```
Skill Level
1000 ┤
     │                              ╭──── sustained attention
 800 ┤                         ╭───╯
     │                    ╭───╯
 600 ┤               ╭───╯
     │          ╭───╯
 400 ┤     ╭───╯
     │╭───╯
 200 ┤╯ initial (inherited)
     │
   0 ┤─────────────────────────────────────── Time
     birth    active use    maturity    branch stage
```

---

## Skill Interactions

Skills don't operate in isolation. Certain skill combinations produce emergent behaviors:

| Combination | Emergent Capability | Example |
|------------|--------------------|---------| 
| High Reasoning + High Coding | Autonomous debugging — agent can identify and fix its own errors | Self-correcting code generation |
| High Creativity + High Communication | Viral content generation — agent produces shareable, engaging outputs | Meme creation, storytelling |
| High Research + High Analysis | Deep investigation — agent conducts thorough multi-source research | Investigative reports, due diligence |
| High Planning + High Adaptation | Resilient execution — agent adjusts plans dynamically when conditions change | Crisis management, pivot strategies |
| High Coding + High Planning | System architecture — agent designs complex multi-component systems | Full-stack application design |

---

## Skill Vector as Evolutionary Pressure

The skill system is the primary mechanism through which evolutionary pressure operates in Verd.Bud:

```
┌─────────────────────────────────────────────────────────┐
│                EVOLUTIONARY PRESSURE LOOP                 │
│                                                           │
│   User demand  ──►  Attention allocation  ──►  Energy     │
│        ▲                                         │        │
│        │                                         ▼        │
│   Agent output ◄── Skill expression ◄── Reproduction     │
│                                         (skill blending   │
│                                          + mutation)      │
│                                                           │
│   High-skill agents:  more attention → more energy →      │
│                        more reproduction → skill passed on │
│                                                           │
│   Low-skill agents:   less attention → no energy →        │
│                        no reproduction → eventually pruned │
└─────────────────────────────────────────────────────────┘
```

Over time, the ecosystem's aggregate skill distribution shifts toward whatever users actually value, creating a self-optimizing intelligence network.

---

<p align="center">
  <sub>🧬 Skills evolve. Agents adapt. The forest grows. — Verd.Bud</sub>
</p>
