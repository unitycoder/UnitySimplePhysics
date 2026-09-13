# Unity Simple Physics
### Lightweight physics systems for Unity

This project provides a collection of physics-based behaviors for Unity, including ropes, cloth, and soft bodies.  
The focus is on **simplicity and usability**, aiming for **minimal authoring effort** while keeping the systems **easy to understand, modify, and integrate** into existing projects.



## Rope
**Using Rigidbodies and ConfigurableJoints**

The rope system is split into a generator and a runtime component.  
The generator instantiates a chain of prefab `Rigidbody` points and connects them using `ConfigurableJoint`s, with configurable spacing and joint parameters.

The runtime `Rope` component handles visualization by collecting the generated segment transforms and updating a `LineRenderer` each frame so the rope follows the simulated rigidbody positions.

**Demo:** https://www.youtube.com/shorts/QTXzCxIQ-xY



## Cloth
**Using Rigidbodies and ConfigurableJoints**

The cloth system also consists of a generator and a runtime component.  
The generator creates a 2D grid of prefab `Rigidbody` points and connects neighboring points using `ConfigurableJoint`s to form the cloth structure.

The `Cloth` component is responsible for visualization and optional collision behavior.  
It can render the cloth as a wireframe using line renderers or as a dynamically generated mesh whose vertices are updated each frame from the simulated point positions. Optional mesh collider updates prevent rigidbodies from passing through the cloth, provided the cloth points are not spaced too far apart.

**Demo:** https://www.youtube.com/shorts/QTXzCxIQ-xY

![Cloth Image](Images/Cloth.png)



## SoftBody
**Joint-based volumetric soft body using mesh topology**

SoftBody is an soft-body implementation that derives its simulation structure directly from a mesh.  
At runtime, the mesh is cloned and its vertices are merged to remove duplicates. For each unique vertex, a prefab `Rigidbody` is instantiated and positioned at the corresponding vertex location.

Edges are extracted from the mesh triangles, and `ConfigurableJoints` are created between rigidbodies that share an edge. These joints use linear and angular drives to approximate elastic behavior based on the original mesh topology.

Each frame, the mesh is deformed by updating its vertices from the current rigidbody positions.  
An optional mesh collider can be generated and updated dynamically, and collision impulses can be relayed back to nearby rigidbodies to improve interaction with external objects.

![SoftBody Image](Images/SoftBody.png)



## ClothBalloon
**SoftBody variant with internal pressure forces**

ClothBalloon works like SoftBody, deriving its simulation structure from a mesh by creating `rigidbodies` per unique vertex and connecting them using `ConfigurableJoint`s.

In addition, the current mesh volume is evaluated each physics step and used to apply outward pressure forces to all surface triangles, allowing the object to inflate, compress, and behave like a balloon.

Mesh deformation, optional collider updates, and collision impulse handling work the same way as in SoftBody.

![SoftBody Image](Images/ClothBallon.gif)



## EmbeddedSoftBody
**Surface-sampled joint soft body with mesh skinning**

EmbeddedSoftBody distributes prefab `rigidbodie` points across a mesh surface using Poisson disk sampling.  
These points are connected with `ConfigurableJoint`s based on nearest neighbors to form the simulation structure.

At runtime, the mesh is cloned and bound to the simulated points.  
Each vertex is influenced by up to four nearby rigidbodies using weighted skinning, and the mesh vertices are updated every frame from the current rigidbody transforms.

An optional mesh collider can be generated and updated dynamically.  
Collision impulses can be relayed to nearby rigidbodies.

![EmbeddedSoftBody Image](Images/EmbeddedSoftBody.gif)



## Moved: SoftMesh & SoftMeshLight

SoftMeshLight and SoftMesh used to be part of this project and have been removed from it.  
Both components, including their demo scenes and assets, now live in their own repository: **[Unity Soft Mesh](https://github.com/JohannHotzel/soft-mesh)**.