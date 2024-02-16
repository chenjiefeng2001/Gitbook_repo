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

A series of techniques that includes patch rendering, displacement mapping, and subdivision surfaces that any vertex shader-capable hardware can do and is a powerful form pf geometry comprehension.

Usually tessellation levels are decided by the CPU, as we currently have no programmable tessellation hardware, but there are a few fixed-function hardware tessellation systems that you may be able to use. This is the technique’s major limitation — to a limited degree, we can remove triangles (by sending the vertices to be clipped), but we cannot add triangles.

By using the vertex shaders as a function evaluator with the vertex stream bringing in the function parameters.

There are two components that are needed for displacement comprehension:

- **Displacement mapping**: 	A method of retrieving a scalar displacement along  the surface normal. Without it, your displacement comprehension becomes standard surface patch 

- **Surface basis**: 	Every displacement compression shader requires a basis system that defines the base surface before displacement. The simplest is just planar, although it could be as complex as a subdivision surface.

### Displacement Mapping

Displacement mapping has at least four ways to get the displacement  value into the vertex shader. The more advanced methods require explicit hardware support and are not cov ered here. 

 The technique presented here works on any vertex shader hardware by **treating the displacement map as a 1D vertex stream**. It’s a generalization of the technique that I presented in Direct3D *ShaderX*, which had an implied planar basis that with a few minor modification works for any surface basis

In the vertex stream the displacement value is stored explicitly. If kept in a separate stream, it can be accessed via the CPU as a standard displacement map, or you can choose to pack it with other vertex elements.

### Pre-Filtering Displacement Maps

One form of filtering that can be used with vertex stream displacement is to store the displacement value that would occur at the lower tessellation levels with the usual displacement value.

>  If you store displacement values in UBYTE4, you could pack three lower lev els in the other three channels, which gives you an effective linear mip filter (but with point min/mag filter).
>
> 