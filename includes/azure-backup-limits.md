 (biztonsági tárolókban<properties
   lap címe = "Azure Backup limits table"
   Leírás = "ismerteti a rendszer korlátozások Azure Backup."
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />
<tags
   ms.service="backup"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/05/2015"
   ms.author="trinadhk";"jimpark"; "aashishr" />


Az alábbi korlátozások vonatkoznak a Azure biztonsági másolatot.

| Korlát azonosító | Alapértelmezett korlát |
|---|---|
|Egyes tárolóra ellen lehet regisztrálni kiszolgálók/gépek száma|a Windows Server/ügyfél/SCDPM 50 <br/> 200 IaaS VMs|
|Azure tárolóra tárolóban lévő adatok egy adatforráshoz mérete|Max 54400 GB-<sup>1</sup>|
|Minden egyes Azure előfizetés létrehozott biztonsági tárolókban száma|25 (biztonsági másolat tárolókban) <br/> 25 helyreállítási szolgáltatások tárolóból elemre egy terület|
|Biztonsági másolat ütemezhető legyen a napi hányszor|a Windows Server/ügyfél napi 3 <br/> napi SCDPM 2 <br/> A IaaS VMs naponta egyszer|
|Adatok lemez biztonsági mentése az Azure virtuális gép csatolva|16|

- <sup>1</sup> A 54400 GB-os korlát IaaS virtuális biztonsági másolat nem vonatkozik.

