# KNOWLEDGE EXTENSION STANDARDS

This file describes the architecture conventions for extending knowledge within the project using Cognitive Components.

## Knowlifier Cognitive Components Project

- I know kung fu. *Neo*

Knowlifier (pronounced no-li-faier [noʊˈlɪfaɪər]) is a project that enables translating knowledge into a modular structure, distributing and using it as compatible libraries and packages (Cognitive components).

We believe that very soon in the moment of Singularity all knowledge on Earth will be knowlified - and we are going Knoware. 

With Cogntive components you just copy-paste the packages of knowledge into your projects, and - voilá, I know kung fu.

## Cognitive Components

Cognitive Components packages consist of md files and describe a specific subject domain.
Alongside md files that describe knowledge, configuration json, yaml, or toml files may be used as needed to provide integration settings with software components.

A single Cognitive Component knowledge package contains:

- concepts and definitions
- data and facts
- process descriptions

Packages may have dependencies and reference each other.

## Cognitive Components Location

Cognitive Components are located in the `knowledge` folder.
Cognitive Components added as dependency packages reside in `knowledge/_lib`
Cognitive Components developed within this project reside in separate folders inside `knowledge/` in the format `knowledge/[component-name]`
As creating a well-organized component is a lot of work that will require experimentation, the folder `knowledge/_sandbox` is accessible at design-time but not at run-time (see [IKNOW.md File](#iknow-md-file)).

## Cognitive Components Purpose

Cognitive Components packages can be copied into a working project and extend the cognitive capabilities of agents, including:
- **Design-time**: when copying Cognitive Components into Agentic IDE (Cursor, Codex, Claude Code, Cline, Windsurf, etc.), rules can be configured in `.cursor/rules` or `.clinerules` that use all or selected concepts and procedures from the components in code editing mode, thereby extending the knowledge and capabilities of Cursor/Claude Code/Codex agents in editing mode
- **Run-time**: when copying Cognitive Components into OpenClaw and [insert more examples] projects, these components extend the knowledge and skills of agents in execution mode

## IKNOW.md File

This file must be present in the project root. It explains the logic of operation and records which components are used in code editing mode (in IDE) and which in code execution mode.

This file contains configurations and permissions for the components in this project:

- **Design-time components** — cognitive components used in code editing mode from the IDE. They may contain commands and instructions for structuring knowledge, creating and editing new cognitive components, formatting knowledge, invoking MCP service servers during knowledge loading into databases, testing designed processes and workflows, etc.

- **Run-time components** — cognitive components used in code execution mode by the agent. These components perform the necessary business processes.

The design-time vs. run-time separation — it is important to explicitly separate components, restricting run-time access to design-time components so that during execution users cannot trigger editing of component files unless explicitly intended.

### Permissions 

Permissions define access to components at run-time and design-time:
- READ: the agents can read and understand the knowledge specified in the components, including terms, principles and data. If the link to the folder or file is not specified explicitly as READ for this component (in design-time or run-time), then the system should ignore these files as if this knowledge did not exist (in design-time or run-time respectively).
- WRITE: the agents can write (change all or specified files inside the component). If the link to the folder or file is not specified explicitly as WRITE for this component (in design-time or run-time), then the system **IS NOT ALLOWED** to modify these files as if these files did not exist (in design-time or run-time respectively).
- RUN: the agents can run the rulebooks (md files describing the processes and workflows). If the link to the folder or file is not specified explicitly as RUN for this component (in design-time or run-time), then the system **IS NOT ALLOWED** to use the instructions described in these rulebooks as if these files did not exist (in design-time or run-time respectively).

Example of permissions:

````yaml
READ:
  - knowledge/_lib/*

WRITE:
  - knowledge/component-name1/*
  - knowledge/component-name2/concepts/*

RUN:
  - knowledge/component-name1/*
````

The permissions for the current project are listed below. Edit this file at design-time to add your components if necessary.

## Design-time components

````yaml
READ:
  - knowledge/_lib/*
  - knowledge/_sandbox/*
  - knowledge/customer-calls/*
  - knowledge/landing-pages/*

WRITE:
  - knowledge/_sandbox/*
  - knowledge/customer-calls/*
  - knowledge/landing-pages/*

RUN:
  - knowledge/_sandbox/*
  - knowledge/customer-calls/*
  - knowledge/landing-pages/*
````

## Run-time components

````yaml
READ:
  - knowledge/customer-calls/*
  - knowledge/landing-pages/*

WRITE: []

RUN: []
````
