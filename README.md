# TR7AE-Mesh-Format
An overview and explanation of the Tomb Raider Legend/Anniversary mesh format.


## Section Data

The Section Data is where we find the size of the file and relocations.

**uint32 magic** = magic of the file, which is always 1413694803 in unsigned int, 4 bytes

**int32 size** = size of the file, calculated from MeshHeader to the end of the file, doesn't include Section Data

**byte m_type** = type of the file. GNC is 0

**byte_bf5** = unknown, always 0

**uint16 version ID** = unknown, always 0 (?)

Then we have **packedData** which is a bitfield. First 8 bits unknown. Last 24 bits contain the count of relocations.

**uint32 id** = unknown, always 0(?)
**uint32 specMask** = unknown, always 4294967295

Next come the **relocations**.

Relocations are offsets to offsets.

All offsets in the file must have a relocation. vertexList, normalList, faceList, etc.

A relocation starts with **typeAndSectionInfo** which is a bitfield. first 3 bits unknown. the next 13 bits declare to which file the relocation applies to (Ex. 5 = 5_0.gnc, 29 = 29_0.gnc).

**uint16 typeSpecific** = unknown, always 0 (?)
**uint32 offset** = offset to the offset


## Mesh Header

After the Section Data we have the mesh header, which contains info about bones count, offsets, model scale etc.

**int version** = version of mesh, always 79823955

**int numSegments** = count of segments (commonly called bones)

**int numVirtSegments** = count of virtual segments (weird way of calling weights)

**int segmentList** = offset to segments

next 16 bytes are inside their own struct, it contains information for the model scale. All three variables always have the same value.

**float X** = x axis scale
**float Y** = y axis scale
**float Z** = z axis scale

**int numVertices** = vertex count

**int vertexList** = vertex buffer offset

**int numNormals** = unknown. It does NOT refer to normal vectors since these are in the vertices.

**int normalList** = offset to whatever the hell Crystal Dynamics refers with "normal". This offset always refers to the second file (29_0.gnc in the case of Anniversary Lara).

**int numFaces** = faces count. Absolutely useless given the game seems to not read this value at all.

**int faceList** = This is NOT the offset to faces. Another non sense bullshit from Crystal Dynamics.

**int OBSOLETEaniTextures** = unused. Always 0.

**float maxRad** = controls how close the camera needs to be for the model to disappear.

**float maxRadSq** = unknown

**int OBSOLETEstartTextures** = unused. Always 0.

**int OBSOLETEendTextures** = unused. Always 0.

**int AnimatedListInfo** = offset to animated textures? This offset always refers to the second file (29_0.gnc in the case of Anniversary Lara).

**int AnimatedInfo** = something...else...about...animated textures? This offset always refers to the second file (29_0.gnc in the case of Anniversary Lara).

**int ScrollInfo** = offset to...whatever scrollInfo is. This offset always refers to the second file (29_0.gnc in the case of Anniversary Lara).

**int TextureStripInfo** = offset to faces

**int envMappedVertices** = offset to...whatever envMappedVertices are. This offset always refers to the second file (29_0.gnc in the case of Anniversary Lara).

**int eyeRefEnvnvMappedVertices** = offset to...whatever eyeRefEnvMappedVertices are. This offset always refers to the second file (29_0.gnc in the case of Anniversary Lara).

**int MaterialVertexColor** = offset to vertex colors. This offset always refers to the second file (29_0.gnc in the case of Anniversary Lara).

**int SpectralVertexColors** = offset to...whatever SpectralVertexColors are. Always 0 (?)

**int pnShadowFaces** = unknown. Always 0(?)

**int pnShadowEdges** = unknown. Always 0(?)

**int boneMirrorDataOffset** = offset to boneMirrorData(?)

**int drawgroupCenterOffset** = offset to drawgroupCenter(?)

**int numMarkUps** = unused for character models, always 0

**int markUpOffset** = unused for character models, always 0

**int numTargets** = unused for character models, always 0

**int TargetOffset** = unused for character models, always 0

**uint cdcRenderDataID** = can vary through models though it is apparently irrelevant

**byte cdcRenderModelData** = declares type of model, always 0
