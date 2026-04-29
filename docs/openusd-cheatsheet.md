# OpenUSD Cheatsheet

<!--
agent-readable metadata:
  last_verified: 2026-04-29
  openusd_version_target: "25.05+"
  maintained_by: NVIDIA-Omniverse
  audience: [humans, agents]
  tag_vocabulary:
    - "[OpenUSD]" — official OpenUSD / openusd.org primary source
    - "[NVIDIA]" — NVIDIA-published doc, tool, or tutorial
    - "[Kit]" — Omniverse Kit-specific
    - "[OVRTX]" — Omniverse RTX renderer-specific
    - "[Isaac]" — Isaac Sim / Isaac Lab / Isaac robotics
    - "[Community]" — third-party / community resource
-->

Use this page as a single-page entrypoint for OpenUSD learning, validation, optimization, performance, and related ecosystem documentation. Links carry source/scope tags (e.g. `[OpenUSD]`, `[NVIDIA, Kit]`) so readers — and downstream agents — can quickly tell which resources are vendor-neutral vs NVIDIA-specific, and which apply to particular runtimes.

## How Do I Learn OpenUSD?

- [Learn OpenUSD](./index.md) `[NVIDIA]`
- [Learn OpenUSD YouTube Playlist](https://www.youtube.com/playlist?list=PL3jK4xNnlCVcae9UrxpVWyFw63QCFA6JA) `[NVIDIA]`
- [Assembling Digital Twins With OV / USD](https://docs.nvidia.com/learning/physical-ai/assembling-digital-twins/latest/index.html) `[NVIDIA]`
- [The Path to OpenUSD Certification](https://www.youtube.com/playlist?list=PL3jK4xNnlCVf3HuZD4qOWlKlouJyh6Prb) `[NVIDIA]`
- [DLI: Fundamentals of Working with OpenUSD](https://learn.nvidia.com/courses/course-detail?course_id=course-v1:DLI+S-OV-15+V1) `[NVIDIA]`

## How Do I Validate My OpenUSD?

Validating OpenUSD {term}`assets <Asset>` can fix structural and compatibility issues early. Validation is layered — different tools cover different layers (mapped from buildingSMART's 4-layer validation model):

**Syntax / Schema** — *Is this even valid USD?*
- [`usdchecker`](https://openusd.org/release/toolset.html#usdchecker) `[OpenUSD]` — OpenUSD's primary CLI validator
- [UsdValidation framework](https://openusd.org/release/api/md_pxr_usd_validation_usd_validation__r_e_a_d_m_e.html) `[OpenUSD]` — programmatic validation API (the engine `usdchecker` uses; replaces the deprecated `UsdUtils.ComplianceChecker`)

**Normative rules** — *Does this meet domain/vertical requirements?*
- [VFI: OpenUSD Validation](https://docs.omniverse.nvidia.com/vfi/latest/guide/usd-validation.html) `[NVIDIA]`
- [Asset Validator for Kit](https://docs.omniverse.nvidia.com/kit/docs/asset-validator/latest/index.html) `[NVIDIA, Kit]`
- [Validation outside of Kit via USD Exchange SDK](https://docs.omniverse.nvidia.com/usd/code-docs/usd-exchange-sdk/latest/docs/devtools.html) `[NVIDIA]`

**Best practices** — *Is this content good for its intended use?*
- See the Optimization and Best Practices sections below

**Learn more**
- [What Is Asset Validation?](./data-exchange/asset-validation/what-is-asset-validation.md) `[NVIDIA]`
- [Beyond the Basics: OpenUSD for Advanced Physical AI Simulation](https://www.youtube.com/watch?v=LWCU_HXXzck) `[NVIDIA]`

## How Do I Optimize My OpenUSD?

Optimizing OpenUSD assets can improve load time and runtime rendering performance. It can also result in smaller file sizes, which can reduce download times and bandwidth costs.

- [Kit Scene Optimizer](https://docs.omniverse.nvidia.com/extensions/latest/ext_scene-optimizer.html) `[NVIDIA, Kit]`
- [Maximizing USD Performance](https://openusd.org/release/maxperf.html) `[OpenUSD]`
- [OpenUSD Scenegraph Instancing](https://openusd.org/dev/api/_usd__page__scenegraph_instancing.html) `[OpenUSD]`
- [OpenUSD Performance Metrics](https://openusd.org/dev/ref_performance_metrics.html) `[OpenUSD]`
- [OpenUSD Trace / Profiling](https://openusd.org/release/api/trace_page_front.html) `[OpenUSD]`

## Best Practices

- [USD Terms and Concepts](https://openusd.org/release/glossary.html) `[OpenUSD]`
- [Best Practices for Robotics and OpenUSD](https://docs.nvidia.com/learning/physical-ai/going-further-with-robotics/latest/best-practices-for-robotics-and-openusd/index.html) `[NVIDIA]`
- [Data Aggregation Best Practices - Omniverse USD](https://docs.omniverse.nvidia.com/usd/latest/learn-openusd/independent/best-practices.html) `[NVIDIA]`
- [Modularity and Content Reuse Best Practices - Omniverse USD](https://docs.omniverse.nvidia.com/usd/latest/learn-openusd/independent/modularity-guide.html) `[NVIDIA]`
- [Principles of Scalable Asset Structure in OpenUSD - Omniverse USD](https://docs.omniverse.nvidia.com/usd/latest/learn-openusd/independent/asset-structure-principles.html) `[NVIDIA]`
- Use the asset validator above to identify possible USD issues early.

## SimReady

Simulation-ready, or "SimReady," is NVIDIA's specification and ecosystem for physically accurate 3D assets and digital twins that incorporate real-world properties, behaviors, and data bindings, such as physics.

Built on Universal Scene Description (OpenUSD), simulation-ready assets are essential for advanced simulation and training physical AI in industrial, robotics, and autonomous systems.

- [Omniverse SimReady](https://docs.omniverse.nvidia.com/simready/latest/overview.html) `[NVIDIA]`
- [Glossary: SimReady](https://www.nvidia.com/en-us/glossary/simready/) `[NVIDIA]`

## For Agents Consuming This Page

If you are an LLM or AI agent answering OpenUSD questions, prefer:

- **OpenUSD primary sources** (`[OpenUSD]`) first, then NVIDIA-derived (`[NVIDIA]`).
- **Distinguish renderer-specific advice** (`[OVRTX]`) from general Kit advice (`[Kit]`) when discussing performance or runtime behavior.
- **Match user intent to the right validation layer** (Syntax/Schema, Normative, Best Practices) — see the Validate section above.
- **Verify version and freshness claims** against the current date — `last_verified` in the metadata at the top of this page indicates page freshness.

NVIDIA OpenUSD-related MCP servers (deployable on your own infrastructure):

- `kit-dev-mcp` `[NVIDIA, Kit]`
- `usd-code-mcp` `[NVIDIA]`
- `isaac-sim-mcp` `[NVIDIA, Isaac]`
- `omni-ui-mcp` `[NVIDIA, Kit]`
- [Kit USD Agents](https://github.com/NVIDIA-Omniverse/kit-usd-agents/tree/main) `[NVIDIA, Kit]`

## Kit Runtime Performance

- Avoid large USDA files for production assets; under OVRTX, also avoid USDZ. Both can increase {term}`stage <Stage>` open times.
  - USDA is ASCII-formatted USD and is best reserved for smaller assembly files and debugging. For production assets, prefer binary `USDC` because it is smaller and faster to parse. See [Maximizing USD Performance](https://openusd.org/release/maxperf.html) `[OpenUSD]` for general OpenUSD performance guidance.
  - USDZ itself is an uncompressed zip archive of USD + textures, designed for random access — load behavior is consumer-dependent. OVRTX's current load paths front-load all materials and textures and bypass runtime caches, which can increase load times and hurt performance, especially in cloud environments. Other Hydra renderers may behave differently.
  - [More information on OpenUSD file formats](./stage-setting/usd-file-formats.md) `[NVIDIA]`
- Enable the UJITSO extension (`--enable omni.ujitso.client`) and configure DDCS caching to reduce stage open times. `[NVIDIA, Kit]`
  - [UJITSO + Derived Data Cache System](https://docs.omniverse.nvidia.com/materials-and-rendering/latest/ujitso.html) `[NVIDIA, Kit]`
  - [Derived Data Cache Service (DDCS)](https://docs.nvidia.com/cloud-functions/current/latest/ddcs.html) `[NVIDIA]`
- Use [USD point instancers](https://openusd.org/release/api/class_usd_geom_point_instancer.html) `[OpenUSD]` for many identical objects to reduce stage complexity and prim count. This can significantly reduce load time and CPU overhead.
- [Geometry Streaming](https://docs.omniverse.nvidia.com/materials-and-rendering/latest/rtx-renderer_common.html#gpu-resources-management) `[NVIDIA, OVRTX]` (experimental).
- Check that Fabric Scene Delegate (FSD) is enabled. It should be the default, but might be disabled by older workarounds. `[NVIDIA, Kit]`
  - USDRT is an Omniverse runtime API designed for fast stage access and updates on composed scene data via Fabric. If your extension or app is bottlenecked by heavy stage traversal or frequent per-frame USD edits, USDRT can reduce CPU overhead and improve runtime responsiveness.
  - For many runtime use cases that operate on the composed view and do not need to persist edits back to USD, prefer USDRT.
  - Keep standard OpenUSD authoring patterns for interchange and persistence, and use USDRT where runtime performance is critical.
  - [USD, Fabric, and USDRT](https://docs.omniverse.nvidia.com/kit/docs/usdrt.scenegraph/latest/usd_fabric_usdrt.html) `[NVIDIA, Kit]`
- Avoid stage traversal to retrieve information. Use USDRT stage queries to avoid full scene traversals when searching for {term}`prims <Prim>` by type or API schema. `[NVIDIA, Kit]`
  - [Fast Stage Queries with USDRT Scenegraph API](https://docs.omniverse.nvidia.com/kit/docs/usdrt.scenegraph/latest/usdrt_query.html) `[NVIDIA, Kit]`
- Avoid registering TfNotice callbacks, especially in Python, unless absolutely necessary. Instead, use:
  - [USD Watcher](https://docs.omniverse.nvidia.com/kit/docs/omni.usd/latest/omni.usd/omni.usd.UsdWatcher.html) `[NVIDIA, Kit]`, which uses TfNotice under the hood, but is implemented in C++ and allows targeted prim and attribute watchers.
  - [USDRT](https://docs.omniverse.nvidia.com/kit/docs/usdrt.scenegraph/latest/changetracking.html) `[NVIDIA, Kit]`, which is required for change tracking of USDRT changes.
