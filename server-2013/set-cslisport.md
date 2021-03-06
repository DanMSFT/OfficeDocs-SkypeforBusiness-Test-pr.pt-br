﻿---
title: Set-CsLisPort
TOCTitle: Set-CsLisPort
ms:assetid: 8a8a8f95-9366-4d87-bf2a-9767e5eb9996
ms:mtpsurl: https://technet.microsoft.com/pt-br/library/Gg398700(v=OCS.15)
ms:contentKeyID: 49307397
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Set-CsLisPort

 

_**Tópico modificado em:** 2015-03-09_

Creates a Location Information Server (LIS) port, creates an association between a port and a location (creating a new location if that location doesn’t exist), or modifies an existing port and its associated location. The association between a port and location is used in an Enhanced 9-1-1 (E9-1-1) Enterprise Voice implementation to notify an emergency services operator of the caller’s location. Este cmdlet foi introduzido no Lync Server 2010.

## Sintaxe

    Set-CsLisPort -ChassisID <String> -PortID <String> [-City <String>] [-CompanyName <String>] [-Country <String>] [-Description <String>] [-HouseNumber <String>] [-HouseNumberSuffix <String>] [-Location <String>] [-PortIDSubType <InterfaceAlias | InterfaceName | LocallyAssigned>] [-PostalCode <String>] [-PostDirectional <String>] [-PreDirectional <String>] [-State <String>] [-StreetName <String>] [-StreetSuffix <String>] <COMMON PARAMETERS>

    Set-CsLisPort -ChassisID <String> -PortID <String> [-Description <String>] [-PortIDSubType <InterfaceAlias | InterfaceName | LocallyAssigned>] <COMMON PARAMETERS>

    Set-CsLisPort -City <String> -CompanyName <String> -Country <String> -HouseNumber <String> -HouseNumberSuffix <String> -Location <String> -PostalCode <String> -PostDirectional <String> -PreDirectional <String> -State <String> -StreetName <String> -StreetSuffix <String> <COMMON PARAMETERS>

    COMMON PARAMETERS: [-Confirm [<SwitchParameter>]] [-WhatIf [<SwitchParameter>]]

## Examples

## EXAMPLE 1

Example 1 creates or updates a LIS port location entry. The command in this example includes three parameters: ChassisID, PortID, and PortIDSubtype. The value of the ChassisID is the MAC address 99-99-99-99-99-99, the value of the PortID is 4200, and the value of the PortIDSubType is 1. (Note that a value of 1 for PortIDSubType translates into a value of InterfaceAlias. This parameter and value could also have been entered like this: -PortIDSubType InterfaceAlias.) These three parameters are required to create a unique instance of a port location.

Note that this example does not include any address information. It’s possible to create a port entry on the Location Information Server without associating it with an address. However, emergency calls routed through this port may not (depending on subnet or switch locations that have been defined) contain enough information for the emergency operator to identify a location.

IMPORTANT: If a LIS port location with this key combination already exists, it will be replaced by the values in this command. That means that if this port were associated with an address (a physical location), that association would no longer exist because we didn’t include any location information in this command. The location will still exist in the location database, but it will not be associated with this port.

    Set-CsLisPort -ChassisID 99-99-99-99-99-99 -PortID 4200 -PortIDSubType 1

## EXAMPLE 2

Example 2 updates the port created in Example 1 by adding address information. If the address does not exist in the location database, this cmdlet will create that location.

    Set-CsLisPort -ChassisID 99-99-99-99-99-99 -PortID 4200 -PortIdSubType 1 -Location "30/1000" -HouseNumber 1234 -PreDirectional NE -StreetName First -StreetSuffix Avenue -City Redmond -State WA -Country US -PostalCode 99999

## EXAMPLE 3

This example updates all locations defined for ports with a MAC address (ChassisID) of 99-99-99-99-99-88. The first line in this example begins with a call to the **Get-CsLisPort** cmdlet, which retrieves all the ports that have been defined in the LIS service. That collection of ports is piped to the **Where-Object** cmdlet, which finds all the items in the collection with a ChassisID equal to (-eq) 99-99-99-99-99-88. This collection of ports with the ChassisID 99-99-99-99-99-88 is assigned to the variable $a.

In the second line of this example, we pipe the contents of variable $a (the collection of LIS ports with the ChassisID 99-99-99-99-99-88) to the **Set-CsLisPort** cmdlet. This cmdlet will take each item in that collection and update it with the values in the parameters specified (Location, HouseNumber, StreetName, StreetSuffix, City, State, Country, and PostalCode).

    $a = Get-CsLisPort | Where-Object {$_.ChassisID -eq "99-99-99-99-99-88"}
    $a | Set-CsLisPort -Location "30/1000" -HouseNumber 1234 -StreetName First -StreetSuffix Avenue -City Redmond -State WA -Country US -PostalCode 99999

## Detailed Description

Enhanced 9-1-1 allows an emergency operator to identify the location of a caller without having to ask the caller for that information. In the case where a caller is calling from a Voice over Internet Protocol (VoIP) connection, that information must be extracted based on various connection factors. The VoIP administrator must configure a location map (called a wiremap) that will determine a caller’s location. This cmdlet allows the administrator to map physical locations to the port through which the client is connected.

The combination of ChassisID, PortID, and PortIDSubType makes a unique port location. If you enter a ChassisID/PortID/PortIDSubType key combination that already exists, this cmdlet will update the location for that port based on the location parameters that are supplied. If the key combination does not exist, a new port location will be created.

If a location with an address exactly matching the address parameters entered here (including null values) does not exist in the location database, a new address will be created based on the parameters entered with this cmdlet. (You can retrieve a list of locations by calling the **Get-CsLisLocation** cmdlet.) The **Set-CsLisPort** cmdlet does not require or prompt for location parameters; you can create a port entry without associating it with a location. It’s also possible to create an invalid location with this cmdlet. A valid location consists of, at minimum, the Location, HouseNumber, StreetName, City, State, and Country. If you do not supply all of these parameters, calls that are received by the referenced port may not contain the information required by the emergency operator (depending on whether valid settings are available for a switch, subnet, or wireless access point that can be used in place of port settings). It is recommended that you be as specific as possible with the location parameters and fill in as many as possible.

One of the required parameters of this cmdlet is ChassisID. ChassisID is the Media Access Control (MAC) address of the port’s network switch. If this switch does not exist in the location database, this cmdlet will create that switch. You can retrieve existing switches by calling the **Get-CsLisSwitch** cmdlet. Keep in mind that although a new switch entry will be created, that switch will not be automatically associated with the location information entered using the **Set-CsLisPort** cmdlet; you must set the switch location with the **Set-CsLisSwitch** cmdlet.

Who can run this cmdlet: By default, members of the following groups are authorized to run the **Set-CsLisPort** cmdlet locally: RTCUniversalServerAdmins. To return a list of all the role-based access control (RBAC) roles this cmdlet has been assigned to (including any custom RBAC roles you have created yourself), run the following command from the Windows PowerShell prompt:

Get-CsAdminRole | Where-Object {$\_.Cmdlets –match "Set-CsLisPort"}

## Parâmetros


<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Required</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>ChassisID</em></p></td>
<td><p>Required</p></td>
<td><p>System.String</p></td>
<td><p>The MAC address of the port’s switch. This value must be in the form nn-nn-nn-nn-nn-nn, such as 12-34-56-78-90-ab, or as an IP address. If the combination of ChassisID, PortID, and PortIDSubType is unique, a new port location will be created. If this combination is not unique, the port location with that key combination will be updated with the parameter values supplied with the command.</p></td>
</tr>
<tr class="even">
<td><p><em>PortID</em></p></td>
<td><p>Required</p></td>
<td><p>System.String</p></td>
<td><p>The ID of the port associated with this location.</p></td>
</tr>
<tr class="odd">
<td><p><em>City</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>The location city of this port.</p>
<p>Maximum length: 64 characters.</p></td>
</tr>
<tr class="even">
<td><p><em>CompanyName</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>The name of the company at this location.</p>
<p>Maximum length: 60 characters</p></td>
</tr>
<tr class="odd">
<td><p><em>Confirm</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Solicita confirmação antes da execução do comando.</p></td>
</tr>
<tr class="even">
<td><p><em>Country</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>The country/region this port is in.</p>
<p>Maximum length: 2 characters</p></td>
</tr>
<tr class="odd">
<td><p><em>Description</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>A detailed description of this port location.</p></td>
</tr>
<tr class="even">
<td><p><em>HouseNumber</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>The house number of the location for the port. For a company this is the number on the street where the company is located.</p>
<p>Maximum length: 10 characters</p></td>
</tr>
<tr class="odd">
<td><p><em>HouseNumberSuffix</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>Additional information for the house number, such as 1/2 or A. For example, 1234 1/2 Oak Street or 1234 A Elm Street.</p>
<p>Note: To designate an apartment number or office suite, you must use the Location parameter. For example, -Location &quot;Suite 100/Office 150&quot;.</p>
<p>Maximum length: 5 characters</p></td>
</tr>
<tr class="even">
<td><p><em>Location</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>The name for this location. Typically this value is the name of a location more specific than the civic address, such as an office number, but it can be any string value.</p>
<p>Maximum length: 20 characters</p></td>
</tr>
<tr class="odd">
<td><p><em>PortIDSubType</em></p></td>
<td><p>Optional</p></td>
<td><p>Microsoft.Rtc.Management.Lis.PortIDSubType</p></td>
<td><p>The subtype of the port. This value can be entered as a numeric value or a string, but it must be a valid subtype. Valid subtypes are:</p>
<p>1: InterfaceAlias</p>
<p>5: InterfaceName</p>
<p>7: LocallyAssigned</p>
<p>Default: 7 (LocallyAssigned)</p></td>
</tr>
<tr class="even">
<td><p><em>PostalCode</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>The postal code associated with this location.</p>
<p>Maximum length: 10 characters</p></td>
</tr>
<tr class="odd">
<td><p><em>PostDirectional</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>The directional designation of a street name. For example, NE or NW for Main Street NE or 7th Avenue NW.</p>
<p>Maximum length: 2 characters</p></td>
</tr>
<tr class="even">
<td><p><em>PreDirectional</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>The directional designation for a street name that precedes the name of the street. For example, NE or NW for NE Main Street or NW 7th Avenue.</p>
<p>Maximum length: 2 characters</p></td>
</tr>
<tr class="odd">
<td><p><em>State</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>The state or province associated with this location.</p>
<p>Maximum length: 2 characters</p></td>
</tr>
<tr class="even">
<td><p><em>StreetName</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>The name of the street for this location.</p>
<p>Maximum length: 60 characters</p></td>
</tr>
<tr class="odd">
<td><p><em>StreetSuffix</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>The type of street designated in a street name, such as Street, Avenue, or Court.</p>
<p>Maximum length: 10 characters</p></td>
</tr>
<tr class="even">
<td><p><em>WhatIf</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Descreve o que aconteceria se o comando fosse executado sem ser executado de fato.</p></td>
</tr>
</tbody>
</table>


## Input Types

Accepts pipelined input of LIS port objects.

## Return Types

This cmdlet creates or modifies an object of type System.Management.Automation.PSCustomObject.

## Consulte Também

#### Outros Recursos

[Remove-CsLisPort](remove-cslisport.md)  
[Get-CsLisPort](get-cslisport.md)  
[Get-CsLisLocation](get-cslislocation.md)  
[Get-CsLisSwitch](get-cslisswitch.md)

