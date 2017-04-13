<properties
    pageTitle="Az Azure VNET szeretne egy RemoteApp VNET áttelepítése |} Microsoft Azure"
    description="Megtudhatja, hogy miként az Azure VNET egy RemoteApp VNET áttelepítése"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="how-to-migrate-a-hybrid-collection-from-a-remoteapp-vnet-to-an-azure-vnet"></a>Hogy miként telepítheti át a hibrid webhelycsoport egy RemoteApp VNET az Azure VNET

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Jó hír! Azt, hogy közvetlenül a meglévő Azure virtuális hálózatok (VNETs) RemoteApp-specifikus VNETs helyett a hibrid RemoteApp gyűjtemények üzembe engedélyezte. A szolgáltatás segítségével kihasználhatja az új VNET szolgáltatások (például készült ExpressRoute), illetve hozzáférést a hibrid webhelycsoportok közvetlen hálózati más Azure-szolgáltatások és az adott VNET rendszerbe állított virtuális gépeken futó.  (Ez kapja meg jobb teljesítményt és könnyebben beállítási mint VNET-VNET konfigurációk).


Tegyük fel, hogy már létrehozott egy hibrid *OriginalCollection* egy RemoteApp *RemoteAppVNET*nevű VNET nevű RemoteApp webhelycsoport. Íme egy új Azure VNET *AzureVNET*nevű áttelepítendő a lépéseket.

1.  Az [adatkezelési portál](http://manage.windowsazure.com/) **hálózatok** lapon hozzon létre egy VNET nevű *AzureVNET*, a ugyanazon a helyen, a DNS-beállításait, és a címterület (a legalább egyike a *AzureVNET* alhálózat) használata közben *RemoteAppVNET*használható.
2.  *AzureVNET* konfigurálja úgy, hogy bármelyik host, vagy kérje a hálózati kapcsolat, amely *OriginalCollection* tartományhoz csatlakoztatott az Active Directory-példányhoz.
3.  A **Távoli alkalmazások** lapon hozzon létre egy új RemoteApp *Új webhelycsoport*neve. (Használja a **VNET a létrehozás** lehetőséget, és nem a **Gyors létrehozása**.)
3.  Állítsa be a *NewCollection* egy *AzureVNET*az alhálózathoz telepítendő.
4.  Állítsa be a *NewCollection* a ugyanazt a képet, és a tartomány csatlakozási adatai használata közben *OriginalCollection*használható.
5.  Néhány óra elteltével *NewCollection* abban a webhelycsoport egy aktív állapot jelennek meg.

Most ha már nincs szükség a felhasználói adatok az eredeti gyűjteményből áttelepítése a gyűjteményben, hajtsa végre ezeket a lépéseket tovább:

6.  *OriginalCollection*törlése.
7.  *RemoteAppVNET*törlése.

És ezzel elkészült!

Másik megoldás ha van szüksége a felhasználói adatok az eredeti gyűjteményből áttelepítése a gyűjteményben, kövesse az alábbi lépéseket tovább:

6.  Küldés e-mailben [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) az Azure előfizetés azonosítója, az eredeti gyűjtemény nevét és az új webhelycsoport nevét, és kérje meg, hogy a felhasználói adatok áttelepítése.
7.  2 üzleti napon belül a RemoteApp csapatának helyezi át a felhasználói hozzáférés lista és az összes felhasználói dokumentumokat és a felhasználói beállításokat az eredeti gyűjteményből az új webhelycsoport.
8.  *OriginalCollection*törlése.
9.  *RemoteAppVNET*törlése.

És most ezzel elkészült!

Ha kérdése van, vagy speciális segítségre van szüksége, kérjük, e-mailben [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).
