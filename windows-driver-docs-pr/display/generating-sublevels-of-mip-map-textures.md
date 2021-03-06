---
title: Generating Sublevels of MIP Map Textures
description: Generating Sublevels of MIP Map Textures
ms.assetid: fbfb0d1b-468d-4e7f-865e-bdc7d19f5516
keywords: ["MIP map textures WDK DirectX 9.0 , generating sublevels", "sublevels of MIP-map textures WDK DirectX 9.0"]
---

# Generating Sublevels of MIP Map Textures


## <span id="ddk_generating_sublevels_of_mip_map_textures_gg"></span><span id="DDK_GENERATING_SUBLEVELS_OF_MIP_MAP_TEXTURES_GG"></span>


A display driver indicates support of automatically generating the sublevels of MIP-map textures by setting the DDCAPS2\_CANAUTOGENMIPMAP bit of the **dwCaps2** member of the [**DDCORECAPS**](https://msdn.microsoft.com/library/windows/hardware/ff549248) structure. The driver specifies this DDCORECAPS structure in the **ddCaps** member of a [**DD\_HALINFO**](https://msdn.microsoft.com/library/windows/hardware/ff551627) structure. DD\_HALINFO is returned by the driver's [**DrvGetDirectDrawInfo**](https://msdn.microsoft.com/library/windows/hardware/ff556229) function. The display driver also indicates whether a particular surface format supports automatically generating sublevels by setting the D3DFORMAT\_OP\_AUTOGENMIPMAP flag in the **dwOperations** member of the [**DDPIXELFORMAT**](https://msdn.microsoft.com/library/windows/hardware/ff550274) structure for the format.

When a texture surface is created, the Direct3D runtime sets the DDSCAPS3\_AUTOGENMIPMAP bit of the **dwCaps3** member of the DDSCAPSEX ([**DDSCAPS2**](https://msdn.microsoft.com/library/windows/hardware/ff550292)) structure to indicate that the MIP-map sublevels for this texture can be automatically generated. If Direct3D directs some textures to automatically generate their MIP-map sublevels and some textures to not automatically generate, the driver can only perform blit operations (D3DDP2OP\_TEXBLT) on these textures as described in the following scenarios:

-   The driver cannot blit from a source texture that auto-generates MIP maps to a destination texture that does not.

-   If the driver blits from a source texture that does not auto-generate MIP maps to a destination texture that does, the driver only blits the topmost matching level. The sublevels from the source texture are ignored. The destination sublevels can be generated.

-   Similarly, if the driver blits from source to destination textures that both auto-generate MIP maps, the driver only blits the topmost matching level. The sublevels from the source texture are ignored. The destination sublevels can be generated.

To generate the sublevels of a MIP-map texture, the driver receives a D3DDP2OP\_GENERATEMIPSUBLEVELS command along with a [**D3DHAL\_DP2GENERATEMIPSUBLEVELS**](https://msdn.microsoft.com/library/windows/hardware/ff545570) structure. In order to receive this command, the texture's surface format must expose the D3DFORMAT\_OP\_AUTOGENMIPMAP flag.

For [driver-managed resources](driver-managed-resources.md), when the driver evicts and replaces a resource in video memory, the driver must use the last set filter type to automatically generate sublevels. Because Direct3D does not control the eviction and replacement of the resource, Direct3D does not send a D3DDP2OP\_GENERATEMIPSUBLEVELS command to the driver.

The Direct3D runtime cannot call the driver's [*DdLock*](https://msdn.microsoft.com/library/windows/hardware/ff549599) function or use any other [DDI](direct3d-driver-ddi.md) to access the sublevels of an auto-generated MIP-map texture. This implies that the sublevels for auto-generated MIP-map textures, like lightweight MIP-map textures, are "implicit" and can be specified by the driver as appropriate. The driver is not required to specify "complete" surface data structures. Note, however, that Direct3D must be able to call the driver's *DdLock* or [*DdBlt*](https://msdn.microsoft.com/library/windows/hardware/ff549205) functions, send the D3DDP2OP\_BLT command, or use any other DDI (for [driver-managed textures](driver-managed-textures.md), dynamic textures or vendor-specific formats only) to access the top level of an auto-generated MIP-map texture.

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[display\display]:%20Generating%20Sublevels%20of%20MIP%20Map%20Textures%20%20RELEASE:%20%282/10/2017%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")




