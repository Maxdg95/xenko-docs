# Constraints

<div class="doc-incomplete"/>

<span class="label label-doc-level">Advanced</span>
<span class="label label-doc-audience">Programmer</span>

**Constraints** restrict rigid bodies to certain movement patterns. For example, a realistic knee joint can only move along one axis and can't bend forwards.

Constraints can either link two rigid bodies together, or link a single rigid body to a point in the world. They allow for interaction and dependency among rigid bodies. 

There are six [types of constraints](xref:SiliconStudio.Xenko.Physics.ConstraintTypes):

* hinges
* gears
* sliders
* cones (twist and turn)
* point to point (fixed distance between two colliders)
* six degrees of freedom

For a demonstration of the different constraints, load the **PhysicsSample** sample project.

## Create a constraint

> [!Note]
> Currently, you can only use constraints from scripts. A constraint editor will be added to Game Studio in a future release.

To create a constraint, use the [Simulation](xref:SiliconStudio.Xenko.Physics.Simulation) static method [CreateConstraint](xref:SiliconStudio.Xenko.Physics.Simulation.CreateConstraint\(SiliconStudio.Xenko.Physics.ConstraintTypes,SiliconStudio.Xenko.Physics.RigidbodyComponent,SiliconStudio.Core.Mathematics.Matrix,System.Boolean\)):

```cs
CreateConstraint(ConstraintTypes type, RigidbodyComponent rigidBodyA, Matrix frameA, bool useReferenceFrameA);
```

This links [RigidBodyA](xref:SiliconStudio.Xenko.Physics.Constraint.RigidBodyA) to the world at its current location.
The boolean [useReferenceFrameA](xref:SiliconStudio.Xenko.Physics.Simulation.CreateConstraint\(SiliconStudio.Xenko.Physics.ConstraintTypes,SiliconStudio.Xenko.Physics.RigidbodyComponent,SiliconStudio.Core.Mathematics.Matrix,System.Boolean\)) specifies which coordinate system the limit is applied to (either [RigidBodyA](xref:SiliconStudio.Xenko.Physics.Constraint.RigidBodyA) or the world).

> [!Note]
> * In the case of [ConstraintTypes.Point2Point](xref:SiliconStudio.Xenko.Physics.ConstraintTypes), the frame represents a pivot in A. Only the translation vector is considered. [useReferenceFrameA](xref:SiliconStudio.Xenko.Physics.Simulation.CreateConstraint\(SiliconStudio.Xenko.Physics.ConstraintTypes,SiliconStudio.Xenko.Physics.RigidbodyComponent,SiliconStudio.Core.Mathematics.Matrix,System.Boolean\)) is ignored.
> * In the case of [ConstraintTypes.Hinge](xref:SiliconStudio.Xenko.Physics.ConstraintTypes), the frame represents a pivot in A and Axis in A. This is because the hinge allows only a limited angle of rotation between the rigid body and the world.
> * In the case of [ConstraintTypes.ConeTwist](xref:SiliconStudio.Xenko.Physics.ConstraintTypes), [useReferenceFrameA](xref:SiliconStudio.Xenko.Physics.Simulation.CreateConstraint\(SiliconStudio.Xenko.Physics.ConstraintTypes,SiliconStudio.Xenko.Physics.RigidbodyComponent,SiliconStudio.Core.Mathematics.Matrix,System.Boolean\)) is ignored.
> * [ConstraintTypes.Gear](xref:SiliconStudio.Xenko.Physics.ConstraintTypes) needs two rigid bodies to be created. This function will throw an exception.

```cs
CreateConstraint(ConstraintTypes type, RigidbodyComponent rigidBodyA, RigidbodyComponent rigidBodyB, Matrix frameA, Matrix frameB, bool useReferenceFrameA)
```

This method links [RigidBodyA](xref:SiliconStudio.Xenko.Physics.Constraint.RigidBodyA) to  [RigidBodyB](xref:SiliconStudio.Xenko.Physics.Constraint.RigidBodyB).

> Note:
> * In the case of [ConstraintTypes.Point2Point](xref:SiliconStudio.Xenko.Physics.ConstraintTypes), the frame represents a pivot in A or B. Only the translation vector is considered. [useReferenceFrameA](xref:SiliconStudio.Xenko.Physics.Simulation.CreateConstraint\(SiliconStudio.Xenko.Physics.ConstraintTypes,SiliconStudio.Xenko.Physics.RigidbodyComponent,SiliconStudio.Core.Mathematics.Matrix,System.Boolean\)) is ignored.
> * In the case of [ConstraintTypes.Hinge](xref:SiliconStudio.Xenko.Physics.ConstraintTypes) the frame represents pivot in A/B and Axis in A/B. This is because the hinge allows only a limited angle of rotation between the rigid body and the world in this case.
> * In the case of [ConstraintTypes.ConeTwist](xref:SiliconStudio.Xenko.Physics.ConstraintTypes), [useReferenceFrameA](xref:SiliconStudio.Xenko.Physics.Simulation.CreateConstraint\(SiliconStudio.Xenko.Physics.ConstraintTypes,SiliconStudio.Xenko.Physics.RigidbodyComponent,SiliconStudio.Core.Mathematics.Matrix,System.Boolean\)) is ignored.
> * In the case of [ConstraintTypes.Gear](xref:SiliconStudio.Xenko.Physics.ConstraintTypes), [useReferenceFrameA](xref:SiliconStudio.Xenko.Physics.Simulation.CreateConstraint\(SiliconStudio.Xenko.Physics.ConstraintTypes,SiliconStudio.Xenko.Physics.RigidbodyComponent,SiliconStudio.Core.Mathematics.Matrix,System.Boolean\)) is ignored. The frame just represents the axis either in A or B; only the translation vector (which should contain the axis) is used.

The boolean [useReferenceFrameA](xref:SiliconStudio.Xenko.Physics.Simulation.CreateConstraint\(SiliconStudio.Xenko.Physics.ConstraintTypes,SiliconStudio.Xenko.Physics.RigidbodyComponent,SiliconStudio.Core.Mathematics.Matrix,System.Boolean\)) determines which coordinate system ([RigidBodyA](xref:SiliconStudio.Xenko.Physics.Constraint.RigidBodyA) or [RigidBodyB](xref:SiliconStudio.Xenko.Physics.Constraint.RigidBodyB)) the limits are applied to.

## Add constraints to the simulation

After you create a constraint, add it to the simulation from a script by calling:

```cs
this.GetSimulation().AddConstraint(constraint);
```
or:
```cs
var disableCollisionsBetweenLinkedBodies = true;
this.GetSimulation().AddConstraint(constraint, disableCollisionsBetweenLinkedBodies);
```

The parameter [disableCollisionsBetweenLinkedBodies](xref:SiliconStudio.Xenko.Physics.Simulation.AddConstraint\(SiliconStudio.Xenko.Physics.Constraint,System.Boolean\))
 stops linked bodies colliding with each other.

Likewise, to remove a constraint from the simulation, use:

```cs
this.GetSimulation().RemoveConstraint(constraint);
```

## See also
* [Colliders](colliders.md)