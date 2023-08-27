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

Boxes are fully reconstructed each time one of their attributes is updated, allowing for interactive modification within the Editor. This is done automatically by the blueprint construction script. The default Box attributes are set to one of a standard Cornell Box, scaled out 5 times, for practical purposes.

Take a look at the [Attributes section](attributes) for a comprehensive list of features and available attributes. 

An example map is provided in the plugin, in the Maps folder: _Demo.umap_. This map showcases various features through different Cornell Boxes instanced in the scene.

![Img](Packaging/Images/Demo.png)

This blueprint is intended to be mostly used with real-time Lumen and Raytracing enabled (including raytraced shadows), as well as with Path Tracing. It is nevertheless possible to use it in Static Lighting scenarios, by changing the mobility attribute in the "Advanced" section of "Lighting" attributes.

## Attributes

The following screenshot gives an overview of them:

![Img](Packaging/Images/Attributes.png)

## Technical considerations

- The box is fully transformable in 3D through the usual transformation attribute of actors, and in particular 3D scaling (each individual element will behave correctly, such as proper Rect Light dimensions update). For scaling, it is nevertheless recommended to directly use the individual box dimensions attributes, as it gives fine-grained control on the final result.

- Geometry Scripting is not used, because dynamic meshes are not 100% functional with Lumen. Instead, the project relies on instancing and scaling of elementary static meshes (each individual wall, the emissive light source, the sample content, etc.).

- This project is done exclusivelly using Blueprints. While not very difficult to implement and understand, some parts are in fact a little bit more trickier than you might think: the lighting setup, with a lot of parts relative to each other and various corner cases. This is not just a simple scaling of a predefined box (otherwise for example, you could not change the box dimensions while keeping the same wall thickness). Also, the code have been made compact: blueprint code is heavilly factorized, and uses the minimum possible assets (basically one cube and one plane). There is one important variable acting as the box specification, from which everything is derived in the code.

## Screenshots

![Img](Packaging/Images/2.png)
![Img](Packaging/Images/3.png)
![Img](Packaging/Images/5.png)
![Img](Packaging/Images/6.png)
