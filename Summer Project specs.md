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

**Hair**
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

*not final*
- Title
- Abstract
1. Introduction
	1. What is the rigging community focusing on? (Weirdly Stagnant baring a few areas)
	2.  Are these new systems ready to use? 
		1. Thesis statement/question
2. Method
	1. What are we trying to measure and How?
3. Integration
	1. Blender(Ctrl)
	2. Houdini Apex (Natural progression from existing systems[maybe])
	3. Unreal Engine Control Rig
		1. Each section looks like:
			1. Brief Paradigm description and basic workflow
			2. What needed to be made
			3. Actual run (complications or lack there of)
4. Discussion
	1. ????
5. Conclusion
	1. ????
- Bibliography