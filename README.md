# MMA Architecture 

MMA is an architectural framework for managing communications that do not communicate directly with each other.

Quick start guide:
1. Middleware is a static partial class, use it for Subscribe and publish Action, Func, Coroutines, Task
2. Module is a sealed partial class, used to as a federated manager in a Scene to handle a specification
3. Application works as a entity of a Module of the scene, these are GameObject/s, children of the Module GameObject


# Structure of comunication Workflow

Module => Service.T => Middleware => Module N+1...
