<properties
    pageTitle="Az Active Directory és a DNS-az Azure webhely helyreállítási védelme |} Microsoft Azure"
    description="Ez a cikk ismerteti, hogyan katasztrófa helyreállítási megoldást végrehajtásához az Active Directory Azure webhely helyreállítás segítségével."
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="08/31/2016"
    ms.author="pratshar"/>

# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Az Active Directory és a DNS-az Azure webhely helyreállítási védelme

Vállalati alkalmazások, például a SharePoint, a Dynamics AX és az SAP attól függenek, az Active Directory és a DNS-infrastruktúrát helyesen fog működni. Alkalmazások katasztrófa helyreállítási megoldást hoz létre, esetén érdemes szem előtt tartani az, hogy szeretne-e védelemmel és az Active Directory és a DNS-visszaállítása annak érdekében, hogy dolog, amit működik helyesen katasztrófa esetén a többi alkalmazásösszetevők előtt.

Webhely-helyreállítás egy Azure szolgáltatás, amely biztosítja vészhelyreállítás orchestrating replikációs, feladatátadási és helyreállítási a virtuális gépeken futó. Webhely helyreállítási számos különböző replikációs esetek egységes védelmét, és zökkenőmentesen feladatátvevő virtuális gépeken futó és lehetőséget választva személyes, nyilvános vagy szolgáltatási felhőket alkalmazásokat támogatja.

Webhely helyreállítási használ, az Active Directory a teljes automatikus Vészhelyreállítási terv elkészítése hozhat létre. Megszakadása végrehajtásakor feladatátvevő kezdeményezzen másodpercek bárhonnan, és első néhány perc alatt az Active Directory lépéseket. Ha az elsődleges webhelyen több alkalmazásokhoz, például a SharePoint és az SAP az Active Directory már telepítette, és azt szeretné, hogy a teljes webhely átadni, végződhet-alatt az Active Directory először a webhely helyreállítási használ, és a többi alkalmazást használ, az alkalmazás-specifikus helyreállítási tervek majd átveszi.

Ez a cikk ismerteti, hogyan katasztrófa helyreállítási megoldás létrehozása az Active Directory, miként végezheti el a tervezett, a tervezett és próba feladatátadás egyetlen kattintással helyreállítási csomagra, a támogatott konfigurációk és a vonatkozó követelmények.  Az Active Directory és Azure webhely helyreállítási tudnia kell, mielőtt megkezdené.

Kétféleképpen lehet ajánlott a környezet komplexitását alapján.

### <a name="option-1"></a>A beállítás 1

Ha az alkalmazások és egy tartományvezérlőnek kisszámú van, és azt szeretné, hogy a teljes webhely átadni, majd használata ajánlott webhely helyreállítási való replikáció a tartományvezérlőnek a másodlagos webhelyet (hogy, esetén feladat átvétele Azure vagy egy másodlagos webhelyen). Ugyanazon replikált virtuális gép is használható próba feladatátvételi.

### <a name="option-2"></a>A beállítás 2

Ha nagyszámú alkalmazások és egynél több tartományvezérlőnek a környezetben van, vagy ha egyszerre csak néhány alkalmazások átadni, akkor javasoljuk, hogy a tartomány vezérlő virtuális gép webhely helyreállítási replikálása mellett, egy további tartományvezérlőnek a célwebhely (Azure vagy egy másodlagos helyszíni adatközponthoz) is ki tudja beállítani.

>[AZURE.NOTE] Ha a 2-beállítás, az továbbra is kell a webhely-helyreállítás segítségével tartományvezérlőnek bizonyos próba feladatátvevő módon van végrehajtása. Olvassa el a [feladatátvevő szempontok tesztelése](#considerations-for-test-failover) további információt.


Az alábbi szakaszok ismertetik, hogyan engedélyezhető a tartományvezérlőnek a webhely helyreállítási védelmét, és hogy miként állíthatja be a tartományvezérlőnek Azure-ban.


## <a name="prerequisites"></a>Előfeltételek

- Az Active Directory és a DNS-kiszolgáló helyszíni példányban.
- Egy a helyreállítási Webhelyszolgáltatások Azure a Microsoft Azure-előfizetésben tárolóból elemre.
- Ha meg van esetében, amelyek Azure Futtatás VMs biztosítása érdekében az Azure virtuális gép készenléti értékelési eszköz legyenek Azure VMs és Azure Webhelyszolgáltatások helyreállítás szolgáltatással kompatibilis.


## <a name="enable-protection-using-site-recovery"></a>Webhely-helyreállítás segítségével védelem bekapcsolása


### <a name="protect-the-virtual-machine"></a>A virtuális gép védelme

A tartomány vezérlő/DNS virtuális gép webhely helyreállítási a védelmet. A virtuális gép típusa (a Hyper-V vagy VMware) alapján webhely helyreállítási beállításainak konfigurálása Azt javasoljuk, hogy az összeomlást egységes replikációs gyakoriságát 15 perces.

###<a name="configure-virtual-machine-network-settings"></a>Virtuális gép hálózat beállításainak konfigurálása

A tartomány vezérlő/DNS virtuális gép hálózat beállításainak konfigurálása webhely helyreállítási, hogy a virtuális a program csatolja a megfelelő hálózati feladatátvétel után. Például ha, hogy esetében, amelyek a Hyper-V VMs Azure is választhat a virtuális VMM felhőalapú tárolóhelyen vagy a védelem csoport alább látható módon a hálózat beállításainak konfigurálása

![Virtuális hálózati beállítások](./media/site-recovery-active-directory/VM-Network-Settings.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Az Active Directory védelme az Active Directory ismétlésekkel

### <a name="site-to-site-protection"></a>Webhely védelem

Hozzon létre egy tartományvezérlőnek a másodlagos webhelyen, és adja meg az elsődleges a helyszínen van használatban, amikor a kiszolgáló tartomány vezérlő szerepkörbe átalakít egy tartomány nevét. Az **Active Directory-helyek és szolgáltatások** beépülő modul segítségével a webhely hivatkozásra objektumra, amelyhez a rendszer hozzáadja beállításainak konfigurálása. Beállítások konfigurálása a webhely hivatkozásra, megadhatja, hogy mikor replikáció két vagy több helyek között, és milyen gyakran. További információt talál [Az ütemezési helyek közötti replikáció](https://technet.microsoft.com/library/cc731862.aspx) .

###<a name="site-to-azure-protection"></a>Webhely-Azure-védelem

Kövesse az utasításokat követve [Hozzon létre egy tartományvezérlőnek az Azure virtuális hálózat](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Amikor a kiszolgáló tartomány vezérlő szerepkörbe átalakít adja meg az ugyanazt a elsődleges webhelyen használt tartománynevet.

Kattintson [a DNS-kiszolgáló virtuális hálózatok átkonfigurálása](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), a DNS-kiszolgáló használata Azure-ban.

![Azure hálózati](./media/site-recovery-active-directory/azure-network.png)

## <a name="test-failover-considerations"></a>Teszt feladatátvevő kapcsolatos szempontok

Teszt feladatátadás, amely magában, hogy nincs hatással van a termelési munkaterhelésekből elszigetelt gyártási hálózatról hálózatban.

Legtöbb alkalmazással is szükséges a tartományvezérlőnek és a DNS-kiszolgáló működik, a jelenléti, hogy az alkalmazás nem sikerült fölé, mielőtt a tartományvezérlőnek a elszigetelt hálózat tesztelése feladatátvevő használandó hozható létre kell. A művelet legegyszerűbben védelem a tartomány vezérlő/DNS virtuális gépen a webhely-helyreállítás engedélyezése és a virtuális gép próba feladatátvevő futtatható az alkalmazáshoz a helyreállítási terv próba feladatátvevő futtatása előtt. Az alábbiakban az ehhez szükséges lépéseket:

1. A tartomány vezérlő/DNS virtuális gép a webhely helyreállítási védelem bekapcsolása.
2. Hozzon létre egy elszigetelt hálózaton. Alapértelmezés szerint az Azure-ban létrehozott olyan virtuális hálózat legyen elkülönítve a más hálózatokon. Azt javasoljuk, hogy a hálózat az IP-címtartományokat egyeznie a vállalati hálózaton-e. Ne engedélyezze a hely közötti kapcsolatot a hálózaton.
3. Adja meg a DNS IP-címet, a hálózaton létre, a DNS virtuális gép megszerezni a várt IP-címét. Ha Ön éppen esetében, amelyek Azure, majd meg kell adnia az IP-cím a virtuális **Target IP** beállítással virtuális tulajdonságok feladatátvevő használt. Ha Ön éppen esetében, amelyek egy másik a helyszíni webhely és használ DHCP kövesse az utasításokat követve [DNS és DHCP próba feladatátvételi beállítása](site-recovery-failover.md#prepare-dhcp)

>[AZURE.NOTE] Teszt meghibásodáskor virtuális géphez kiosztott IP-címét az IP-cím érhető el a próba feladatátvevő hálózati esetén ugyanaz, mint azt volna tervezett vagy nem tervezett feladatátvételkor IP-címét. Ha nem, a virtuális gép kap egy másik IP-címet a próba feladatátvevő hálózaton elérhető.

4. A tartomány vezérlő virtuális gépen futtassa a belőle próba feladatátvevő a elszigetelt hálózaton. A tartomány vezérlő virtuális gép legújabb elérhető összes alkalmazás egységes helyreállítási pont használatával végezze el a tesztet feladatátvételt. 
5. Futtassa a alkalmazás helyreállítási terv próba feladatátvevő.
6. Tesztelés befejeződése után a próba feladatátvevő feladat tartomány vezérlő virtuális gép és a **feladatok** lapon a webhely helyreállítási portálon a helyreállítási terv "Kész" megjelölése

### <a name="dns-and-domain-controller-on-different-machines"></a>A különböző számítógépeken DNS és vezérlő

Ha ugyanazon a gépen virtuális a tartományvezérlőnek nem DNS kell hozzon létre egy DNS virtuális a próba feladatátvételi. Ha ezek a azonos virtuális, kihagyhatja ebben a szakaszban.

Egy friss DNS-kiszolgálót használ, és hozzon létre a szükséges zónák. Például ha az Active Directory-tartománya contoso.com, létrehozhat egy DNS-zóna a nevét a contoso.com a. A bejegyzéseket, az Active Directory megfelelő frissíteni kell a DNS-ben, az alábbi képlettel történik:

1. Győződjön meg róla, ezek a beállítások vannak még mielőtt más virtuális gép a helyreállítási tervben tervezés:

    - A zone erdő legfelső szintű neve neve kell legyen.
    - A zóna biztonsági fájl kell lennie.
    - A zone biztonságos és a nem biztonságos frissítések engedélyezve kell lennie.
    - A DNS-virtuális gép IP-címét a feloldó a tartomány vezérlő virtuális gép kell mutatnia.

2. Futtassa a következő parancsot a tartomány vezérlő virtuális gépen:

    `nltest /dsregdns`

3. A DNS-kiszolgálón zóna hozzáadása, a nem biztonságos frissítések engedélyezése és a szöveg beszúrása a hozzá tartozó DNS:

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1


## <a name="next-steps"></a>Következő lépések

Olvasás [milyen munkaterhelésekből is védelme?](../site-recovery/site-recovery-workload.md) Ha többet szeretne tudni az Azure-webhely helyreállításhoz vállalati munkaterhelésekből védelme.
