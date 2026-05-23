---
name: sml-installation
description: Install the SML runtime (Rust parser with Python bindings) so that agents can parse and execute SML tags programmatically.
---

# SML Installation Guide

Use this guide when you need to install the SML runtime for programmatic parsing and execution.

## Quick Install (Python)

```bash
pip install sml
```

This installs pre-built wheels with the Rust parser compiled for your platform.

## From Source

Requirements:
- Rust 1.75+ (install via https://rustup.rs)
- Python 3.9+
- maturin (`pip install maturin`)

```bash
git clone https://github.com/sergio-sisternes-epam/aslm.git
cd aslm
maturin develop --release
```

## Verify Installation

```python
from sml import parse, execute, SmlRegistry

# Parse SML from a prompt
doc = parse('<skill interface="testing" language="python">Run tests</skill>')
print(doc.node_count)       # 1
print(doc.invocations())    # ['testing']

# Set up a registry
registry = SmlRegistry()
registry.register_interface("testing", "Run automated tests")
registry.register_implementation(
    "pytest-impl", "testing",
    language="python", framework="pytest"
)

# Execute (pass-through without custom handlers)
result = execute(doc, registry)
print(result)  # "Run tests"
```

## Platform Support

| Platform | Architecture | Status |
|----------|-------------|--------|
| Linux | x86_64 | ✅ |
| Linux | aarch64 | ✅ |
| macOS | x86_64 | ✅ |
| macOS | aarch64 (Apple Silicon) | ✅ |
| Windows | x86_64 | ✅ |

## Rust API (Direct)

Add to your `Cargo.toml`:

```toml
[dependencies]
sml-core = "0.1"
```

```rust
use sml_core::{parse, SkillRegistry, ExecutionContext};

let doc = parse(r#"<skill interface="testing">content</skill>"#).unwrap();
let registry = SkillRegistry::new();
let ctx = ExecutionContext::new(registry);
let result = ctx.execute(&doc).unwrap();
```
