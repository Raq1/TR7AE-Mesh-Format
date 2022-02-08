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

uint32 id = unknown, always 0(?)
uint32 specMask = unknown, always 4294967295

Next come the relocations.

Relocations are offsets to offsets.

All offsets in the file must have a relocation. vertexList, normalList, faceList, etc.

typeAndSectionInfo is a bitfield. first 3 bits unknown. the next 13 bits declare to which file the relocation applies to (Ex. 5 = 5_0.gnc, 29 = 29_0.gnc).

uint16 typeSpecific = unknown, always 0 (?)
uint32 offset = offset to the offset
