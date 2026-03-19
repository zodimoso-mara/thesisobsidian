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
    - It is standard practice to have every vertex have a connected weight of one. This is called Normalisation.
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
    3. Bone “types” or “styles” (organisation)
        1. Def|Ref|Mch
            1. (Bone Utils?)
        2. Naming Conventions
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

  

However, this does not fully explain what is happening, as this alone would cause skew problems when rotating. This is why many software use the more expensive method of decomposing the transforms into their components: translation, rotation, and scale, and using this formula: (T2+(R2S2T1)), R2R1, S2S1) as shown by Nikita Lisitsa in a blog from July 2023, where the first transform is the parent’s global and the second is the child’s local. While the rotation and scale matrices are simply multiplied together, the translation matrix is more intricate, requiring the scale and rotation to be multiplied by the parent’s translate and then added to the local translate. This method does assume uniform scaling, which is required by most game engines. For a more flexible function, the order should be parent scale by child translate, then parent rotation, and finally, added to the child’s translate. (Gabor)  ([[#^[1]]]|Comment #[ 1]])

  

**Conclusion**

Rigging ([[#^[2]]]|Comment #[ 2]]) is a skill set that is simultaneously sought after while we look for ways to replace it or lower the skill floor. While lowering the skill floor isn’t a bad thing, it is still extremely important to have artists with the fundamental skill set, not only because auto and modular rigging can not easily account for all situations, styles, or formats, but also to improve these systems, people need to be experimenting and mutating the mechanisms we use and standardise. Take the new modules we have in Houdini at the beginning of Kinefx; there were only a handful of nodes for basic operations, parenting, posing, ik, and blending. After a few updates and experiments, they added new nodes, such as the reverse foot ik node, which was a packaged reverse foot rig. Like many things in the technical field, this only came about from discussion and widespread experimentation, however it is not used by many rigging artists, making more advanced or powerful rigs due to its simplicity and difficulty integrating into other systems like FK/IK switches or snaps. This is the major short fall of all non-bespoke rigging; it's one static, simple or outdated mechanism, it is not fully flexible, and it is not easy to integrate into other systems. This is why Rigging artists need to focus on fundamentals like constraints, standard mechanisms, and scripting to be the people in charge of why and how the easy-to-use tools are used.

**Opening**

Computer graphics have entered a new era of accessibility, not because of AI, but because of growing technology and distribution, making it easier to learn, experiment, and create. Like with most technologies, this variety of experience leads to growth and innovation as well as discourse. The topic most often seen around CGI is based entirely on software superiority rather than standards, implementation, or paradigms. This is almost completely superfluous, as most software works under similar if not identical principles conceived of in the 90s. With render engine’s efficiency being the only empirical measure to compare, despite these engines increasingly becoming software agnostic. The major differences argued for are in what the software adds in terms of tools and workflows. After all, many of the products of CGI software are transferable between software. Everything from mesh to animation data can be transferred between most software. This means the more relevant topic is in tools, user experience, and accessibility. Many tools are seemingly universal, such as mesh tools like loop cut and extrude. Some tools, however, can be implemented in certain software packages but not in others. For example, manifold extrude was added to Blender in version 2.90 in 2020, whereas Maya wouldn’t add it until 2025 under the name smart extrude. ([https://developer.blender.org/docs/release_notes/2.90/modeling/](https://developer.blender.org/docs/release_notes/2.90/modeling/),[https://help.autodesk.com/view/MAYAUL/2025/ENU/?guid=GUID-21E37C98-B7CA-4B9A-9883-8276FD8AB819](https://help.autodesk.com/view/MAYAUL/2025/ENU/?guid=GUID-21E37C98-B7CA-4B9A-9883-8276FD8AB819))

This is completely normal; each software’s developers have different goals and focuses, and different policies for implementation. Blender, for example, is somewhat on the bleeding edge, constantly changing and growing, whereas Maya is relied on for its static stability and will only implement a feature that is tested and known to be reliable. This is evident by the fact that 3DS Max, another Autodesk software, implemented it first in 2021 and is vastly more powerful than Blender, which needs 3rd party addon support to compare. (same Maya link, [https://superhivemarket.com/products/smart-extrude](https://superhivemarket.com/products/smart-extrude)) This is why this discussion should be considered nuanced and nearly impossible in terms of software as a whole. Then, narrowing the scope to individual toolsets such as modelling, texturing, compositing, animating, etc., there is one toolset that is uniquely software dependent, not only in workflow but in what it produces: rigging.

Rigs are unique in that they cannot be immediately translated between software. Skeletal meshes, which are comprised of a mesh that is deformed by a skeleton, can be exported,d but the rig, the mechanisms and controls the animators use to pose the skeleton, are fully software locked. Unlike mesh data and even skeletal data, the data that makes a rig a rig is dependent on several aspects of the software. For instance, in Maya, constraints are nodes or effectively objects; they even appear in the outliner. However, in Blender, they are a property of the bone itself, stored in a dictionary of constraints. In the case of drivers, if they were scripted or otherwise required specific notation, there is an additional problem of translating the syntax from one software and or API to another. Control shapes hold an even larger problem, as inHoudinii and Blender, the bone shape is overridden with a control shape, in maya controls are typically made of curve objects instead. All of these things compound into making rigs difficult or impossible to translate directly, however this does not make the actual tools and principles of rigging software locked. Maya, Blender, and Houdini{unreal?} are each excellent examples of what have been identified as the major paradigms of rigging. Analysing these paradigms and their implementation will assist in breaking down the barrier between software as well as discovering which tools and methods should be shared or improved.

**Fundamentals**

Houdini works on a node-based system, where each node modifies the state of its input and returns the modified state. In programming terms, this is called functional-procedural. This method of state manipulation allows Houdini to easily and naturally interact with the math that goes into computer graphics. In 2020 Houdini released Kinefx and subsequently in 2023 APEX, a rigging framework using Houdini’s preferred SurfaceOPerator(SOP) context rather than the original non-procedural style. () However, Kinefx does not just work at the SOP level; it also has nodes for the VectorOPerator(VOP), a system considered mid-level due to its being just a few steps above pure vector math or code. This also means VectorEXpressions or VEX script can be utilised to take advantage of scripting features as well as Houdini’s vector operators. This all culminates in a flexible and understandable network of tools that allow for intricate control as well as well-rounded generalised modular control.

In Houdini, there is a popular philosophy called “Everything is points” (APEX docs), which is because ostensibly everything is ultimately boiled down to a vector or point, so that computers only need to use linear algebra and other math systems to create and manipulate graphics. This principle holds especially true for rigging, in all software, as in its simplest form, a bone is just a point in space with an orientation and a scalar. In Kinefx, skeletons are SOP geometry, and joints are SOP points, just with a few extra attributes like name and transform. ([https://www.sidefx.com/docs/houdini/character/kinefx/skeletons.html](https://www.sidefx.com/docs/houdini/character/kinefx/skeletons.html)) This is more or less true for most implementations of skeletons and joints. Both Maya and Blender ostensibly use similar structures, except that Blender adds a tail to the joint to make it into a bone object, using this as a target for the orientation of one of the axes.

Orientation is a term to describe a bone's rest or starting rotation and is typically only modifiable under specific contexts; rotating the bone outside of these contexts will not alter the orientation matrix. Orientation in Houdini, and probably Maya, is handled by a 3x3 matrix independent of the positional matrix. This matrix is comprised of the x,y, and z axes defining the orientation of the joint, which can be set to any three vectors. Ideally, these vectors will be set perpendicular to each other, as you would expect, but which way the point is still contextual. For instance, certain controls are optimally set to align with world space(or rather root/armature space); however, other controls are best aligned with the direction of the limb they are controlling. The philosophy of how bones should be oriented varies from software to software. For instance, it's best practice for the bones in the deform skeleton should point towards their children with a second axis reserved for up or orientation and the final axis used as the main axis of rotation. While all software follows these principles, they each choose different axes for each role. In Houndini, Z is the pointing axis, Y is up, and X is the main, In Maya X is the pointing with Y as up and in Blender Y is pointing and Z is up. While these are only suggestions, barring Bender’s y-axis, each software provides some tools for automating orientation, and this is where these preferences are derived. It's important to note that these tools are delicate, especially in Houdini and Maya, as the math for these alignments is complicated, specifically for determining the up direction, as the pointing direction is a lookat function. In Houdin, it's good practice to set points where joints are desired, then use the construction plane tool, which can align to the points after selecting them in order, and finally add the joints snapped to this grid. Importantly, in Houdini, a VEX script can be used in a rigatributevop node, to create a custom orientation preference from scratch. Maya uses a construction plane as well; however, it is not able to align to points requiring the use of look-at constraints. Maya’s bone orientation tool is the only place users can adjust a bone's orientation without affecting the position of its children, implying the rotation and orientation matrices are not meaningfully separated like in Houdini. Blender has a rather wide selection of options since it doesn’t need to calculate the pointing axis, which makes it so the only real concern is what Blender calls bone roll. This can be automatically calculated to roughly align with a world or local axis or even with a specific bone’s current roll.

A joint’s scale and local position are determined by these orientations, which is partially why such care is put into its construction. This is especially important when it comes to parenting, where the math starts to lean heavily into linear algebra. Parenting is a term that describes using another object’s transforms as an additional basis vector, after the global basis vector, to calculate the transforms and transform space of an object. This can be most simply described by multiplying the global transforms of a parent by the child's local transform to return the child's global position. <mark style="background:#fff88f">(lisyarus)</mark> However, this does not fully explain what is happening, as this alone would cause skew problems when rotating. This is why many software use the more expensive method of decomposing the transforms into their components: translation, rotation, and scale, and using this formula: (T2+(R2S2T1)), R2R1, S2S1) as shown by Nikita Lisitsa in a blog from July 2023, where the first transform is the parent’s global and the second is the child’s local. <mark style="background:#fff88f">(lisyarus)</mark> While the rotation and scale matrices are simply multiplied together, the translation matrix is more intricate, requiring the scale and rotation to be multiplied by the parent’s translate and then added to the local translate. This method does assume uniform scaling, which is required by most game engines. Notably Maya does suffer from skew problems do to amusing uniform scaling.<mark style="background:#fff88f"> (Maya Skewing)</mark>  For a more flexible function, the order should be parent scale by child translate, then parent rotation, and finally, added to the child’s translate. <mark style="background:#fff88f">(Gabor)</mark> 

Everything described so far is the lowest relevant level of rigging being used here. But this foundation is necessary for understanding the intricacies of a rig and its mechanisms. With what has been discussed so far bones can be chained together through parenting, creating a hierarchy tree. This is its simplest form is called a skeleton.




thanks to All-Purpose EXecution or APEX

























Comment [1]

Either put this with constraints or just fucking don't [[#↑]]

Comment [2]

Conclusion para? [[#↑]]

Comment [3]

Either put this with constraints or just fucking don't [[#↑]]

