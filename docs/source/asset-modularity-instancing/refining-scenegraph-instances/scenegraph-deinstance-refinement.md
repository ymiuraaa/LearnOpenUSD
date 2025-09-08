# Deinstancing Refinement

## What Is Deinstancing Refinement?

The simplest way to edit or override an instance is to disable instancing for that instance.

```{figure} ../../images/asset-modularity-instancing/deinstance-refinement.mp4
Deinstance refinement
```

At any point downstream, we can enable or disable instancing on a prim. If we have a use case where we just need to open one box, what we'll do is we'll set `instanceable = false` and now we can apply the overrides to open thet box. 

If you just have one thing that needs to be promoted to be treated uniquely from the rest of the copies, it's totally reasonable to deinstance it in a stronger layer.

You will still benefit from all the performance benefits from composition and modular asset reuse. You are just losing a little bit of the instancing performance benefits for the copy you deinstanced.

Deinstancing is simple and convenient, but if you find yourself wanting to deinstance many copies of a prototype to introduce more diversity in your scenes, then one or more of the other refinement techniques may be better suited.

## Exercise: Deinstance Refinement

If we wanted to hide the decal on a box, a simple approach is to use de-instancing. We will lose some optimization in the process, but get back full editability for that copy of the box.

1. **Run** in the terminal:

Windows:
```powershell
.\scripts\usdview.bat .\instancing\ex_sg_deinstance_refine\Scenario.usd  --camera ExCam_01
```
Linux:
```sh
./scripts/usdview.sh ./instancing/ex_sg_deinstance_refine/Scenario.usd --camera ExCam_01
```

```{tip}
**Click** *Camera > Select Camera > ExCam_01* if you ever lose your place in the scene or want to get back to this camera position.
```

2. **Click** on the top left box in the Viewport.

![](../../images/asset-modularity-instancing//top-left-box-closeup.png)

3. **Click** *Window > Interpreter* to open the Interpreter window.
4. **Run** the following code in the Interpreter window:
```python
box = usdviewApi.prim
box.SetInstanceable(False)
decal = box.GetChild("SM_CubeBox_A04_Decal_01")
decal_vis = decal.GetAttribute("visibility")
decal_vis.Set(UsdGeom.Tokens.invisible)
```

This code de-instanced the cube box asset we selected by setting the instanceable metadata to false. We were then able to edit the child decal prim to hide it without any problems. You box should look like this now with the hidden decal:

![](../../images/asset-modularity-instancing//hidden-decal.png)

5. **Run** the following code in the Interpreter window:
```python
from pprint import pprint
stats = UsdUtils.ComputeUsdStageStats(usdviewApi.stage)
pprint(stats)
```

The downside is that we are introducing more total prims by using less instancing. Here's a summarized comparison:

Scenario | Prims | Instances | Prototypes 
---|---|---|---
Instancing | 1711 | 1450 | 3
1 De-instanced Box | 1741 | 1449 | 3

Let's say we needed to make changes to more boxes. Let's apply the same decal visibility change to another box.

6. **Click** on the box to the right of your current selection in the Viewport.

![](../../images/asset-modularity-instancing//middle-box.png)

7. **Run** the following code in the Interpreter window:
```python
box = usdviewApi.prim
box.SetInstanceable(False)
decal = box.GetChild("SM_CubeBox_A04_Decal_01")
decal_vis = decal.GetAttribute("visibility")
decal_vis.Set(UsdGeom.Tokens.invisible)
```

The decal on that box should now be hidden too.

![](../../images/asset-modularity-instancing//hidden-decals-2.png)

8. **Run** the following code in the Interpreter window:
```python
from pprint import pprint
stats = UsdUtils.ComputeUsdStageStats(usdviewApi.stage)
pprint(stats)
```

The more we de-instance, the more optimizations we lose. Here's a summarized comparison:

Scenario | Prims | Instances | Prototypes 
---|---|---|---
Instancing | 1711 | 1450 | 3
1 De-instanced Box | 1741 | 1449 | 3
2 De-instanced Boxes | 1771 | 1448 | 3

While simple and convenient, this refinement approach is significantly more unoptimized than others if you are making identical refinements to more than one asset. The next refinement examples will show more optimal approaches to achieve the same.

9. **Close** usdview.