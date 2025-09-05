# OpenUSD Instancing Frequently Asked Questions

% TODO: Finish cleaning this up.
## What are the differences between Scenegraph Instancing and Point Instancing?

Okay. So what we've been talking about mostly is what USD call scene graph instancing. And this is instancing through the composition system we have implicit prototypes derived from the composition arcs.

This by sort of, leveraging the composition system. This allows us to have instances and instance descendants identifiable via path.

So our proxies have a path. They can view them in the tree view and USD view. You can get debug information about them.

You can query them. And the instancing is transparent, right. Which is if you instance something in OpenUSD, you still basically see the same scene graph, the same, the same thing in your viewport just with one, metadata value set to false.

It's very easy to instance things in scene graph instancing. Okay. What is point instancing. So this is schema based instancing.

It's a schema that you can author an OpenUSD. Not it doesn't involve the composition system you order.

You author explicit prototypes generally as descendants of your scene, of your point instance, sir, and your instances are identifiable not by path but by index. It's instancing of point instances is tends to be an invasive process because you're doing this via schema attributes.

There's not really an easy way to do instance without, editing. You know, every attribute in your point instance, sir, or a bunch of attributes of points that are not every attribute.

Sorry. It may be combined with the scene graph instancing though, so you can have a scene graph instance, that, contains a point.

And sir, you can have a point in sensor that uses a scene graph instance as a prototype. So, you can nest these infinitely as far as OpenUSD is concerned.

But the more nesting you do, the more complicated your asset structure is. So be careful about over overdoing it.

Scenegraph Instancing
* Composition-based instancing
* Implicit prototypes derived from composition arcs
* Instance and instance descendants identifiable via path
* Transparent deinstancing

Point Instancing
* Schema-based instancing
* Explicit prototypes specified in scene description
* Instances identifiable via index
* Invasive deinstancing
* May be combined with scenegraph instancing



Scenegraph Instancing:
* What we've been talking about is scene graph instancing
* Good for reusing complex components (like full shelf assemblies, robots).
* Instances are part of the readable scenegraph; each has a "root prim” that’s editable, but the subgraph is read-only.
Point Instancing:
* Designed for massive numbers of simpler items where the overhead of an instance outweights the benefits of reuse.
* Example: a tree with 100,000 leaves
* Even more efficient, but you lose some ability to edit each instance individually.
