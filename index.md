---
title: Home
layout: home
---

# VEdition Documentation

![banner](img/banner.jpg)

Welcome to the _VEdition_ technical documentation. Currently, these pages are mostly targeted to internal use, and provide the theorical foundation of the model and some instructions about using its software.

VEdition will provide a new digital edition of Goethe's Venetian epigrams (here GVE for short); given the highly peculiar nature of this text, this digital-born project also aims to propose a new digital model and software tools for authorial philology applied to similar scenarios.

So, beyond the central case study provided by VEdition, GVE proposes:

- a new digital model for a dynamic, compact and transformative representation of text, both on textual and visual layers.
- a full-stack open source software solution to create edition data using this model, highly customizable and modular so that it can be easily tailored to each specific project.

As they get a more stable state, the corresponding software repositories are getting published at the [VeDPH GitHub](https://github.com/vedph/). Currently, these repositories are still private, so you won't be able to open the following links unless you have been granted access to them.

- **backend**:
  - [gve-core](https://github.com/vedph/gve-core): core models, logic and API for both consuming and creating data.
- **frontend**:
  - [gve-snapshot-rendition](https://github.com/vedph/gve-snapshot-rendition): Typescript library with a vanilla custom web component
  - [gve-shell](https://github.com/vedph/gve-shell): Angular libraries with frontend components for creating.
  - [cadmus-gve-app](https://github.com/vedph/cadmus-gve-app): Cadmus-based editor frontend.

The software solution is designed for containerization and distributed in Docker images. So, it can be easily run off the shelf both in a web server and in a local machine, whatever its platform (Unix, MacOS, Windows).

Data produced by the editor can be represented with JSON data, or even plain text, and is stored in a standard document-based database (MongoDB), leveraging the power of the full-fledged [Cadmus content creation framework](https://vedph.github.io/cadmus-doc/).
