# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

1DBinaryGas is a Julia-based simulation for studying one-dimensional binary gas systems. The project focuses on statistical mechanics, kinetic theory, and thermodynamic properties of simplified gas systems.

## Project Structure

- `src/` - Main simulation code (Julia modules and functions)
- `tests/` - Test files using Julia's Test framework
- `docs/` - Documentation files
- `data/` - Simulation results and data files (git-ignored)

## Development Commands

### Julia Environment

Initialize and activate the Julia environment:
```bash
julia --project=.
```

In Julia REPL, activate the project and install dependencies:
```julia
using Pkg
Pkg.activate(".")
Pkg.instantiate()
```

### Testing

Run all tests:
```bash
julia --project=. -e 'using Pkg; Pkg.test()'
```

Run tests with coverage:
```bash
julia --project=. --code-coverage=user -e 'using Pkg; Pkg.test()'
```

Run a specific test file:
```bash
julia --project=. tests/test_specific.jl
```

### Running Simulations

Execute main simulation scripts from project root:
```bash
julia --project=. src/main.jl
```

Run with multiple threads for performance:
```bash
julia --project=. -t auto src/main.jl
```

### Documentation

Generate documentation (if using Documenter.jl):
```bash
julia --project=docs docs/make.jl
```

## Architecture Guidelines

### Code Organization

- Physics/simulation logic should be modular and separated by concern (e.g., particle dynamics, collision detection, thermodynamic calculations)
- Use Julia's type system to define particle types, system states, and simulation parameters
- Leverage multiple dispatch for different simulation algorithms or particle interaction types
- Store configuration parameters in structured types rather than global variables

### Data Management

- Simulation results go in `data/` directory (automatically git-ignored)
- Use HDF5 (.h5) or Julia's native JLD2 format for large datasets
- CSV files for small, human-readable outputs
- Keep data files separate from analysis/plotting scripts

### Performance Considerations

- Profile code with `@profile` and `@benchmark` (BenchmarkTools.jl) before optimizing
- Use `@inbounds` and `@simd` for performance-critical loops after verifying correctness
- Consider `StaticArrays.jl` for small, fixed-size arrays (e.g., particle positions/velocities)
- Avoid type instability - use `@code_warntype` to check

### Testing Strategy

- Write unit tests for individual physics calculations and utility functions
- Include integration tests for full simulation runs with known analytical solutions
- Test edge cases (single particle, zero temperature, collision edge cases)
- Use `@testset` to group related tests

## Language-Specific Notes

- This is a Julia project - do not suggest Python, C++, or other language implementations unless specifically requested
- Follow Julia conventions: snake_case for functions, CamelCase for types
- Use explicit return statements for clarity in physics calculations
- Prefer immutable structs for simulation parameters, mutable structs for evolving state
