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

rest of the data is unknown, it's usually just a bunch of zeros and some unknown bytes at the end. BoneMirrorData and drawGroupCenter are unknown, no structs were found in the PDB.

## Segments

SEGMENTS:

As mentioned in Mesh Header, segments are bones.

**float minXPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**float minZPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**float minYPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**uint minWPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**float maxXPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**float maxZPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**float maxYPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**uint maxWPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**float pivotXPos** = position X of segment

**float pivotZPos** = position Z of segment

**float pivotYPos** = position Y of segment

**float pivotwPos** = unknown(?)

**uint flags** = didn't find any flags enum for segments in the PDB, always 0(?)

**int16 FirstVertex** = declares first vertex that the segment affects

**int16 LastVertex** = declares last vertex that the segment affects

**uint HInfo** = offset to the segment's HInfo

## VirtSegments

VIRTSEGMENTS:

As mentioned in Mesh Header, VirtSegments are weights.

**float minXPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**float minZPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**float minYPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**uint minWPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**float maxXPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**float maxZPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**float maxYPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**uint maxWPos** = unknown, probably something to do with collision? Setting it to 0 has no noticeable effect

**float pivotXPos** = position X of virtsegment

**float pivotZPos** = position Z of virtsegment

**float pivotYPos** = position Y of virtsegment

**uint flags** = didn't find any flags enum for virtsegments in the PDB, always 8(?)

**int16 FirstVertex** = declares first vertex that the virtsegment affects

**int16 LastVertex** = declares last vertex that the virtsegment affects

**int16 index** = declares the segment to which the remaining weight goes

**int16 weightIndex** = declares the segment to which the weight goes

**float weight** = declares the influence

First of all, the data about the positions (from minXPos to pivotYPos) has to be the same as the segment declared in weightIndex, but inversed. So, for example, if pivotXPos in the Segment is 7.8, it becomes -7.8 for the VirtSegment

let's make an example about index, weightIndex and weight:

if the weight influence is 0.8, then 0.8 goes to weightIndex, and the rest (0.2) goes to index.


## HInfo

Hinfo is a block of data referenced by the segments (see Segments > HInfo)

HInfo contains info about HSpheres, HBoxes, HMarkers and HCapsules.

Out of all of these, the only one I know well is HMarkers, which are basically attachments. For example they are used to attach Lara's guns to her thighs when unequipped, and to her hands when equipped.

**int numHSpheres** = spheres count

**uint32 hsphereList** = spheres offset

**int numHBoxes** = boxes count

**uint32 hboxList** = boxes offset

**int numHMarkers** = markers count

**uint32 hmarkerList** = markers offset

**int numHCapsulses** = capsules count

**uint32 hcapsuleList** = capsules offset



not writing up the whole structs of all of these because I experimented very little with them so I know very little of what they do. Probably impossible to write from scratch without Crystal Dynamics' tools, so just set the HInfo offset of the segments to 0 or copy the data from the source GNC file.

## Vertices

VERTICES:

**int16 X** = vertex position x

**int16 y** = vertex position y

**int16 z** = vertex position z

**byte nx** = normal vector x

**byte ny** = normal vector y

**byte nz** = normal vector z

**byte pad** = unknown, always 0

**int16 bone_ID** = declares to what segment the vertex is assigned to

**int UV** = vertex UV


## TextureStripInfo/Faces

The name might sound like this is gonna be some weird stuff but in reality it's just how Crystal Dynamics calls faces.

**int16 vertexCount** = declares the vertex count of the mesh (one single mesh, not the whole model)

**int16 drawgroup** = declares when a mesh has to appear/disappear, probably cannot be calculated in any way, just write as 0 to make always appear

**int tpageid** = texture ID. The ID of the matching texture will match the ID.

**float sortPush** = unknown, always 0

**float scrollOffset** = unknown, always 0

**uint nextTexture** = declares the offset to the next section of faces (aka next mesh)


then you write all the face indices which are ushorts.




## Writing custom GNC file

**Vertices**

Vertices must be written in a very specific order.

The first vertices to be written are the ones that only have ONE weight. And also, they must be written in a bone order. So for example, first 10 vertices are for bone 0, then the next ones are for bone 1, then the ones for bone 2, and so on.

This is required because we will later need to declare the vertices in the bones.

After the vertices with one weight are written, we can write the ones with 2 weights.

Now I will have to explain something that will probably mess up your brain but it is what it is.

The bone IDs in the vertices with 2 weights will keep going on from the ones with one weight.

Let's say you have 109 bones. so you'd expect that the last bone ID is 108 (because the bones start from 0). But it's not.

The last bone ID is the last VirtSegment that you have in your list. So, if you have 109 bones and 114 VirtSegments, the last bone ID will be 222.



**Skeleton**

luckily the bones are not as complicated as you'd, except for some unknown transforms which apparently have absolutely no effect ingame so they can just be set to 0.

so you jump to the pivot floats, you just write the X, Z and Y positions of the bones (W is always 1065353216). Flag is always 0 for the Segments. Then you write the first vertex from the vertex list that is using the bone, and then the last. Then you write the bone parent. Then you write HInfo offset (0...because I have no idea how to calculate the HInfo yet, probably impossible without CD's tools, so just copy them from the source gnc and then fix the offset I guess)



**VirtSegments**

VirtSegments is the system the game uses to give weights to the vertices. Before writing the pivot positions, let's jump to FirstVertex and LastVertex. Those always have the same purpose. After that we have Index and weightIndex. Index is basically the "second" bone, to which the rest of the weight goes. weightIndex is the bone where the weight goes. And weight is the weight value. So, if the weight value is 0.3, 0.3 goes to weightIndex while 0.7 goes to Index.
