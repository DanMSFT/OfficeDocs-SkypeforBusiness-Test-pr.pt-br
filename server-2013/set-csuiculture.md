﻿---
title: Set-CsUICulture
TOCTitle: Set-CsUICulture
ms:assetid: 53fbc126-1df9-4ea0-8055-4e9530ab89d6
ms:mtpsurl: https://technet.microsoft.com/pt-br/library/Gg398354(v=OCS.15)
ms:contentKeyID: 49306728
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Set-CsUICulture

 

_**Tópico modificado em:** 2015-03-09_

Enables you to modify the culture (that is, the language and regional settings) used by the Shell de Gerenciamento do Lync Server. Este cmdlet foi introduzido no Lync Server 2010.

## Sintaxe

    Set-CsUICulture -Culture <String> [-Confirm [<SwitchParameter>]] [-WhatIf [<SwitchParameter>]]

## Examples

## EXAMPLE 1

The command shown in Example 1 sets the culture for the Shell de Gerenciamento do Lync Server to U.S. English. This is done by using the language code en-US.

    Set-CsUICulture -Culture "en-US"

## EXAMPLE 2

Example 2 sets the culture for the Shell de Gerenciamento do Lync Server to the default culture value. The default value is the culture setting used when you originally installed Lync Server.

    Set-CsUICulture -Culture "default"

## Detailed Description

Although Lync Server is available in multiple languages, the software is not a true Multilingual User Interface (MUI) application. Among other things, this means that the user interface for the Shell de Gerenciamento do Lync Server does not change languages any time you change the operating system language. For example, suppose you have installed the U.S. English version of Lync Server and are also running the Windows operating system under U.S. English. If you change the operating system culture (that is, the language and regional settings) to Danish, the Shell de Gerenciamento do Lync Server will not automatically follow suit; instead, the Shell de Gerenciamento do Lync Server user interface (including error messages and help text) will remain in U.S. English. If you need to change the culture for the Shell de Gerenciamento do Lync Server, you must run the **Set-CsUICulture** cmdlet.

There are two things to keep in mind when using the **Set-CsUICulture** cmdlet. First, the cmdlet can only be used to modify Shell de Gerenciamento do Lync Server settings on the local computer; the **Set-CsUICulture** cmdlet cannot change a remote instance of Shell de Gerenciamento do Lync Server. This is due to the fact that all the users of a computer share the same instance of the Shell de Gerenciamento do Lync Server: allowing a remote user to change the culture would instantly change the culture for any other users of the Shell de Gerenciamento do Lync Server, including the local user.

Second, you can only set the language to U.S. English ("en-US") or to the language used when you initially installed Lync Server ("default"). For example, if you originally installed an Italian version of Lync Server, then you have two choices for configuring the UI culture: English or Italian.

Who can run this cmdlet: By default, members of the following groups are authorized to run the **Set-CsUICulture** cmdlet locally: RTCUniversalUserAdmins, RTCUniversalServerAdmins, RTCUniversalReadOnlyAdmins.

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
<td><p><em>Culture</em></p></td>
<td><p>Required</p></td>
<td><p>System.String</p></td>
<td><p>Enables you to specify the culture used for the Shell de Gerenciamento do Lync Server. Set the culture to &quot;en-US&quot; for U.S. English, or set the culture to &quot;default&quot; to use the language used when you originally installed Lync Server.</p></td>
</tr>
<tr class="even">
<td><p><em>Confirm</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Solicita confirmação antes da execução do comando.</p></td>
</tr>
<tr class="odd">
<td><p><em>WhatIf</em></p></td>
<td><p>Optional</p></td>
<td><p>System.Management.Automation.SwitchParameter</p></td>
<td><p>Descreve o que aconteceria se o comando fosse executado sem ser executado de fato.</p></td>
</tr>
</tbody>
</table>


## Input Types

None. The **Set-CsUICulture** cmdlet does not accept pipelined input.

## Return Types

The **Set-CsUICulture** cmdlet does not return any values or objects. Instead, the cmdlet modifies existing instances of the System.Globalization.CultureInfo class.

## Consulte Também

#### Outros Recursos

[Get-CsUICulture](get-csuiculture.md)

