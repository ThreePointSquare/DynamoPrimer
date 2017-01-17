## Graph Strategies

Now that we have our awesome drainage simulator, let's use the tool in our project. We have a new task ahead of us: designing a parametric roof. In this section we'll walk through best practices for defining our roof surface. We'll also discuss how to properly implement the drainage simulator into the workflow. First we will cover several guidelines on when to implement the practices before we begin developing our roof.

### Reduce Complexity

As you develop your Dynamo program and test ideas, it can quickly grow in size and complexity. While it is important that you create a functioning program, it is equally important to do it as efficiently as possible. Not only will your program run faster and more predictably, you along with other users will understand its logic later on.

**Modularize with Groups:**

* To create functionally distinct modules as you build a program
* If you need to move large parts of the program around while maintaining modularity and alignment
* Colors can be used to differentiate what groups are doing \(inputs vs functions\)
* To streamline Custom Node creation

![](/assets/groups.JPG)

> For how to use Groups, refer to Managing Your Program.

**Develop efficiently with Code Block:**

* If typing a number or node name is faster than searching \(Point.ByCoordinates, Number, String, Formula\)
* If you have created a large collection of simple nodes they can be written in a single block \(Node to Code\)
* If you want to define a function

![](/assets/codeblock.JPG)

> For how to use Code Block, refer to What's a Code Block.

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

![](/assets/nodetocode.JPG)

> For how to use Node to Code, refer to Design Script Syntax.

### Maintain Readability

In addition to making your program as simple and efficient as possible, strive for graphic clarity. Despite your best efforts to make your program intuitive with logical groupings, relationships and functions might not be readily apparent. A simple Note inside of a group or renaming a slider can save you \(or another user\) from unnecessary confusion or panning across the graph.

**Visual continuity with Node Alignment:**

* Often and while building your program
* Prior to shipping the program to another user
* Cleanup Node Layout will automatically align your graph, though less precisely than doing it yourself
* Implies logical grouping.

![](/assets/alignment.JPG)

> For how to use Node Alignment, refer to Managing Your Program.

**Descriptive labeling by renaming:**

* Useful on inputs, especially if what they plug into will be off the screen
* Be wary of renaming nodes other than an inputs
  * An alternative to this is creating a custom node from a node cluster and renaming that; it will be understood that it contains something else

![](/assets/renaming.JPG)

**Explain with Notes:**

* If something in the program requires a plain language explanation
* If a node group is large and can’t be easily understood right away

![](/assets/notes.JPG)

> For how to add a Note, refer to Managing Your Program.

### Flex Continuously

**Monitor data with Watch:**

* As you build the program to verify that a module of functionality is returning what you expected, “The Watch Nodes are essential to managing the data that is flowing through your Visual Program.” 3.3 DP

> For how to use Watch, refer to Library.

### Portability

**Embed input values with Presets:**

* If there are particular forms or conditions that you want embedded in the file
* To avoid adjusting the sliders in a program with intense calculations

**Contain groups with Custom Nodes:**

* If a portion of your program can be collected into a single container
* If a portion of a program will be reused often in other programs
* If you want to share a portion of a program with the Dynamo Community

_screenshot_

> For how to use Custom Nodes, refer to Custom Node Introduction.

### Script Strategically

### Exercise - Architectural Roof
Now that we have established several best practices, let’s apply them to the architectural roof program that was put together rather quickly. Though it is functioning quite well, the state of the graph is a "mind-map" of the author. We will walk through our best practices to organize and describe the program so other users can understand how to use it.

![](/assets/00.jpg)


> Download the example file that accompanies this exercise \(Right click and "Save Link As..."\). A full list of example files can be found in the Appendix.  

**1. Groups:**  

![](/assets/01ws.png)

> 1. Import 3D site model
> 2. Translate point grid based on Sine equation
> 3. Sample portion of point grid
> 4. Create architectural roof surface
> 5. Create glass curtain wall
> 6. Apply Python drainage simulator

**2. Alignment:**

![](/assets/02ws.png)

> 1. Vertical alignment
> 2. Horizontal alignment

**3. Rename Sliders and add Notes:**

![](/assets/03ws.png)

> 1. Notes
> 2. Number sliders with descriptive names
> 3. Multi-color Groups

**4. Node to Code and Custom Node: **

![](/assets/04ws.png)

> 1. Custom Node
> 2. Node to Code

**5. Presets:**

**6. Integrate Scripting:**

