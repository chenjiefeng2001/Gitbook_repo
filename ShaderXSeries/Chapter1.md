# Using Vertex Shaders for Geometry Comprehension

## Introduction to DX9

### Vertex Shaders
In this book the shader version 1.1, as this executed in hardware on the greatest number of machines.

> This improves the amount of batching that can occur. Static branching also makes it easier to use different compression methods on different models. 

## New Vertex Stream Declaration Format

The vertex stream declaration system from DirectX 8 was completely overhauled to make it both easier to use and add new capabilities. From a compression point of view, the most interesting items are the new vertex data types and the extra control over where each element comes from in the stream (stream offset).

## Limitations

When under DX8 drivers, most new capabilities of the DX9 vertex stream declarations can't be used. Under DX7 drivers, you must stick to FVF-style declarations.

> the D3DDEVCAPS2_VERTEXELE MENTS CAN SHARE STREAM OFFSET capbit must beset. Another limitation is that stream offsets must align on DWORD boundaries (4 bytes)
> The new vertex data types now have cap bits for each new type that DirectX 9 introduced (and UBYTE4 from DirectX 8); you must check these before using them. If the cap bit for the data type that you want is set, use it; otherwise, you will have to emulate the functionality via vertex shader code or change the vertex data to a format that is available on this hardware.

> ### NOTE
> The DirextX 9 documentation states the following about each new vertex data type: “This type is valid for vertex shader version 2.0 or higher.”This appears to be a documentation bug; if the cap bit is set, you can use it with any vertex shader version. There is already hardware that supports this, even on hardware that doesn’t support vertex shader version 2.0. (ATI supports some of the new data types on all its vertex shader-capable hardware.)

## New Vertex Data Types

Most of these new types are signed, unsigned, and normalized version of the existing DX8 data types, but a few add new capabilities. The following table list data types sorted by bits per channel:

| Data Type  | Number of Channels   | Bits Per Type   |Bit Per Channel| Range in Vertex Shader Register | Cap Bit | Notes |
|:------------: | :------------: | :------------: | :------------: | :------------: | :------------: |:------------: |
| D3DCOLOR | 4 | 32 | 8 | [0,1] | N | a |
| UBYTE4 | 4 | 32 | 8 | [0,255] | Y |  |
| UBYTE4N | 4 | 32 | 8 | [0,1] | Y |  |
| UDEC3 | 3 | 32 | 10 | [0,1024] | Y | b |
| DEC3N | 3 | 32 | 10 | [-1,1] | Y | b |
| SHORT2 | 2 | 32 | 16 | [-32768,32767] | N |  |
| SHORT4 | 4 | 64 | 16 | [-32768,32767] | N |  |
| USHORT2N | 2 | 32 | 16 | [0,1] | Y |  |
| USHORT4N | 4 | 64 | 16 | [0,1] | Y |  |
| SHORT2N | 2 | 32 | 16 | [-1,1] | Y |  |
| SHORT4N | 4 | 64 | 16 | [-1,1] | Y |  |
| FLOAT16_2 | 2 | 32 | 16 | [-6.55e4,6.55e4] | Y | c |
| FLOAT16_4 | 4 | 64 | 16 | [-6.55e4,6.55e4] | Y | c |
| FLOAT1 | 1 | 32 | 32 | [–3.48e38, 3.48e38] | N | d |
| FLOAT2 | 2 | 64 | 32 | [–3.48e38, 3.48e38] | N | d |
| FLOAT3 | 3 | 96 | 32 | [–3.48e38, 3.48e38] | N | d |
| FLOAT4 | 4 | 128 | 32 | [–3.48e38, 3.48e38] | N | d |

## Reclaiming Two Bits

If you use the DEC3N and UDEC3 it will take 6 bits, then we have lost 2 bits. To use it effectively, we could cause two different vertex elements to point to the same memory in the vertex buffer, we can get access to our two bits.



```C++
D3DVERTEXELEMENT9 decl[]=
{
    {
    0,		//stream number
    0,		//stream offset in bytes
    D3DDECLTYPE UDEC3, // vertex type for this access
 	D3DDECLMETHOD DEFAULT, // not used so leave at default
 	D3DDECLUSAGE NORMAL, // usage (used to bind in the vertex shader)
 	0 // usage number (you can have n normals)
},
//second element,a UBYTE4 that accesses the same menory as the normal above.
{
    0,		//stream number 
    0,		//stram offset
     D3DDECLTYPE UBYTE4,	//vertex type for this access
     D3DDECLMETHOD DEFAULT,	//not used so leave at default
    D3DDECLUSAGENORMAL,	//usage(used to bind in the vertex shader)
 1 	//usage no(so you can have nnormals)
},
D3DDECL_END()
};
```

```C
structVSINPUT
 {
	 float4 normal :NORMAL0,
	 float4 enc2Bit :NORMAL1
 };
 voidmain(VSINPUTinput)
 {
 	//access normal as usual
 	float3 normal=input.normal;
 	//decodeour2bits(0-3)
	 float twobits=floor(input.enc2Bit.w/64.0);
 }
```

### A Better Comprehension Transform Data Type

The new DEC3N data types allow us to easily design format with three channels with 10,10 and 12 bits precision. This is a useful format for comprehension transformed positions.

Many natural and man-made objects have a dominant axis spine of many animals. By giving that axis the extra two bits.

```C++
struct VS_INPUT{
    float4 position: POSITION0,
    float4 enc2bit: POSITION1,
}
void main(VS_INPUT input)
{
    // get the 10,10,10 portion of the position
	 float3 cpos = input.position;
 // decode our 2 bits (0-3)
	 float two bits = floor(input.enc2Bit.w / 64.0);
 // factor in the extra bits and convert back into the 0-1 range
 	cpos.z = (cpos.z + two bits) * 0.25;
 // transform by the inverse compression matrix
 	float4 pos = mul( float4(cpos,1), InvCompressionTransform );
}
```
### Displacement Comprehension

The displacement comprehenison is a techniques called by *Tom FOrsyth* -- which is a complete techniques chains includes patch rendering,displacement mapping and subdivision surface that any vertex shader-capable hardware can do and is a powerful form of geometry comprehension.

### N- Patches

N-Patches are a type of bicubic patch where the control points are determined from a triangle's positions and normals. N-Patches come into two variations, both with cubic interpolated linearly and quadratically.

The algorithms calculatates the control points for the patch and then evaluates at each point on the base triangle.

### Making It Fast Using a Linear Basis

Effectively, there are two frequencies at which this vertex shader needs executing; the control points need calculating only once per patch, whereas the evaluation needs running at every vertex. Some consoles can execute this pattern on the GPU, but on current PC architectures you can either generate the control points on the CPU and upload them to vertex constant memory pr recalculate the control points at every vertex;

```cpp
 float3 VertexPos[3 * NUM BASE TRIANGLE];
 float3 VertexNormals[3 * NUM BASE TRIANGLE];
 // bicubic control points
 float3 b300,b030,b003, b210,b120,b021, b201,b102,b012, b111;
float3 n200,n020,n002;
 void generateControlPointsWithLinearNormals(float baseIndex);
 {
 float3 v0= VertexPos[(baseIndex*3)+0];
 float3 v1= VertexPos[(baseIndex*3)+1];
 float3 v2= VertexPos[(baseIndex*3)+2];
 float3 n0= VertexNormal[(baseIndex*3)+0];
 float3 n1= VertexNormal[(baseIndex*3)+1];
 float3 n2= VertexNormal[(baseIndex*3)+2];
 //ForthebookI’lldoonebicubicpatchcontrolpointhere,fortherest
 //seeexamplecodeonCD/WeborreferenceATI’sCurvedPNPatchpaper[3]
 float3 edge=v1-v0;
 //E-(E.N)N
 float3 tangent1=edge;
 float tmpf=dot(tangent1,n0);
 tangent1-=n0*tmpf;
 b210=v0+(tangent1*rcp3);
 }
 void evaluateNPatchLinearNormal(float i,float j,outfloat3 pos,outfloat3 norm)
 {
 float k=1-i-j;
 float k2=k*k;
 float k3=k2*k;
 float i2=i*i;
 float i3=i2*i;
 float j2=j*j;
 float j3=j2*j;
 //bicubicposition
 pos=(b300*k3)+(b030*u3)+(b003*v3)+
 (b210*3*k2*i)+(b120*3*k*i2)+(b201*3*k2*j)+
 (b021*3*i2*j)+(b102*3*k*j2)+(b012*3*i2*j)+
 (b111*6*k*i*j);
 //linearnormal
 norm=(w*n200)+(i*n020)+(j*n002);
 }
 void main(float3vertexStream:POSITION0)
 {
 	float i= vertexStream.x;
	 float j= vertexStream.y
	 float baseIndex= vertexStream.z*256;
	 float 3pos,norm;
	 generateControlPointsWithLinearNormals(baseIndex);
	 evaluateNPatchLinearNormal(i,j,pos,norm);
 }
```

### Making It Fast Using a Linear Basis

Evaluating N-Patches via a vertex shader can be quite expensive. If you are also using a displacement map, the inherent surface curve usually isn’t very important anyway. Usually when using displacement compression, we would like a basis that has a smooth surface normal but relies on the displacement map to handle the position. A linear basis has all these properties: The surface normal is smooth between patches (assuming the vertex normals are smooth), but the position before the displacement is planar. The surface normal is generated from the linear interpolation of the vertex normals (in a similar manner to how Phong shading interpolates the lighting normal).

A linear basis only requires the mesh vertex data, and as these can be shared between patches, it’s usually better to store vertex indices rather than a triangle index at every interior point. This usually increases the number of vertices that can be stored in constant memory, which increases performance as more patches can be evaluated per call at the expense of slightly larger per-vertex data.

```glsl
//HLSLforadisplacedlinearbasissurfacewithindexedvertices
 float MAXDISPLACEMENTHEIGHT=100; //thisisjustanexamplevalue
 float3 VertexPos[NUMBASEVERTICES];
 float3 VertexNormal[NUMBASEVERTICES];
 float2 VertexUV[NUMBASEVERTICES];
 struct VSIN
 {
 float2 barycentric;
 float3 indices;
 float displacement;
 };
 void main( VSINinput)
 {
 float i=input.barycentric.x;
 float j=input.barycentric.y
 float k=1.0–i–j;
 float i0=input.indices.x*256;
 float i1=input.indices.y*256;
 float i2=input.indices.z*256;
 float3 pos=i*VertexPos[i0]+j*VertexPos[i1]+k*VertexPos[i2];
 float3 normal=i*VertexNormal[i0]+j*VertexNormal[i1]+k*VertexNormal[i2];
 float2 uv=i*VertexUV[i0]+j*VertexUV[i1]+k*VertexUV[i2];
 normal=normalized(normal);
 pos=pos+input.displacement*normal*MAXDISPLACEMENTHEIGHT;
 }
```

Barycentric coordinates are in the range [0,1] and are the same for each triangle at a particular subdivision. Indices only require a maximum of 256 values (there are currently only 256 constants).

So a byte per index is enough. For the triangle indexed version, this is 1 byte + 1 byte displacement and  a shared 8 bytes(two floats), and for the vertex indexed version it is 3 bytes + 1 byte displacement and a shared 8 bytes (two floats). 

### Lighting Normal

As getting a perturbed lighting normal proves to be difficult, the best option is not to bother at run time. If the displacement map is fixed, you can just create a normal map off-line that encodes the lighting normal. Even if you are vertex lighting, you can feed the normal map values into the vertex shader un the same manner as the displacement values.

### Conclusion

 **Vertex shaders can be used as effective geometry decompressors; with tight packing of vertex data and techniques like displacement compression, we can save considerable memory and, more importantly, bandwidth. The cost of using extra vertex shader instructions is usually not a problem, as in most cases this isn’t a bottleneck; by using this “spare” vertex throughput to save bandwidth, it may make things run faster. Displacement compression requires changes to the tools (these are described elsewhere) but are an important future technique that you should be thinking about implementing in the near and long term.**

---

## Using Lookup Tables in Vertex Shaders

When writing the vertex shader code, you almost always want to squeeze out a few instructions. Maybe you have do it in order to **stay within the instruction limit**, which can easily be reached when doing complex animation and lighting calcula tions. Or maybe you simply want to speed up your code to gain some extra frames per second. 

A table lookup can be implemented quite easily using the address register $a_0$ to index an array of constant registers $c_{tableBase},\cdots,c_{tableBase+tableSize-1}$ containing the actual table data.

Generally, you want to keep the table as small as possible. Therefore, it is often necessary to interpolate between consecutive table values.

> Do not forget about the Nyquist theorem1 when representing continuous func tions via lookup tables, or else you’ll face aliasing. That is, make sure the table is not too small — which implies that encoding terms and functions by means of lookup tables is not feasible if the range you’re interested in exhibits high fre quencies. Also note that the table size directly affects the precision of the interpolated result.

The example code:

```glsl
const unsigned intTABLESIZE(64);
 const unsigned intTABLEBASE(10);
 for(unsignedintuiI(0);uiI<TABLESIZE;++uiI)
 {
 float d2(uiI/(float)(TABLESIZE–1));
 float alpha(time/(0.1f+1000.0f*d2));
 float c(cosf(alpha));
 float s(sinf(alpha));
 D3DXVECTOR4 vLookup(c,s,0.0f,0.0f);
 pD3DDev->SetVertexShaderConstant(TABLEBASE+uiI,&vLookup,1);
 }
 float fIndexScale((float)(TABLESIZE–1));
 float fIndexOffset(0.0f);
 D3DXVECTOR4 vIndex(fIndexScale,fIndexOffset,0.0f,0.0f);
 constunsignedintTABLEINDEX(9);
 pD3DDev->SetVertexShaderConstant(TABLEINDEX,&vIndex,1);
```

> What remains to do is write the vertex shader to animate each particle. The code will be split into several pieces showing all necessary steps to get the stars spinning on the GPU. The following part computes the table index.
>
> ```glsl
>  #define srcPos v0 //(x,y,z,1)
>  #define temp0 r0
>  #define temp1 r1
>  #define temp2 r2
>  #define worldPos r3
>  #define TABLEINDEX 9
>  #define TABLEBASE 10
>  vs.1.1
>  #ifdefDX9
>  dcl_position0 srcPos
>  #endif
>  //calculated^2andtableindex
>  dp3 temp0,srcPos,srcPos
>  mad temp1,temp0,c[TABLEINDEX].x,c[TABLEINDEX].y
>  //getfractionoftableindex
>  expp temp0.y,temp1.y
>  //settableindexforrelativeaddressingoflookuptable
>  #ifdefDX9
>  add a0.x,temp1.y,-temp0.y
>  #else //DX8
>  mov a0.x,temp1.y
>  #endif
> ```


---

## 3D Planets on the GPU

Rendering planets in a 3D application is a difficult task. Previously, if a programmer wantered to include planets, the CPU had to juggle planet rendering and any others tasks the program might have. Now it is possible to perform almost the entire task on the GPU using vertex and pixel shaders.

Moreover, the procedural model presented here allows a near infinite number of different planets to be rendered. This article examines rendering planets entirely on the GPU using *Nvidia;s CG*

**The most important task in rendering is to generate the geometry.** This is usually done by first generating a sphere and then deforming the points on the sphere with some type fractal.

The sphere can be generated using the parametric equation:

$$
X = sin(u)*sin(v\\
Y= cos(u)*sin(v)\\
Z=cos(v)
$$

```C
float fxsin;
float fxcos;
float fysin;
float fycos;
sincos(In.pos.x,fxsin,fxcos);
sincos(In.pos.y,fysin,fycos);
Sphere.x = fxsin*fysin;
Sphere.y= fxcos*fysin;
Sphere.z=fycos;
```
