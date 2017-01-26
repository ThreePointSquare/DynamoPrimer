## Scripting Strategies

Say we want to simulate how rainfall will drain off of a surface in Dynamo. How will we set this up? What parameters will we use to drive the inputs? How will we define the shape of the surface? When will we use a visual program and when will we script using Python? Ultimately, we'll want to simulate rainfall across the roof of our building using Dynamo. But first, let's setup a tool to simulate rainfall on a generic surface. That way we can reuse the tool on another design iteration or even a completely different project. This first section will walk through basic best practices for setting up our drainage simulation tool. Other best practices for libraries, labeling, and styling can be found in [Scripting Reference](http://dynamoprimer.com/en/12_Best-Practice/12-3_Scripting-Reference.html).

![](images/12-1/scripting-window.JPG)

> For how to implement Python scripting, refer to [Python Node](http://dynamoprimer.com/en/09_Custom-Nodes/9-4_Python.html).

### Know When to Script

Python scripting is a powerful tool capable of achieving much more complex relationships than visual programming, yet their capabilities also overlap significantly. This makes sense because nodes are effectively pre-packaged code, and we could probably write an entire Dynamo program in Python. However, we use visual programming because the interface of nodes and wires creates an intuitive flow of graphic information. Knowing where Python's capabilities go beyond visual programming will give you major clues to when it should be used without foregoing the intuitive nature of nodes and wires.

![](images/12-1/coding.jpg)

**Use certain operations:**

* Looping

* Recursion

> The rainfall simulation will require looping.

**Access external libraries:**

* If the Dynamo libraries are lacking a certain functionality

> Refer to [Scripting Reference](http://dynamoprimer.com/en/12_Best-Practice/12-3_Scripting-Reference.html) for a list of what each Dynamo library gives you access to.

### Think Parametrically

When scripting in Dynamo, an inevitably parametric environment, it is wise to structure your code relative to the framework of nodes and wires it will be living in. Consider your Python Node as though it is any other node in the program with a few specific inputs, a function, and an expected output. This immediately gives your code inside the node a small set of variables from which to work, the key to a clean parametric system. Here are some guidelines for better integrating code into a visual program.

**Identify the external variables at play:**

* Try to determine the given parameters in your design problem so that you can construct a model that directly builds off that data.

* Before writing code, identify the variables:

  * A minimal set of inputs

  * The intended output

  * Constants

![variables](images/12-1/variables.jpg)

> Several parameters have been established prior to writing code.
>
> 1. The surface we will simulate rainfall on.
> 2. The number of rain drops \(agents\) we want.
> 3. How far we want the rain drops to travel.
> 4. Toggle between descending the steepest path versus traversing the surface.
> 5. Python Node with the respective number of inputs.
> 6. A Code Block to make the returned curves blue.

**Design the internal relationships:**

* Parametricism allows for certain parameters or variables to be edited in order to manipulate or alter the end result of an equation or system.

* Whenever entities in your script are logically related, aim to define them as functions of each other. This way when one is modified, the other can update proportionally.

* Minimize number of inputs by only exposing key parameters:

  * If a set of parameters can be derived from more parent parameters, only expose the parent parameters as script inputs. This increases the usability of your script by reducing the complexity of its interface.

![parameters](images/12-1/parameters.JPG)

> This code is from the example in [Python Node](http://dynamoprimer.com/en/09_Custom-Nodes/9-4_Python.html).

> 1. Inputs.
> 2. Variables internal to the script.
> 3. A loop that uses these inputs and variables to perform its function.

> Tip: Place as much emphasis on the process as you do on the solution.

**Don't repeat yourself \(the DRY principle\):**

* When you have multiple ways to express the same thing in your script, at some point the duplicate representations will fall out of sync which can lead to maintenance nightmares, poor factoring, and internal contradictions.

* The DRY principle is stated as "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system":

  * When this principle is successfully applied, all the related elements in your script change predictably and uniformly and all the unrelated elements do not have logical consequences on each other.

```
### BAD
for i in range(4):
  for j in range(4):
    point = Point.ByCoordinates(3*i, 3*j, 0)
    points.append(point)
```

```
### GOOD
count = IN[0]
pDist = IN[1]

for i in range(count):
  for j in range(count):
    point = Point.ByCoordinates(pDist*i, pDist*j, 0)
    points.append(point)
```

> Tip: Before duplicating entities in your script (such as constant in the example above), ask yourself if you can link to the source instead.

### Structure Modularly

As your code gets longer and more complex the “big idea”, or overarching algorithm becomes increasingly illegible. It also becomes more difficult to keep track of what \(and where\) specific things happen, find bugs when things go wrong, integrate other code, and assign development tasks. To avoid these headaches it’s wise to write code in modules, an organizational strategy that breaks up code based on the task it executes. Here are some tips for making your scripts more manageable by way of modularization.

**Write code in modules:**

* A "module" is a group of code that performs a specific task, similar to a Dynamo Node in the workspace.

* This can be anything that should be visually separated from adjacent code \(a function, a class, a group of inputs, or the libraries you are importing\).

* Developing code in modules harnesses the visual, intuitive quality of Nodes as well as the complex relationships that only textual code can achieve.

![modules](images/12-1/modules.JPG)

> These loops call a class named "agent" that we will develop in the exercise.

> 1. A code module that defines the start point of each agent.
> 2. A code module that updates the agent.
> 3. A code module that draws a trail for the agent's path.

**Spotting code re-use:**

* If you find that your code does the same \(or very similar\) thing in more than once place, find ways to cluster it into a function that can be called.

* "Manager" functions control program flow and primarily contain calls to "Worker" functions that handle low-level details, like moving data between structures.

**Only show what needs to be seen:**

* A module interface expresses the elements that are provided and required by the module.

* Once the interfaces between the units have been defined, the detailed design of each unit can proceed separately.

**Separability/Replaceability:**

* Modules don’t know or care about each other.

**General forms of modularization:**

* Code Grouping:

  ```python
  # IMPORT LIBRARIES
  import random
  import math
  import clr
  clr.AddReference('ProtoGeometry')
  from Autodesk.DesignScript.Geometry import *

  # DEFINE PARAMETER INPUTS
  surfIn = IN[0]
  maxSteps = IN[1]
  ```

* Functions:

  ```python
  # EXAMPLE FUNCTION
  def get_step_size():
    area = surfIn.Area
    stepSize = math.sqrt(area)/100
    return stepSize

  stepSize = get_step_size()
  ```

* Classes:

  ```python
  # EXAMPLE CLASS
  class MyClass:
    i = 12345

    def f(self):
      return 'hello world'

  numbers = MyClass.i
  greeting = MyClass.f
  ```

### Flex Continuously

While developing Python scripts in Dynamo, it is wise to constantly make sure that what is actually being created is in line with what you are expecting. This will ensure that unforeseen events-- syntax errors, logical discrepancies, value inaccuracies, anomalous outputs etc.-- are quickly discovered and dealt with as they surface rather than all at once at the end. Because Python scripts live inside nodes on the canvas, they are already integrated into the data flow of your visual program. This makes the successive monitoring of your script as simple as assigning data to be outputted, running the program, and evaluating what flows out of the Python Node using a Watch Node. Here are some tips for continuously inspecting your scripts as you construct them.

**Test as you go:**

* Whenever you complete a cluster of functionality:

  * Step back and inspect your code.

  * Be critical. Could a collaborator understand what this is doing? Do I need to do this? Can this function be done more efficiently? Am I creating unnecessary duplicates or dependencies?

  * Quickly test to make sure it is returning data that “makes sense”.

* Assign the most recent data you are working with in your script to the OUT identifier so that the node is always outputting relevant data when the script updates:

  * Keep an eye on geometry using the Watch3D node.

  * Keep an eye on string messages using the Watch Node.

**Anticipate “edge cases”:**

* While scripting, crank your input parameters to the minimum and maximum values of their allotted domain to check if the program still functions under extreme conditions.

* Even if the program is functioning at its extremes, check if it is returning unintended null/empty/zero values.

* Sometimes bugs and errors that reveal some underlying problem with your script will only surface during these edge cases.

  * Understand what is causing the error and then decide if it needs to be fixed internally or if a parameter domain needs to be redefined to avoid the problem.

> Tip: Always assume the that the user will use every combination of every input value that has been exposed to him/her. This will help eliminate unwanted surprises.

### Debug Efficiently

**Use watch bubble:**

* Check the data returned at different places in the code by assigning it to the OUT variable.

**Write meaningful comments:**

* A module of code will be much easier to debug if its intended outcome is clearly described.

```
# Assign your output to the OUT variable.
OUT = cubes
```

**Leverage the code's modularity:**

* The source of an issue can be isolated if the code is modular.

* Once the faulty module has been identified, fixing the problem is considerably simpler.

* When a program must be modified, code that has been developed in modules will be much easier to change:

  * You can link new or debugged modules to an existing program with the confidence that the rest of the program will not change.

### Exercise - Steepest Path

> Download the example file that accompanies this exercise \(Right click and "Save Link As..."\). A full list of example files can be found in the Appendix. [SteepestPath.dyn](datasets/12-1/SteepestPath.dyn)

This script will derive the path a ball would take if released at a given point on a surface. It will construct the paths by stitching together small and discrete steps taken by walking agents.

![](/12_Best-Practice/images/12-1/gd01.JPG)

Let’s walk through how we want it to work. The first thing we need to do is import libraries.

![](/12_Best-Practice/images/12-1/gd02.jpg)

> We will need to import all the libraries that we intend on using.

Next we will define the script's inputs, which will display as input ports on the node.

![](/12_Best-Practice/images/12-1/gd03.jpg)

> We will need to provide some key parameters:
>
> 1. The surface we want to walk down.
> 2. The number of agents we want to walk.
> 3. The maximum number of steps the agents are allowed to take.

Now let's create the body of our script, the agent class.

![](/12_Best-Practice/images/12-1/gd04.jpg)

> We will need to define a class, or blueprint, for an agent with the intention of walking down a surface by choosing to travel in the steepest possible direction each time it takes a step:
>
> 1. Name.
> 2. Global attributes that all the agents share.
> 3. Instance attributes that are unique to each agent.
> 4. A function for taking a step.
> 5. A function for cataloging the position of each step to a trail list.

Initialize the agents by defining their start locations.

![](/12_Best-Practice/images/12-1/gd05.jpg)

> We will need to instantiate all the agents we want to observe walk down the surface and define their initial attributes:
>
> 1. Where they will start their journey on the surface.
> 2. A new empty trail list.

Update each agent at each step.

![](/12_Best-Practice/images/12-1/gd06.jpg)

> We will then need to enter a nested loop where for each agent and for each step, we update and record their position into their trail list. At each step we will also make sure the agent hasn’t reached a point on the surface where it cannot take another step which will allow it to descend. If that condition is met, we will end that agent's trip.

Now that our agents have completed their paths, let's graphically represent them as lines.

![](/12_Best-Practice/images/12-1/gd07.jpg)

> After all the agents have either reached their limit of descent or their maximum number of steps we will create a polycurve through the points in their trail list and output the polycurve trails.

Our script for finding the steepest paths.

![](/12_Best-Practice/images/12-1/gd08.jpg)

