<properties
   pageTitle="Keresse meg és jelölje be a Windows virtuális képek |} Microsoft Azure"
   description="Megtudhatja, miként állapítható meg a publisher ajánlat és Termékváltozat képek Windows virtuális gép és az erőforrás-kezelő telepítési modellt létrehozásakor."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="navigate-and-select-windows-virtual-machine-images-in-azure-with-powershell-or-the-cli"></a>Keresse meg és jelölje be a Windows virtuális gép képek PowerShell vagy a CLI Azure-ban

Ez a témakör ismerteti, hogyan virtuális kép közzétevők, ajánlatok, termékváltozatban, és a verziókkal található minden olyan helyen, amelybe lehet telepíteni. Példa adhat, néhány gyakran használt Windows virtuális képeket a következők:

## <a name="table-of-commonly-used-windows-images"></a>Windows-képek gyakran használt táblázata


| PublisherName                        | Ajánlat                                 | Raktári szám                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| MicrosoftDynamicsNAV             | DynamicsNAV                                | a Skype 2015                             |
| MicrosoftSharePoint              | MicrosoftSharePointServer                  | 2013-ban                             |
| MicrosoftSQLServer               | SQL2014-WS2012R2                           | Enterprise-optimalizált-a-DW      |
| MicrosoftSQLServer               | SQL2014-WS2012R2                           | Enterprise-optimalizált-a-OLTP    |
| MicrosoftWindowsServer           | WindowsServer                              | 2012-R2-adatközponthoz                  |
| MicrosoftWindowsServer           | WindowsServer                              | 2012-adatközponthoz               |
| MicrosoftWindowsServer           | WindowsServer                              | 2008 R2 SP1 CSOMAG |
| MicrosoftWindowsServer           | WindowsServer                              | Windows-kiszolgáló-technikai előzetes |
| MicrosoftWindowsServerEssentials | WindowsServerEssentials                    | WindowsServerEssentials          |
| MicrosoftWindowsServerHPCPack    | WindowsServerHPCPack                       | 2012R2                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
