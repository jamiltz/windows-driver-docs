---
title: Setting Hardware Information in the Registry
description: Setting Hardware Information in the Registry
ms.assetid: 82f5d399-58c3-4bed-a3f2-3501f21fa3e8
keywords: ["hardware WDK video miniport", "registry WDK video miniport", "VideoPortSetRegistryParameters", "VideoPortGetRegistryParameters"]
---

# Setting Hardware Information in the Registry


## <span id="ddk_setting_hardware_information_in_the_registry_gg"></span><span id="DDK_SETTING_HARDWARE_INFORMATION_IN_THE_REGISTRY_GG"></span>


[*HwVidFindAdapter*](https://msdn.microsoft.com/library/windows/hardware/ff567332) can call the [**VideoPortGetRegistryParameters**](https://msdn.microsoft.com/library/windows/hardware/ff570316) and [**VideoPortSetRegistryParameters**](https://msdn.microsoft.com/library/windows/hardware/ff570365) functions to get and set configuration information in the registry. For example, *HwVidFindAdapter* might call **VideoPortSetRegistryParameters** to set up nonvolatile configuration information in the registry for the next boot. It might call **VideoPortGetRegistryParameters** to get adapter-specific, bus-relative configuration parameters written into the registry by an installation program.

It is recommended that miniport drivers set certain hardware information in the registry to display useful information to the user and for assistance in debugging. A miniport driver can set a chip type, DAC type, memory size (of the adapter), and a string to identify the adapter. This information is shown by the Display program in Control Panel.

The driver sets this information by calling **VideoPortSetRegistryParameters**. Typically, the driver makes the call in its *HwVidFindAdapter* routine.

The following table describes the information that the driver can register and provides details for the *ValueName* and *ValueData* parameters of **VideoPortSetRegistryParameters**:

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Information for Entry</th>
<th align="left"><em>ValueName</em></th>
<th align="left"><em>ValueData</em></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Chip type</p></td>
<td align="left"><p>HardwareInformation.ChipType</p></td>
<td align="left"><p>Null terminated string containing the chip name.</p></td>
</tr>
<tr class="even">
<td align="left"><p>DAC type</p></td>
<td align="left"><p>HardwareInformation.DacType</p></td>
<td align="left"><p>Null terminated string containing the DAC name or ID.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Memory size</p></td>
<td align="left"><p>HardwareInformation.MemorySize</p></td>
<td align="left"><p>ULONG containing, in MB, the amount of video memory on the adapter.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Adapter ID</p></td>
<td align="left"><p>HardwareInformation.AdapterString</p></td>
<td align="left"><p>Null-terminated string containing the name of the adapter.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>BIOS</p></td>
<td align="left"><p>HardwareInformation.BiosString</p></td>
<td align="left"><p>Null-terminated string containing information about the BIOS.</p></td>
</tr>
</tbody>
</table>

 

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[display\display]:%20Setting%20Hardware%20Information%20in%20the%20Registry%20%20RELEASE:%20%282/10/2017%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")




