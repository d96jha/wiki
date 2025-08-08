**USD (Universal Scene Description)** is a **modular, extensible 3D scene format** developed by Pixar to handle **complex, collaborative, non-destructive workflows** for digital content creation.

It’s not just a file format — it’s a **whole framework** and data model designed to scale from simple props to massive film or game scenes.

---

## 🧱 High-Level Architecture of USD

USD is composed of **layers**, **composition arcs**, and **scene graph primitives (prims)**, wrapped together with powerful **file formats**, **APIs**, and **tools**.

Here’s a breakdown of the **main parts of USD**, conceptually and structurally:

---

## 🔁 1. **Stage** – The Composed Scene

The **stage** is the **root** of a USD scene in memory. It is created by opening a USD file (usually `.usd`, `.usda`, `.usdc`, or `.usdz`).

```python
from pxr import Usd
stage = Usd.Stage.Open("my_scene.usda")
```

* Think of it as a **live, composed view** of all the underlying scene data.
* The stage resolves **references, payloads, variants, overrides**, etc.
* Similar to a scene graph in a 3D engine.

---

## 🌳 2. **Prims** – Scene Graph Nodes

A **prim** (short for primitive) is a node in the **scene graph**.

```usd
def Xform "Car"
{
    def Mesh "Wheels" { ... }
}
```

Types of prims:

* `Xform`: transforms/grouping
* `Mesh`: polygonal geometry
* `Scope`: non-transform grouping
* `Camera`, `Light`, `Material`, etc.

> Everything in USD is organized in a **hierarchical graph of prims**.

---

## 🧬 3. **Attributes & Properties**

Each prim has **properties**, including:

* **Attributes**: values like `points`, `normals`, `color`, `intensity`
* **Relationships**: references to other prims (e.g., material bindings)

Attributes can be:

* Static: `float myAttr = 1.0`
* Time-varying: animated over a timeline

---

## 🗂️ 4. **Layers** – Files or Parts of Files

USD stores scene data in **layers**, often mapped to files:

* `.usda` – ASCII (human-readable)
* `.usdc` – Binary (faster, more compact)
* `.usdz` – Zipped bundle (used in AR/VR)

Each layer contains **a portion of the scene**, which may:

* Define prims and attributes
* Override existing values
* Reference other layers

> Layers are composable and can **stack** using composition arcs (below).

---

## 🧩 5. **Composition Arcs** – Layer Assembly Mechanisms

USD supports several **composition arcs**, mechanisms to pull together and organize data from different layers/files:

| Arc Type        | Purpose                                    | Example                          |
| --------------- | ------------------------------------------ | -------------------------------- |
| **Reference**   | Insert a prim from another file/layer      | `reference = @model.usd@`        |
| **Payload**     | Like reference, but **lazy-loaded**        | `payload = @heavyAsset.usd@`     |
| **Inherit**     | Inherit properties from a base prim        | Reuse behaviors                  |
| **VariantSet**  | Selectable variations (e.g., LODs)         | `Car.variantSet = "Red", "Blue"` |
| **Specializes** | Override a base while preserving hierarchy | Common in look dev               |

These arcs allow for:

* Non-destructive editing
* Layer overrides
* Collaboration across teams

---

## 🧱 6. **Value Resolution and Layer Stacking**

If a property (like a material) is defined in multiple layers:

* **Stronger layers override weaker ones**
* Layers can be ordered explicitly or implicitly

This is the backbone of **non-destructive editing**.

---

## 🧵 7. **Variants and VariantSets**

Allow **choices** in the scene graph, like options for LODs, colors, shapes.

```usd
def Xform "Car"
{
    variantSet "colorVariant" = "Red"
    {
        variant "Red" {
            def Material "RedPaint" { ... }
        }
        variant "Blue" {
            def Material "BluePaint" { ... }
        }
    }
}
```

---

## 🎞️ 8. **Time & Animation**

USD supports **time-varying attributes**, which is how it handles animation:

```usd
float3 xformOp:translate.timeSamples = {
    0: (0, 0, 0),
    1: (10, 0, 0),
    2: (20, 0, 0),
}
```

Supports:

* Keyframed animation
* Skinning, blend shapes
* External animation clips (via composition arcs)

---

## 🎨 9. **Materials & Shading (via `UsdShade`)**

USD defines materials using `UsdShade`, with support for:

* PBR shading (Metallic/Roughness)
* Custom shader graphs
* Material binding per geometry or per face set

Material definitions are often stored separately and referenced in.

---

## 🔁 10. **References to External Assets**

USD supports referencing:

* Geometry
* Textures
* Materials
* Animations
* Other USD files

Supports **lazy-loading** for heavy assets (via **payloads**).

---

## 📦 11. **USDZ – Archive Format**

`.usdz` is a **zip-compressed, single-file USD package**:

* No compression internally (for performance)
* Includes referenced USD files, textures, materials
* Common for **AR/VR and mobile platforms** (used in iOS/ARKit)

---

## 🧰 Bonus: Key USD Tools

| Tool        | Description                            |
| ----------- | -------------------------------------- |
| `usdview`   | GUI scene viewer for `.usd`, `.usdz`   |
| `usdcat`    | Concatenate or flatten USD files       |
| `usdrecord` | Renders USD stage to image (via Hydra) |
| `usdzip`    | Packs/Unpacks `.usdz` archives         |
| Python API  | Core of most USD-based workflows       |

---

## 🧠 TL;DR – USD Parts Summary

| Part                 | What It Does                                 |
| -------------------- | -------------------------------------------- |
| **Stage**            | In-memory composed view of the scene         |
| **Prims**            | Scene graph nodes (Mesh, Xform, Light, etc.) |
| **Attributes**       | Properties of prims, may be animated         |
| **Layers**           | Files or fragments that can be stacked       |
| **Composition Arcs** | Reference, inherit, variant, payload, etc.   |
| **VariantSets**      | Switchable scene configurations              |
| **Animation**        | Time-varying attributes                      |
| **Materials**        | Bound with `UsdShade`                        |
| **Payloads**         | Heavy assets, loaded on demand               |
| **USDZ**             | Zipped format for portable scenes            |

---

> **My note**:
>
> I know next to nothing about this, just curious.
