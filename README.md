# PANDORA

<img src="assets\pandora-logo.png">

**A modern C++ simulation engine: define a world, give objects behavior, run it in real time, and visualize it live or later.**

PANDORA is an experimental simulation engine built from the ground up in modern C++.

It is domain-agnostic: the engine provides the world, time, scheduling, and data infrastructure, and the science lives in the components, systems, and scripts you define on top of it. A jet engine, a fluid flow, a population of cells, a galaxy, or an agent-based model are all the same kind of problem to PANDORA: objects with state, evolving step by step.

It is architecturally close to a game engine (scene, entities, components, fixed-timestep main loop, systems, job scheduling) but deliberately leaves out everything that exists purely for presentation or player experience: audio, animation, and asset-heavy content pipelines.

The primary use case:

1. Set up a scene (programmatically or from a description file).
2. Define objects (entities with data components). Each object can carry a script that decides what it does next.
3. Run the simulation in real time, with no window or GPU required.
4. Choose how to visualize it: live while it runs, deferred (from a recording, after the run), or not at all.

## Goals

- Headless-first runtime: the engine runs with no window, graphics context, or display
- Deterministic, fixed-timestep simulation loop with real-time and as-fast-as-possible modes
- Scene, entity, and component management
- Systems that operate on component data, designed for cache-friendly iteration
- Per-object scripts: any entity can have behavior that decides its next action each step
- Multithreaded execution with a job system and task graph
- CPU and GPU compute (C++ threads, OpenMP, CUDA)
- General-purpose scientific simulation: any domain can be modeled by defining its own components, systems, and scripts
- Physics and collision as optional simulation modules, not a built-in assumption
- State recording, snapshots, and replay
- Runtime logging, profiling, and metrics
- Optional visualization, either live during the run or deferred from recordings, decoupled from the runtime

## Architecture

    PANDORA
    │
    ├── Core
    │   ├── Application
    │   ├── Simulation Loop
    │   ├── Time (fixed step, real-time pacing)
    │   ├── Events
    │   ├── Logging
    │   └── Memory
    │
    ├── World
    │   ├── Entities
    │   ├── Components
    │   ├── Systems
    │   ├── Scenes
    │   └── Transforms
    │
    ├── Scripting
    │   ├── Script Component (per entity)
    │   ├── Script Host / Runtime
    │   └── Script API (world queries, actions)
    │
    ├── Domain Modules (optional, built on the core)
    │   ├── Physics (rigid bodies, collision)
    │   ├── Fluids
    │   ├── Biology / Cells
    │   └── User-defined models
    │
    ├── Jobs
    │   ├── Thread Pool
    │   ├── Scheduler
    │   └── Task Graph
    │
    ├── Compute
    │   ├── CPU
    │   ├── OpenMP
    │   └── CUDA
    │
    ├── Data
    │   ├── Scene Description (load/save)
    │   ├── Snapshots
    │   ├── Recording / Replay
    │   └── Metrics Export
    │
    └── Visualization (optional, separate layer)
        ├── Renderer
        ├── Debug Draw
        ├── Viewport
        ├── Inspector
        └── Profiler

The runtime never depends on the visualization layer. Visualization reads simulation state, either live from a running simulation or later from a recording; it does not drive it.

## Simulation Model

- **Fixed timestep:** simulation advances in constant steps so results are reproducible.
- **Real-time mode:** steps are paced against the wall clock.
- **Headless / fast mode:** steps run as quickly as possible, for batch runs and experiments.
- **Systems:** ordered or dependency-scheduled units of logic that run over component data each step.
- **Object scripts:** an entity may have a script component. Each step, the script reads the entity's state and the world around it and decides what to do next (change state, move, spawn or remove objects, emit events). Scripts act through a controlled API and are scheduled by the engine, so they stay compatible with parallel execution and determinism.
- **Determinism:** the same scene, seed, and step count should produce the same result.

## Parallel Computing

Parallel execution is a core feature. The engine exposes infrastructure for computationally heavy systems:

- C++ multithreading
- Engine job system
- OpenMP
- CUDA

Target workloads include particle systems, physics, N-body problems, agent-based simulation, procedural generation, and scientific simulation.

## What Can Be Simulated

Because the core is domain-agnostic, PANDORA can be used to simulate anything that can be described as entities with state and rules for how that state changes over time. Examples:

- **Engineering:** jet engines and turbomachinery, combustion, heat transfer, structural mechanics, control systems
- **Fluid dynamics:** airflow, water, particle-based and grid-based fluids
- **Biology:** cells, tissues, populations, ecosystems, epidemics
- **Physics and chemistry:** N-body systems, molecular dynamics, reaction networks
- **Complex systems:** agent-based models, traffic, swarms, economies
- **Anything else:** if it can be expressed as components, systems, and scripts, it can run on the engine

Domain models are built as reusable modules on top of the engine (for example a fluid solver or a cell model) and are not part of the core. The engine does not promise validated numerical accuracy for any domain by itself; correctness of a model is the responsibility of the model's author.

## Visualization

Visualization is an optional layer on top of the simulation, and it can be used in two ways:

- **Live:** a viewer attached to the running simulation shows state as it evolves, in real time.
- **Deferred:** the simulation runs headless and records its state; the recording is visualized and replayed afterwards.

Both modes use the same viewer and read the same state data. The simulation behaves identically whether or not a viewer is attached.

It will initially use **OpenGL** behind an abstraction so other backends remain possible. It is limited to what is needed to observe a simulation: debug and primitive rendering, cameras, a viewport, an inspector, and profiling views. There are no material pipelines, post-processing, or animation systems.

## Project Scope

PANDORA is a simulation engine, not a game engine. The following are out of scope:

- Audio
- Animation (skeletal, blend trees, etc.)
- Visual (node-based) scripting
- Asset pipelines for production content (model/texture/material authoring)
- Networking / multiplayer
- Mobile and console platforms
- Marketplaces and large-scale production tooling

## Development Roadmap

### Phase 1 - Foundation

- [ ] Core library
- [ ] Application lifecycle
- [ ] Logging
- [ ] Time and fixed-timestep simulation loop
- [ ] Real-time and headless run modes

### Phase 2 - World

- [ ] Entity/component architecture
- [ ] Systems and system scheduling
- [ ] Transform system
- [ ] Scene setup API
- [ ] Scene description loading/saving

### Phase 2b - Object Scripting

- [ ] Script component attachable to any entity
- [ ] Script host and per-step execution
- [ ] Script API (query world, act on self and others)
- [ ] Deterministic, parallel-safe script scheduling
- [ ] Scripts referenced from scene descriptions

### Phase 3 - Parallel Runtime

- [ ] Worker thread pool
- [ ] Job system
- [ ] Task graph scheduling
- [ ] Parallel systems
- [ ] Performance instrumentation

### Phase 4 - Domain Modules

- [ ] Module API for defining domain models on top of the core
- [ ] Rigid body dynamics and collision (first reference module)
- [ ] Deterministic stepping
- [ ] Reference demos from other domains (fluids, cell/agent models)

### Phase 5 - Data and Observability

- [ ] State snapshots
- [ ] Recording and replay
- [ ] Metrics export
- [ ] Profiling

### Phase 6 - High-Performance Compute

- [ ] Compute abstraction
- [ ] OpenMP integration
- [ ] CUDA integration
- [ ] Large-scale particle / N-body demo
- [ ] GPU-accelerated fluid or field solver demo

### Phase 7 - Visualization

- [ ] OpenGL render backend
- [ ] Window and input (visualization only)
- [ ] Debug draw and cameras
- [ ] Viewport and live viewer (attached to a running simulation)
- [ ] Deferred viewer (replay from recordings)
- [ ] Scene hierarchy and inspector

## Status

PANDORA is an experimental project under active development. Architecture and APIs are expected to change significantly.

## Philosophy

**Simulation First**

The engine exists to run worlds, not to present them. Rendering is an observer that can attach live or look at a recording later.

**Headless by Default**

Everything must work without a window, GPU, or display. Visualization is always opt-in.

**C++ First**

The engine exposes modern C++ rather than hiding it behind another environment.

**Performance and Parallelism Matter**

Memory layout, concurrency, scheduling, and profiling are fundamental concerns, and the engine should use multicore CPUs and GPUs effectively.

**Build to Understand**

PANDORA is also a long-term exploration of engine architecture, concurrency, HPC, and real-time systems.

## License

License to be determined.
