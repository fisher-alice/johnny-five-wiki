## Control System

The **Control System** is the **Board**, which is the root of all projects. From the **Board**, any number of **[Components](#component)** may be connected and controlled. Examples: Arduino UNO, Electric Imp, BeagleBone Black, etc.

## Component

A **Component** is any sensor or effector. A **Component** is represented in a program by an instance of a class initialized with an appropriate **[Controller](#controller)**. A **Component** may also have a sub-classification in the form of a **[Device](#device)**. Examples: LED, LCD, Servo, Temperature sensor

## Controller

A **Controller** contains general or **Component**-specific programming commands that conform to a common interface.

## Device

A **Device** is a type sub-classification of a **Component**. Motors are a good example: a motor can be "directional" or "non-directional".