<properties
   pageTitle="Azure architektúra útmutató – a IaaS: Végrehajtása egy DMZ Azure és az Internet között |} Microsoft Azure"
   description="Hogyan lehet egy biztonságos hibrid hálózat architektúrája Internet-hozzáféréssel rendelkező megvalósítása Azure-ban."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-the-internet"></a>A DMZ Azure és az Internet között végrehajtása

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Ez a cikk ismerteti a biztonságos hibrid a hálózat, fogadja el az Azure internet hálózati forgalmat a helyszíni hálózaton kiterjed alkalmazásával kapcsolatos gyakorlati tanácsokat. A hivatkozás architektúra kiterjeszti az [Azure és a helyszíni adatközponthoz között egy DMZ végrehajtása]a cikkben ismertetett architektúra[implementing-a-secure-hybrid-network-architecture]. Ajánlott, olvassa el ezt a dokumentumot, és a hivatkozás megértéséhez architektúra ehhez a dokumentumhoz olvasási előtt.

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Erőforrás-kezelő] [ resource-manager-overview] és klasszikus. A hivatkozás architektúra erőforrás-kezelő Microsoft javasolja új telepítési használja. 

Ez a felépítés esettel jellemző használata a következők:

- Hol munkaterhelésekből Futtatás részben a helyszíni hibrid alkalmazásokat és részben az Azure.

- A helyszíni és az internetről a bejövő forgalom Azure infrastruktúra.

## <a name="architecture-diagram"></a>Architektúra diagramja

Az alábbi ábra kiemeli a fontos e architektúra összetevőket:

> A Visio-dokumentum, amely tartalmazza a architektúra diagram letölthető a [Microsoft letöltőközpontból][visio-download]. Ez az ábra be van kapcsolva a "DMZ – nyilvános" lapra.

[! [0]][0]

- **Nyilvános IP-cím (PIP).** Ez az a nyilvános végpont IP-címét. Csatlakozik az internethez, a külső felhasználók hozzá tudnak férni a rendszer – erre a címre.

- **Hálózati virtuális készülék (NVA).**  NVA általános kifejezés, amely egy virtuális feladatok például engedélyezésének vagy megtagadja a hozzáférést a tűzfalat, optimalizálás WAN műveletek (beleértve a hálózati tömörítés), egyéni útválasztás vagy más hálózati funkciókat ismerteti.

- **Azure terheléselosztó.** Az internetről származó minden bejövő felkérést a terheléselosztó haladnia, és a nyilvános DMZ a NVAs szállítandó bejövő alhálózat.

- **Nyilvános DMZ bejövő alhálózat.** Az alhálózathoz fogadja el a Azure terheléselosztó érkező kérések. Bejövő felkérést átadni a NVAs a DMZ a közül.

- **Nyilvános DMZ kimenő alhálózat.** A NVA által jóváhagyott kérelmek a webes réteg számára a belső terheléselosztó keresztül az alhálózathoz továbbítja.

## <a name="recommendations"></a>Javaslatok

Azure felajánlja a különféle számos különböző erőforrásokat és erőforrástípus, így a hivatkozás architektúra lehet kiépítve számos különböző módon. A fenti ajánlást követő hivatkozás architektúra telepíteni egy Azure erőforrás-kezelő sablon nyújtott. Ha úgy dönt, hogy hozzon létre saját hivatkozás architektúrája, kivéve, ha rendelkezik az adott követelmény, hogy nem férnek el a ajánlást célszerű követnie az alábbi javaslatokat.

### <a name="nva-recommendations"></a>NVA javaslatok

Az interneten, és egy másikat a forgalmat a helyszíni származó származó forgalmához NVAs az egyik készlete végrehajtása. Akkor használjon csak egy NVAs mindkét mivel ez a tervezés biztosít a hálózati forgalmának engedélyezésére két adatcsoport között nincs biztonsági külső biztonsági kockázatot. A tervezés használni, mivel a biztonsági szabályok ellenőrzése komplexitását csökkenti a előny, és teszi világosabb is, mely szabályokat minden bejövő hálózati kérés felelnek meg. NVAs az egyik készlete például csak egy másik szabálykészletet NVAs megvalósítása csak a forgalmat a helyszíni közben az internetforgalom szabályokat hajtja végre.

Tartalmazza az egy réteg 7 NVA alkalmazás kapcsolatok NVA szintre leállítás, és a kódmentes rétegek a affinitás karbantartásához. Ez garantálja szimmetrikus kapcsolódási, amelyben válasz-forgalmat a kódmentes rétegek a keresztül a NVA adja eredményül.  

### <a name="public-load-balancer-recommendations"></a>Nyilvános betöltés terheléselosztó javaslatok ###

Méretezhetőség és elérhetőség megtartásához telepítése a nyilvános DMZ NVAs bejövő egy [elérhetőségének beállítása] [ availability-set] , és egy [internetes szemben lévő terheléselosztó] [ load-balancer] internetes kérelmek terjesztheti az elérhetőség megadása NVAs keresztül.  

Állítsa be a csak a az internetes forgalmat szükséges portokon összehívások terheléselosztó. Ha például korlátozása 80-as port bejövő HTTP kérelmek és a 443-as porttal bejövő HTTPS-kérések.

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

Egy internetes terheléselosztó kezdettől fogva bejövő nyilvános DMZ alhálózat elé az infrastruktúra tervezése. Akkor is, ha a architektúra initally egy egyetlen NVA van szüksége, egyszerűbb méretezhető több NVAs kapcsolatban, ha az interneten szemben lévő terheléselosztó már telepítve van lesz.

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

Az internet szemben lévő terheléselosztó szükséges a nyilvános DMZ az egyes NVA egy [állapot vizsgálati]végrehajtásához bejövő alhálózat[lb-probe]. A rendszerállapot vizsgálati, amely nem válaszol a végpont tekinthető nem érhető el, és a terheléselosztó fog közvetlen valaki más NVAs elérhetősége mindazon. Figyelje meg, hogy az összes NVAs nem válaszol, ha az alkalmazás meghiúsul, ezért fontos, hogy konfigurálva a riasztási DevOps, ha megfelelő NVA példányok száma egy küszöbérték alá esik figyelő.

## <a name="manageability-considerations"></a>Kezelhetőség kapcsolatos szempontok

Korlátozza a figyelő és felügyeleti funkciókat a bejövő nyilvános DMZ NVA meg csak a kezelés alhálózat Ugrás listájából kérelmekre válaszolni. [A DMZ Azure és a helyszíni adatközponthoz között végrehajtási] tárgyalt[ implementing-a-secure-hybrid-network-architecture] dokumentum, adja meg a helyszíni hálózaton keresztül az átjáró a metszéspont mezőre az egyetlen hálózat útvonal való hozzáférés korlátozása az adatkezelési alhálózat.

Átjáró a helyszíni hálózaton kiszolgálóhoz való csatlakozás Azure nem működik, ha még mindig érheti el az Ugrás párbeszédpanel egy MEZŐPONT felveszi a metszéspont mezőbe, és a távelérési az internetről telepítésével.

## <a name="security-considerations"></a>Biztonsági megfontolások

A hivatkozás architektúra biztonsági többszintű hajtja végre:

- Szemben lévő terheléselosztó az internet a NVAs kérelmek arra utasítja, a bejövő nyilvános DMZ alhálózat csak, és csak az alkalmazáshoz szükséges portokat a.

- A bejövő és kimenő nyilvános DMZ alhálózat NSG szabályok megakadályozzák a NVAs blokkolja a NSG szabályokat kívül eső kérések kezekbe.

- A hálózati Címfordítást a NVAs útválasztás beállításának irányítja a 80-as és 443-as port a webes réteg terheléselosztó bejövő felkérést, de minden más portokra kérelmek figyelmen kívül hagyja a.

Figyelje meg, hogy nem kell bejelentkezni az összes portokon összes bejövő felkérést. Rendszeresen naplózza a naplókat, ezek jelezheti, hogy behatolási kísérletek várható paraméterek kívül eső kérések figyelmet.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Továbbfejlesztett fájlblokkolás/fázis forgalom alkalmazás rétegek közötti NSGs használatával

Minden, az adatok, üzleti és webes meghatározási korlátozása közöttük forgalom NSGs használatával. Ez azt jelenti, hogy a vállalati réteg egy NSG használ a webes réteg származnak, nem minden forgalom blokkolása, és az adatok réteg minden forgalom az üzleti réteg származnak nem blokkolja-NSG használja. Ha a követelmény, hogy bontsa ki a következő rétegek szélesebb elérésének engedélyezése a NSG szabályokat, összehasonlítani ezeknek a követelményeknek, a biztonsági kockázatok ellen. Minden új bejövő járda véletlen vagy purposeful adatok folyás vagy alkalmazás károkért lehetőséget jelöli.

## <a name="solution-deployment"></a>Megoldás üzembe helyezése

A hivatkozás-architektúra, amely a fenti ajánlást egy telepítésének Github érhető el. A hivatkozás architektúra egy virtuális hálózati (VNet), a hálózati biztonsági csoport (NSG), a terheléselosztó és a két virtuális gépeken futó (VMs) tartalmazza.

A hivatkozás architektúra telepítheti Windows vagy Linux VMs az alábbi útmutatást követve: 

1. Kattintson a jobb gombbal az alábbi gombra, és válassza a bármelyik "hivatkozás megnyitása új lapon" vagy "A hivatkozás megnyitása új ablakban":  
[![Azure telepítése](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2FvirtualNetwork.azuredeploy.json)

2. Miután a hivatkozásra az Azure-portálon megnyílt, értéket kell megadnia az egyes beállításokat: 
    - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza az **Új létrehozása** , és adja meg `ra-public-dmz-network-rg` a szövegmezőbe.
    - Jelölje ki a régió a **hely** legördülő listából.
    - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
    - Jelölje ki az **Operációs rendszer típusa** a legördülő lista, **a windows** vagy **Linux rendszerhez**.
    - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
    - Kattintson a **vásárlás** gombra.

3. Várja meg, a telepítés befejezéséhez.

4. Kattintson a jobb gombbal az alábbi gombra, és válassza a bármelyik "hivatkozás megnyitása új lapon" vagy "A hivatkozás megnyitása új ablakban":  
[![Azure telepítése](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fworkload.azuredeploy.json)

5. Miután a hivatkozásra az Azure-portálon megnyílt, értéket kell megadnia az egyes beállításokat: 
    - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza az **Új létrehozása** , és adja meg `ra-public-dmz-wl-rg` a szövegmezőbe.
    - A **hely** legördülő listából válassza ki a régió.
    - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
    - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
    - Kattintson a **vásárlás** gombra.

6. Várja meg, a telepítés befejezéséhez.

7. Kattintson a jobb gombbal az alábbi gombra, és válassza a bármelyik "hivatkozás megnyitása új lapon" vagy "A hivatkozás megnyitása új ablakban":  
[![Azure telepítése](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fsecurity.azuredeploy.json)

8. Miután a hivatkozásra az Azure-portálon megnyílt, értéket kell megadnia az egyes beállításokat: 
    - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza a **Meglévő használatát** , és adja meg `ra-public-dmz-network-rg` a szövegmezőbe.
    - Jelölje ki a régió a **hely** legördülő listából.
    - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
    - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
    - Kattintson a **vásárlás** gombra.

9. Várja meg, a telepítés befejezéséhez.

10. A paraméter fájlokat olyan csomagolásukkor rendszergazdai felhasználónevével és jelszavával az összes VMs, és a javasolt, hogy azonnal módosítsa egyaránt. Minden egyes virtuális a környezetben jelölje ki azt az Azure-portálra, és válassza a **jelszó alaphelyzetbe állítása** a **támogatási + hibaelhárítási** lap. Jelölje be a **jelszó alaphelyzetbe állítása** **üzemmód** legördülő mezőben, majd jelölje be az új **felhasználó nevét** és **jelszavát**. A **frissítés** gombra kattintva áll fenn.


<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[iptables]: https://help.ubuntu.com/community/IptablesHowTo
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md
[load-balancer]: ../load-balancer/load-balancer-internet-overview.md
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[0]: ./media/blueprints/hybrid-network-secure-vnet-dmz.png "Biztonságos hibrid hálózat architektúrája"
