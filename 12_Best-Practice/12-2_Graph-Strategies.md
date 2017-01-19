## Graph Strategies

Now that we have our awesome drainage simulator, let's use the tool in our project. We have a new task ahead of us: designing a parametric roof. In this section we'll walk through best practices for defining our roof surface. We'll also discuss how to properly implement the drainage simulator into the workflow. First we will cover several guidelines on when to implement the practices before we begin developing our roof.

### Reduce Complexity

As you develop your Dynamo program and test ideas, it can quickly grow in size and complexity. While it is important that you create a functioning program, it is equally important to do it as efficiently as possible. Not only will your program run faster and more predictably, you along with other users will understand its logic later on.

**Modularize with Groups:**

* To create functionally distinct modules as you build a program
* If you need to move large parts of the program around while maintaining modularity and alignment
* Colors can be used to differentiate what groups are doing \(inputs vs functions\)
* To streamline Custom Node creation

![groups](/12_Best-Practice/images/12-2/groups.png)

> For how to use Groups, refer to [Managing Your Program](http://dynamoprimer.com/en/03_Anatomy-of-a-Dynamo-Definition/3-4_best_practices.html).

**Develop efficiently with Code Block:**

* If typing a number or node name is faster than searching \(Point.ByCoordinates, Number, String, Formula\)
* If you have created a large collection of simple nodes they can be written in a single block \(Node to Code\)
* If you want to define a function

![codeblock](/12_Best-Practice/images/12-2/codeblock.png)

> It was much faster to write a few lines of code than it was to search for and add each node individually. The code block is also far more concise.

> For how to use Code Block, refer to [What's a Code Block](http://dynamoprimer.com/en/07_Code-Block/7-1_what-is-a-code-block.html).

**Condense with Node to Code:**

* If you have created many nodes that could be easily represented in a single code block 
* If a collection of nodes can be condensed to code without eliminating the program’s clarity
* Pros:
  * Easily condenses code into one component that is still editable
  * Can simplify a significant portion of the graph
  * Useful if the ‘mini-program’ will not often be edited
  * Useful for incorporating other code block functionality, like functions
* Cons:
  * Generic naming makes it less legible
  * More difficult to understand for other users
  * No easy way to return to the visual programming version

![nodetocode](/12_Best-Practice/images/12-2/nodetocode.png)

> For how to use Node to Code, refer to [Design Script Syntax](http://dynamoprimer.com/en/07_Code-Block/7-2_Design-Script-syntax.html).

### Maintain Readability

In addition to making your program as simple and efficient as possible, strive for graphic clarity. Despite your best efforts to make your program intuitive with logical groupings, relationships might not be readily apparent. A simple Note inside of a group or renaming a slider can save you \(or another user\) from unnecessary confusion or panning across the graph.

**Visual continuity with Node Alignment:**

* Often and while building your program
* Prior to shipping the program to another user
* Cleanup Node Layout will automatically align your graph, though less precisely than doing it yourself
* Implies logical grouping

![alignment](/12_Best-Practice/images/12-2/alignment.png)

> For how to use Node Alignment, refer to [Managing Your Program](http://dynamoprimer.com/en/03_Anatomy-of-a-Dynamo-Definition/3-4_best_practices.html).

**Descriptive labeling by renaming:**

* Useful on inputs, especially if what they plug into will be off the screen
* Be wary of renaming nodes other than an inputs. An alternative to this is creating a custom node from a node cluster and renaming that; it will be understood that it contains something else

![inputs](/12_Best-Practice/images/12-2/inputs.png)

> To rename a node, right click on its name and choose "Rename Node...".

**Explain with Notes:**

* If something in the program requires a plain language explanation
* If a node group is large and can’t be easily understood right away

![notes](/12_Best-Practice/images/12-2/notes.png)

> For how to add a Note, refer to [Managing Your Program](http://dynamoprimer.com/en/03_Anatomy-of-a-Dynamo-Definition/3-4_best_practices.html).

**Access data efficiently:**

* If you need to access data at any level in a list of lists
* Instead of the List.Map and List.Combine nodes

> For how to use List@Level, refer to [Lists of Lists](http://dynamoprimer.com/en/06_Designing-with-Lists/6-3_lists-of-lists.html#list@level).

### Flex Continuously

**Monitor data with Watch:**

* As you build the program to verify that a module of functionality is returning what you expected

![watch](/12_Best-Practice/images/12-2/watch.png)

> The Watch nodes are being used to compare the raw translation distances with the values passed through the Sine equation.

> For how to use Watch, refer to [Library](http://dynamoprimer.com/en/03_Anatomy-of-a-Dynamo-Definition/3-2_dynamo_libraries.html).

### Ensure Reusability
It is highly likely that someone else will be opening your program at some point, even if you are working independently. They should be able to quickly understand what the program needs and produces from its inputs and outputs. This is especially important when developing a Custom Node to be shared with the Dynamo community and used in someone else’s program. These practices lead to robust programs and nodes that are reusable at a later time.

**Manage the I/O:**

* Minimize inputs and outputs as much as possible
* Determine which inputs and outputs go into scripts
* Keep inputs generic

**Embed input values with Presets:**

* If there are particular forms or conditions that you want embedded in the file
* To avoid adjusting the sliders in a program with long run times

> For how to use Presets, refer to [Managing Your Data with Presets](http://dynamoprimer.com/en/03_Anatomy-of-a-Dynamo-Definition/3-5_presets.html).

**Contain groups with Custom Nodes:**

* If a portion of your program can be collected into a single container
* If a portion of a program will be reused often in other programs
* If you want to share a portion of a program with the Dynamo Community

![customnode](/12_Best-Practice/images/12-2/customnode.png)

> Condensing the point translation program into a Custom Node makes a robust, unique program portable and far easier to understand. Well named input ports will help other users understand how to use the node.

> For how to use Custom Nodes, refer to [Custom Node Introduction](http://dynamoprimer.com/en/09_Custom-Nodes/9-1_Introduction.html).

### Script Strategically

Python scripting is a powerful tool to use in your program as shown in Python Nodes and Scripting Strategies. Where the capability of visual programming ends, scripting brings a host of tools to analyze, simulate, automate, and more. Knowing when to introduce scripting will ensure that you are taking full advantage of its capabilities.

**Implement Looping:**

* If you need to implement looping

**Access external libraries:**

* If the Dynamo Library is lacking a certain functionality

**Implement Recursion:**

* If you need to implement subdivision

> For how to implement Python scripting, refer to [Python Node](http://dynamoprimer.com/en/09_Custom-Nodes/9-4_Python.html).

### Exercise - Architectural Roof

> Download the example file that accompanies this exercise \(Right click and "Save Link As..."\). A full list of example files can be found in the Appendix. [RoofDrainageSim.zip](12_Best-Practice/datasets/12-2/RoofDrainageSim.zip)

Now that we have established several best practices, let’s apply them to a program that was put together quickly. Though the program succeeds in generating the roof, the state of the graph is a "mind-map" of the author. It lacks any organization or description of its use. We will walk through our best practices to organize, describe, and analyze the program so other users can understand how to use it.

![mindmap](/12_Best-Practice/images/12-2/00.jpg)

> The program is functioning, but the graph is disorganized.

Let's start by establishing some hierarchy in the graph with Groups.

![groups](/12_Best-Practice/images/12-2/1-2.jpg)

> Now we can start to see the structure of the program by identifying what each part does.
>
> 1. Import 3D site model
> 2. Translate point grid based on Sine equation
> 3. Sample portion of point grid
> 4. Create architectural roof surface
> 5. Create glass curtain wall

With logical groupings established, align the nodes to create visual continuity across the graph.

![alignment](/12_Best-Practice/images/12-2/2.jpg)

> Visual continuity allows the user to see implicit relationships between nodes.

Make the program more accessible by adding another layer of graphic improvements. Add notes to describe how the program works, give inputs descriptive names, and assign colors to different types of groups. 

![notes-rename](/12_Best-Practice/images/12-2/2-2.jpg)

> These graphic improvements tell the user more about what the program is doing. The different group colors help to distinguish inputs from functions.

> 1. Notes
> 2. Inputs with descriptive names

Before we start to condense the program, let's find a strategic location to introduce the Python script drainage simulator. Plug the output of the first scaled roof surface into the respective scripting input.

![integratescripting](/12_Best-Practice/images/12-2/3.jpg)

> We've chosen to integrate scripting at this point in the program so the drainage simulation can be run on the original, single roof surface. That specific surface is not being previewed, but it saves us from having to choose the top surface of the chamfered Polysurface.
>
> 1. Source geometry for script input
> 2. Python node
> 3. Input sliders
> 4. On/off "switch"

Let's simplify the graph now that everything is in place.

![customnode-notetocode](/12_Best-Practice/images/12-2/3-2.jpg)

> Condensing our program with Node to Code and Custom Node has greatly reduced the size of the graph. The groups that create the roof surface and walls have been converted to code since they are very specific to this program. The point translation group is contained in a Custom Node as it could be used in another program. In the example file, create your own custom node from the translate points group.

> 1. Custom Node
> 2. Node to Code

As a final step, create presets for exemplary roof forms.

![presets](/12_Best-Practice/images/12-2/presets.jpg)

> These presets will help users see the potential of the program. 

> 1. Preset 1
> 2. Preset 2

**Our Program:**

![ourprogram](/12_Best-Practice/images/12-2/3-3.jpg)

