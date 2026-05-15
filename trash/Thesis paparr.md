A simple comparative analysis of rigging software.

1. Intro
    1. ?????
2. Houdini
    1. What is a bone
    2. What is a rig
    3. How do we manipulate them
    4. How do we make them
3. Blender
    1. Paradighms
    2. A different way to bone
    3. Bone “types” or “styles” (organisation)
    4. Addons
4. Maya
    1. Starting from scratch
    2. Transfering knowledge
    3. What are standards, and why
5. Scripting
    1. Python, Python, Visual Basic
    2. Documentation
    3. When to scrip and why
    4. Auto rigs, modules. And bespoke.
6. Restating notable similarities and differences
    1. Dynamic systems
7. When to use each software(conclusion)

Fundamentals of rigging:

- Bones are fundamentally just points in space, or in math terms, vectors.
    - Bones can be more complicated, but any additional properties are software-specific
- Parenting looks like this:**globalTransform(bone)=globalTransform(parent)⋅localTransform(bone)**;[https://lisyarus.github.io/blog/posts/gltf-animation.html#section-skeletal-animation](https://lisyarus.github.io/blog/posts/gltf-animation.html#section-skeletal-animation)
- Constraints are applied either before or after the local matrix is called. Then, the hierarchy is applied.
    - [https://wiki.blender.jp/Dev:Source/Data_System/Dependency_Graph/Overview#:~:text=Users%20mostly%20do%20not%20need%20to%20know,A.%20More%20indirect%20cycles%20are%20also%20possible](https://wiki.blender.jp/Dev:Source/Data_System/Dependency_Graph/Overview#:~:text=Users%20mostly%20do%20not%20need%20to%20know,A.%20More%20indirect%20cycles%20are%20also%20possible).
    - This sometimes depends on the software, if users aren't given a choice on when it should be applied.
    - Constraints can fully override the local matrix if not told to be applied separately
    - Constants can be multiplied by a percentage to ease them on or off, or if multiple constraints are applied, they are typically interpolated in a ratio
        - Except in Maya, where you can only interpolate between 2 constraints
        - [https://docs.blender.org/api/htmlI/x7484.html#:~:text=If%20there%20is%20only%20a%20single%20constraint,1.0%20means%20the%20constraint%20has%20full%20effect](https://docs.blender.org/api/htmlI/x7484.html#:~:text=If%20there%20is%20only%20a%20single%20constraint,1.0%20means%20the%20constraint%20has%20full%20effect).
- Bone orientation is the initial rotation matrix of the bone or joint and can only be affected in specific contexts or access calls.
    - Desired orientation is typically to have parent bones point at children's bones in a connected chain; otherwise, any orientation is permitted.
    - Orientation standards are software-dependent (softly)
        - Blender has the y-axis pointing to the child or tail of the bone, with z as the up direction
        - Houdini Users prefer to have the Z axis point to the child joint, with Y as the up direction
        - Maya prefers to have the x-axis point to the child's joint(see if I can
        - find my infographic), with Y as the up direction
    - Unless necessary for pivoting in a specific way, most bone orientations should align with the world axis.
    - The non-look-at and non-up axis direction is used as the primary source of rotation if in a chain, or should be limited to one axis of rotation.
- Rotation modes
    - Quanternian should be used by default.
    - Euler should be used if either revolutions are required of the bone or the bone should only rotate about one axis.
    - To avoid Euler locking
        - The primary axis of rotation should be the last axis in the Euler.
        - The orientation axis is determined on a case-by-case basis, but it should be the first axis.
        - The remaining axis should be locked and unused.
        - ZYX in this case, z orients the bone, and x is the main axis of rotation for the bone; y is ignored.
- Skinning
    - Bones can drive the vertices that are assigned to a specific group or vertex group; the vertices or the groups can also determine what percentage of effect the bones should have on themselves or each vertex.
    - It is standard practice to have every vertex have a combined weight of one. This is called Normalisation.
        - This means that across all the groups, the vertex is assigned to the weights or percentages that determine the effectiveness of each group should be, when added together, one. This way, two groups cannot have equal control of a vertex (barring 50:50), preventing strange deformation.
        - This is not a hard set rule, but a standard of practice that can be ignored if, for some reason, desired.
- Controls
    - Control should be unobtrusive and intuitive
        - This not only pertains to what the control effects are but also to how they should be used.
    - Ideally, controls are aligned with world space unless otherwise necessary, such as FK controls or models that are posed with angled feet/hands.
    - Ideally, controls are either simple flat shapes or an outline of part of the mesh closest to the bone, creating a pseudo outline effect.
- Drivers
    - Drivers are either a function that maps one set of values to an output or an expression that returns a value.
    - Drivers can draw from any property and use it to change almost any property
    - Drivers primarily using math act similarly to curves used for animation, where time is replaced by the input value
    - [https://blenderartists.org/t/b256-drivers-lagging-behind/499036](https://blenderartists.org/t/b256-drivers-lagging-behind/499036)
- Dynamics?
    - Dynamics are defined in multiple ways
        - Physics driven
            - These are systems driven by either rigid or soft body collisions.
        - Automatic updating
            - These are systems that update based on the state of other systems
            - For the animator, this is drivers and constraints
            - in-game engines this is reactive rigs that interact in real time with things like the environment.
        - Easy controls
            - These are systems that simplify the work the animator has to do or keep up with by tying more complex actions to simpler ones.
            - This can be as simple as IK constraints or as complicated as a reverse foot rig with one control instead of 6-8 controls.
- Differences
    - Paradighm
        - Functional
            - Houdini or nodebased
        - Modular
            - Blender semi-functional semi-oop
        - Procedural
            - Maya (gross) literally terminal commands strung together
    - Toolsets
        - Splitting bones
        - Batch actions(parenting etc)
        - Renaming
        - Weight painting|mirroring
        - Constraints
            - Prebuilt
            - Novel
        - Rotation mode switcher
        - Addons/plugins/3rd party tools
            - Blender
            - NgSkinTools
            - Houdini
- Scripting
    - Unique Application Programming Interface(API)
        - Vex|Hscript|HScripttComands
        - Mel
    - Python
        - MayaPy|PyMel|cmds
        - Hou.py|KineFX
        - bpy
- Auto|Modular|Bespoke
    - Blender Autorigs
    - Hou nodes
    - Maya bifrost
    - Maya autorigs
- Example rigs
    - As an example of making rigs across software, I made a generic leg rig across the three main animation software, but in different ways. One bespoke, one autorig, one modular.

Defintions

Skeleton: A hierarchical tree of bones, often used to describe the bones that deform the mesh.

Rig: The controls and mechanisms that drive the skeleton and interact with the user.

Armature: The whole system, both the rig and skeleton, as well as anything encapsulated with them.

Skeletal Mesh: The mesh and deforming part of a skeleton. Typically, what is shared between software. *can include non-deforming bones when necessary, i.e. socket bones.

Should we consider what other people think is the best rigging software?

No, because that is not the point of this, right now. We should identify the standards being advocated and explain how they can be applied universally.

Philosophies/Standards

Blender

Pierick picaut

Blender studios

ARP

Houdini

Brundleth Waite

Houdini?

Rokandic

Maya

???????

An interesting aspect of rigging is that it is one of the few things that can not be exported and imported between software. Armatures, Skeletal meshes and animations can be, but control rigs cannot, due to the vast differences in execution across the board. Hopefully, as modular rigging advances, this problem can be mitigated or solved outright, since rigs will increasingly be a set of simple functions. Even the terms to describe aspects of rigging are seemingly interchangeable. Skeleton and armature are often used interchangeably with rig, despite each having unique definitions.

Rigging is a skill set that is simultaneously sought after while we look for ways to replace it or lower the skill floor. While lowering the skill floor isn’t a bad thing, it is still extremely important to have artists with the fundamental skill set, not only because auto and modular rigging can not easily account for all situations, styles, or formats, but also to improve these systems, people need to be experimenting and mutating the mechanisms we use and standardise. Take the new modules we have in Houdini at the beginning of Kinefx; there were only a handful of nodes for basic operations, parenting, posing, ik, and blending. After a few updates and experiments, they added new nodes, such as the reverse foot ik node, which was a packaged reverse foot rig. Like many things in the technical field, this only came about from discussion and widespread experimentation, however it is not used by many rigging artists, making more advanced or powerful rigs due to its simplicity and difficulty integrating into other systems like FK/IK switches or snaps. This is the major short fall of all non-bespoke rigging; it's one static, simple or outdated mechanism, it is not fully flexible, and it is not easy to integrate into other systems. This is why Rigging artists need to focus on fundamentals like constraints, standard mechanisms, and scripting to be the people in charge of why and how the easy-to-use tools are used.

Points I want to make:

Rigging artists need to have fundamental, in-depth knowledge of software, tools, and rigging from scratch.

Despite rigging being untranslatable and operating under different paradigms, it is fundamentally the same, with the main difference being in how difficult it is to implement mechanisms.

The push for modular rigging is great, but inherently limited, and thus, there is still a need for rigging artists who can develop and improve tools and modules.

Maya is archaic despite being node-based.

Notes:

Vector shit

- - math[https://www.cs.trinity.edu/~jhowland/class.files.cs357.html/blender/blender-stuff/m3d.pdf](https://www.cs.trinity.edu/~jhowland/class.files.cs357.html/blender/blender-stuff/m3d.pdf)
    - Touchesighner docs[https://derivative.ca/UserGuide/3D_Parenting](https://derivative.ca/UserGuide/3D_Parenting)
    - Math [https://course.khoury.northeastern.edu/cs4300old/s11/L10/L10.html](https://course.khoury.northeastern.edu/cs4300old/s11/L10/L10.html)
    - The transform stack [https://math.hws.edu/graphicsbook/c2/s4.html](https://math.hws.edu/graphicsbook/c2/s4.html)
    - Code [https://www.cs.cmu.edu/~jkh/462_f02/slides/06_hierarchy.pdf](https://www.cs.cmu.edu/~jkh/462_f02/slides/06_hierarchy.pdf)
    - [https://www.youtube.com/watch?v=gCs3uC43EsY](https://www.youtube.com/watch?v=gCs3uC43EsY)
    - Real breakdown [https://gabormakesgames.com/blog_transforms_traversal.html](https://gabormakesgames.com/blog_transforms_traversal.html)
    - Practicle [https://www.youtube.com/watch?v=tghkLkUOeiQ](https://www.youtube.com/watch?v=tghkLkUOeiQ)
    - [https://graphics.stanford.edu/courses/cs148-09-fall/lectures/transform.pdf](https://graphics.stanford.edu/courses/cs148-09-fall/lectures/transform.pdf)

Your professor is right that there’s no single method of vector multiplication - there are multiple ways! The inner product (a dot product is a certain type of inner product) takes two vectors and gives a scalar; the outer product takes two vectors and gives an operator (a special type of function). Further, if you don’t limit yourself to a multiplication involving two things, you get more special cases! Like the cross product, which takes in (n-1) vectors from an n-dimensional space and spits out an orthogonal vector.

[https://www.reddit.com/r/math/comments/9ha1tc/why_does_the_dot_product_seemingly_take_the_place/](https://www.reddit.com/r/math/comments/9ha1tc/why_does_the_dot_product_seemingly_take_the_place/)

[https://dev.epicgames.com/community/learning/talks-and-demos/jZ74/workshop-rigging-in-unreal-engine](https://dev.epicgames.com/community/learning/talks-and-demos/jZ74/workshop-rigging-in-unreal-engine)

  

A simple comparative analysis of rigging software.

1. Intro
    1. ????
2. Houdini
    1. What is a bone
        1. Point in space
        2. Orientation and matricies
        3. Parenting
    2. What is a rig
        1. Hierarchy tree
        2. Constraints
        3. mechanisms
    3. How do we manipulate them?
        1. controls
    4. How do we make them
        1. APEX
        2. HDAS
        3. Modular
        4. Auto
3. Blender
    1. Paradighms
    2. A different way to bone
        1. Controls
        2. User friendly
        3. Euler vs Quant
    3. <mark style="background:#d3f8b6">Bone “types” or “styles” (organisation)</mark>
        1. Def|Ref|Mch
            1. (Bone Utils?)
        2. <mark style="background:#affad1">Naming Conventions</mark>
    4. Addons
        1. Bone manager
        2. Lazy weight
            1. Ngskintools?
        3. Bone Dynamics pro vs spring bones vs wiggle bones
4. Maya
    1. Starting from scratch
        1. Not unlike Nodes
        2. ~~5~~ 4 contractions?
    2. Transfering knowledge
        1. Despite massive differences[limitations], everything translates
        2. Bifrost
        3. That one guy who made this shit from scratch, and it's better
        4. We should be making new constraints and modules
    3. What are standards, and why
        1. Funcional
        2. Modular
        3. Procedural
5. Scripting
    1. Python, Python, Visual Basic
    2. Documentation
    3. When to scrip and why
    4. Auto rigs, modules. And bespoke.
        1. Chain Name
        2. (Bone Utils?)
6. Restating notable similarities and differences
    1. Dynamic systems
7. When to use each software(conclusion)
    1. Just don’t

  

However, this does not fully explain what is happening, as this alone would cause skew problems when rotating. This is why many software use the more expensive method of decomposing the transforms into their components: translation, rotation, and scale, and using this formula: (T2+(R2S2T1)), R2R1, S2S1) as shown by Nikita Lisitsa in a blog from July 2023, where the first transform is the parent’s global and the second is the child’s local. While the rotation and scale matrices are simply multiplied together, the translation matrix is more intricate, requiring the scale and rotation to be multiplied by the parent’s translate and then added to the local translate. This method does assume uniform scaling, which is required by most game engines. For a more flexible function, the order should be parent scale by child translate, then parent rotation, and finally, added to the child’s translate. (Gabor)

  

**Conclusion**

Rigging is a skill set that is simultaneously sought after while we look for ways to replace it or lower the skill floor. While lowering the skill floor isn’t a bad thing, it is still extremely important to have artists with the fundamental skill set, not only because auto and modular rigging can not easily account for all situations, styles, or formats, but also to improve these systems, people need to be experimenting and mutating the mechanisms we use and standardise. Take the new modules we have in Houdini at the beginning of Kinefx; there were only a handful of nodes for basic operations, parenting, posing, ik, and blending. After a few updates and experiments, they added new nodes, such as the reverse foot ik node, which was a packaged reverse foot rig. Like many things in the technical field, this only came about from discussion and widespread experimentation, however it is not used by many rigging artists, making more advanced or powerful rigs due to its simplicity and difficulty integrating into other systems like FK/IK switches or snaps. This is the major short fall of all non-bespoke rigging; it's one static, simple or outdated mechanism, it is not fully flexible, and it is not easy to integrate into other systems. This is why Rigging artists need to focus on fundamentals like constraints, standard mechanisms, and scripting to be the people in charge of why and how the easy-to-use tools are used.
~~
**Introduction** 
Computer graphics have entered a new era of accessibility, not because of AI, but because of improving technology and distribution, making it easier to learn, experiment, and create. Like with most technologies, this variety of experience leads to growth and innovation as well as discourse. In the CGI space, most software works under similar if not identical principles conceived in the 90s. However, each implements these principles under different paradigms. With render engine’s efficiency being the only empirical measure to compare, despite these engines increasingly becoming software-agnostic, the major differences argued for are in what the software adds in terms of tools and workflows. Analysing these paradigms and their implementation will assist in breaking down the barrier between software as well as discovering which tools and methods should be shared or improved. After all, many of the products of CGI software are transferable between software. Everything from mesh to animation data can be transferred easily between software. This is why tools, user experience, and accessibility are used as the main point of comparison. Similar tools appear from software to software, but due to differences in paradigm, these tools end up behaving and being used differently in each software. For example, manifold extrude was added to Blender in version 2.90 in 2020, whereas Maya wouldn’t add it until 2025 under the name smart extrude. <mark style="background:#fff88f">(Belnder manifold extrude, Maya Smart extrude)  </mark>
This is because each software’s developers have different goals and focuses, and different policies for implementation. Blender, for example, is somewhat on the bleeding edge, constantly changing and growing, whereas Maya is relied on for its unchanging stability and will only implement a feature that is tested and known to be reliable. This is evident by the fact that 3DS Max, another Autodesk software, implemented smart extrude first in 2021, and it is vastly more powerful than Blender's, which needs 3rd party add-on support to compare. <mark style="background:#fff88f">(Maya Smart extrude, Smart extrude Addon)</mark> These tools and features can then easily be compared despite being seeming identical on the surface. Among all of the possible tool sets that can be used, rigging stands out as the one product that can not be shared between software.  
Skeletal meshes, which are comprised of a mesh that is deformed by a skeleton, can be exported, but a rig, the mechanisms and controls the animators use to easily pose the skeleton, are fully software locked. Unlike mesh data and even skeletal data, the data that makes a rig is dependent on several aspects of the software. For instance, in Maya, constraints are nodes or effectively objects; they even appear in the outliner. However, in Blender, they are a property of the bone itself, stored in a dictionary of constraints. Each aspect of a rig is dependent on how software implements them; this does not make the actual mechanisms and principles of rigging software locked. Maya, Blender, and Houdini are each excellent examples of some of the major schools of rigging. Understanding each and how they differ will begin a push towards making rigs easier to share and transfer.  
**Fundamentals**  
Houdini works on a node-based system, where each node modifies the state of its input and returns the modified state. In programming terms, this is called functional-procedural. This method of state manipulation allows Houdini to easily and naturally interact with the math that goes into computer graphics. In 2020, Houdini released Kinefx, and subsequently in 2023, APEX, a rigging framework using Houdini’s preferred SurfaceOPerator(SOP) context rather than the original object-level nodes. (Kinefx release version) Kinefx also provides VectorOperator nodes and VectorEXpresons for more specific tasks. This all culminates in a flexible and understandable network of tools that allow for intricate control as well as well-rounded generalised construction.  
In Houdini, there is a popular philosophy called “Everything is points”, which is because ostensibly everything is ultimately boiled down to a vector or point, so that computers only need to use linear algebra and other maths systems to create and manipulate graphics. (APEX docs) This principle holds especially true for rigging, in all software, as in its simplest form, a bone is just a point in space with an orientation and a scalar. In Kinefx, skeletons are SOP geometry, and joints are extended SOP points, with added attributes like name and transform. <mark style="background:#fff88f">(Houdini Skel docs) </mark>This is more or less true for most implementations of skeletons and joints. Both Maya and Blender ostensibly use similar structures, except that Blender adds a tail to the joint to make it into a bone object, using this tail as a target for the orientation of one of the axes.  
Orientation is a term used to describe a bone's rest or starting rotation and is typically only modifiable under specific contexts; rotating the bone outside these contexts will not alter the orientation matrix.<mark style="background:#fff88f"> (Hou Orientation)</mark> How orientation is set determines the behaviour of a bone or control. For instance, certain controls are optimally set to align with world space(or rather armature space); however, other controls are best aligned with the direction of the limb they are controlling. The preference for how bones should be oriented varies from software to software. It's best practice for the bones in the deform skeleton to point towards their children, with a second axis reserved for up direction and the remaining axis used as the axis of rotation. While all software follows these principles, they each choose different axes for each role. In Houdini, Z is the pointing axis, and Y is up, In Maya X is the pointing with Y as up and in Blender Y is pointing and Z is up. While these are only suggestions, barring Bender’s y-axis, each software provides some tools for automating orientation, and these presets are where these preferences are derived. It's important to note that the math for orientation can be complicated, especially in Houdini and Maya, where bones not placed on the same plane disrupt calculations. In Houdini, it's good practice to snap joints to the construction plane tool to remove calculation errors. Then a VEX script can be written in a rigatributevop node, to calculate orientations. Maya uses a construction plane as well; however, it is not able to align to points by itself. Maya’s bone orientation tool is the only way to calculate orientation, making desired results more difficult to obtain. Blender has a rather wide selection of options since it doesn’t need to calculate the pointing axis, which makes it, so the only real concern is what Blender calls bone roll, x and z direction. This can be automatically calculated to roughly align with a world or local axis or even with a specific bone’s current roll.  
A joint’s orientation contributes to its location and scale as well. This is especially true when parenting. Parenting uses another object’s transforms as an additional basis vector, after the global basis vector, to calculate the transforms and transform space of an object. This can be most simply described by multiplying the global transforms of a parent by the child's local transform to return the child's global position. <mark style="background:#fff88f">(Nikita Lisitsa)</mark> However, this does not fully explain what is happening, as this alone would cause skew problems when rotating. This is why many software use the more expensive method of decomposing the transforms into their components: translation, rotation, and scale, and using this formula: (T2+(R2S2T1)), R2R1, S2S1) as shown by Nikita Lisitsa in a blog from July 2023, where the first transform is the parent’s global and the second is the child’s local. <mark style="background:#fff88f">(Nikita Lisitsa)</mark> While the rotation and scale matrices are simply multiplied together, the translation matrix is more involved. This formula does assume uniform scaling, which is required by most game engines anyway. Notably, Maya does suffer from skew problems due to non-uniform scaling, implying it doesn't utilise decomposition.<mark style="background:#fff88f"> (Maya Skewing)</mark> This is more relevant when discussing constraints as they modify this formula in different ways.  
Deforming the mesh with a skeleton requires weight painting, a fairly tedious process. While there are tools built into most software that will attempt to automate this process, they are often unable to achieve satisfactory results, and even third-party tools often come up short. This makes the weight paint tools of each software an important measure when constructing a pipeline. Houdini Kinefx provides a set of not only automatic weight nodes but also a rather robust weight paint node called Joint Capture Paint.  
Joint Capture Paint is a great example of a basic weight paint toolkit comprised of a set of brushes that let users "paint" the mesh in the viewport, adding or smoothing existing weights. All of these tools are tied to hotkeys, which can be displayed on screen, making it easier to remember. <mark style="background:#fff88f">(Houdini Joint Capture Weight)</mark> This is the bare minimum for most weight painting tools; however, modern tools often extend their tool sets with tools like mirror weights, normalise, or lock weights. Joint Capture Weight also includes all of these features, though due to them being packed in one node, it can feel unintuitive at times. While this is partially due to Houdini giving the user granular control that other software would not, this is also due to Houdini not preferring the viewport for editing. This, coupled with other things like mirroring, not actively updating but requiring user input, or the node not locking groups but unlocking groups, can make the node feel less intuitive. Maya's weight painting tools can also be difficult to understand and use, arguably to an even greater extent, as its tools are generally supplemented with external tools like ngSkinTools or brSmootthWeights. () Blender's brush system is a bit easier to understand and use, as it requires only one menu, provides intuitive locking, and custom brushes. Blender also automatically mirrors weights. Blender users similarly suggest the use of external tools like LazyWeights to improve and extend this functionality, most notably in its ability to allow for selecting vertices and setting or smoothing the weights precisely, which Maya also allows for, though without the use of add-ons, this process can feel convoluted.  
Normalisation is why group locking is so important. Normalisation is the practice of ensuring that every vertex has a total weight of one. This means that across all the groups the vertex is assigned to, the percent effectiveness of each group adds up to one, thus preventing unpredictable deformation. This is not a hard set rule, but a standard of practice that can be ignored. While this is a useful feature, it can be difficult to predict the algorithm's behaviour, especially when removing weights. This is why group locking is used to limit the possibilities of groups a vertex will end up in.  
Everything described so far is the lowest relevant level of rigging for most developers. But this foundation is necessary for understanding the intricacies of a rig and its mechanisms. Broader concepts start with describing hierarchies of bones as skeletons. A skeleton is often used as the driver for the mesh's deformation, thus also being called the deform layer. This skeleton and any mesh bound to it via skinning is called a SkellMesh; this is what is passed between software. Finally, the rig is the layer of bones, constraints, interfaces and controls that the animator will interact with. All of these layers together are typically called an armature. These definitions will be used going forward; however, they are by no means official or agreed upon, as even without switching software communities, these terms are used interchangeably.  
The rig layer is the focus of technical animators, as many of the improvements being created are implemented here. How this rig layer is built varies by software, as mechanisms are constrained by the features provided. While many of these features produce comparable results, the mechanisms rely on different designs and implementations. This is due to the varying feature sets and design goals of each software.

To make things easier, a universal starting point was made in Blender, as Blender's native and third-party skin tools are fairly robust. This SkelMesh will be exported to Maya and Houdini, then built on using their tools. This pipeline provides consistent, quick results. As mentioned earlier, while Houdini's skinning tools are solid, Houdini was not designed for heavy viewport-based editing, and Maya's tools are difficult without 3rd party support. Houdini seems to prefer this pipeline as its character importer provides the same three exports that the bone deform node needs to process the SkelMesh. This could also be due to the way Houdini processes state to begin with, but there is strong evidence that the main intent of Kinefx and Motionclip is to utilise motion capture rigs and data. <mark style="background:#fff88f">(Kinefx Retargeting)</mark> Houdini encourages the automation of rigging using components like the ik node. One of the main characteristics of components is that they can be instantiated multiple times with one node. One ik node can handle all of the limbs of a spider or a dog. Even when building an ik system from scratch using a vop network, like Brundleth Waite in 20XX <mark style="background:#fff88f">(BrundlthWaiteSpiderVideo)</mark>, it is encouraged to make it iterable over multiple IK chains.  
Figure 01  
Custom IK vop loop  
The reason custom vop networks are used over KineFX's nodes is that KineFX, while powerful, isn't quite as flexible as artists may like. This is a problem inherent to modularity. While modules can remove tedium and expedite simple tasks, they are often not designed to be further modified, making them hard to tune if not immediately corrected and difficult to use in more abnormal rigs. Even a standard KineFX rig requires a decent amount of nodes and branches, made even worse if a system needs to be built custom. Ideally, once a new system is made, it can be packaged as an Houdini Digital Asset or HDA and used again. This can obfuscate the graph, preventing access to many parameters, requiring parameters to be passed up to the hda node for ease of use. Without scripting, however, this can become quite tedious, with some things being impossible. For instance, linking multiparms (multiparameters) is possible via promotion, but if the developer builds a multiparm with a specific set of parameters, a Python script needs to be made to properly link them. Python in Houdini runs after most other scripts and processes, and is slower, causing lag or even errors, making these solutions teeter on the edge of acceptable. These issues are why APEX was made.  
All-Purpose EXecution or APEX is an extension to KineFX, focusing heavily on point evaluation and efficiency. APEX introduces a new type of graph that doubles down on everything being points to the point that the nodes themselves act as points in some ways.  
<mark style="background:#affad1">Figure 02  </mark>
<mark style="background:#affad1">The image from the website shows the APEX graph as points.  </mark>
This, at a glance, returns to a similar structure seen in the object-level rigging, where every bone was a node; it actually returns various levels of control to the developer and user. While a rig can be made from the ground up, node by node, APEX adds an additional layer of Python API to KineFX, making automation easier and preferred. This gives roughly three layers of support. At the top is the autorig component node that contains premade component scripts that can be selected, or can use custom scripts. At this level, the developer needs only to apply tags to bones and pass those tags to the component the node will build everything. This avoids the need to keep up with all of the names of bones and what parameters they should be passed to. Tags are invaluable in their ease of use, since one of the difficulties found in KineFX is how to pass non-geometry information effectively. Apex also changes how poses are processed, not only making them faster but also streamlining them. In KineFX, when making an HDA component, developers would add several rig poses that would hide all but the relevant bones, then they would set the parent class of the hda to the KineFX rig pose script.<mark style="background:#fff88f"> (BrundlthWaiteSpiderVideo)</mark> This was not intended and very unstable. For instance, using multiple rigpose nodes and selecting all controls, only one of the rig poses' controls would be seen in the graph editor. Additionally, to interact with the parameters of the bones, they would need to be passed up and organised. After some experimentation, developers would instead utilise a single rig pose node, constructing the rig section by section and merging each piece. APEX only needs to evaluate at the end of the graph, and encourages control-based setting management for things like ik fk switching.  
The APEX scene animate node provides many quality of life features for animators, like reset animation, change animation layer, and a variety of features under the right click menu, including copy/paste global transforms. Additionally, there are a range of options in the viewport providing parameter windows, quick constraints, selection sets, axis snapping and more tools that are often found in other animation software. This elevates the animation experience in Houdini to the level of most other animation software, where KineFX had yet to fully touch on the animation tools.

Figure 03
![[Pasted image 20260404001326.png]]
The various viewport functions

Control management is always a problem rigging artists face, as not all controls need to be on screen at all times, for instance, the fk controls don't need to be on when the ik controls are on to avoid visual noise and miss clicks. APEX does not appear to compensate for control clutter.

Figure 04
![[Screencast_20260403_233527.gif|284]]
Example Electra Rig blending ik and fk via a control rather than parameter 

 This is not helped by the use of simple control shapes like locators, cubes and circles; however, these can be changed on the developers' whim, including the position and colour, using the APEX Configure Controls node. Using the selection set menu, controls can be hidden, and if set up correctly, this can act as control management. It is important to note the selection sets act more as an ostensibly unorganised outliner without modification, and modifying selection sets can feel tedious.  
An example was created using APEX, where a simple leg and hip rig was made. In APEX this took five autorig component nodes: root,fk, multiik, fk/ik blend and reverse foot. Finally, configure a control node, and add character and animate scene nodes to clean things up and make it usable.
<mark style="background:#affad1">Figure 05</mark>
<mark style="background:#affad1">APEX Graph</mark>
<mark style="background:#affad1">Figure 06 </mark>
<mark style="background:#affad1">Rig squating </mark>
This rig use the starting skellmesh made in Blender, showcasing the ability to take any input rig and quickly make a rig for it. APEX excells at this, and KineFX holds up decently. Besides speed and ease, KineFX is more tedious to set up and requires components to be made first as KineFX Comes with only a handful. Overall Houdini APEX not just revolutionises Houdini's rigging and animation system, its sets a solid standard for what future modular based rigging systems can look like. While APEX does fall under a modular paradigm it avoids the short comings of said paradigm by providing tools and access to making new modules or modifying existing ones.  
**Blender**
Paradigm is a term used often when discussing programming languages, how they function and what workflows they encourage.  <mark style="background:#fff88f">(paradigms)</mark> Given rigging is similar to programming and often times relies on it heavily, co-opting this term is appropriate. The current paradigms in rigging describe the philosophy for how mechanisms are designed and interacted with. For instance APEX uses a modular or component paradigm that encourages the use of scripts to build rigs. This paradigm does abstract the actual rig mechanisms from the developers who don't try and create new components but that seems to be the goal. This paradigm allows quick access to rigging at any skill level, with relatively little work. Maya's paradigm is much older by contrast and far more linear. At the surface level scripts can be made to build rigs but these scripts are not very flexible due to other aspects of Maya. For instance Maya does not provide symmetry tools, you can sysmatrize a rig with some effort or third party support but there is no one click solution like in other software. Houdini APEX doesn't worry about this as much due to its tag system but that relies on looping over the rig constantly, where as Maya's rig is built once and updated constantly. Maya's paradigm is restrained by the systems natively provided by Autodesk, and these restraints are felt. Houdini allows for changing state non linearly due to its version of node based editing. Maya by contrasts has  actions that are always possible, creating a constantly updating state. This can be describe as linear or object based. Interestingly Blender falls between these two camps without support. While blender does not natively provide rigging modules it does provide functions that can be applied as properties, as opposed to separate from the objects. Blender rigging also provides armature layers, bone shape override, and armature context modes similar to APEX, if not implemented differently, but not found in Maya by default. This puts Blender in a strange multi paradigm state that allows for some advanced features of modularity, but also requires more tedious linear stlye operations. 

This is further exemplified by Blender's bone object. To start with Blender does not have object level bones rather bones are a aspect of an armature object and can be edited in either edit mode or pose mode contexts. Edit mode sets the default state of a rig, bones parenting, postilion, scale, and orientation, where as pose mode not only allows for animation but applying constraints which are properties of bones rather than objects or functions. To avoid orientation issues, Blender gives each bone a tail to determine the pointing angle, and is where a child would start if attached. Parenting and naming on the other hand are manually, though naming can be expedited with Blender's batch renamer. Thankfully both of these operations are helped by the symatrize tool that can mirror bones including all of its properties baring drivers. In addition to rig symmetry the use of a mirror modifier, a non destructively mesh mirror, halves skinning work. This is why Blender is being used to create the starting skellmesh in these examples.
Like Maya, Blender relies on physical mechanisms to create more complicated rig actions. Unlike Maya and Houdini, Blender's strong context modes force all of the components of these mechanisms to be bones. This requires more emphasis on bone organisation and naming conventions to make rigs easier to work on. For instance there are Four main bone groups, DEForm, REFerence, MeCHanical, ConTRoL. Def bones are what directly drive the mesh, Blender gives bones a property that sets whether they should be used as deform bones or not, using prefixes is still advised for clarity. Ref bones act as a layer between the rest of the rig and def bones, to avoid potential problems like nonuniform scale, or uncontrolled deformation. The ref bones will be parented or driven by the last two bone types and typically not interacted with directly. Mch bones and their subtype TarGeT bones are typically used as bones that have constraints applied to them or as targets of constraints. These bones are also kept away form the animator, but are what gives the animator the ease to animate more effectively. Finally the CTRL bones and their subtypes: TWeaK and WidGeT, are the layer that is animated. These typically are given shapes and colours and organised in to bone layers for ease of access to the animator.  Wgt bones are interesting as they act more like targets, their purpose is to override a CTRL bone shape's visual location. This does not actually alter the pivot of the CTRL but does give the appearance, making controls feel more intuitive. 

User friendliness is the biggest hurdle a rigging artist will face. Controls need to feel intuitive and understandable, properties need to be easy to access, control shapes need to be unobtrusive, etc. One of the biggest factors at play with user experience are the controls, not just how they move but what they look like and how they are organised. Implementation of control shapes varies, while Houdini and Blender use shape overriding, changing the rendered shape of a bone or joint, Maya requires the use of separate objects. Control clutter has been a known problem for decades and debates on how to handle it vary wildly. As mentioned Houdini APEX opts for using simple small controls and a system in the selection sets menu that allows for hiding bones. Maya by contrast struggles with how exactly to handle control management. It lacks a specialised manager and turning controls on and off by hand requires the outliner or scripting. Control shapes vary wildly from slim controls fitting the silhouette of the model to boxy obvious controls. One solution was to hide controls out right and either use a menu to select which part of the rig to manipulate or click blindly. <mark style="background:#fff88f">(Ephemeral rigging)</mark> Blender has a bone manager in the form of bone layers which unlike Maya's layers which are global, bone layers are a property of the armature object. Accessing the bone layer menu can be done with a hotkey like Houdini, but it is more common to use a script to draw some sort of user interface to the side panels.  Blender equally has no strict philosophy on how controls should look. Older styles of controls are very boxy as seen with Blender's riggify auto-rig script, with new styles opting for thinner controls like planes and circles. A newer philosophy in rigging is to build controls around surrounding mesh copying a selection of edges as the bone shape, this pulls the controls into the silhouette of the mesh ideally making them less obtrusive. 

Maya and Blender both share the problem of relying on parameters to handle some functions on a rig, KineFX also shares this issue while APEX seems to remove all excessive properties. While some properties can be driven by the motion of a control. Maya reverse foot controls are often a parameter whereas Blender and APEX use the motion of a control, some properties can not be naturally translated. Further still some quality of life features a rigging artist can provide come in the form of scripts, which typically need buttons or menus to access. These things can clutter a screen and animation graph quickly which is why a general push towards dynamic controls has been made over the past few years. Dynamic controls at its simplest describes mechanisms that streamline the animation process for a user. For instance <mark style="background:#fff88f">NAME in DATE</mark> created a hand rig that uses one control for general hand poses and one for each finger for more refined poses. While this rig still has a level where animators can move joints individually the dynamic controls condense the amount of animation curves down to only a few controls. <mark style="background:#fff88f">(BLADE HAND RIG GUY)</mark> These systems relieve the strain on animators by cutting down the amount of work it takes to pose a character, while still allowing for freedom. Even systems as simple as reverse foot systems  like seen in APEX or newer blender rigs, fall into this category of dynamic. 
The other side of dynamic rigging comes in the form of simulation driven mechanisms. Using either rigid body chains or simple soft body meshes as drivers, bones can move reactively to the mesh while still being editable and exportable. This type of mechanism is being used more and more to save performance, and retain control. The simulation alone cannot produce good results as its so simplified it will doubtlessly clip or behave unpredictably. This is where the dynamic mechanism comes in, by rigging the base of a chain of  bones, developers can approximate how the bones should react giving a starting point for the simulation to build on. This avoids many of the problems of relying on overly simple animations, and makes it easier to edit as there is now a clear mechanism to tune and control. Two blender rigs were made for a mesh made by SimplyAChair in 2023, that utilises these principles for a skirt and hair rig. (SIMPLYACHAIR) The skirt rig in particular showcasing the different ways mechanisms can be built. The earlier rig seen in figure <mark style="background:#affad1">XX</mark>  is based on a design shown by <mark style="background:#fff88f">Piarick Picaut in 20XX</mark> utilising a Blender constraint called transformation(x) effectively a simple driver that can relate one range of motion to another. The system works well but requires a significant number of drivers, making tuning unintuitive and slow. 

<mark style="background:#affad1">Figure 07</mark>
<mark style="background:#affad1">A skirt rig heavily  utilizing transformation(x) constraints</mark>

To alleviate the tuning process a new mechanism was conceived relying on a system of look at constraints and floor constrains. Floor constraints are typically used to keep feet controls above a surface avoiding clipping. However, floor constraints can target bones as well, and these bones can be parented to other bones, such as the leg in this case. This allows for a reactive system that can move when the leg moves in certain directions, without the need for drivers. Adding a mechanism for the base of the bone chain that uses a look at constraint targeting the bone with the floor constraint and a simple mechanism can now drive natural cloth motion. This mechanism has many strengths mainly due to its lighter computation and more intuitive mechanism. To tune the amount the cloth rotates or moves only requires the floor bone's rest position be changed. 

<mark style="background:#affad1">Figure XX</mark>
<mark style="background:#affad1">A skirt rig using floor mechanisms</mark>

This implementation in Houdini or Maya would require constructing a similar limit distance or floor constraint, which will need to be built from scratch, or utilise functions like Rigid body Springs. This is one of Blender's biggest strengths, its wider library of constraint modules built up by community input over several years. The downside to Blender's constraints is that they are more difficult to make or edit unlike APEX. Maya can be the most difficult to design mechanisms and constraints for as it only provides only 6 prebuilt constraints and a node system that is unintuitive. 

Autodesk Maya set many of the trends seen in modern software today, tools like Unreal and Houdini try to use similar hotkey layouts and terms as Maya to keep a general sense of cohesion. This is reinforced by most college taught artists who started in Maya though this is less and less common. As mentioned several times already Maya uses a very linear workflow, the more important difference is seen in how Maya encapsulates state. Maya uses a node based system though unlike Houdini these nodes are not meant to be edited but more represent changes and relation ships. State is so separated that shape and transform are separate nodes that have to be linked together. This is unlike Blender where an object holds all of this information together, even though Blender also stores things like Shape as independent data. 

The impact of community resources and knowledge can not be understated when discussing software differences. A communities transparency and documentation not only lowers the barrier to entry but allows for new tools to be developed, improved, and supported. This can double back into being damaging however as things like tutorials can become over saturated. This usually means quality tools and documentation get lost in a sea of worthless tools and information, something starting to be felt in Blender communities, mostly in terms of tutorial videos. The opposite of this problem looks like Maya and Houdini were up-to-date tutorials can be hard to find, information on newer features or even existing features can feel obfuscated and interacting with other users is difficult. These software tend to rely on company culture and industry spaces to propagate and spread information and tools. Another aspect of difficulty in finding information about software is discerning what things are named. This is especially bad when coming from another software as different communities use different names for the same tools or the same name for different tools. For instance the Maya community uses the word “tween” for the same action that Blender calls “breakdown”.  While an argument could be made that some terms used by specific software are unintuitive, its prudent to remember most of the terms used in CGI are taught and not immediately understood.

Comparing the options of each software like this helps with understanding how to translate mechanisms but also helps with how mechanisms are conceived. For instance Houdini does not have a floor constraint, so the aforementioned skirt mechanism designed for Blender would not immediately translate to Houdini. However, Houdini does have ray cast nodes, RBD spring constraint nodes, and strong scripting as possible alternatives that may not have been considered without the fore knowledge of The Blender floor constraint. Learning about these methods can improve the mechanism design by forcing developers to think in different ways such as thinking about this system using repulsive springs. Maya similarly has a distance node that returns a distance value between two matrices, making this node a place to start in Maya. This does mean Maya starts at a significantly lower level in comparison. Unlike Houdini however Maya does not provide many higher level components if any. This means systems and behaviours require starting nearly from scratch to translate mechanisms.

Comparing Maya's reverse foot setup to Blender's and Houdini's further shows how drastically different Maya's features can be. A reverse foot rig is how feet are more easily controlled on an ik leg rigs. The foot needs to roll in several different directions often pivoting from the toe or ball of the foot as well as the sides. A forward foot rig would pivot from the ankle and pivot the toe, much like a FK rig. The reverse foot system creates new pivots with the toe pivot, typically found at the furthest tip of the foot on the floor, being the root before being parented to the foot control. It is called "reverse" foot due to the parenting hierarchy being in reverse: toe - ball - ankle, however this is the simplest form. The design being used for comparison is a modern system that pivots in several places, notably the heel, left and right sides, toe, ball, and toe again. The most important aspect of this system is the use of a dynamic control system, the pivot control influences when and how most of the pivots are rotated. 
<mark style="background:#affad1">Figure 08</mark>
<mark style="background:#affad1">Pivot control in action.</mark>
A heel and toe pivot control are sometimes added to allow for pivoting at these point, and a toe raise mechanism that allows for control of the toe, typically up and down. The pivot control can take these responsibility, for instance making the x rotation control the toe's rotation, however this can quickly complicate the use of the control to the point of being unintuitive. Finally, its worth mentioning that the Blender and Houdini implementation are mostly the same, so Blender will be the primary comparison for Maya unless something  specific needs to be mentioned. Such as APEX's reverse foot component having the transition from ball raise to toe raise come from moving the pivot control further up, while the implementation in Blender in Maya have the user pull the control forward and back.  
<mark style="background:#affad1">Figure 09</mark>
<mark style="background:#affad1">Extra controls and how the pivot control can be used to control them instead.</mark>
<mark style="background:#affad1">Figure 10</mark>
<mark style="background:#affad1">Pivot transition from ball to toe in Blender and Houdini</mark>
<mark style="background:#affad1">Figure 11 </mark>
<mark style="background:#affad1">Blender and Maya reverse foot rigs behaving identically</mark>
As Seen <mark style="background:#d3f8b6">Figure 11</mark> the two systems produce the same results with the same general control pattern and feel. Where they differ is purely in the mechanical implementation. Houdini's functional procedural approach to rigging allows for a mechanical or control rig to be split from the main rig, then after the mechanical rig has been evaluated the deform skeleton can be blended with the mechanical rig. This system of having the reference layer copy transforms from the mechanical rig allows for easy blending between different systems. Blender rigs use a similar system though the mechanical rig is a separate layer not a branch of the existing rig. The main difference lies in the mechanism used. Blender and Maya cannot solely rely on a reverse parenting system to produce natural and intuitive results. The main issue stemming from the need to keep each part of the foot connected and pinned when not being effected. In Blender this is prevented on the ball roll system only. A bone rotates, influenced by the pivot controller, from the ball of the foot pointing back. A child of this bone is duplicated from the ankle bone, when the ball raise mechanical bone is rotated this bone is where the ankle should be and how it should be rotated, so while in ball raise mode the mechanical ankle bone that drives the reference bone uses a copytransform constraint targeting the child bone mentioned earlier. This works for the toe as well accept the toe pivots from the front of the foot. The problem with the ball roll specifically is that the toe will follow the ankle bone as it is parented to it. This parented behaviour is correct when pivoting from the toe so will be kept as is. Instead while in ball raise mode the toe mechanical bone copies the rotation of the ball raise pivot bone mentioned earlier, except since this bone points the opposite direction of the toe bone the copy rotate uses local space evaluation.
Maya by contrast lacks an easy way to counter rotate, instead developers opt for single bone IK constraints on each joint pair, ankle - ball and ball - toe. This makes it the foot easier to control the behaviour of each joint. <mark style="background:#fff88f">(Maya reverse foot)</mark> As for the actual mechanisms one of two methods can be used locators or joint chains. As mention several times already bones are just points in space, Maya has an object called a locators which is also just a point in space with an un-rendered shape. Some schools of rigging will suggest that joints only be used for the deform skeleton and all other operations be done with locators and or groups. <mark style="background:#fff88f">(/w locators!) </mark>While other schools of thought promote a bone chain instead, which allows for more control of orientation.<mark style="background:#fff88f"> (/wbones)</mark> This example uses locators for clarity. 
<mark style="background:#affad1">Figure XX</mark>
<mark style="background:#affad1">Maya and Blender Mechanical layers.</mark>
As mentioned earlier the Blender rig can operate in one of two modes depending on which constraint is made to influence the ankle mechanical bone's position. In Blender and Houdini constraints can be gradually turned on and of based on the value of a influence parameter that can be anything between 0 and 1 and acts a percent multiplier on the matrix the constraint will multiply the bones constraint by. This makes the switching of things like IK/FK or ball raise toe raise as simple as using drivers where one constraint is driven by the position of the pivot constraint, and the other is driven by the first constraint's influence but inverted, typically done by subtracting the influence of the first constraint by one. 
Maya also has a influence parameter for constraints but this implementation uses influence as a ratio. This not only means this parameter's value can be any positive real number, but also that with only one constraint the influence can either be on or off with no smooth transition.  Unless there was a second constraint of the same type applied, then there would be two values that could be smoothly transitioned between. Again however this is not sufficient as having both constraints on would cause incorrect behaviour due to the constraints effectively playing tug of war. To fix this, another inverse driver is used where the constraint that forces the toe to behave like it normally would is set to the inverse of the constraint that locks the toe in place during ball raise mode. To reiterate, in Blender and Houdini blending constraints is a built in natural function, In Maya this behaviour needs to be built from scratch using workarounds. 
Speaking briefly on drivers Blender and Houdini provide a set of options that optimises the driver workflow, such as copy parameter as new driver, copy driver, or mirror driver, Blender also combines its driver and expressions into one concept as an expression is just a driver in the form of code more or less, this condenses all driver editing to one window or menu. Maya also requires strange syntax for expressions, Blender and Houdini either expect a return statement or uses the value of a function or code snipet, Maya requires the name of the value being driven (which includes the name of the object) to be set equal to the desired expression. This makes since only when you see in the graph editor that Maya's expression editor creates a expression node setting the inputs and out puts based on the values provided, further exemplifying Maya's linear ideology and its design philosophy of  everything is a command or node.
While both rigs end with the same results it can not be over stated the tedium and confusion provided by one method over the other. Blender and Houdini offer streamlined powerful tools, features, and constraints to easily build systems while Maya utilises its own aged ideologies and workflows.
This also extends to scripting or add-on support, as an example Maya lacks a native rename tool let alone a batch rename tool, both are invaluable tools for rigging artists. Blender does have a batch renamer but lacks a convenient way to name chains of bones that would share a name but be suffixed with a number or letter, think fingers, spine or hair strands. These two scripts both use python and reveal how both Maya and Blender commit actions and store data. For instance blender relies on structs whose values can be accessed like dictionaries. Maya by contrast uses commands similar to shell script, this means developers can not access objects directly. Nodes represent these commands when they were executed. Maya calls data using names of nodes, though this also has a complication with how Maya stores and returns names. Maya builds names using the objects name at the very end, then all of its ancestors back to a root object are added beforehand separating them with pipe characters (|). For the renamer script this means each name needs to be sanitised before editing and slicing, as the function that returns selected objects, returns these long form names, while the rename function takes in just the new name of the object. By contrast Blender and even Houdini return whole objects when accessing selected objects. However, Blender returns selected bones in an unordered form making iteration more difficult.  Due to Blender's python api fully utilising the language's features, changing the name of a bone is as easy as calling the name parameter and setting it equal to the new name. In Maya the python api acts more like a wrapper for MEL script, the shell script like proprietary language Maya uses before reaching the c# level. This means that even using python functions that affect the state of the Maya file need are constrained to this shell command structure. This means to rename and object the rename function needs to be used, this function accepts the original name of the object and the new name to change to. 
**Conclusion**
Rigging and animation software are just as different as they are the same. More competitive modern tools are pushing the boundaries of rigging into modularity an automation. As these tools develop the possibility of cross software rigging pipelines becomes more and more likely.  Blender and Houdini communities have developed better automation tools and components allowing for faster easier rigs to be built. This has lowered the skill floor to allow new rigging artists to enter the community. With this influx of new artists it is important developers maintain  an understanding of how tools work fundamentally as well. This understanding should extend to how the different rigging software operate and build rigs, allowing developers to build better pipelines and better tools for all software. Houdini's APEX sets the standard for modular rigging making rigging easy for any one. Blender provides a range of tools for building dynamic mechanisms and powerful rigs. Finally Maya uses older less intuitive systems and designs forcing development to take longer and more unstable. 
Analysing these paradigms and their implementation will assist in breaking down the barrier between software as well as discovering which tools and methods should be shared or improved. The paradigms used by these systems show a clear progression from the linear manual approaches to the automated and modularized. While in their infancy Maya bifrost rigging and the Blender rigging nodes addon show just how successful the modular paradigm has become.
          <mark style="background:#9254de">          </mark>

The early days of CGI were rife with limitations.











Comment [1]

Either put this with constraints or just fucking don't [[#↑]]

Comment [2]

Conclusion para? [[#↑]]

Comment [3]

Either put this with constraints or just fucking don't [[#↑]]




1 History
2Method