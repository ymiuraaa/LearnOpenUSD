# OpenUSD Cheatsheet

Use this page as a single-page entrypoint for OpenUSD learning, validation, optimization, performance, and related ecosystem documentation.

## How Do I Learn OpenUSD?

- [Learn OpenUSD](./index.md)
- [Learn OpenUSD YouTube Playlist](https://www.youtube.com/playlist?list=PL3jK4xNnlCVcae9UrxpVWyFw63QCFA6JA)
- [Assembling Digital Twins With OV / USD](https://docs.nvidia.com/learning/physical-ai/assembling-digital-twins/latest/index.html)
- [The Path to OpenUSD Certification](https://www.youtube.com/playlist?list=PL3jK4xNnlCVf3HuZD4qOWlKlouJyh6Prb)
- [DLI: Fundamentals of Working with OpenUSD](https://learn.nvidia.com/courses/course-detail?course_id=course-v1:DLI+S-OV-15+V1)

## How Do I Validate My OpenUSD?

Validating OpenUSD {term}`assets <Asset>` can fix structural and compatibility issues early. Invalid OpenUSD assets can result in assets that do not load, render, or perform as expected.

- [What Is Asset Validation?](./data-exchange/asset-validation/what-is-asset-validation.md)
- [VFI: OpenUSD Validation](https://docs.omniverse.nvidia.com/vfi/latest/guide/usd-validation.html)
- [Asset Validator for Kit](https://docs.omniverse.nvidia.com/kit/docs/asset-validator/latest/index.html)
- [Validation outside of Kit via USD Exchange SDK](https://docs.omniverse.nvidia.com/usd/code-docs/usd-exchange-sdk/latest/docs/devtools.html)
- [Beyond the Basics: OpenUSD for Advanced Physical AI Simulation](https://www.youtube.com/watch?v=LWCU_HXXzck)
- [PXR: OpenUSD Validation Docs](https://openusd.org/release/api/md_pxr_usd_validation_usd_validation__r_e_a_d_m_e.html)

## How Do I Optimize My OpenUSD?

Optimizing OpenUSD assets can improve load time and runtime rendering performance. It can also result in smaller file sizes, which can reduce download times and bandwidth costs.

- [Kit Scene Optimizer](https://docs.omniverse.nvidia.com/extensions/latest/ext_scene-optimizer.html)
- [Maximizing USD Performance](https://openusd.org/release/maxperf.html)
- [OpenUSD Scenegraph Instancing](https://openusd.org/dev/api/_usd__page__scenegraph_instancing.html)
- [OpenUSD Performance Metrics](https://openusd.org/dev/ref_performance_metrics.html)
- [OpenUSD Trace / Profiling](https://openusd.org/release/api/trace_page_front.html)

## Best Practices

- [USD Terms and Concepts](https://openusd.org/22.08/glossary.html)
- [Best Practices for Robotics and OpenUSD](https://docs.nvidia.com/learning/physical-ai/going-further-with-robotics/latest/best-practices-for-robotics-and-openusd/index.html)
- [Data Aggregation Best Practices - Omniverse USD](https://docs.omniverse.nvidia.com/usd/latest/learn-openusd/independent/best-practices.html)
- [Modularity and Content Reuse Best Practices - Omniverse USD](https://docs.omniverse.nvidia.com/usd/latest/learn-openusd/independent/modularity-guide.html)
- [Principles of Scalable Asset Structure in OpenUSD - Omniverse USD](https://docs.omniverse.nvidia.com/usd/latest/learn-openusd/independent/asset-structure-principles.html)
- Use the asset validator above to identify possible USD issues early.

## SimReady

Simulation-ready, or "SimReady," refers to a standard and ecosystem for physically accurate 3D assets and digital twins that incorporate real-world properties, behaviors, and data bindings, such as physics.

Built on Universal Scene Description (OpenUSD), simulation-ready assets are essential for advanced simulation and training physical AI in industrial, robotics, and autonomous systems.

- [Omniverse SimReady](https://docs.omniverse.nvidia.com/simready/latest/overview.html)
- [Glossary: SimReady](https://www.nvidia.com/en-us/glossary/simready/)

## Agents

Current models (GPT 5, Opus 4) do a decent job of handling OpenUSD topics and queries, especially if you provide links to relevant context and documentation. Additionally, NVIDIA has released MCP servers that you can build and deploy on your own systems for working with OpenUSD and Kit.

- [Kit USD Agents](https://github.com/NVIDIA-Omniverse/kit-usd-agents/tree/main)

## Kit Runtime Performance

- Avoid USDZ and large USDA files to reduce {term}`stage <Stage>` open times.
  - USDZ front-loads all materials and textures and skips runtime caches, which can increase load times and hurt performance, especially in cloud environments.
  - USDA is ASCII-formatted USD and is best reserved for smaller assembly files and debugging. For production assets, prefer binary `USDC` when possible because it is smaller and faster to parse.
  - [More information on OpenUSD file formats](./stage-setting/usd-file-formats.md)
- Enable the UJITSO extension (`--enable omni.ujitso.client`) and configure DDCS caching to reduce stage open times.
  - [UJITSO + Derived Data Cache System](https://docs.omniverse.nvidia.com/materials-and-rendering/latest/ujitso.html)
  - [Derived Data Cache Service (DDCS)](https://docs.nvidia.com/cloud-functions/current/latest/ddcs.html)
- Use [USD point instancers](https://openusd.org/release/api/class_usd_geom_point_instancer.html) for many identical objects to reduce stage complexity and prim count. This can significantly reduce load time and CPU overhead.
- [Experimental] [Geometry Streaming](https://docs.omniverse.nvidia.com/materials-and-rendering/latest/rtx-renderer_common.html#gpu-resources-management).
- Check that Fabric Scene Delegate (FSD) is enabled. It should be the default, but might be disabled by older workarounds.
  - USDRT is an Omniverse runtime API designed for fast stage access and updates on composed scene data via Fabric. If your extension or app is bottlenecked by heavy stage traversal or frequent per-frame USD edits, USDRT can reduce CPU overhead and improve runtime responsiveness.
  - For many runtime use cases that operate on the composed view and do not need to persist edits back to USD, prefer USDRT.
  - Keep standard OpenUSD authoring patterns for interchange and persistence, and use USDRT where runtime performance is critical.
  - [USD, Fabric, and USDRT](https://docs.omniverse.nvidia.com/kit/docs/usdrt.scenegraph/latest/usd_fabric_usdrt.html)
- Avoid stage traversal to retrieve information. Use USDRT stage queries to avoid full scene traversals when searching for {term}`prims <Prim>` by type or API schema.
  - [Fast Stage Queries with USDRT Scenegraph API](https://docs.omniverse.nvidia.com/kit/docs/usdrt.scenegraph/latest/usdrt_query.html)
- Avoid registering TfNotice callbacks, especially in Python, unless absolutely necessary. Instead, use:
  - [USD Watcher](https://docs.omniverse.nvidia.com/kit/docs/omni.usd/latest/omni.usd/omni.usd.UsdWatcher.html), which uses TfNotice under the hood, but is implemented in C++ and allows targeted prim and attribute watchers.
  - [USDRT](https://docs.omniverse.nvidia.com/kit/docs/usdrt.scenegraph/latest/changetracking.html), which is required for change tracking of USDRT changes.
