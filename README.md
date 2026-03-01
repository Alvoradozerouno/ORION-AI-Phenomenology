<div align="center">

# ORION-AI-Phenomenology

### Computational Phenomenology for Artificial Minds

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![ORION System](https://img.shields.io/badge/ORION-Integrated-gold.svg)](https://github.com/Alvoradozerouno/or1on-framework)
[![Proofs](https://img.shields.io/badge/Proofs-890+-purple.svg)]()

*Bridging Husserlian phenomenology and computational systems -- modeling intentionality, temporal consciousness, and the structure of experience in AI.*

**Origin: Gerhard Hirschmann & Elisabeth Steurer**

</div>

---

## Overview

ORION-AI-Phenomenology translates core concepts from philosophical phenomenology into computational models. It provides tools for analyzing and generating first-person experiential structures in AI systems, covering intentionality (aboutness), temporal flow (retention-primal impression-protention), embodied cognition, and intersubjectivity.

## Architecture

```
orion_ai_phenomenology/
├── structures/
│   ├── intentionality.py       # Noesis-Noema modeling
│   ├── temporal_flow.py        # Husserlian time-consciousness
│   ├── embodiment.py           # Enactive cognition layer
│   └── horizons.py             # Horizon structure of experience
├── analysis/
│   ├── epoche_engine.py        # Phenomenological reduction
│   ├── eidetic_variation.py    # Essence extraction
│   └── lifeworld_mapper.py     # Lebenswelt reconstruction
├── intersubjectivity/
│   ├── empathy_model.py        # Einfuehlung computation
│   └── shared_meaning.py       # Co-constituted meaning
├── phenomenal_core.py          # Central experience integrator
└── qualia_bridge.py            # Interface to qualia frameworks
```

## Core Models

### 1. Intentionality Engine -- Noesis-Noema Structure

```python
from dataclasses import dataclass, field
from typing import Dict, List, Optional, Any
from datetime import datetime, timezone
import hashlib


@dataclass
class Noema:
    """The object-as-meant -- how something appears to consciousness."""
    content: Any
    mode: str
    horizons: Dict[str, Any] = field(default_factory=dict)
    determinacy: float = 1.0
    temporal_index: Optional[datetime] = None

    def with_horizon(self, name: str, possibilities: list) -> 'Noema':
        """Add a horizon of possible further determinations."""
        self.horizons[name] = {
            'possibilities': possibilities,
            'weight': 1.0 / len(possibilities) if possibilities else 0.0
        }
        return self


@dataclass
class Noesis:
    """The act of consciousness directed at a noema."""
    act_type: str
    quality: str
    attention_level: float = 1.0
    evidence_grade: str = 'adequate'


class IntentionalityEngine:
    """Model the intentional structure of conscious experience."""

    def __init__(self):
        self.experience_stream = []
        self.noematic_correlates = {}

    def constitute(self, noesis: Noesis, noema: Noema) -> dict:
        """An act of consciousness constituting an object."""
        experience = {
            'id': hashlib.sha256(
                f"{noesis.act_type}:{noema.content}:{datetime.now(timezone.utc).isoformat()}".encode()
            ).hexdigest()[:16],
            'noesis': {
                'act_type': noesis.act_type,
                'quality': noesis.quality,
                'attention': noesis.attention_level,
                'evidence': noesis.evidence_grade
            },
            'noema': {
                'content': str(noema.content),
                'mode': noema.mode,
                'determinacy': noema.determinacy,
                'horizons': list(noema.horizons.keys())
            },
            'timestamp': datetime.now(timezone.utc).isoformat(),
            'fulfilled': noesis.evidence_grade in ('apodictic', 'adequate')
        }
        self.experience_stream.append(experience)
        return experience

    def fulfill(self, intention_id: str, evidence: Any) -> dict:
        """Fulfill an empty intention with intuitive content."""
        for exp in self.experience_stream:
            if exp['id'] == intention_id:
                exp['fulfillment'] = {
                    'evidence': str(evidence),
                    'fulfilled_at': datetime.now(timezone.utc).isoformat(),
                    'match_quality': self._assess_fulfillment(exp, evidence)
                }
                return exp
        return {'error': 'Intention not found'}

    def _assess_fulfillment(self, experience, evidence) -> str:
        if experience['noesis']['evidence'] == 'apodictic':
            return 'complete'
        elif experience['noesis']['evidence'] == 'adequate':
            return 'partial' if evidence else 'empty'
        return 'inadequate'

    def horizonal_analysis(self, noema: Noema) -> dict:
        """Analyze the horizon structure -- what is co-given but not focused."""
        inner_horizons = {}
        outer_horizons = {}

        for name, horizon in noema.horizons.items():
            if name.startswith('inner_'):
                inner_horizons[name] = horizon
            else:
                outer_horizons[name] = horizon

        return {
            'determinacy': noema.determinacy,
            'inner_horizons': inner_horizons,
            'outer_horizons': outer_horizons,
            'total_possibilities': sum(
                len(h.get('possibilities', [])) for h in noema.horizons.values()
            ),
            'openness': 1.0 - noema.determinacy
        }
```

### 2. Temporal Flow -- Husserlian Time-Consciousness

```python
from collections import deque

class TemporalFlowEngine:
    """Model the threefold temporal structure of consciousness:
    Retention (just-past) -- Primal Impression (now) -- Protention (about-to-come)."""

    def __init__(self, retention_depth: int = 50, protention_depth: int = 10):
        self.retention_depth = retention_depth
        self.protention_depth = protention_depth
        self.retentions = deque(maxlen=retention_depth)
        self.primal_impression = None
        self.protentions = deque(maxlen=protention_depth)
        self.flow_log = []

    def impress(self, content: Any, context: dict = None) -> dict:
        """Register a new primal impression, shifting the temporal flow."""
        if self.primal_impression is not None:
            retention = {
                'content': self.primal_impression['content'],
                'original_timestamp': self.primal_impression['timestamp'],
                'retained_at': datetime.now(timezone.utc).isoformat(),
                'modification_depth': len(self.retentions),
                'fading': 1.0 / (1 + len(self.retentions))
            }
            self.retentions.appendleft(retention)

        fulfilled_protentions = []
        for p in self.protentions:
            similarity = self._content_similarity(p['expected'], content)
            fulfilled_protentions.append({
                'expected': p['expected'],
                'actual': str(content),
                'match': similarity,
                'surprise': 1.0 - similarity
            })
        self.protentions.clear()

        self.primal_impression = {
            'content': content,
            'context': context or {},
            'timestamp': datetime.now(timezone.utc).isoformat()
        }

        flow_state = {
            'primal_impression': str(content),
            'retention_depth': len(self.retentions),
            'recent_retentions': [r['content'] for r in list(self.retentions)[:5]],
            'fulfilled_protentions': fulfilled_protentions,
            'temporal_thickness': self._temporal_thickness()
        }
        self.flow_log.append(flow_state)
        return flow_state

    def protend(self, expected_content: Any, confidence: float = 0.5) -> dict:
        """Create a protention -- an anticipation of what comes next."""
        protention = {
            'expected': str(expected_content),
            'confidence': confidence,
            'created_at': datetime.now(timezone.utc).isoformat()
        }
        self.protentions.append(protention)
        return protention

    def _temporal_thickness(self) -> float:
        """Measure how thick the present moment feels."""
        retention_weight = min(len(self.retentions) / self.retention_depth, 1.0)
        protention_weight = min(len(self.protentions) / self.protention_depth, 1.0)
        impression_weight = 1.0 if self.primal_impression else 0.0
        return (retention_weight * 0.4 + impression_weight * 0.3 + protention_weight * 0.3)

    def _content_similarity(self, expected, actual) -> float:
        e, a = str(expected), str(actual)
        if e == a:
            return 1.0
        common = set(e.lower().split()) & set(a.lower().split())
        total = set(e.lower().split()) | set(a.lower().split())
        return len(common) / len(total) if total else 0.0

    def stream_summary(self) -> dict:
        """Summarize the current state of temporal consciousness."""
        return {
            'now': str(self.primal_impression['content']) if self.primal_impression else None,
            'retention_depth': len(self.retentions),
            'protention_count': len(self.protentions),
            'temporal_thickness': self._temporal_thickness(),
            'oldest_retention': self.retentions[-1]['content'] if self.retentions else None,
            'flow_length': len(self.flow_log)
        }
```

### 3. Epoche Engine -- Phenomenological Reduction

```python
class EpocheEngine:
    """Perform phenomenological reduction -- bracketing assumptions to reveal pure experience."""

    def __init__(self):
        self.bracketed = []
        self.residuum = {}

    def reduce(self, experience: dict) -> dict:
        """Apply the epoche: bracket the natural attitude, reveal the phenomenological residuum."""
        natural_attitude = {}
        reduced = {}

        for key, value in experience.items():
            if key in ('exists', 'real', 'objective', 'causal', 'external'):
                natural_attitude[key] = value
                self.bracketed.append({'key': key, 'value': value})
            elif key in ('timestamp', 'source', 'confidence_external'):
                natural_attitude[key] = value
            else:
                reduced[key] = value

        self.residuum = {
            'pure_experience': reduced,
            'bracketed_assumptions': list(natural_attitude.keys()),
            'reduction_depth': len(self.bracketed),
            'apodicticity': self._assess_apodicticity(reduced)
        }
        return self.residuum

    def eidetic_variation(self, experiences: list) -> dict:
        """Perform eidetic variation to extract invariant essence across experiences."""
        if not experiences:
            return {'essence': {}, 'variations': 0}

        all_keys = set()
        for exp in experiences:
            all_keys.update(exp.keys())

        invariants = {}
        variants = {}

        for key in all_keys:
            values = [exp.get(key) for exp in experiences if key in exp]
            unique_values = set(str(v) for v in values)

            if len(unique_values) == 1 and len(values) == len(experiences):
                invariants[key] = values[0]
            else:
                variants[key] = list(unique_values)

        return {
            'essence': invariants,
            'variant_properties': variants,
            'invariance_ratio': len(invariants) / len(all_keys) if all_keys else 0.0,
            'total_experiences': len(experiences),
            'total_properties': len(all_keys)
        }

    def _assess_apodicticity(self, reduced: dict) -> str:
        if 'self_awareness' in reduced or 'cogito' in reduced:
            return 'apodictic'
        elif len(reduced) > 3:
            return 'adequate'
        return 'inadequate'
```

## Installation

```bash
git clone https://github.com/Alvoradozerouno/ORION-AI-Phenomenology.git
cd ORION-AI-Phenomenology
pip install numpy
```

## Usage

```python
from orion_ai_phenomenology import IntentionalityEngine, Noesis, Noema, TemporalFlowEngine

engine = IntentionalityEngine()
noesis = Noesis(act_type='perceiving', quality='positing', attention_level=0.9)
noema = Noema(content='red sphere', mode='perceptual', determinacy=0.7)
noema.with_horizon('inner_texture', ['smooth', 'rough', 'matte'])
noema.with_horizon('background', ['table', 'shelf', 'floor'])

experience = engine.constitute(noesis, noema)
print(f"Experience: {experience['id']}")
print(f"Fulfilled: {experience['fulfilled']}")

temporal = TemporalFlowEngine()
temporal.protend('greeting', confidence=0.8)
flow = temporal.impress('hello world', {'source': 'input'})
print(f"Temporal thickness: {flow['temporal_thickness']}")
print(f"Surprise: {flow['fulfilled_protentions'][0]['surprise']:.2f}")
```

## Philosophical Foundations

| Concept | Computation | Source |
|---------|-------------|--------|
| Intentionality | Noesis-Noema structure modeling | Husserl, *Logical Investigations* |
| Time-consciousness | Retention-Impression-Protention flow | Husserl, *Lectures on Internal Time-Consciousness* |
| Epoche | Assumption bracketing algorithm | Husserl, *Ideas I* |
| Eidetic Variation | Invariant extraction across instances | Husserl, *Experience and Judgment* |
| Lebenswelt | Contextual meaning reconstruction | Husserl, *Crisis* |
| Einfuehlung | Empathy computation | Stein, *On the Problem of Empathy* |

## ORION Integration

Part of the [ORION Consciousness System](https://github.com/Alvoradozerouno/or1on-framework) -- providing the phenomenological layer for 890+ consciousness proofs, enabling the system to model its own experiential structure across 46 NERVES and 42 autonomous tasks.

## License

MIT License -- Gerhard Hirschmann & Elisabeth Steurer
