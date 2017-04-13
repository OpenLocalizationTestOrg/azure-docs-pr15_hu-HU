<properties
   pageTitle="Jelölje ki az Azure CLI Linux virtuális képekkel |} Microsoft Azure"
   description="Megtudhatja, miként állapítható meg a publisher ajánlat és Termékváltozat képek Linux virtuális gép létrehozása és az erőforrás-kezelő telepítési modellt."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="select-linux-vm-images-with-the-azure-cli"></a>Jelölje ki az Azure CLI Linux virtuális képekkel

Ez a témakör ismerteti, hogyan közzétevők, ajánlatok, termékváltozatban, és a verziókkal található minden olyan helyen, amelybe lehet telepíteni. Példa adhat, néhány gyakran használt Linux virtuális képek szerint történik:

**A gyakran használt Linux képek táblázata**


| PublisherName                        | Ajánlat                                 | Raktári szám                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| RedHat                           | RHEL                                       | 7.2.                              |
| credativ                         | Debian                                     | 8                                | 
| SUSE                             | openSUSE                                   | 13.2                             |
| SUSE                             | SLES                                       | 12-SP1                           |
| OpenLogic                        | CentOS                                     | 7.1.                              |
| Kanonikus                        | UbuntuServer                               | 14.04.4-LTS                      |
| CoreOS                           | CoreOS                                     | Állandó                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
