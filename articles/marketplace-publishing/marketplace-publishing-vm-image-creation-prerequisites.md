<properties
   pageTitle="Műszaki Előfeltételek hozhat létre virtuális gép képként a Azure piactér |} Microsoft Azure"
   description="Ismerje meg hozhat létre, és egy virtuális gép kép bevezetéshez az Azure piactéren elérhető mások számára meg kell vásárolnia követelményei."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="04/29/2016"
  ms.author="hascipio; v-divte"/>

# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>A virtuális gép képfájl létrehozása az Azure piactéren elérhető a technikai előfeltételei
A folyamat alapos megkezdése előtt és érthetőbbé, ahol, és miért minden egyes lépés végrehajtása. Szerint lehetséges, meg kell készítse el a vállalat adatainak és egyéb adatok, letöltéséhez szükséges eszközök és technikai összetevőket hozhat létre az ajánlat létrehozási folyamat megkezdése előtt. Ezek az elemek törlése a néző Ez a cikk kell lennie.  

## <a name="download-needed-tools--applications"></a>Szükséges eszközök és a alkalmazások letöltése
Rendelkeznie kell készen áll a folyamat megkezdése előtt az alábbiakat:

- Attól függően, hogy mely operációs rendszer céloz meg telepítse az [Azure PowerShell-parancsmagok](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) vagy [Linux parancssor eszköz](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) az [Azure letöltési](https://azure.microsoft.com/downloads/) lapjáról.
- Telepítse az Azure tároló Explorer a Codeplex webhelyen.
- Letöltése és telepítése a hitelesítő próba eszköz az Azure tanúsítvánnyal:
  - [http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913). A tanúsítvány eszköz futtatása a Windows-alapú számítógépen van szüksége. Ha nem érhető el a Windows-alapú számítógépre, futtatását is lehetővé teszi az eszköz használatával a Windows-alapú virtuális Azure-ban.

## <a name="platforms-supported"></a>Támogatott platformok
Fejleszthet vagy Linux rendszerben Windows Azure-alapú VMs. A közzétételi folyamat – például létrehozása az Azure-kompatibilis virtuális merevlemez (virtuális) – különböző eszközök és lépései attól függően, hogy mely operációs rendszer esetén elemeket:  

- Ha Linux használata esetén keresse meg a [virtuális gép kép közzétételi útmutató](marketplace-publishing-vm-image-creation.md)"Az Azure-kompatibilis virtuális (Linux-alapú) létrehozása" szakaszban.
- Ha Windows használata esetén keresse meg a [virtuális gép kép közzétételi útmutató](marketplace-publishing-vm-image-creation.md)a "Létrehozása az Azure-kompatibilis virtuális (Windows-alapú)" című szakaszát.

> [AZURE.NOTE] Egy Windows-alapú számítógépre való hozzáféréssel kell rendelkeznie:
- A tanúsítvány érvényességi eszköz futtatásával.
- Hozzon létre a virtuális hitelesítő Beküldési a megosztott virtuális access aláírás URL-CÍMÉT.

## <a name="develop-your-vhd"></a>A virtuális kidolgozása
A helyszíni vagy felhőalapú Azure VHD fejleszthet:

- Felhőalapú fejlesztési azt jelenti, hogy minden fejlesztési lépést a egy állomáson Azure virtuális távolról történik.
- A helyszíni fejlesztési egy virtuális le, és a helyszíni infrastruktúra használatával elkészítésének szükséges. Bár ez nem lehetséges, nem javasoljuk ezt. Figyelje meg, hogy Windows vagy az SQL fejlesztésének helyszíni kell lennie, hogy a helyszíni megfelelő licenccel rendelkező. Nem tartalmazza, vagy egy virtuális létrehozása után az SQL Server telepítése. Az ajánlat is az Azure portálról jóváhagyott SQL kép kell alapjául. Ha úgy dönt, hogy a helyszíni fejlesztése, eltérően, amelyeket fejlesztéséért a felhőben néhány lépést kell végrehajtania. [A helyszíni virtuális kép létrehozása](marketplace-publishing-vm-image-creation-on-premise.md)a található vonatkozó információkat.

## <a name="next-steps"></a>Következő lépések
Most, hogy a Előfeltételek felül, és a szükséges műveletek végrehajtása, áthelyezheti előre a virtuális gép kép ajánlat a [virtuális gép kép közzétételi útmutató](marketplace-publishing-vm-image-creation.md)ismertetett módon létrehozásával.

## <a name="see-also"></a>Lásd még:
- [Első lépések: a Microsoft Azure piactéren felajánlás közzététele](marketplace-publishing-getting-started.md)
- [Windows operációs rendszert futtató az Azure előzetes portálon virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md)


[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
