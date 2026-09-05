# Metal-AGENTS

An agent skill that answers Apple Metal and Metal Shading Language questions by referencing locally
cloned repositories and a curated index of Apple's official documentation, rather than relying solely
on training data. It is agent-agnostic and works with any coding agent that supports skills (Claude
Code, Codex, etc.).

[references/LINKS.md](references/LINKS.md) indexes Apple's language specification, framework
reference, and samples. Consult the relevant live documentation for semantics and availability,
and match it to the target OS and API version.

### Reference repos included

| Repo | Purpose |
|------|---------|
| `references/metal-cpp` | Apple's official C++ bindings for Metal, Metal 4, MetalFX, QuartzCore, and Foundation |
| `references/MoltenVK` | Production Metal usage, Vulkan-to-Metal mapping, and per-GPU-family limits |
| `references/SPIRV-Cross` | MSL code generation, MSL version gating, and MSL language constraints |
| `references/metal-cpp-samples` | Runnable metal-cpp examples |
| `references/metal-samples` | Runnable Objective-C Metal examples |
| `references/applegpu` | Apple GPU instruction set, disassembler, and hardware notes |
| `references/metal-benchmarks` | Apple GPU microarchitecture, instruction throughput, and concurrency measurements |

### Linked, not vendored

| File | Purpose |
|------|---------|
| `references/LINKS.md` | Index of Apple's Metal Shading Language specification, Feature Set Tables, framework reference, guides, and sample library |

## Installation

Install once into the shared agent skills directory, then symlink it into each agent's skills folder.

The reference repositories are Git submodules. Initialize them without nested dependencies for
source lookup:

```bash
git clone git@github.com:rygo6/Metal-AGENTS.git ~/.agents/skills/metal
cd ~/.agents/skills/metal
git submodule update --init
```

Do not pass `--recursive` or clone with `--recurse-submodules`.

The commands above use the recorded reference revisions. To deliberately refresh existing
references, review local changes first, then run `git submodule update --remote` and inspect the result.

Then link it into the agents you use:

```bash
mkdir -p ~/.claude/skills ~/.codex/skills
ln -s ~/.agents/skills/metal ~/.claude/skills/metal
ln -s ~/.agents/skills/metal ~/.codex/skills/metal
```

On Windows, use `mklink /J` to create a junction instead (run in `cmd`, no admin rights needed):

```bat
mklink /J "%USERPROFILE%\.claude\skills\metal" "%USERPROFILE%\.agents\skills\metal"
mklink /J "%USERPROFILE%\.codex\skills\metal"  "%USERPROFILE%\.agents\skills\metal"
```

## Usage

Once installed, request the `metal` skill by name or use your agent’s skill picker or invocation syntax.

## Related skills

Use `vulkan` for Vulkan and MoltenVK-as-a-Vulkan-driver questions, `opengl` for native OpenGL and
OpenGL ES, and `webgl` for the browser API.
