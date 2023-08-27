# ue5-parametric-cornell-box
A Parametric Cornell Box Actor for Unreal Engine 5

## Purpose
This project provides a blueprint allowing to create fully parametric Cornell Boxes in the Unreal Editor.

![The "classic" Cornell Box](Packaging/Images/1.png)

A [Cornell Box](https://en.wikipedia.org/wiki/Cornell_box) is a common test case used by graphics professionals and researchers to measure the quality of a global illumination and light transport method, such as - in the case of Unreal Engine 5 - _Lumen_, _Path Tracing_, and _Static Lighting_.

The purpose of this product is to be used as a tool in the Editor to analyze and understand Lumen behavior (direct lighting, specular lighting, global illumination, reflections/mirrors) in a tightly controlled environment: a Cornell Box.

## Usage
The functionality is exposed in the Editor as a Blueprint Actor: to instantiate a new Cornell Box, simply drag and drop the __BP_ParametricCornellBox__ blueprint in the scene. The newly instanced Box is then configurable through dedicated attributes in the Details panel of the actor: dimensions, lighting setup, walls material and colors, and box contents.

![Img](Packaging/Images/BP_Icon.png)

Boxes are fully reconstructed each time one of their attributes is updated, allowing for interactive modification within the Editor. This is done automatically by the blueprint construction script. 

Take a look at the [Attributes section](attributes) for a comprehensive list of features and available attributes. 

An example map is provided in the plugin, in the Maps folder: _Demo.umap_. This map showcases various features through different Cornell Boxes instanced in the scene.

![Img](Packaging/Images/Demo.png)

This blueprint is intended to be mostly used with real-time Lumen and Raytracing enabled (including raytraced shadows), as well as with Path Tracing. It is nevertheless possible to use it in Static Lighting scenarios, by changing the mobility attribute in the "Advanced" section of "Lighting" attributes.

## Attributes

The following screenshot gives an overview of them:

![Img](Packaging/Images/Attributes.png)

### Individual Breakdown

#### Cornell Box - Geometry

- __Box Depth (X)__
- __Box Width (Y)__
- __Box Height (Z)__
- __Wall Thickness__

These attributes are simply the box dimensions, and the global wall thickness. The defaults are set to one of a standard Cornell Box, scaled out 5 times, for practical purposes. 

By default a box is oriented following the -X direction (the opening of the box is visible when looking towards X), and its base is located at the Floor wall. 

Be sure to keep wall thickness to a reasonable value, in order to prevent potential light leaking with Lumen.

##### Advanced

- __Wall Visibilities__: allows to show/hide specific walls of the box. By default, the Front wall is not visible.
- __Geometry Mobility__: allows to change mobility of the wall geometry. By default, walls have Static mobility, so that they are both compatible with dynamic and static lighting.

#### Cornell Box - Lighting

Lighting is made of two distinct elements: a Rect Light, and an Emissive Surface keeping the same size as the light. Their attributes and behavior depends on the Light Type selected.

- __Light Type__
  
  The following types are implemented:
  - _None_ : no lighting. This is to be used for scenarios where an external lighting source is wanted (for example, a sun light) ;
  - _Rect Light_ : the Rect Light is enabled, along with the Emissive Surface, but this last one does not emits any real light (for cosmetic purposes only, it is possible to disable its visibility in advanced attributes) ;
  - _Emissive Surface_ : the Emissive Surface only, that emits real light in the scene. The emissive intensity is modulated proportionally to the emissive surface area, to keep the same intensity regarless of the surface size (like a Rect Light) ;
  - _Reflected Light_ : the Rect Light oriented towards the Emissive Surface set to a pure diffuse material, so that the is fully reflecting the received light toward the scene. This is roughly equivalent to an Emissive Surface ;
  - _Directional Light_ : a "local" directional light, implemented as the Rect Light set with a barn angle of zero, and a barn length set to the height of the box relative to the wall attachment side of the light.
- __Light Side__

  To which side of the box the light is attached: _Top_, _Bottom_, _Left_, _Right_, _Front_, _Back_
- __Light Color__

  Default is [1.0 1.0 0.9], that is, white with a really slight tint of yellow
- __Light Intensity__

  Value is in lumens.
- __Light Width__ / __Light Height__

  The Width and Height of the light, defining the visible light square. This is supported for all light types, including Emissive Surface.
- __Light Position__
  Offsets in X and Y of the origin of the light on the currently attached wall.

##### Advanced 

- __Visible Rect Light Surface__: show/hide the visible source surface of Rect Light and Directional Light.
- __Light Z Offset__: allows the light to be "detached" from the wall, for more advanced lighting setups.
- __Light Rotation Offset__: rotation of the full light system, around the barycenter of the box. Use with caution, as if used in conjuction with light position, you might be a bit confused on the results.
- __Light Attenuation Radius Scale__

By default the attenuation radius of the underlaying Rect Light is computed automatically according to the dimensions of the box and the 3D scale of the actor, so that the box is always at least __within__ the radius (we take the largest box dimension as reference). It is nevertheless possible to tweak the radius to greater values by applying a scale to the final computed radius through this attribute.
- __Light Mobility__: allows to change mobility of the lighting system. By default it is set to Movable, so that light does not produce any static lighting. Change it to Static (or Stationary) for Static Lighting scenarios.

Note that for Static Lighting, all the geometry of the box (walls and box content) have their UVs correctly setup, as well as a lightmap resolution of 256, which should be enough for human-scale scenes.

### Cornell Box - Material


### Cornell Box - Content

## Technical considerations

- The Box is transformable in 3D through the usual transformation attribute of Actors. For scaling, while it works, it is nevertheless preferable to directly change the box dimension attributes ;

- Geometry Scripting is not used, because dynamic meshes are not 100% functional with Lumen. Instead, the project relies on instancing and scaling of elementary static meshes (each individual wall, the emissive light source, the sample content, etc.).

- This project is done exclusively using Blueprints. While the code is not very difficult to implement and understand, some parts are in fact a little bit trickier than you might think: the lighting setup in particular, with a lot of parts relative to each other and various corner cases. This is not just a simple scaling of a predefined box (otherwise for example, you could not change the individual box dimensions while keeping the same wall thickness, and the Rect Light would not change its size). Also, the code has been made compact: blueprint code is heavily factorized, and uses the minimum possible assets (basically one cube and one plane). In this regard, there is two important private variables acting as a complete descriptive box specification, from which everything is derived in the code.

## Screenshots

![Img](Packaging/Images/2.png)
![Img](Packaging/Images/3.png)
![Img](Packaging/Images/5.png)
![Img](Packaging/Images/6.png)
