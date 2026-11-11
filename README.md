# PANDORA Game Engine

<p align="center"><img src="assets/pandora-logo.svg" alt="PANDORA Game Engine logo" width="640"></p>

> *Build worlds. Shape play. Open the box.*

PANDORA is a planned modular, high-performance C++ game engine for creating interactive 2D and 3D games. The goal is to give developers a flexible foundation for building game worlds, defining gameplay, and bringing those experiences to life through rendering, simulation, and tools.

## Vision

PANDORA aims to bring modern C++ engine architecture and real-time graphics together in a flexible game-development environment. Its architecture is intended to support everything from small prototypes to expansive interactive worlds, while keeping the core systems modular and extensible.

## Core Areas

- **Worlds and gameplay** — Organize scenes, entities, and the rules that make a game interactive.
- **Rendering** — Build real-time visual experiences, with a path toward both 2D and 3D games.
- **Simulation and physics** — Model movement, collision, and other systems that evolve during play.
- **Performance** — Make efficient use of CPU, memory, GPU, and other computational resources.
- **Tools and workflow** — Support iteration on game content and development workflows.

These are areas of focus for the planned engine, not currently implemented capabilities.

## Potential Applications

PANDORA is intended to support user-created games and interactive experiences, such as:

- 2D and 3D game projects
- Physics-driven and simulation-focused games
- Interactive worlds and prototypes
- Real-time visual experiences

These are potential uses, not currently implemented capabilities.

## Planned Toolchain and Technologies

The project is expected to use **Modern C++** with **MSBuild** for its build system. The planned graphics stack includes **OpenGL**, **GLFW**, **GLEW**, **GLM**, and **ImGui**. GLEW is the planned OpenGL extension loader. These technologies are not yet integrated or in use in this repository.

Future exploration may include **OpenMP**, **CUDA**, compute shaders, and **Vulkan**. No support for these technologies is currently implemented or promised.

## Project Status

PANDORA is in an **early development and planning stage**. This repository establishes the project identity, documentation, and initial structure only. There is no engine implementation, game loop, renderer, MSBuild solution/project, or working functionality yet.

## Building

There is currently no buildable target or configured dependency setup. As implementation begins, build instructions will be added for MSBuild and the project’s dependencies, including GLEW.

## Roadmap

All items below are planned; none are complete.

- [ ] Define project goals, architectural principles, and initial game-engine requirements.
- [ ] Establish contribution guidelines and development conventions.
- [ ] Design the foundational C++ and MSBuild project structure.
- [ ] Evaluate and integrate the initial graphics and visualization dependencies.
- [ ] Prototype the core game loop, world, and gameplay architecture.
- [ ] Explore rendering, input, and physics systems for interactive games.
- [ ] Develop tools and workflows for building and iterating on game content.
- [ ] Document supported workflows and expand example projects.

## Repository Structure

```text
PANDORA/
├── README.md
├── LICENSE
├── .gitignore
├── assets/             # Project identity and documentation assets
│   └── pandora-logo.svg
├── docs/               # Project and design documentation
└── src/                # Reserved for future source code; currently empty
```

## Contributing

PANDORA is at the beginning of its development. Early feedback, ideas, and discussion are welcome through GitHub issues. Before proposing code changes, please open an issue to discuss scope and direction; contribution and development guidelines will be added as the project takes shape. Please do not interpret the roadmap as a promise of delivery dates or existing functionality.

## License

PANDORA Game Engine is distributed under the [Apache License 2.0](LICENSE).
