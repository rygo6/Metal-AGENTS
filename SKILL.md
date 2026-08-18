---
name: metal
description: Answer questions and help implement, debug, optimize, or profile Apple Metal and Metal Shading Language by referencing local API headers, Metal-backed implementation source, shader translation source, samples, and Apple GPU microarchitecture notes. Use for Metal and Metal 4, MetalFX, MetalKit, Metal Performance Shaders, CAMetalLayer, MSL shaders, command buffers, pipelines, heaps, argument buffers, residency, synchronization, tile-based deferred rendering, GPU families, and Xcode GPU debugging. Always consult the local repos and the linked Apple documentation index before answering.
---

# Metal Local Reference Skill

Apple does not publish the Metal specification, framework documentation, or sample library as clonable git repositories. This skill therefore has two halves, and you should use both:

1. **Local repos** in `references/` — the authoritative API surface plus real Metal-using source you can read and grep offline.
2. **`references/LINKS.md`** — a curated index of Apple's canonical documentation URLs. Fetch these live rather than trusting a cached copy; Apple revises them per OS release.

```text
references/metal-cpp/          ← Apple's official C++ bindings: full Metal, MetalFX, QuartzCore, Foundation API surface
references/MoltenVK/           ← Production Metal usage; Vulkan→Metal mapping and per-GPU-family limits
references/SPIRV-Cross/        ← MSL code generation, MSL version gating, and MSL language constraints
references/metal-cpp-samples/  ← Runnable metal-cpp examples
references/metal-samples/      ← Runnable Objective-C Metal examples
references/applegpu/           ← Apple GPU instruction set, disassembler, and hardware notes
references/metal-benchmarks/   ← Apple GPU microarchitecture, instruction throughput, and concurrency measurements
references/LINKS.md            ← Index of Apple's official Metal documentation (fetch live)
```

Use the separate `vulkan` skill for Vulkan and MoltenVK-as-a-Vulkan-driver questions, `opengl` for native GL/GLES, and `webgl` for the browser API.

## Choose the authoritative source

| Topic | Primary source |
|---|---|
| Exact class, protocol, method, enum, and option-set declarations | `references/metal-cpp/Metal/`, `MetalFX/`, `QuartzCore/`, `Foundation/` |
| Metal 4 API surface (`MTL4*`) | `references/metal-cpp/Metal/MTL4*.hpp` |
| Metal Shading Language rules | Apple's MSL specification PDF in `references/LINKS.md` |
| GPU family capabilities and limits | Apple's Feature Set Tables PDF in `references/LINKS.md`, cross-checked against `references/MoltenVK/MoltenVK/MoltenVK/GPUObjects/` |
| Framework semantics, lifecycle, and guides | Apple developer documentation URLs in `references/LINKS.md` |
| Real-world Metal object, encoder, and heap usage | `references/MoltenVK/MoltenVK/MoltenVK/GPUObjects/` and `Commands/` |
| MSL emitted by a real translator, plus MoltenVK's shader pipeline | `references/MoltenVK/MoltenVKShaderConverter/` |
| Translating GLSL/SPIR-V or HLSL concepts to MSL | `references/SPIRV-Cross/spirv_msl.cpp` and `spirv_msl.hpp` |
| Minimal working app patterns | `references/metal-cpp-samples/learn-metal/`, `references/metal-samples/objc/` |
| Instruction cost, occupancy, and ALU behavior | `references/metal-benchmarks/README.md` and `InstructionThroughput/` |
| Shader disassembly and ISA questions | `references/applegpu/applegpu.py`, `docs.html`, `README.md` |

Treat Apple's MSL specification, Feature Set Tables, and framework documentation as normative. Treat `metal-cpp` as the exact, machine-checkable API surface. Treat MoltenVK, SPIRV-Cross, samples, and the reverse-engineering repos as strong evidence of real behavior, not as definitions of Metal.

## Answer API questions

1. Identify the target: OS version, Metal version (classic `MTLCommandBuffer` path versus the Metal 4 `MTL4*` path), device GPU family, and language (Swift, Objective-C, or C++).
2. Confirm the declaration, enum values, and availability in `references/metal-cpp/` — the headers carry `API_AVAILABLE`-equivalent version annotations.
3. Read the matching Apple documentation page from `references/LINKS.md` for ownership, lifetime, threading, and encoder-ordering semantics that headers do not express.
4. Cross-check real usage in `references/MoltenVK/MoltenVK/MoltenVK/` when the question is about correct sequencing or non-obvious constraints.
5. State clearly which behavior is documented, which is observed in an implementation, and which is version- or family-gated.

## Answer Metal Shading Language questions

1. Establish the MSL version and target, since attributes, address spaces, and built-ins are version-gated.
2. Consult the MSL specification PDF linked in `references/LINKS.md` for normative language rules.
3. For address spaces, argument buffers, function constants, and resource binding, check how `references/SPIRV-Cross/spirv_msl.cpp` emits and gates the construct — it encodes many MSL restrictions explicitly.
4. Read shaders in `references/metal-samples/` and `references/metal-cpp-samples/` for working syntax.
5. Keep MSL distinct from GLSL and HLSL; do not carry over semantics that MSL does not share.

## Debug and optimize

1. Reproduce against the smallest matching sample in `references/metal-cpp-samples/` or `references/metal-samples/`.
2. Check the usual failure surface: device and family support, pipeline state creation errors, shader library compilation logs, resource storage modes, `MTLHeap` and residency, argument buffer tiers, drawable acquisition and lifetime on `CAMetalLayer`, render pass load/store actions, and command buffer error state.
3. For Metal 4, additionally check command allocator reuse, argument tables, residency sets, and compiler task completion.
4. For Apple GPUs, reason about tile-based deferred rendering explicitly: memoryless attachments, tile memory budget, load/store action cost, and imageblock usage. The Apple documentation index links the TBDR tuning guide.
5. For instruction-level performance, use `references/metal-benchmarks/README.md` for throughput and occupancy, and `references/applegpu/` when the question is genuinely about the ISA.
6. Recommend Xcode's GPU capture and counters for anything requiring real measurement; do not present static analysis as measured data.

## Answering strategy

- Search or read the local repos before answering, then fetch the relevant Apple page from `references/LINKS.md` when the answer depends on documented semantics.
- Cite repository-relative file paths and full URLs so the evidence is reproducible.
- Keep Metal 4 and the classic Metal API distinct; they coexist and have different objects and lifetimes.
- Do not infer device support from a header's presence. GPU family and OS version determine availability; verify against the Feature Set Tables or a runtime query.
- Never quote a stale copy of Apple documentation. This skill deliberately stores links, not mirrored pages, so answers track Apple's current text.
- If submodules are absent, run `git submodule update --init` from the skill directory before relying on memory. Do not add `--recursive`: these repos are read-only references and are never built, so nested build dependencies are pure overhead.
