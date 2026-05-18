# Summer Project specs

The agreed upon question is as follows:

```
What amount of predevelopment is required so that each software's workflows are executable in a comparable amount of time?
```


To elaborate, predevelopment is being used to mean any amount of extra work or third party iteration. This can be measured in time or number of new scripts/tools. A software's workflow is the repeatable general order of operations needed to preform a large task. Workflows are primarily influenced by paradigms established by both the software and user. Workflows are a determining factor when choosing a software for development. Ideally a software would provide workflows that are both fast and intuitive while being modifiable if needed. Notably no software comes with a workflow pattern that does not need to be changed to better suite general use. Regardless each production will require varying types of workflows, not just to speed up production but to suite the projects style guides. For instance a stylised character will need to stretch and morph at the whim of the animator, while a realistic character will need to move naturally and obey physics. There is a reason to focus on workflow and paradigm's effects on rigging production as a measure of worth. It is to clarify why, if every software can eventually produce similar results, does it matter which software is used? The load baring word here being eventually. You can design a reverse foot system for any software, but Maya's implementation is less intuitive, flexible and more time consuming than a comparable design implemented in Blender.

#### Current plan
To properly test each software's workflow gaps, a range of rigs need to be made covering different concerns. 
This project will utilise a humanoid character which will require dynamic hair and cloth rigging, A dragon, and a robot. The dragon will cover quadrupedal and wing rigging as well as non-human body plans. The robot will test mechanical rigging focusing on interlocking mechanisms and dynamic constraint systems.
Each rig will strive to:
- minimise controls using dynamic systems, 
- create natural deformation, 
- be exportable to any software.
Ideally Simple animation tests will be made to properly showcase each rig.
User interfaces will not be a primary focus but will be attempted.
Skinning and base skeletons will be made in Blender and exported to each software as a starting point to avoid talking about skinning. 

We have not yet defined a specific time frame for each workflow to aim for. A rough guess would be to shoot for 3 days. 
Most of the time spent on this project will be on predevelopment, creating systems and tools that can be implemented quickly and repeatably.
The paper will measure the necessary effort in the amount of tools needed and the difficulty in making them. 

The software we are going to test are as follows:
- Unreal - cutting edge agnostic frame work built on real time systems
- Houdini Apex - powerful node based rigging supposedly with a focus on modularity, automation, and granular control.
- Blender - traditional rigging workflow to use as a control 

### Potential problems

**Wings**

Wings vary in complexity from convoluted beetle wings to simple bat wings, but all require multiple parts moving in unison smoothly.  It is also important to consider how the wings need to be animated and if there is a way to make that animation process easier. 
Houdini at first seems to have a  wing component but the Groom Deform component is more for rigging simulated hair.

**Hair
**
The Humanoid character has stylised hair. The rig for the hair needs to loosely follow momentum (not necessarily gravity)  and interact with the character's body. Ideally it should also be editable if necessary.

**Clothes**

The same general problem as hair but the character specifically has a split skirt which needs to interact with every leg movement appropriately.

**Robotics** 

Surprisingly most of the fundamental systems for robotics already exist as simple constraints in Blender and Unreal, only requiring driver management. Houdini however may need more effort, though this opinion may be just due to how hard it is to learn about Apex nodes.

**Automation**

Blender and Houdini allow for scripting and modularity with relative ease. Unreal does have scripting implementations but strongly encourages and relies on node based modular solutions. This is both a strength and situational weakness as some things like tools are typically better used as scripts. In the same vain Blender and Houdini rely to much on scripting, using them in place of modular graphs or requiring scripts to automate anything. 

**Nodes and Scripts**

Each API and system can be obtuse or unintuitive in areas so some tools maybe easy to make in one software and impractical in others. This does mean each rig may not have identical features, for instance Blender And Unreal can easily have physics driven clothes but Houdini may struggle or use a different system outright. This is expected as each system ooperates on different priorities.
## Outline

_not final_

- Title
- Abstract/Artist statement
	- Working in a modern real time development pipeline requires new tools and applications to be used to improve productivity here I explore XXXXXX and use my past experiences to determine if this XXXX is a useful implantation of the the artists time

1. Introduction
	1. What is the rigging community focusing on? (Weirdly Stagnant baring a few areas)
	2. Are these new systems ready to use?
	3. Thesis statement/question
2. Method
	1. What are we trying to measure and How?
	2. Integration
		1. Blender(Ctrl)
		2. Houdini Apex (Natural progression from existing systems)
		3. Unreal Engine Control Rig (bleeding edge)
3. Integration/Analysis
	1. This is where you talk about you - not the software: I did this thing, it worked/didn't work. I got this result. I
	    
4. Discussion
5. Next time I will do this differently
    this worked really well
6. Conclusion
	1. automation is great but human is still required and the software is XXXX
	    

- Bibliography
	- Conference Talks
		-  [APEX 101\|Houdini Talks \| SideFX](https://www.sidefx.com/learn/talks/apex-101/ "APEX 101|Houdini Talks | SideFX")
		* [A Modular Approach to Rigging in Houdini\|Houdini Talks \| SideFX](https://www.sidefx.com/learn/talks/a-modular-approach-to-rigging-in-houdini/ "A Modular Approach to Rigging in Houdini|Houdini Talks | SideFX")
		* [Procedural Character Rigging Techniques in APEX \| Esther Trilsch \| Houdini 20\.5 HIVE Paris \| Videos \& Movies on Vimeo](https://vimeo.com/970300875 "Procedural Character Rigging Techniques in APEX | Esther Trilsch | Houdini 20.5 HIVE Paris | Videos \& Movies on Vimeo")
		* [Unreal Fest 2025\: Control Rig\: Tips \& Tricks for Games \| Talks and demos](https://dev.epicgames.com/community/learning/talks-and-demos/XYDP/unreal-engine-unreal-fest-2025-control-rig-tips-tricks-for-games "Unreal Fest 2025: Control Rig: Tips \& Tricks for Games | Talks and demos")
		* [youtube\.com\/watch\?v\=o\_YJOcJQRrE\&t\=288s](https://www.youtube.com/watch?v=o_YJOcJQRrE&t=288s "youtube.com/watch?v=o_YJOcJQRrE\&t=288s")
		* [youtube\.com\/watch\?v\=HennjwB6zZ8](https://www.youtube.com/watch?v=HennjwB6zZ8 "youtube.com/watch?v=HennjwB6zZ8")
		* [Blender Conference 2024 \| dr\. Sybren](https://stuvel.eu/post/2024-10-27-blender-conference-2024/ "Blender Conference 2024 | dr. Sybren")
		* [Blender Conference 2022 \| dr\. Sybren](https://stuvel.eu/post/2022-11-07-blender-conference/ "Blender Conference 2022 | dr. Sybren")
		* [Blender Conference 2023 \| dr\. Sybren](https://stuvel.eu/post/2023-10-29-blender-conference-2023/ "Blender Conference 2023 | dr. Sybren")
		* [youtube\.com\/watch\?v\=XS8gW\_pBiBs](https://www.youtube.com/watch?v=XS8gW_pBiBs "youtube.com/watch?v=XS8gW_pBiBs")
		* [youtube\.com\/watch\?v\=LLnXBvEnG7o](https://www.youtube.com/watch?v=LLnXBvEnG7o "youtube.com/watch?v=LLnXBvEnG7o")
		* [youtube\.com\/watch\?v\=ypWk6ZgQ\-iM](https://www.youtube.com/watch?v=ypWk6ZgQ-iM "youtube.com/watch?v=ypWk6ZgQ-iM")
		* [youtube\.com\/watch\?v\=AymxOGtJoew](https://www.youtube.com/watch?v=AymxOGtJoew "youtube.com/watch?v=AymxOGtJoew")
	- BOOKS
		* [3D Character Rigging in Blender\: Bring your characters to life through rigging and make them animation\-ready by Jaime Kelly\, Paperback \| Barnes \& Noble®](https://www.barnesandnoble.com/w/3d-character-rigging-in-blender-jaime-kelly/1145012557 "3D Character Rigging in Blender: Bring your characters to life through rigging and make them animation-ready by Jaime Kelly, Paperback | Barnes \& Noble®")
		* [Amazon\.com\: A Complete Guide to Character Rigging for Games Using Blender\: 9781032203003\: Halač\, Armin\: Books](https://www.amazon.com/Complete-Guide-Character-Rigging-Blender/dp/1032203005 "Amazon.com: A Complete Guide to Character Rigging for Games Using Blender: 9781032203003: Halač, Armin: Books")
		* [Books and stuff \- Google Docs](https://docs.google.com/document/d/1-pLMMKSgmbL4pRRd1zZc1suiKh0KW_8prUq6zccsyXA/edit?tab=t.0#heading=h.uudwmvwm347a "Books and stuff - Google Docs")
		* [Complete Guide to fast 3D Animation and Rigging \(5 book series\) Kindle Edition](https://www.amazon.com/Complete-Guide-fast-Animation-Rigging/dp/B08RCWZC1B "Complete Guide to fast 3D Animation and Rigging \(5 book series\) Kindle Edition")
		* [Amazon\.com\: Design \& Development With Unreal Engine 5 and Blender\: Learn to design a unique workflow toward creating characters and worlds in Unreal Engine eBook \: Grinberg\, Michael\: Kindle Store](https://www.amazon.com/Design-Development-Unreal-Engine-Blender-ebook/dp/B0CW18FWWS "Amazon.com: Design \& Development With Unreal Engine 5 and Blender: Learn to design a unique workflow toward creating characters and worlds in Unreal Engine eBook : Grinberg, Michael: Kindle Store")
		* [Amazon\.com\: Maya \& Unreal Engine \| Complete Guide to fast 3D Animation and Rigging eBook \: Creatives\, Class\: Kindle Store](https://www.amazon.com/Unreal-Engine-Complete-Animation-Rigging-ebook/dp/B07YBJT9D3 "Amazon.com: Maya \& Unreal Engine | Complete Guide to fast 3D Animation and Rigging eBook : Creatives, Class: Kindle Store")
		* [3D Game Animation with Unreal Engine 5\: Uncover real\-time animation techniques for modern games and interactive experiences by Trygve Bjellvåg\, Paperback \| Barnes \& Noble®](https://www.barnesandnoble.com/w/3d-game-animation-with-unreal-engine-5-trygve-bjellvag/1148774197 "3D Game Animation with Unreal Engine 5: Uncover real-time animation techniques for modern games and interactive experiences by Trygve Bjellvåg, Paperback | Barnes \& Noble®")
	- Papers
		* [Rigging octopuses in Penguins of Madagascar \| Proceedings of the 2015 Digital Production Symposium](https://dl.acm.org/doi/10.1145/2791261.2791274 "Rigging octopuses in Penguins of Madagascar | Proceedings of the 2015 Digital Production Symposium")
		* [Sketch to pose in Pixar\'s presto animation system \| ACM SIGGRAPH 2015 Talks](https://dl.acm.org/doi/10.1145/2775280.2792583 "Sketch to pose in Pixar\'s presto animation system | ACM SIGGRAPH 2015 Talks")
		* [MultiThreading\_forWebsite \- disney\_MultiThreading\_SIGGRAPH2017\.pdf](https://www.multithreadingandvfx.org/course_notes/2017/disney_MultiThreading_SIGGRAPH2017.pdf "MultiThreading_forWebsite - disney_MultiThreading_SIGGRAPH2017.pdf")
		* [Designing an interaction with an octopus \| ACM SIGGRAPH 2016 Talks](https://dl.acm.org/doi/10.1145/2897839.2927434 "Designing an interaction with an octopus | ACM SIGGRAPH 2016 Talks")
		* [AutoSpline \| ACM SIGGRAPH 2016 Talks](https://dl.acm.org/doi/10.1145/2897839.2927439 "AutoSpline | ACM SIGGRAPH 2016 Talks")
		* [A hybrid approach to procedural tree skeletonization \| ACM SIGGRAPH 2017 Talks](https://dl.acm.org/doi/10.1145/3084363.3085065 "A hybrid approach to procedural tree skeletonization | ACM SIGGRAPH 2017 Talks")
	* List Videos
		* 


	- not wikis
#### USE AI

### I did this thing it was cool because XXX It made the pipeline better / worse because XXXXX