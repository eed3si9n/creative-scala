# タートル代数と代数的データ型

In this chapter we explore a new way of creating paths---turtle graphics---and learn some new ways of manipulating lists and functions.

<div class="callout callout-info">
例題を Doodle の sbt console 内で実行した場合は、何もしなくても動作するはずです。そうじゃない場合は、以下の import 文を使って Doodle を使用可能な状態にする必要があります。

```tut:silent
import doodle.core._
import doodle.core.Image._
import doodle.syntax._
import doodle.jvm.Java2DFrame._
import doodle.backend.StandardInterpreter._
```
</div>

## Turtle Graphics

So far our paths have used an absolute coordinate system. 
For example, if we wanted to draw a square we'd use code like

```tut:silent:book
import doodle.core.PathElement._

val path = 
  Image.openPath(
    List(moveTo(10,10), lineTo(-10,10), lineTo(-10,-10), lineTo(10, -10), lineTo(10, 10))
  )
```

It's often easier to define paths in terms of relative coordinates, specifying how far we move forward or turn relative to our current location.
This is how a turtle graphics system works. 
Here's an example.

```tut:silent:book
import doodle.turtle._
import doodle.turtle.Instruction._

// Create a list of instructions for the turtle
val instructions: List[Instruction] = 
  List(forward(10), turn(90.degrees), 
       forward(10), turn(90.degrees), 
       forward(10), turn(90.degrees), 
       forward(10))

// Ask the turtle to draw these instructions, creating an Image
val path: Image = Turtle.draw(instructions)
```

So where's the turtle in all this? 
This model was developed in the 60s by Seymour Papert in the programming language Logo. 
The original Logo could control a robot that drew on paper with a pen. 
This robot was called a turtle, due to its rounded shape, and way of programming this robot became known as turtle graphics.

Using turtle graphics and another concept, known as an L-system, we can create images that mimic nature such as the plant in [@fig:turtles:plant].

![A plant generated using turtle graphics and an L-system.](src/pages/turtles/plant.pdf+svg){#fig:turtles:plant}
