<properties
   pageTitle="SAP – használata Windows virtuális gépeken futó |} Microsoft Azure"
   description="SAP – használatáról a Windows virtuális gépeken (VMs) a Microsoft Azure törlése"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-service-management"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="10/04/2016"
   ms.author="sedusch"/>

# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>SAP – használata Windows virtuális gépeken futó Azure-ban

Az a felhő számítások egy körben alkalmazott kifejezés, amely belül az informatikai üzleti, a további és további fontosság van elveszti a kisvállalkozások nagy- és multinacionális vállalatok felfelé. Microsoft Azure egy széles dokumentumhasználat új lehetőségeket kínál, amelyek a Microsoft Cloud Services Platform. Most már az ügyfelek gyorsan kiépítése és vonja kiépítése alkalmazások Felhőszolgáltatások, mint, így nem korlátozódik műszaki vagy költségvetés korlátozások. Helyett, hogy időt és a tervezett hardver infrastruktúra be, cégek is koncentráljon az alkalmazás, üzleti folyamatok és annak előnyökkel jár a vevők és a felhasználók számára.

A Microsoft Azure virtuális gépeken futó a Microsoft mint szolgáltatás (IaaS) platformot egy teljes infrastruktúrát kínál. SAP-alapú NetWeaver alkalmazások támogatottak a Azure virtuális gépeken futó (IaaS). Az alábbi tanulmányok ismertetik, hogyan lehet tervezése és az SAP NetWeaver alapú alkalmazások végrehajtása Windows virtuális gépeken Azure-ban. SAP NetWeaver alapú alkalmazások [Linux virtuális](virtual-machines-linux-classic-sap-get-started.md)gépeken is alkalmazhat.

[AZURE.INCLUDE [virtual-machines-common-classic-sap-get-started](../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP – a Azure - NetWeaver HA

Cím: SAP NetWeaver a Azure - fürtözés SAP ASC/SCS példányok Azure SIOS DataKeeper a Windows Server Feladatátvevőfürt használata

Összefoglalás: "a dokumentum leírja, hogy miként Azure a könnyen hozzáférhető SAP ASC/SCS konfiguráció beállítása SIOS DataKeeper használatával. SAP – a hiba összetevők például SAP ASC/SCS vagy várólistára helyezés replikációs szolgáltatások Windows Server Feladatátvevőfürt konfigurációjának megosztott lemez igénylő egyetlen pont védi. Az SAP-összetevők olyan elengedhetetlen SAP rendszer funkcióit. Ezért magas rendelkezésre állás funkciók kell tenni, győződjön meg arról, hogy a megfelelő összetevők is fenntartása kiszolgáló vagy egy virtuális Windows fürt konfigurációk gépi és a Hyper-V környezetben elvégzettként betöltő helyet. Augusztus 2015 kezdve magára Azure nem adhat meg, amely a Windows szükséges megosztott lemez alapú könnyen hozzáférhető konfigurációk az alábbi kritikus SAP-összetevők szükséges. A termék által SIOS DataKeeper segítségével, azonban a Windows Server Feladatátvevőfürt konfigurációk az SAP ASC/SCS szükség szerint az Azure IaaS platformon beépíthető. Ez a papír lépés lépésből álló megközelítésben ismerteti telepítése a Windows Server Feladatátvevőfürt konfiguráció SIOS Datakeeper Azure-ban megosztott szállított. A papír az Azure, a Windows és az SAP oldalon, amelyet a magas elérhetősége konfigurációt optimális módon működnek a részletek konfigurációk magyarázza el. A papír kiegészíti az SAP-dokumentáció és az SAP platformokon adni az elsődleges erőforrások telepítések és az SAP-szoftver telepítések képviselik.

Frissített: Augusztus 2015

[Ez az útmutató letöltése](http://go.microsoft.com/fwlink/?LinkId=613056)
