<properties
   pageTitle="A biztonságos hibrid hálózat architektúrája végrehajtási Azure-ban |} Microsoft Azure"
   description="Hogyan lehet egy biztonságos hibrid hálózat architektúrája megvalósítása Azure-ban."
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

# <a name="implementing-a-dmz-between-azure-and-your-on-premises-datacenter"></a>A DMZ Azure és a helyszíni adatközponthoz között végrehajtása

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Ez a cikk ismerteti, hogy a helyszíni hálózaton kiterjed Azure biztonságos hibrid hálózaton alkalmazásával kapcsolatos gyakorlati tanácsokat. A hivatkozás architektúra végrehajtja a DMZ egy helyszíni és az Azure virtuális hálózat használata a felhasználó által definiált útvonalak (UDRs) között. A DMZ könnyen hozzáférhető hálózati virtuális készülékek (NVAs), amelyek biztonsági funkciókat, például a tűzfalak és csomag vizsgálati megvalósítása tartalmazza. A VNet az összes kimenő forgalmának azért, hogy kötelező bújtatott az internethez, a helyszíni hálózaton keresztül, a naplózásra is. 

Ez a felépítés a helyszíni adatközponthoz végrehajtani, vagy egy [virtuális Magánhálózati átjáró]segítségével kapcsolatot igényel[ra-vpn], vagy egy [készült ExpressRoute] [ ra-expressroute] kapcsolat.

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Erőforrás-kezelő] [ resource-manager-overview] és klasszikus. A hivatkozás architektúra erőforrás-kezelő Microsoft javasolja új telepítési használja.

Ez a felépítés esettel jellemző használata a következők:

- Hol munkaterhelésekből Futtatás részben a helyszíni hibrid alkalmazásokat és részben az Azure.

- A helyszíni és az internetről a bejövő forgalom Azure infrastruktúra.

- Kimenő forgalmának naplózandó szükséges alkalmazásokat. Gyakran számos kereskedelmi rendszer szabályozó követelménye, és megakadályozhatja, hogy a személyes információk nyilvánossá tétel funkciót.

## <a name="architecture-diagram"></a>Architektúra diagramja

Az alábbi ábra kiemeli a fontos e architektúra összetevőket:

> A Visio-dokumentum, amely tartalmazza a architektúra diagram letölthető a [Microsoft letöltőközpontból][visio-download]. Ez az ábra be van kapcsolva a "DMZ – magánjellegű" lapra.

[! [0]][0]

- **A helyszíni hálózaton.** Ez a hálózat és magánjellegű keresztül csatlakoztatott eszközök végrehajtani a szervezet helyi hálózati.

- **Azure virtuális hálózati (VNet).** A VNet tárolja, az alkalmazás és más erőforrások: fut a felhőben.

- **Átjáró.** Az átjáró a helyszíni hálózaton az útválasztó és a VNet közötti kapcsolatot biztosít.

- **Hálózati virtuális készülék (NVA).** NVA általános kifejezés, amely egy virtuális feladatok például engedélyezésének vagy megtagadja a hozzáférést a tűzfalat, optimalizálás WAN műveletek (beleértve a hálózati tömörítés), egyéni útválasztás vagy más hálózati funkciókat ismerteti. 

- **Réteg, üzleti réteg és adatok réteg alhálózat webes.** Ezek a VMs és szolgáltatások, amelyek egy példa 3 szintű alkalmazást futtató a felhőben megvalósítása szolgáltatója alhálózat. Lásd: [Windows VMs fut az Azure-N szintű architektúrája] [ ra-n-tier] további információt.

- **A felhasználó által definiált útvonalak (UDR).** [Felhasználó által definiált útvonalak] [ udr-overview] meghatározása a Azure VNets belül IP-forgalmat. 

> [AZURE.NOTE] Attól függően, hogy a virtuális Magánhálózati kapcsolat követelményeinek beállíthatja szegély átjáró Protocol (BGP) útvonalak UDRs használatával megvalósítását a továbbítási szabályok, amelyek irányítja a forgalmat a helyszíni hálózaton keresztül biztonsági helyett.

- **Adatkezelési alhálózat.** Az alhálózathoz tartalmazza, amelyek a kezelési és felügyeleti lehetőségeket a összetevők futtatása a VNet megvalósítása VMs. 

## <a name="recommendations"></a>Javaslatok

Azure felajánlja a különféle számos különböző erőforrásokat és erőforrástípus, így a hivatkozás architektúra lehet kiépítve számos különböző módon. A fenti ajánlást követő hivatkozás architektúra telepíteni egy Azure erőforrás-kezelő sablon nyújtott. Ha úgy dönt, hogy hozzon létre saját hivatkozás architektúrája, kivéve, ha rendelkezik az adott követelmény, hogy nem férnek el a ajánlást célszerű követnie az alábbi javaslatokat.

### <a name="rbac-recommendations"></a>RBAC javaslatok

Hozzon létre több RBAC szerepkörök az alkalmazás az erőforrások kezelésére. Hozzon létre egy [egyéni szerepkör] DevOps[ rbac-custom-roles] való alkalmazásához a infrastruktúra engedélyekkel. Hozzon létre egy központi informatikai rendszergazda [egyéni szerepkör] [ rbac-custom-roles] hálózati erőforrások és az informatikai rendszergazda [egyéni szerepkör] külön értékpapír kezelésének[ rbac-custom-roles] biztonságos hálózati erőforrások, például a NVAs kezeléséhez. 

A DevOps szerepkör telepítése az alkalmazás-összetevők, valamint a monitor, és indítsa újra a VMs engedélyekkel kell tartalmaznia. A központi informatikai rendszergazdai szerepkör tartalmaznia kell figyelni a hálózati erőforrások engedélyek. Ezek közül egyik sem szerepkörök NVA erőforrások hozzáféréssel kell rendelkeznie, mint ez kell a biztonsági informatikai rendszergazdai szerepkör korlátozni.

### <a name="resource-group-recommendations"></a>Erőforrás-csoport javaslatok

Azure erőforrások, például VMs, VNets és terheléselosztókkal könnyen kezelhető csoportosítja őket közös erőforrás csoportokat. A fenti szerepkörök majd rendelhet minden erőforráscsoport való hozzáférés korlátozása.

Azt javasoljuk, hogy a következő létrehozása:

- Amelyben a alhálózat (kivéve a VMs), NSGs és a helyszíni hálózaton csatlakozhat az átjáró-erőforrások erőforráscsoport. A központi informatikai rendszergazdai szerepkör hozzárendelése az erőforrás csoport.

- A VMs tartalmazó a NVAs (a terheléselosztó is beleértve), az Ugrás párbeszédpanel és más management VMs és az átjáró alhálózat, hogy minden forgalom az a NVAs kezd a UDR erőforráscsoport. A biztonsági informatikai rendszergazdai szerepkörök hozzárendelése az erőforráscsoport.

- Erőforrás-csoportok a minden alkalmazás réteg terheléselosztó és VMs tartalmazó elkülönítésére. Ne feledje, hogy az erőforráscsoport a az egyes réteg alhálózat kerülni a Webhelyfiókok tartalmazzák. A DevOps szerepkör hozzárendelése az erőforrás csoport.

### <a name="virtual-network-gateway-recommendations"></a>Virtuális hálózati átjáró javaslatok

A helyszíni forgalmat a VNet áthaladt a virtuális hálózati átjáró. Azt javasoljuk, hogy az [Azure virtuális Magánhálózati átjáró] [ guidance-vpn-gateway] vagy egy, [a készült Azure ExpressRoute átjáró][guidance-expressroute]. 

### <a name="nva-recommendations"></a>NVA javaslatok

NVAs kezelésére és figyelése hálózati forgalmának engedélyezésére más szolgáltatásokat nyújt. A Microsoft Azure piactéren számos külső cégtől NVAs, beleértve a kínál:

- [A webes alkalmazás tűzfal barracuda] [ barracuda-waf] és [Barracuda NextGen tűzfal][barracuda-nf]

- [Összefüggő hálózatok VNS3 tűzfal/útválasztó/VPN][vns3]

- [Fortinet FortiGate-virtuális][fortinet]

- [SecureSphere webes alkalmazás tűzfal][securesphere]

- [DenyAll webes alkalmazás tűzfal][denyall]

- [Jelölje be a pont vSEC][checkpoint]

A külső NVAs egyik sem felel meg igényeinek, ha egy egyéni NVA VMs használatával hozhat létre. Példa egyéni NVAs létrehozása olvassa el a DMZ található a hivatkozás-architektúra, amely a következő funkciók:

- Adatforgalom használata [IP-továbbítási] továbbítás[ ip-forwarding] a NVA NIC meg.

- Forgalom engedélyezett a NVA át, csak akkor, ha szükség. Az összefoglaló architektúra minden NVA virtuális egy egyszerű Linux útválasztó bejövő forgalmat a hálózati kapcsolat *eth0*és a hálózati kapcsolat *eth1*keresztül elküldött egyéni parancsfájlok által meghatározott kimenő forgalmának egyező szabályok érkezésekor. 

- Forgalom az adatkezelési alhálózat a rendszer nem hatolhat át a NVAs, és a NVAs csak az adatkezelési alhálózat állítható be. Ha a forgalmat a kezelés alhálózat szükséges a NVAs irányítása, nem a NVAs javítás, ha nem tudnak kell az adatkezelési alhálózathoz nem továbbításához.  

- Az elérhetőség mögött egy terheléselosztó beállítása a VMs esetében a NVA szerepelnek. Az átjáró alhálózat a UDR NVA kérelmek a terheléselosztó irányítja.

Fontolja meg egy másik ajánlást sorozat több NVAs összekapcsolása minden NVA speciális biztonsági feladat elvégzéséhez. Ez lehetővé teszi, hogy az egyes biztonsági funkció-NVA alapon kell kezelni. Például egy NVA tűzfal végrehajtási sikerült helyét sorozat és-Identitáskezelés services szolgáltatást futtató NVA. A útján esetében könnyű kezelés hozzáadása további hálózati Ugrás, előfordulhat, hogy az időtartam növelése, így biztosítva, hogy ez az alkalmazás teljesítményének nincs hatással.

### <a name="nsg-recommendations"></a>NSG javaslatok

A virtuális Magánhálózati átjáró közzététele egy nyilvános IP-címet a kapcsolatot a helyszíni hálózaton. Javasoljuk, (NSG) hálózati biztonsági csoport létrehozása a bejövő NVA alhálózat végrehajtó szabályok összes forgalmat a helyszíni hálózaton nem származó.

Is javasoljuk, hogy a második szintű, egy helytelenül konfigurált vagy letiltott NVA kihagyásával bejövő forgalom elleni védelem minden alhálózathoz NSGs végrehajtása. A webes réteg alhálózat az összefoglaló architektúra például egy NSG szabályok figyelmen kívül hagyja az összes kérelmeket, amelyek nincsenek képletkészítőkhöz a helyszíni hálózaton (192.168.0.0/16) vagy a VNet és egy másik szabály figyelmen kívül hagyja a összes kérések nem a 80-as port kapott hajtja végre. 

### <a name="internet-access-recommendations"></a>Az Internet-hozzáférés javaslatok

[Kötelező-alagutas] [ azure-forced-tunneling] a webhely VPN-csatorna és az internet segítségével útvonalat a helyszíni hálózaton keresztül az összes kimenő internetes forgalmat a hálózati cím tranlation (hálózati Címfordítást). Ezzel is megakadályozhatja, hogy minden olyan bizalmas adatokat tárolja az adatokat réteg véletlen szivárgás, és azt is, hogy ellenőrzési és minden kimenő forgalmának naplózás.

> [AZURE.NOTE] Teljesen nem blokkolja a webes, üzleti és alkalmazás rétegek az internetes forgalmat. Ha ezek rétegek Azure PaaS szolgáltatások azokat a virtuális diagnosztikai naplózás nyilvános IP-címeket használja arra, töltse le a virtuális extensions és egyéb funkciókat biztosít. Azure diagnosztika is szükséges, hogy összetevők olvasása, és írja be egy internetes függő Azure tárterület-fiókot.

További javasoljuk, hogy ellenőrizze kimenő internetforgalom hatályba bújtatott megfelelően-e. Ha használni szeretné a virtuális Magánhálózati kapcsolat a [Útválasztás és távoli szolgáltatás] [ routing-and-remote-access-service] -kiszolgálókon üzemelő a helyszíni, például a [wireshark eszközben] eszköz használata[ wireshark] vagy a [Microsoft üzenet Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=44226).

### <a name="management-subnet-recommendations"></a>Adatkezelési alhálózat javaslatok

Az adatkezelési alhálózat rendelt végrehajtja a kezelési és felügyeleti funkciókat Ugrás mező tartalmazza. Az Ugrás mező az alábbi javaslatokat végrehajtása:
- Ne hozzon létre egy nyilvános IP-címet a metszéspont jelölőnégyzetet. 
- Hozzon létre egy útvonalat az Ugrás párbeszédpanel eléréséhez a bejövő átjárón keresztül, és egy NSG megvalósítását a az adatkezelési alhálózat csak kérelmekre válaszolni az engedélyezett útvonal.
- Az Ugrás párbeszédpanel korlátozása összes biztonságos adatkezelési feladatok végrehajtását.

### <a name="nva-recommendations"></a>NVA javaslatok

Tartalmazza az egy réteg 7 NVA alkalmazás kapcsolatok NVA szintre leállítás, és a kódmentes rétegek a affinitás karbantartásához. Ez garantálja szimmetrikus kapcsolódási, amelyben válasz-forgalmat a kódmentes rétegek a keresztül a NVA adja eredményül.

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

A hivatkozás architektúra hajtja végre egy terheléselosztó irányítja a hálózati forgalmat a helyszíni NVA eszközök erőforráskészlethez tartozik. Korábbiakban tárgyalt NVA eszközök végrehajtása a hálózati forgalmat útválasztási szabályok VMs és telepítik egy [elérhetősége állítsa]be[availability-set]. A tervezés lehetővé teszi, hogy figyelemmel követheti a NVAs a kapacitásának adott idő alatt és NVA eszközök felvétele a betöltés növekedését választ.

A szokásos Termékváltozat VPN-átjáró legfeljebb 100 MB tartós kapacitásának támogatja. A nagy teljesítmény Termékváltozat legfeljebb 200 MB biztosít. A nagyobb sávszélessége fontolja meg az egy készült ExpressRoute átjáró. A virtuális Magánhálózati kapcsolat-nél kisebb késés legfeljebb 10 GB/s sávszélesség készült ExpressRoute biztosít.

> [AZURE.NOTE] A [végrehajtási egy hibrid hálózat architektúrája Azure és a helyszíni VPN] cikkek[ guidance-vpn-gateway] és [egy hibrid hálózat architektúrája és a készült Azure ExpressRoute végrehajtásához] [ guidance-expressroute] az Azure átjárók méretezhetőség billentyűzetre kérdéseit.

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

A hivatkozás architektúra egy NVA eszközök Azure-ban erőforráskészlethez tartozik a helyszíni kérelmek terjesztése terheléselosztó hajtja végre. Hálózati forgalmának engedélyezésére útválasztási szabályok végrehajtása VMs NVA eszközök és telepítik egy [elérhetősége állítsa]be[availability-set]. A terheléselosztó rendszeresen egy állapot vizsgálati végrehajtott egyes NVA a lekérdezést, és a készlet bármelyik nem reagál NVAs eltávolítja. 

Használata a készült Azure ExpressRoute VNet és a helyszíni hálózaton, [Adjon meg egy virtuális Magánhálózati átjárót feladatátvevő megadására] közötti kapcsolatot megadására[ guidance-vpn-failover] Ha készült ExpressRoute kapcsolat elérhetetlenné válik.

Részletes információt a virtuális Magánhálózati és készült ExpressRoute kapcsolatokhoz elérhetősége fenntartása, olvassa el a cikk, [egy hibrid hálózat architektúrája Azure és a helyszíni VPN végrehajtási] [ guidance-vpn-gateway] és [egy hibrid hálózat architektúrája és a készült Azure ExpressRoute végrehajtásához][guidance-expressroute].

## <a name="manageability-considerations"></a>Kezelhetőség kapcsolatos szempontok

Az összes alkalmazás és erőforrás-figyelése az Ugrás mező az adatkezelési alhálózat kell elvégezni. Attól függően, hogy az alkalmazás igényeknek megfelelően alakíthatja előfordulhat, hogy a kezelés alhálózat vehet fel további felügyeleti források, de ismét a további erőforrások közül kell érhető el az Ugrás párbeszédpanel keresztül.

Átjáró a helyszíni hálózaton kiszolgálóhoz való csatlakozás Azure nem működik, ha még mindig érheti el az Ugrás párbeszédpanel egy MEZŐPONT felveszi a metszéspont mezőbe, és a távelérési az internetről telepítésével.

Az összefoglaló architektúra minden réteg alhálózat védett NSG szabályokat, és hozhat létre egy szabályt, nyissa meg a 3389 RDP hozzáférést a Windows VMs vagy port 22 SSH hozzáférést a Linux VMs szükség lehet. Más kezelési és felügyeleti eszközök megkövetelheti szabályok további portok megnyitásához.

Ha a készült ExpressRoute megadására a helyszíni adatközponthoz és Azure közötti kapcsolatot használ, használja az [Azure kapcsolódási eszközkészlet (AzureCT)] [ azurect] figyelésére és kapcsolódási problémák megoldásához.

> [AZURE.NOTE] Kifejezetten figyelemmel kísérésére és a parancs [végrehajtása egy hibrid hálózat architektúrája Azure és a helyszíni VPN] VPN és készült ExpressRoute kapcsolatok kezelése célozza további információt találhat[ guidance-vpn-gateway] és [egy hibrid hálózat architektúrája és a készült Azure ExpressRoute végrehajtásához][guidance-expressroute].

## <a name="security-considerations"></a>Biztonsági megfontolások

A hivatkozás architektúra biztonsági többszintű hajtja végre: 

### <a name="routing-all-on-premises-user-requests-through-the-nva"></a>Az összes a helyszíni felhasználói kérések keresztül a NVA továbbítása

Az átjáró alhálózat a UDR nincsenek képletkészítőkhöz helyszíni kapott összes felhasználói kérések letiltja. A UDR fázisokat a magánjellegű DMZ-alhálózat sorolja fel a NVAs kérelmeket, és ezek kérelmek átadni az alkalmazás Ha engedélyezve van a NVA szabályokat. Más útvonalak adhatók hozzá a UDR, de azok nem véletlenül figyelmen kívül hagyása az NVAs vagy blokk felügyeleti forgalom az adatkezelési alhálózat szánt biztosítása.

A NVAs elé a terheléselosztó is működik biztonsági eszközként figyelmen kívül hagyja a forgalmat portokon, amelyek nem nyissa meg a terheléselosztási szabályok szerint. A hivatkozás-architektúrában terheléselosztókkal csak a 80-as port HTTP-kérések és a 443-as port HTTPS-kérések figyelik. A terheléselosztókkal hozzáadva további szabályok dokumentálni kell, és a forgalom annak érdekében, hogy nincs biztonsági problémák lépnek fel kell nyomon követni.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Blokkokból álló/fázis forgalom alkalmazás rétegek közötti NSGs használatával

Minden, az adatok, üzleti és webes meghatározási korlátozása közöttük forgalom NSGs használatával. Ez azt jelenti, hogy a vállalati réteg egy NSG használ a webes réteg származnak, nem minden forgalom blokkolása, és az adatok réteg minden forgalom az üzleti réteg származnak nem blokkolja-NSG használja. Ha a követelmény, hogy bontsa ki a következő rétegek szélesebb elérésének engedélyezése a NSG szabályokat, összehasonlítani ezeknek a követelményeknek, a biztonsági kockázatok ellen. Minden új bejövő járda véletlen vagy purposeful adatok folyás vagy alkalmazás károkért lehetőséget jelöli.

### <a name="devops-access"></a>Az access DevOps

A műveletek, amelyek minden réteg [RBAC] használatával DevOps végezhet korlátozása[ rbac] engedélyek kezelése. Engedélyek megadása használatakor a [legalacsonyabb jogosultság elve][security-principle-of-least-privilege]. Jelentkezzen be az összes felügyeleti műveleteket, és végezze el annak biztosítására, konfigurációs módosításokat tervezettnél normál eseményeket.

> [AZURE.NOTE] Bővebb információkat, példák és esetek Azure, hálózati értékpapír kezelésével kapcsolatban lásd a [Microsoft-felhőszolgáltatások és a hálózati biztonság][cloud-services-network-security]. Részletes információt a felhőben erőforrások védelme lásd: [Bevezetés a Microsoft Azure biztonsági][getting-started-with-azure-security]. További biztonsági kérdések megcímezheti az Azure átjáró kapcsolaton keresztül a című témakör tartalmaz [egy hibrid hálózat architektúrája Azure és a helyszíni VPN végrehajtási] [ guidance-vpn-gateway] és [egy hibrid hálózat architektúrája és a készült Azure ExpressRoute végrehajtásához][guidance-expressroute].

## <a name="solution-deployment"></a>Megoldás üzembe helyezése

A hivatkozás-architektúra, amely a fenti ajánlást egy telepítésének Github érhető el. A hivatkozás architektúra egy virtuális hálózati (VNet), a hálózati biztonsági csoport (NSG), a terheléselosztó és a két virtuális gépeken futó (VMs) tartalmazza.

A hivatkozás architektúra telepítheti Windows vagy Linux VMs az alábbi útmutatást követve: 

1. Kattintson a jobb gombbal az alábbi gombra, és válassza a bármelyik "hivatkozás megnyitása új lapon" vagy "A hivatkozás megnyitása új ablakban":  
[![Azure telepítése](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet%2Fazuredeploy.json)

2. Miután a hivatkozásra az Azure-portálon megnyílt, értéket kell megadnia az egyes beállításokat: 
    - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza az **Új létrehozása** , és adja meg `ra-private-dmz-rg` a szövegmezőbe.
    - A **hely** legördülő listából válassza ki a régió.
    - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
    - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
    - Kattintson a **vásárlás** gombra.

3. Várja meg, a telepítés befejezéséhez.

4. A paraméter fájlokat olyan csomagolásukkor rendszergazdai felhasználónevével és jelszavával az összes VMs, és a javasolt, hogy azonnal módosítsa is. Minden egyes virtuális a környezetben jelölje ki azt az Azure-portálra, és válassza a **jelszó alaphelyzetbe állítása** a **támogatási + hibaelhárítási** lap. Jelölje be a **jelszó alaphelyzetbe állítása** **üzemmód** legördülő mezőben, majd jelölje be az új **felhasználó nevét** és **jelszavát**. A **frissítés** gombra kattintva továbbra is fennáll.

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan tudnak megvalósítani a [DMZ Azure és az Internet között](./guidance-iaas-ra-secure-vnet-dmz.md).
- Megtudhatja, hogyan tudnak megvalósítani a [könnyen hozzáférhető hibrid hálózat architektúrája](./guidance-hybrid-network-expressroute-vpn-failover.md).

<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-create-availability-set.md
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[azure-forced-tunneling]: https://azure.microsoft.com/en-gb/documentation/articles/vpn-gateway-forced-tunneling-rm/
[barracuda-nf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/barracuda-ng-firewall/
[barracuda-waf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/
[checkpoint]: https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/
[cloud-services-network-security]: https://azure.microsoft.com/documentation/articles/best-practices-network-security/
[denyall]: https://azure.microsoft.com/marketplace/partners/denyall/denyall-web-application-firewall/
[fortinet]: https://azure.microsoft.com/marketplace/partners/fortinet/fortinet-fortigate-singlevmfortigate-singlevm/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[ip-forwarding]: ../virtual-network/virtual-networks-udr-overview.md#IP-forwarding
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[ra-n-tier]: ./guidance-compute-n-tier-vm.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[rbac]: ../active-directory/role-based-access-control-configure.md
[rbac-custom-roles]: ../active-directory/role-based-access-control-custom-roles.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[routing-and-remote-access-service]: https://technet.microsoft.com/library/dd469790(v=ws.11).aspx
[securesphere]: https://azure.microsoft.com/marketplace/partners/imperva/securesphere-waf-for-azr/
[security-principle-of-least-privilege]: https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vns3]: https://azure.microsoft.com/marketplace/partners/cohesive/cohesiveft-vns3-for-azure/
[wireshark]: https://www.wireshark.org/
[0]: ./media/blueprints/hybrid-network-secure-vnet.png "Biztonságos hibrid hálózat architektúrája"
