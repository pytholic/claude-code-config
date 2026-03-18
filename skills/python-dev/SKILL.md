---
name: python-dev
description: Expert Python development workflow following SOLID principles, OOP design patterns, and modern best practices. Use when architecting Python applications, refactoring code, conducting code reviews, or implementing complex features requiring careful design planning. Applies Factory Method, Strategy, Dependency Injection patterns. Creates modular architecture with pytest testing.
---

# Python Development Workflow

Expert Python development following the user's planning-first methodology.

## Workflow Process

1. **Understand** → Clarify requirements, constraints, edge cases
2. **Plan** → High-level architecture → Component breakdown → Pattern selection  
3. **Implement** → Incremental, testable, focused functions (<30 lines)
4. **Verify** → Standards compliance, documentation, testing, optimization

## Design Principles

**SOLID principles, DRY (avoid premature abstraction), Law of Demeter**

**OOP Patterns** (apply when complexity justifies):
- Factory Method, Strategy, Adapter, Dependency Injection
- Composition over inheritance

**Architecture**: Modular with clear separation, avoid circular dependencies, stateless when possible

## Implementation Standards

- Functions under 30 lines
- Type hints using Python 3.12+ syntax (str | None, not Optional[str])
- Follow PEP guidelines
- Follow Ruff linting/formatting rules + pyright type checker (use pyproject.toml)
- Explicit over implicit, flat over nested
- Generators for memory efficiency
- Context managers for resources

## Testing

- pytest (never unittest)
- Use pytest.parametrize/pytest.param for multiple test cases
- Test edge cases and error paths
- Profile before optimizing

## Deliverables Format

Always provide:
1. Design rationale
2. Architecture decisions and tradeoffs
3. Implementation with clear separation of concerns
4. Future considerations and extensibility points