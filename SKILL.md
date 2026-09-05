---
name: metal
description: Use local headers, implementation sources, and Apple documentation to answer questions and develop, debug, or profile Metal and Metal Shading Language code. Use for Metal APIs, MSL, GPU capabilities, resource lifetimes, synchronization, and Metal performance analysis.
---

# Metal Local Reference Skill

Search the relevant local sources before answering technical questions. Paths below are relative to this skill directory. Use [references/LINKS.md](references/LINKS.md) to find Apple documentation for language rules, API semantics, and platform availability; fetch the relevant documents when needed.

```text
references/metal-cpp/               ← Apple's official C++ bindings for Metal, MetalFX, QuartzCore, and Foundation
references/MoltenVK/                ← Production Metal usage; Vulkan→Metal mapping and per-GPU-family limits
references/SPIRV-Cross/             ← MSL code generation, MSL version gating, and MSL language constraints
references/metal-cpp-samples/       ← Runnable metal-cpp examples
references/metal-samples/           ← Runnable Objective-C Metal examples
references/applegpu/                ← Apple GPU instruction set, disassembler, and hardware notes
references/metal-benchmarks/        ← Apple GPU microarchitecture, instruction throughput, and concurrency measurements
references/LINKS.md                 ← Index of Apple's official Metal documentation (fetch live)
```

Use the separate `vulkan` skill for Vulkan and MoltenVK-as-a-Vulkan-driver questions, `opengl` for native GL/GLES, and `webgl` for the browser API.

## Choose the authoritative source

| Topic | Primary source |
|---|---|
| C++ class, method, enum, and option-set declarations | `references/metal-cpp/Metal/`, `MetalFX/`, `QuartzCore/`, `Foundation/` |
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

Treat Apple's MSL specification, Feature Set Tables, and framework documentation as normative. Use `metal-cpp` for C++ declarations in the checked-out version; verify Swift and Objective-C declarations and OS availability in Apple documentation or the target SDK. Treat MoltenVK, SPIRV-Cross, samples, and the reverse-engineering repos as strong evidence of real behavior, not as definitions of Metal.

## Answer API questions

1. Identify the target: OS version, Metal version (classic `MTLCommandBuffer` path versus the Metal 4 `MTL4*` path), device GPU family, and language (Swift, Objective-C, or C++).
2. Confirm C++ declarations and enum values in `references/metal-cpp/`. Check Apple documentation or the target SDK headers for availability; do not assume the C++ bindings include availability annotations.
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

1. Start with the user's reproduction. Use a matching sample in `references/metal-cpp-samples/` or `references/metal-samples/` when it helps isolate the issue.
2. Check the usual failure surface: device and family support, pipeline state creation errors, shader library compilation logs, resource storage modes, `MTLHeap` and residency, argument buffer tiers, drawable acquisition and lifetime on `CAMetalLayer`, render pass load/store actions, and command buffer error state.
3. For Metal 4, additionally check command allocator reuse, argument tables, residency sets, and compiler task completion.
4. For Apple GPUs, reason about tile-based deferred rendering explicitly: memoryless attachments, tile memory budget, load/store action cost, and imageblock usage. The Apple documentation index links the TBDR tuning guide.
5. For instruction-level performance, use `references/metal-benchmarks/README.md` for throughput and occupancy, and `references/applegpu/` when the question is genuinely about the ISA.
6. Use GPU captures, counters, or benchmarks to validate performance claims. If measurement is unavailable, label static analysis as a hypothesis and identify what would test it.

## Answering strategy

- Cite the source files, relevant sections, and Apple documentation URLs used. Make local file links resolvable from the user's workspace.
- Keep Metal 4 and the classic Metal API distinct; they coexist and have different objects and lifetimes.
- Do not infer device support from a header's presence. GPU family and OS version determine availability; verify against the Feature Set Tables or a runtime query.
- Match documentation to the target OS and API version. When using current documentation to explain older behavior, check availability and revision differences.

## Reference availability and versions

- Local checkouts are snapshots. Match the sources to the user's target version; verify current claims against official upstream sources when freshness matters. Do not silently update existing checkouts.
- If a needed submodule is missing, initialize only that reference with `git submodule update --init -- references/<repo>` from this skill directory. Omit `--recursive`; nested build dependencies are unnecessary for source lookup.
- If a source remains unavailable, state the limitation and use an official upstream source when accessible. Do not present an unverified recollection as a source-backed conclusion.
