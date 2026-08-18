# Metal-AGENTS

An agent skill that answers Apple Metal and Metal Shading Language questions by referencing locally
cloned repositories and a curated index of Apple's official documentation, rather than relying solely
on training data. It is agent-agnostic and works with any coding agent that supports skills (Claude
Code, Codex, etc.).

Apple does not publish the Metal specification, framework reference, or sample library as git
repositories. Those are indexed as links in `references/LINKS.md` and fetched live, so answers track
Apple's current text instead of a mirrored copy that goes stale.

### Reference repos included

| Repo | Purpose |
|------|---------|
| `references/metal-cpp` | Apple's official C++ bindings — full Metal, Metal 4, MetalFX, QuartzCore, and Foundation API surface |
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

The reference repos are git submodules. Initialize them one level deep only — they are read-only
references that are never built, so nested build dependencies are pure overhead:

```bash
git clone git@github.com:rygo6/Metal-AGENTS.git ~/.agents/skills/metal
cd ~/.agents/skills/metal
git submodule update --init
git submodule update --remote
```

Do not pass `--recursive` or clone with `--recurse-submodules`.

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

Once installed, invoke `/metal` from any agent that supports skills.

## Related skills

Use `vulkan` for Vulkan and MoltenVK-as-a-Vulkan-driver questions, `opengl` for native OpenGL and
OpenGL ES, and `webgl` for the browser API.
