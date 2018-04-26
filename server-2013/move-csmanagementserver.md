﻿---
title: Move-CsManagementServer
TOCTitle: Move-CsManagementServer
ms:assetid: bcead113-fbd2-4fcf-ae01-6a312511ceef
ms:mtpsurl: https://technet.microsoft.com/pt-br/library/Gg412921(v=OCS.15)
ms:contentKeyID: 49307937
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Move-CsManagementServer

 

_**Tópico modificado em:** 2015-03-09_

Moves the Servidor de Gerenciamento Central from one pool to another. Este cmdlet foi introduzido no Lync Server 2010.

## Sintaxe

    Move-CsManagementServer [-ConfigurationFileName <String>] [-LisConfigurationFileName <String>] [-TargetFqdn <Fqdn>] <COMMON PARAMETERS>

    Move-CsManagementServer [-TargetFqdn <Fqdn>] <COMMON PARAMETERS>

    COMMON PARAMETERS: [-Confirm [<SwitchParameter>]] [-Force <SwitchParameter>] [-Report <String>] [-WhatIf [<SwitchParameter>]]

## Examples

## EXAMPLE 1

The command shown in Example 1 moves the Servidor de Gerenciamento Central from its existing pool to a new pool. To perform this live migration (that is, to move a Servidor de Gerenciamento Central that is online and accessible), you must run the command from a computer located in the pool where the server is to be moved.

    Move-CsManagementServer

## EXAMPLE 2

Example 2 moves the Servidor de Gerenciamento Central in a disaster recovery scenario; that is, in a scenario where the existing management server is offline or otherwise inaccessible. To perform this type of migration, you must run the preceding command from a computer located in the pool where the server is to be moved. In addition, you must include the ConfigurationFileName parameter to import your previously saved configuration backup file; the LisConfigurationFileName parameter, to import your previously saved E9-1-1 backup file (if you are using E9-11); and the Force parameter to force the transfer of the Servidor de Gerenciamento Central even though the existing server cannot be contacted.

    Move-CsManagementServer -ConfigurationFileName "C:\CsConfiguration.zip" -LisConfigurationFileName "C:\CsLisConfiguration.zip" -Force

## Detailed Description

The **Move-CsManagementServer** cmdlet enables administrators to move the Servidor de Gerenciamento Central (and the accompanying Repositório de Gerenciamento Central) from one pool to another. Because there is always the potential for data loss, not to mention service interruption, any time you move the Servidor de Gerenciamento Central, it is recommended that you do not make such a transfer unless:

1\. You need to decommission the existing management pool, and must transfer the Servidor de Gerenciamento Central before doing so.

2\. You’ve encountered a disaster recovery scenario in which the existing Servidor de Gerenciamento Central is no longer accessible.

Before you move the Servidor de Gerenciamento Central, you must do the following:

1\. Verify that you have created the new Repositório de Gerenciamento Central. This is done by running the **Install-CsDatabase** cmdlet and using the CentralManagementDatabase parameter.

2\. If you are moving the Servidor de Gerenciamento Central to a Servidor Standard Edition, verify that you have used local setup to run the Prepare Servidor Standard Edition option. This advance preparation is required to add firewall rules that will allow Windows PowerShell to remotely access the new Repositório de Gerenciamento Central.

3\. Verify that there is enough free disk space on the computer where the **Move-CsManagementServer** cmdlet is being run to accommodate the Servidor de Gerenciamento Central.

4\. Verify that the Servidor Front-End service has been installed on the computer where the **Move-CsManagementServer** cmdlet is being run. If this service is not installed, and running, then the move will fail.

5\. Verify that you can successfully run the **Enable-CsTopology** cmdlet on the computer where the **Move-CsManagementServer** cmdlet is going to be run. If the **Enable-CsTopology** cmdlet cannot be run on that computer then the move will fail and you will no longer have a functioning Repositório de Gerenciamento Central.

After you have completed these steps, all you need to do to move the Servidor de Gerenciamento Central from Pool A to Pool B is log on to a computer in Pool B and then call the **Move-CsManagementServer** cmdlet without any additional parameters:

Move-CsManagementServer

When you do that, the **Move-CsManagementServer** cmdlet will consult the topology to determine the prior location of the Servidor de Gerenciamento Central (Pool A), and then transfer the Servidor de Gerenciamento Central and the Repositório de Gerenciamento Central to the current pool (Pool B).

If the move succeeds, the **Move-CsManagementServer** cmdlet will display a list of computers on the screen. In order to finish the move, you must run local setup on each of these computers. Computers in Pool A will still be running an inactive version of the Serviço de Gerenciamento Central; running local setup will delete that service. The computer in Pool B where the Servidor de Gerenciamento Central was moved will be running the Serviço de Gerenciamento Central; however, other computers in the pool will not. Running local setup on these computers will install the Serviço de Gerenciamento Central.

To transfer the Servidor de Gerenciamento Central in a disaster recovery scenario you should have, ideally, used the **Export-CsConfiguration** cmdlet and the **Export-CsLisConfiguration** cmdlet to create backup files of your Lync Server configuration and your Enhanced 9-1-1 (E9-1-1) configuration, respectively. (Because disasters typically occur without warning, you should routinely run these cmdlets and make backup files of your configuration settings.) When calling the **Move-CsManagementServer** cmdlet you should include both the ConfigurationFileName and LisConfigurationFileName parameters in order to read in these backup files.

You must also include the Force parameter any time you are trying to move a Servidor de Gerenciamento Central that is offline or otherwise inaccessible. When you call the **Move-CsManagementServer** cmdlet, the cmdlet temporarily sets the Repositório de Gerenciamento Central to read-only before the move takes place; that helps guard against data loss. In a disaster recovery scenario, however, the Repositório de Gerenciamento Central cannot be marked as read-only. The Force parameter instructs the cmdlet to move the Repositório de Gerenciamento Central even though it has not been configured as read-only.

If the **Move-CsManagementServer** cmdlet fails then you might find yourself in a situation where you no longer have a functioning Servidor de Gerenciamento Central. To restore the Servidor de Gerenciamento Central, you will need to fix the problem that caused the **Move-CsManagementServer** cmdlet to fail, and then re-run the cmdlet. This re-run can take place either on a new pool or on the old pool. If you run the **Move-CsManagementServer** cmdlet on the old pool, that will effectively cancel the move and leave you with your original configuration.

Note that the **Move-CsManagementServer** cmdlet must be run locally; it cannot be called from a remote management session.

Who can run this cmdlet: By default, members of the following groups are authorized to run the **Move-CsManagementServer** cmdlet locally: RTCUniversalServerAdmins. You must also be a local administrator on the computer where the cmdlet is being run.

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
<td><p><em>ConfigurationFileName</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>Full path to the Lync Server configuration backup file created by running the <strong>Export-CsConfiguration</strong> cmdlet. This parameter should only be used in a disaster recovery scenario.</p></td>
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
<td><p>Forces the Servidor de Gerenciamento Central move even if the existing store is offline; this parameter is required in a disaster recovery scenario. Note that there is the potential for some data loss any time you force the movement of the Servidor de Gerenciamento Central.</p>
<p>The Force parameter can also be used if your previous attempts at calling the <strong>Move-CsManagementServer</strong> cmdlet have failed.</p></td>
</tr>
<tr class="even">
<td><p><em>LisConfigurationFileName</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>Full path to the E9-1-1 backup file created by running the <strong>Export-CsLisConfiguration</strong> cmdlet. This parameter should only be used in a disaster recovery scenario.</p></td>
</tr>
<tr class="odd">
<td><p><em>Report</em></p></td>
<td><p>Optional</p></td>
<td><p>System.String</p></td>
<td><p>Enables you to specify a file path for the log file created when the cmdlet runs. For example: -Report &quot;C:\Logs\MoveManagementServer.html&quot;</p></td>
</tr>
<tr class="even">
<td><p><em>TargetFqdn</em></p></td>
<td><p>Optional</p></td>
<td><p>Microsoft.Rtc.Management.Deploy.Fqdn</p></td>
<td><p>Fully qualified domain name of the pool where the Management Server should be moved to.</p></td>
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

None. The **Move-CsManagementServer** cmdlet does not accept pipelined input.

## Return Types

None. The **Move-CsManagementServer** cmdlet does not return any objects.

## Consulte Também

#### Outros Recursos

[Set-CsManagementServer](set-csmanagementserver.md)

