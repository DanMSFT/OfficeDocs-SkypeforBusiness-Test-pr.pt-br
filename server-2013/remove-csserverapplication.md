﻿---
title: Remove-CsServerApplication
TOCTitle: Remove-CsServerApplication
ms:assetid: 55325d8c-9c67-4e88-868d-ce62bc11322e
ms:mtpsurl: https://technet.microsoft.com/pt-br/library/Gg398366(v=OCS.15)
ms:contentKeyID: 49306744
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Remove-CsServerApplication

 

_**Tópico modificado em:** 2015-03-09_

Removes an existing server application. Server applications are applications that are hosted by Lync Server. Este cmdlet foi introduzido no Lync Server 2010.

## Sintaxe

    Remove-CsServerApplication -Identity <XdsIdentity> [-Confirm [<SwitchParameter>]] [-Force <SwitchParameter>] [-WhatIf [<SwitchParameter>]]

## Examples

## EXAMPLE 1

In Example 1, the server application that has the Identity service: EdgeServer:atl-edge-001.litwareinc.com/EdgeMonitor is removed. Because Identities must be unique, this command will never delete more than a single application.

    Remove-CsServerApplication -Identity "service:EdgeServer:atl-edge-001.litwareinc.com/EdgeMonitor"

## EXAMPLE 2

In Example 2, all the non-critical server applications are removed. To carry out this task, the command first calls the **Get-CsServerApplication** cmdlet in order to return a collection of all the server applications currently in use in the organization. This collection is then piped to the **Where-Object** cmdlet, which picks all the applications where the Critical property is equal to False. This filtered collection is then piped to the **Remove-CsServerApplication** cmdlet, which deletes each item in the collection.

    Get-CsServerApplication | Where-Object {$_.Critical -eq $False} | Remove-CsServerApplication

## EXAMPLE 3

Example 3 deletes all the server applications that have been configured for use by the service EdgeServer:atl-cs-001.litwareinc.com. To do this, the **Get-CsServerApplication** cmdlet is used along with the Filter parameter; the filter value "service:EdgeServer:atl-cs-001.litwareinc.com/\*" returns all the applications that have an Identity that begins with the characters "service:EdgeServer:atl-cs-001.litwareinc.com/". In turn, that collection is piped to the **Remove-CsServerApplication** cmdlet , which deletes each application from the EdgeServer:atl-cs-001.litwareinc.com.

    Get-CsServerApplication -Filter "service:EdgeServer:atl-cs-001.litwareinc.com/*" | Remove-CsServerApplication

## Detailed Description

Server applications refer to the individual programs that run under Lync Server. The **Remove-CsServerApplication** cmdlet provides a way for administrators to remove any application running as part of Lync Server. Note that deleting a server application is not the same thing as uninstalling that application. When you run the **Remove-CsServerApplication** cmdlet, the application no longer runs under Lync Server. However, the software itself is not uninstalled, and the application can be re-enabled by running the **New-CsServerApplication** cmdlet.

Who can run this cmdlet: By default, members of the following groups are authorized to run the **Remove-CsServerApplication** cmdlet locally: RTCUniversalServerAdmins. To return a list of all the role-based access control (RBAC) roles this cmdlet has been assigned to (including any custom RBAC roles you have created yourself), run the following command from the Windows PowerShell prompt:

Get-CsAdminRole | Where-Object {$\_.Cmdlets –match "Remove-CsServerApplication }

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
<td><p><em>Identity</em></p></td>
<td><p>Required</p></td>
<td><p>Microsoft.Rtc.Management.Xds.XdsIdentity</p></td>
<td><p>Unique identifier for the server application to be removed. Server application Identities are composed of the service where the application is hosted plus the application name. For example, the server application named QoEAgent might have an Identity similar to this: service:Registrar:atl-cs-001.litwareinc.com/QoEAgent.</p></td>
</tr>
<tr class="even">
<td><p><em>Confirm</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Solicita confirmação antes da execução do comando.</p></td>
</tr>
<tr class="odd">
<td><p><em>Force</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Suppresses the display of any non-fatal error message that might occur when running the command.</p></td>
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

Microsoft.Rtc.Management.WritableConfig.Settings.ServerApplication.Application object. The **Remove-CsServerApplication** cmdlet accepts pipelined instances of the server application object.

## Return Types

The **Remove-CsServerApplication** cmdlet deletes existing instances of the Microsoft.Rtc.Management.WritableConfig.Settings.ServerApplication.Application object.

## Consulte Também

#### Outros Recursos

[Get-CsServerApplication](get-csserverapplication.md)  
[New-CsServerApplication](new-csserverapplication.md)  
[Set-CsServerApplication](set-csserverapplication.md)

