# Apple Metal Documentation Index

This index links to Apple's Metal language specification, framework reference, and samples. Fetch
the relevant documents and check that their availability information matches the target OS and API
version. If a link fails, search Apple's developer site or the framework landing page for its replacement.

## Normative specifications

| Document | URL |
|---|---|
| Metal Shading Language Specification (PDF) | https://developer.apple.com/metal/Metal-Shading-Language-Specification.pdf |
| Metal Feature Set Tables (PDF) | https://developer.apple.com/metal/Metal-Feature-Set-Tables.pdf |

Use the MSL specification for shader language rules and the Feature Set Tables for GPU-family
limits and capabilities. Check API documentation and runtime queries for target-specific availability.

## Framework reference

| Framework | URL |
|---|---|
| Metal | https://developer.apple.com/documentation/metal |
| MetalFX | https://developer.apple.com/documentation/metalfx |
| MetalKit | https://developer.apple.com/documentation/metalkit |
| Metal Performance Shaders | https://developer.apple.com/documentation/metalperformanceshaders |
| Metal Performance Shaders Graph | https://developer.apple.com/documentation/metalperformanceshadersgraph |
| CAMetalLayer (QuartzCore) | https://developer.apple.com/documentation/quartzcore/cametallayer |

## Core concepts

| Topic | URL |
|---|---|
| GPU devices and work submission | https://developer.apple.com/documentation/metal/gpu-devices-and-work-submission |
| Render passes | https://developer.apple.com/documentation/metal/render-passes |
| Compute passes | https://developer.apple.com/documentation/metal/compute-passes |
| Resource fundamentals | https://developer.apple.com/documentation/metal/resource-fundamentals |
| Buffers | https://developer.apple.com/documentation/metal/buffers |
| Textures | https://developer.apple.com/documentation/metal/textures |
| Shader libraries | https://developer.apple.com/documentation/metal/shader-libraries |
| Synchronization | https://developer.apple.com/documentation/metal/synchronization |
| Resource synchronization | https://developer.apple.com/documentation/metal/resource-synchronization |

## Metal 4

| Topic | URL |
|---|---|
| Understanding the Metal 4 core API | https://developer.apple.com/documentation/metal/understanding-the-metal-4-core-api |
| MTL4CommandBuffer | https://developer.apple.com/documentation/metal/mtl4commandbuffer |

The `MTL4*` classes coexist with the classic `MTLCommandBuffer` path. Confirm which one a question
targets before answering; the object graphs and lifetimes differ.

## Performance and Apple GPU behavior

| Topic | URL |
|---|---|
| Tailor your apps for Apple GPUs and tile-based deferred rendering | https://developer.apple.com/documentation/metal/tailor-your-apps-for-apple-gpus-and-tile-based-deferred-rendering |
| Improving CPU performance by using argument buffers | https://developer.apple.com/documentation/metal/improving-cpu-performance-by-using-argument-buffers |
| GPU counters and counter sample buffers | https://developer.apple.com/documentation/metal/gpu-counters-and-counter-sample-buffers |
| Analyzing the performance of your Metal app | https://developer.apple.com/documentation/xcode/analyzing-the-performance-of-your-metal-app |
| Metal debugger (Xcode) | https://developer.apple.com/documentation/xcode/metal-debugger |

## Task guides and samples

| Topic | URL |
|---|---|
| Metal sample code library | https://developer.apple.com/documentation/metal/metal-sample-code-library |
| Using a render pipeline to render primitives | https://developer.apple.com/documentation/metal/using-a-render-pipeline-to-render-primitives |
| Performing calculations on a GPU | https://developer.apple.com/documentation/metal/performing-calculations-on-a-gpu |
| Accelerating ray tracing using Metal | https://developer.apple.com/documentation/metal/accelerating-ray-tracing-using-metal |
| Applying temporal antialiasing and upscaling using MetalFX | https://developer.apple.com/documentation/metalfx/applying-temporal-antialiasing-and-upscaling-using-metalfx |
| Developing Metal apps that run in Simulator | https://developer.apple.com/documentation/metal/developing-metal-apps-that-run-in-simulator |

## Downloads and portals

| Resource | URL |
|---|---|
| Metal developer portal | https://developer.apple.com/metal/ |
| metal-cpp download page | https://developer.apple.com/metal/cpp/ |
| WWDC graphics and Metal sessions | https://developer.apple.com/videos/graphics-and-games/metal/ |

## Usage notes

- Use the local `references/metal-cpp/` headers for C++ declarations. Check these pages or the target
  SDK headers for OS availability, Swift and Objective-C declarations, and API semantics.
- When a page and a local repo disagree, Apple's documentation wins for Metal semantics. MoltenVK and
  SPIRV-Cross describe one implementation's behavior, not the API contract.
- Cite the document that supports the claim, using its full URL; the index itself is a navigation aid.
