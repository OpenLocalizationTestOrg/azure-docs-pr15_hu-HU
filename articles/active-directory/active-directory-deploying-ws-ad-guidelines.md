<properties
   pageTitle="A Windows Server Active Directory Azure virtuális gépeken telepítési útmutatója |} Microsoft Azure"
   description="Ha tudja, hogyan Active Directory tartományi szolgáltatások és az Active Directory összevonási szolgáltatások helyszíni telepítését, megtudhatja, hogyan működnek a Azure virtuális gépeken futó."
   services="active-directory"
   documentationCenter=""
   authors="femila"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/27/2016"
   ms.author="femila"/>

# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Telepítési útmutatója a Windows Server Active Directory Azure virtuális gépeken

Ez a cikk ismerteti a központi telepítés Windows Server Active Directory tartományi szolgáltatások (AD DS) és az Active Directory összevonási szolgáltatások (AD FS) a helyszíni és üzembe helyezése őket a Microsoft Azure virtuális gépeken fontos eltérések.

## <a name="scope-and-audience"></a>Hatókör és a közönség

A cikk azok már ismerő üzembe helyezése a helyszíni Active Directory számára íródott. Bemutatja, hogy az Active Directory üzembe helyezése a Microsoft Azure virtuális gépeken futó/Azure virtuális hálózatok és a hagyományos a helyszíni Active Directory-telepítések közötti különbségeket. Azure virtuális gépeken futó és Azure virtuális hálózatok az infrastruktúra-mint-a-szolgáltatások (IaaS) azoknak a szervezeteknek kínáló kihasználhatja a felhőben számítógépes erőforrások tartoznak.

Azokat, amelyek nem ismeri a AD telepítés, az [Active Directory tartományi szolgáltatások telepítési útmutató](https://technet.microsoft.com/library/cc753963) vagy talál [az Active Directory összevonási szolgáltatások megtervezése](https://technet.microsoft.com/library/dn151324.aspx) szükség szerint.

Ez a cikk azt feltételezi, hogy az olvasót az alábbi fogalmak ismerős:

- A Windows Server AD DS telepítési és kezelése
- Telepítési és a Windows Server AD DS infrastruktúra támogatására DNS-konfigurációja
- A Windows Server Active Directory összevonási szolgáltatások telepítési és kezelése
- Telepítése, beállítása és kezelése a megbízó alkalmazások (webhelyekre és webes szolgáltatások) is igénybe vehet, a Windows Server Active Directory összevonási szolgáltatások tokenek
- Általános virtuális gép fogalmak megértéséhez, például egy virtuális gép, a virtuális lemez és a virtuális hálózatok konfigurálása

Ez a cikk kiemeli a hibrid telepítéshez, melyik Windows Server AD DS vagy az AD FS részben telepített a helyszíni és Azure virtuális gépeken részben rendszerbe követelményei. A dokumentum első fut, az Active Directory tartományi szolgáltatások és az Active Directory összevonási szolgáltatások Windows Server Azure virtuális gépeken és a helyszíni és a fontos döntéseket tervezése és üzembe befolyásoló kritikus eltérések terjed ki. A papír a többi útmutató minden részletesebben döntés pontra, és hogyan vonatkoznak a profilképek különböző telepítési forgatókönyvek ismerteti.

Ez a cikk nem tárgyalja a konfiguráció [Azure Active Directory](http://azure.microsoft.com/services/active-directory/), amely olyan, amelybe az Identitáskezelés és a vezérlő elérést felhő alkalmazások többi szolgáltatás. Azure Active Directory (Azure Active Directory) és a Windows Server Active Directory tartományi szolgáltatások, azonban készült használata a közös munkára az identitás- és az access-kezelési megoldás nyújtsanak informatikai az aktuális hibrid környezetek és alkalmazásokat modern. Segít megérteni a különbségeket és a Windows Server az Active Directory tartományi szolgáltatások és Azure Active Directory közötti kapcsolatokat, vegye figyelembe a következőket:

1. Előfordulhat, hogy futtatja a Windows Server AD DS a felhőben Azure virtuális gépeken való megjelenítésekor Azure ki szeretné terjeszteni a helyszíni adatközponthoz a felhőben.
2. Azure Active Directory adhat a felhasználók egyszeri bejelentkezés a szoftver-mint-a-(szoftver) szolgáltatásalkalmazások használható. A Microsoft Office 365-ben ez technológiát használ, például és Azure vagy más felhő platformon futó alkalmazások is használhatja.
3. Azure Active Directory (az Access vezérlő szolgáltatás), hogy a felhasználó, jelentkezzen be, hogy a felhőben, vagy a helyszíni telepített alkalmazásokat a Facebookról, a Google, a Microsoft vagy más identitásszolgáltatók identitások használható.

Ezeket a különbségeket kapcsolatos további tudnivalókért olvassa el az [Azure identitás](fundamentals-identity.md)című témakört.

## <a name="related-resources"></a>Kapcsolódó források

Előfordulhat, hogy töltse le és az [Azure virtuális gép készenléti értékelési](https://www.microsoft.com/download/details.aspx?id=40898)futtatása. A felmérés automatikusan vizsgálja meg a helyszíni környezet és a található ez a témakör segít a környezet áttelepítése az Azure útmutatásának megfelelően testre szabott jelentést.

Azt javasoljuk, hogy először is nézze át a oktatóanyagok, a segédvonalak és a videók terjed ki az alábbi témaköröket:

- [Állítsa be a felhőben virtuális hálózati az Azure-portálon](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
- [A webhely VPN konfigurálása az Azure-portálon](../vpn-gateway/vpn-gateway-site-to-site-create.md)
- [Egy új Active Directory-erdőt telepítse az Azure virtuális hálózaton](active-directory-new-forest-virtual-machine.md)
- [Azure egy replika az Active Directory tartományi vezérlő telepítése](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
- [Microsoft Azure informatikai szakembereknek IaaS: (01) virtuális gép alapjai](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
- [Microsoft Azure informatikai szakembereknek IaaS: (05) létrehozása virtuális hálózatok és idegen helyszíni kapcsolat](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>– Bevezetés

Azure virtuális gépeken üzembe helyezése a Windows Server Active Directory alapvető követelményei nagyon kevés különböznek az üzembe helyezése a helyszíni virtuális gépeken futó (és bizonyos mértékig fizikai gépek). Például ha a Windows Server az Active Directory tartományi szolgáltatások, ha egy meglévő kópiájába Azure virtuális gépeken rendszerbe tartomány vezérlők (DCs) helyszíni vállalati/tartományerdő, majd az Azure környezetben nagymértékben kezelhető ugyanúgy, mint bármilyen egyéb további Windows Server Active Directory-webhely kezelik előfordulhat, hogy. Ez azt jelenti, hogy alhálózat meg kell adni a Windows Server az Active Directory tartományi szolgáltatások, a létrehozott webhely, a alhálózat adott webhelyhez van kapcsolva, és egyéb megfelelő webhely-hivatkozások használata helyekhez való kapcsolódás. Vannak azonban eltérések, amelyek minden Azure telepítések közös, és néhány, amely szerint az adott telepítéshez változnak. Két alapvető különbség alatti mutatja:

### <a name="azure-virtual-machines-may-need-connectivity-to-the-on-premises-corporate-network"></a>Azure virtuális gépeken futó szükség lehet a helyszíni vállalati hálózati kapcsolat.

Azure virtuális hálózat, amely magában foglalja a webhely vagy a webhely-pont virtuális privát hálózathoz (VPN) összetevője zökkenőmentes kapcsolódhat az Azure virtuális gépeken futó, és a helyszíni gépek Azure virtuális gépeken futó vissza egy helyszíni vállalati hálózathoz csatlakozik szükséges. A virtuális Magánhálózati összetevő azt is lehetővé teszi a helyszíni tartomány számítógépei elérése a Windows Server Active Directory tartomány amelynek tartomány vezérlők kizárólag Azure virtuális gépeken futó van közzétéve. Fontos tudni, azonban, hogy a virtuális Magánhálózati sikertelen, ha hitelesítési és a Windows Server Active Directory függő egyéb műveleteket is sikertelen lesz. Előfordulhat, hogy a felhasználók tud meglévő a bejelentkezési hitelesítő adatait, mind a egyenrangú gyorsítótáras vagy, amelynek jegyek még nem adják vagy váltak elavult hitelesítési ügyfél-kiszolgáló kísérletek sikertelenek lesznek.

[Virtuális hálózati](http://azure.microsoft.com/documentation/services/virtual-network/) talál a bemutató videó és lépésenkénti oktatóprogramok, többek között [az Azure-portálon webhely VPN konfigurálása](../vpn-gateway/vpn-gateway-site-to-site-create.md)listáját.

> [AZURE.NOTE] Windows Server Active Directory-Azure virtuális hálózaton nincs telepítve a kapcsolatot egy helyszíni hálózaton is telepítheti. Ez a témakör útmutatását azonban feltételezik, hogy az Azure virtuális hálózati használatos, mert IP-címek a Windows Server alapvető funkcióját.

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Statikus IP-címek Azure PowerShell kell beállítania.

Dinamikus címek alapértelmezés szerint van rendelve, de rendelhet a Set-AzureStaticVNetIP parancsmagot a statikus IP-cím helyett. Amely akkor állítja be a statikus IP-címet, amely a szolgáltatás javító és a virtuális leállítása vagy újraindítása áll fenn. További tudnivalókért lásd: a [virtuális gépeken futó statikus belső IP-címet](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/).

## <a name="BKMK_Glossary"></a>Fogalmak és definíciók

Az alábbiakban megtalálja a különféle Azure technológiák, amely fog vonatkozni fog a jelen cikkben használt nem teljes listáját.

- **Azure virtuális gépeken futó**: A IaaS ajánlja fel, amely lehetővé teszi a felhasználóknak operációs rendszert futtató szinte bármilyen hagyományos VMs üzembe Azure-ban a helyszíni kiszolgálói terhelést.

- **Azure virtuális hálózati**: A közösségi hálózatokhoz, amely lehetővé teszi, hogy a felhasználók létrehozása és kezelése az Azure virtuális hálózatok és biztonságosan csatolása őket a saját Azure-ban a helyszíni hálózati infrastruktúrát virtuális magánhálózaton (VPN) használatával.

- **Virtuális IP-címe**: egy adott számítógépen vagy hálózati kártya nem kötött internetes IP-címet. Felhőszolgáltatások vannak rendelve egy virtuális IP-cím fogadására a rendszer átirányítja az Azure virtuális hálózati forgalmának engedélyezésére. Egy virtuális IP-címet az egy tulajdonság, amely tartalmazhat egy vagy több Azure virtuális gépeken futó felhőalapú-szolgáltatás. Tartsa szem előtt, hogy az Azure virtuális hálózati tartalmazhatnak egy vagy több felhőszolgáltatások. Virtuális IP-címek natív terheléselosztási funkciók nyújtanak.

- **Dinamikus IP-cím**: Ez az IP-címét, amely belső csak. Úgy kell beállítania a statikus IP-cím (parancsmaggal a Set-AzureStaticVNetIP) az az Adatközpont/DNS-kiszolgálói szerepkörök tároló VMs.

- **Szolgáltatás-javító**: A folyamatot, amelyben Azure automatikusan visszatér szolgáltatás fut ismét után azt észleli, hogy a szolgáltatás nem sikerült. Javító szolgáltatás, amely támogatja az elérhetőség és tűrőképessége Azure tulajdonságát egyike. Valószínű, miközben az eredmény a következő esemény javító egy Adatközpont egy virtuális fut a szolgáltatás hasonlóan, mint egy tervezett indítsa újra a rendszert, de van néhány-hatásai:

 - A virtuális hálózati kártya a virtuális változik.
 - A MAC-cím a virtuális hálózati kártya változik.
 - A virtuális processzor/Processzor azonosítója változik.
 - Az IP-beállítása a virtuális hálózati kártya nem változik, mindaddig, amíg a virtuális egy virtuális hálózathoz kapcsolódik, és a virtuális IP-cím statikus.

 Windows Server Active Directory nincs viselkedést érinti, mert nem azon a MAC-cím vagy processzor/Processzor azonosító függőséget tartalmaz, és az összes Windows Server Active Directory-telepítéshez Azure ajánlott a fent bemutatottak Azure virtuális hálózaton futtatandó.

## <a name="is-it-safe-to-virtualize-windows-server-active-directory-domain-controllers"></a>Az biztonságos különböző helyekre történő virtualizálása Windows Server Active Directory tartományi vezérlők?

Az azonos útmutatók egy virtuális gépen futó helyszíni DCs fizetnie telepítése a Windows Server az Active Directory DCs Azure virtuális gépeken van. A megbízható gyakorlat virtualizált DCs futó, mentésével és visszaállításával DCs szabályai betartják mindaddig, amíg ki. Korlátozások és a profilképek virtualizált DCs fut kapcsolatos további tudnivalókért olvassa el a [Tartomány-vezérlők futtatása a Hyper-V](https://technet.microsoft.com/library/dd363553)című témakört.

Hypervisors biztosítása vagy trivialize technológiát, amelyeket sok elosztott rendszerek, beleértve a Windows Server Active Directory-problémákat okozhat. Például fizikai-kiszolgálón lemezen klónozhatja, vagy nem támogatott módszerekkel visszaállítása a kiszolgáló, és így tovább segítségével SANs állapotát, de azt, amely a fizikai kiszolgálón sokkal nehezebb, mint a hipervizor a virtuális gép pillanatkép visszaállítása. Azure kínál funkciókat, amelyek eredményezhet ugyanezt a nemkívánatos feltételt. Például érdemes nem másolhat DCs virtuális fájlok rendszeres biztonsági mentés végrehajtása, mert visszaállításáról eredményezhet egy hasonló szeretne pillanatképet visszaállítása szolgáltatásainak használata helyett.

Az ilyen visszagörgetése USN buborékok közötti DCs véglegesen eltérő államok vezethet vezet be. Problémákat például okozhatják, amelyek:

- Objektumok eltávolításáról
- Várttól eltérően működik a jelszavak
- Nem következetes attribútum értékek
- Ha a séma fő visszaáll séma eltérések

Milyen hatással vannak az DCs kapcsolatos további tudnivalókért olvassa el a [USN és USN visszaállítása](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback)című témakört.

A Windows Server 2012, [további biztosítékokat Active Directory tartományi szolgáltatásokban kialakításának](https://technet.microsoft.com/library/hh831734.aspx)kezdve. Ezek az óvintézkedések védelme a fent említett problémákkal szemben virtualizált tartomány vezérlők mindaddig, amíg az alapul szolgáló hipervizor platform virtuális-GenerationID támogatja. Azure virtuális GenerationID, ami azt jelenti, hogy futtassa a Windows Server 2012 vagy később Azure virtuális gépeken futó tartomány vezérlők további biztosítékokat támogatja.

> [AZURE.NOTE] Állítsa le kell, és indítsa újra a helyett használja a **Leállítás** parancsot az Azure klasszikus portálon vendég operációs rendszerben futó a tartomány szerepkört Azure virtuális. Ma a klasszikus portál használatával állítsa le a virtuális hatására a virtuális kell felszabadítása a. Egy deallocated virtuális vannak az az előnye, hogy nem jelentkeztek díjak, de is visszaállítja a virtuális GenerationID, vagyis egy Adatközpont a nemkívánatos. Amikor a virtuális GenerationID alaphelyzetbe állítása is alaphelyzetbe állítása az Active Directory tartományi szolgáltatások adatbázis a invocationID, a rendszer elveti az RID készlet és SYSVOL van megjelölve nem mérvadó. További tudnivalókért lásd: az [Active Directory tartományi szolgáltatások (AD DS) virtualizációs – bevezetés](https://technet.microsoft.com/library/hh831734.aspx) és a [Biztonságos Virtualizing elosztott fájlrendszer replikációs szolgáltatása](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx).

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Miért telepítése a Windows Server AD DS a Azure virtuális gépeken futó?

Sok Windows Server AD DS telepítési forgatókönyvek jól alkalmasak telepítéshez, mint a Azure VMs. Tegyük fel például, távolról Ázsiában felhasználók hitelesítését igénylő Európában vállalata. A vállalat még nem telepítette a Windows Server az Active Directory vezérlői Ázsia miatt a költség telepítéshez használni őket, és a korlátozott szakértelmet a kiszolgálók utáni telepítésének kezelése. Emiatt Ázsia érkező hitelesítési kérések vannak által kiszolgált Europe vezérlői optimálisnál eredményekkel. Ebben az esetben egy Adatközpont egy virtuális megadott belül az Azure adatközponthoz Ázsiában kell futtatni a telepítheti. Adott Adatközpont csatol egy Azure virtuális hálózat, amelyhez közvetlenül csatlakozik a távoli hely hatékonyabbá tehetők a hitelesítési teljesítményt.

Azure akkor is jól illeszkedik, egyéb esetben költséges katasztrófa helyreállítási (DR) webhelyek helyettesítésére. Viszonylag alacsony – költségének tartomány vezérlők és a Azure virtuális egyetlen hálózat kisszámú szolgáltatója vonzó alternatív jelöli.

Végezetül érdemes Azure, például a SharePoint, Windows Server Active Directory számára, de még nem függőség a helyszíni hálózaton vagy a vállalati Windows Server Active Directory a hálózati alkalmazás telepítéséhez. Ebben az esetben egy elszigetelt erdő Azure alkalmazást a SharePoint üzembe kiszolgáló követelmények akkor optimális. Kapcsolat a helyszíni hálózaton és a vállalati Active Directory igénylő hálózati alkalmazások telepítése ismét is támogatott.

> [AZURE.NOTE] Mivel az egy réteg-3-as kapcsolat biztosít, a virtuális Magánhálózati összetevő-Azure virtuális hálózati és a helyszíni hálózaton közötti kapcsolatot biztosító is engedélyezheti a helyszíni értekezletállapota megjelenjen az Azure virtuális hálózaton Azure virtuális gépeken futó futó DCs futó tag kiszolgálókat. De ha a virtuális Magánhálózati nem érhető el, közötti kommunikációt a helyszíni számítógépek és Azure-alapú tartomány vezérlők nem fog működni, hitelesítési és más hibákat különféle az eredményül kapott.  

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Szemben között a Windows Server Active Directory tartományi vezérlők a Azure virtuális gépeken futó helyszíni és üzembe helyezése

- Bármely Windows Server Active Directory telepítési forgatókönyvben, amely tartalmazza a több, mint egy egyetlen virtuális IP-cím konzisztencia használni egy Azure virtuális hálózat szükség. Figyelje meg, hogy ez az útmutató feltételezi, hogy a DCs Azure virtuális hálózaton futnak.

- A helyszíni DCs, a statikus IP-címek használata ajánlott. Azure PowerShell használatával csak állítható be a statikus IP-címet. További információt talál [VMs statikus belső IP-címet](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) . Ha figyelése rendszerek vagy más megoldást, jelölje be a statikus IP-beállítása a vendégként való bekapcsolódáshoz operációs rendszerben, az azonos statikus IP-cím rendelhet a virtuális hálózati adapteren tulajdonságainak. De ügyeljen arra, hogy a hálózati kártya elvesznek, ha a virtuális megy a szolgáltatás javító keresztül, vagy a klasszikus portálon állítsa le, és címéhez felszabadítása van. Ebben az esetben a statikus IP-címet a vendégként való bekapcsolódáshoz belül kell alaphelyzetbe kell állítani.

- Üzembe helyezése a VMs egy virtuális hálózaton nem szemléltetnek (vagy megkövetelése) kapcsolatot vissza egy helyszíni hálózaton; a virtuális hálózat csupán lehetővé teszi, hogy a ezt a lehetőséget. Létre kell hoznia egy virtuális hálózati magánjellegű kommunikációját Azure és a helyszíni hálózaton. Kell telepíteni a virtuális Magánhálózati végpont a helyszíni hálózaton. A virtuális Magánhálózati nyitja meg az Azure a helyszíni hálózaton. További tudnivalókért lásd: a [Virtuális hálózati – áttekintés](../virtual-network/virtual-networks-overview.md) és [konfigurálása a webhely VPN az Azure-portálon](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [AZURE.NOTE] [Hozzon létre egy webhely-pont VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) szolgáló lehetőség egyes Windows-alapú számítógépekre közvetlenül csatlakozhat az Azure virtuális hálózat érhető el.

- Függetlenül attól, hogy egy virtuális létrehozása a hálózati vagy sem, kilépési forgalom, de nem bejövő adatok Azure díjai. Különböző Windows Server Active Directory-tervezés választási lehetőségek befolyásolhatják mennyi kilépési forgalom generálja a telepítést. Például üzembe helyezése a csak olvasható tartományvezérlőnek (írásvédett) korlátozza kilépési forgalom, mert bizonyos értéke nem kimenő. De ellen írási hajtanak végre azokat az az Adatközpont és az alkalmazások és szolgáltatások a webhelyen, hogy írásvédett tartományvezérlőre [kompatibilitási](https://technet.microsoft.com/library/cc755190) kell mérni a döntési telepítéshez használni egy írásvédett van szüksége. Forgalom díjak kapcsolatos további tudnivalókért olvassa el az [Azure árak,-a-adatábrázolás](http://azure.microsoft.com/pricing/)című témakört.

- Miközben be van teljes szabályozható, mely kiszolgálón keresztül VMs a helyszíni RAM, például a használni kívánt erőforrások méret lemezre jelölőnégyzetből, és így tovább, a Azure ki kell választania a előre beállított kiszolgálói méretű listájából. Egy az Adatközpont adatok lemezen van szükség az operációs rendszer lemezen kívül ahhoz, hogy a Windows Server Active Directory-adatbázis tárolni.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Telepítheti Windows Server Active Directory összevonási szolgáltatások Azure virtuális gépeken?

Igen, telepítheti a Windows Server Active Directory összevonási szolgáltatások Azure virtuális gépeken, és a [Gyakorlati tanácsok az AD FS-telepítéshez](https://technet.microsoft.com/library/dn151324.aspx) helyszíni egyaránt alkalmazása a Azure Active Directory összevonási szolgáltatások telepítési. De néhány gyakorlati tanácsot, mint például az terheléselosztás, és magas elérhetősége megkövetelése technológiák túl mi Active Directory összevonási szolgáltatások kínál magát. Az alapul szolgáló infrastruktúra kell ellátni. Vegyük tekintse át az egyes e ajánlott eljárások, és tanulmányozza a hogyan azokat úgy lehet elérni az Azure VMs és az Azure virtuális hálózat.

1. **Soha ne fedheti fel a biztonsági jogkivonat-szolgáltatás kiszolgálók közvetlenül az interneten.**

    A fontos, mert a STS problémák biztonsági tokenek. Azonos szintű védelmet a tartományvezérlőnek a eredményt adja, például az Active Directory összevonási szolgáltatások kiszolgálók STS-kiszolgálók kell kezelni. Ha egy STS sérül, rosszindulatú felhasználók képesek vélhetően tartalmazó igények bejelentésére kiválasztása megbízó alkalmazásait és más szervezetek meghatalmazó STS-kiszolgálók hozzáférési jogkivonat kibocsátása.

2. **Telepítse az Active Directory tartományi vezérlők az összes felhasználó tartományok ugyanabba a hálózatba, mint az AD FS-kiszolgálók.**

    Az AD FS-kiszolgálók Active Directory tartományi szolgáltatások segítségével a felhasználók hitelesítő. A tartomány vezérlői ugyanabba a hálózatba, mint az AD FS-kiszolgálók üzembe ajánlott. Ez a üzemkészségének biztosít, abban az esetben az Azure és a helyi hálózat közötti kapcsolat megszakad, és lehetővé teszi a alsó időtartama és bejelentkezések nagyobb teljesítmény.

3. **Telepítse a magas rendelkezésre állásának és a betöltés terheléselosztás több Active Directory összevonási szolgáltatások csomópontot.**

    A legtöbb esetben a hibát AD FS lehetővé teszi, hogy az alkalmazás nem fogadható el, mert az alkalmazások biztonsági tokenek mérete gyakran igénylő akkreditált képviseletének kritikus. Emiatt és AD FS most alapvető fontosságú alkalmazások férhet hozzá a kritikus út helyezkedik el, mert az AD FS-szolgáltatás erősen elérhető több AD FS-proxy és AD FS-kiszolgálón keresztül kell lennie. Terjesztési kérelmek eléréséhez terheléselosztókkal általában telepítik az AD FS-proxy, mind az AD FS-kiszolgálók elé.

4. **Telepítse az internetkapcsolat egy vagy több webes szolgáltatásalkalmazás-Proxy csomópontot.**

    Amikor a felhasználók kell védik az AD FS-szolgáltatási alkalmazást, az Active Directory összevonási szolgáltatások szolgáltatás kell érhető el az internetről. Ez a webes alkalmazás Proxy szolgáltatás üzembe helyezésével érhető el. Egynél több csomópont telepíthető magas elérhetősége céljából, és terheléselosztás erősen ajánlott.

5. **Korlátozhatja a hozzáférést a webes alkalmazás Proxy csomópontokat a belső hálózati erőforrásokat.**

    Ahhoz, hogy a külső felhasználókat, hogy az Active Directory összevonási szolgáltatások elérése az internetről, is telepíteni kell webes szolgáltatásalkalmazás-Proxy csomópontok (vagy a Windows Server korábbi verzióiban AD FS-Proxy). A webes alkalmazás proxy csomópontokat is közvetlenül csak akkor érhető el az internethez. Nem kell lennie a tartományhoz tartozó és azok csak hozzáférésre van szükségük az AD FS-kiszolgálók TCP-portok 443-as és 80 fölé. Javasolt, hogy más számítógépek (különösen tartomány vezérlők) közötti kommunikáció zárolva van.

    Ez a szokásos elért helyszíni egy DMZ segítségével. Tűzfalak whitelist üzemmódot segítségével korlátozása irányítja át a DMZ a helyszíni hálózaton (Ez azt jelenti, hogy csak a megadott IP-címek és megadott portokon keresztül forgalom engedélyezett, és minden más forgalom le van tiltva).

Az alábbi ábra mutatja, a hagyományos a helyszíni Active Directory összevonási szolgáltatások telepítési.

![A hagyományos helyszíni Active Directory összevonási szolgáltatások példányban ábrája](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Azonban Azure ad natív, mert a teljes szolgáltatáskészletet nyújtó tűzfal lehetőséget, egyéb lehetőségek kell forgalmának korlátozása használható. A következő táblázat mutatja az egyes beállítások és a előnyei és hátrányai.

| A beállítás | Előnye | Hátránya |
| ------ | --------- | ------------ |
| [Azure hálózati hozzáférés-vezérlési listák](virtual-networks-acl.md) | Kevésbé költséges és egyszerűbb kiindulási konfiguráció | További hálózati vezérlés konfiguráció szükséges, ha minden új VMs kerülnek a példányban |
| [Barracuda NG tűzfal](https://www.barracuda.com/products/ngfirewall) | Whitelist mód műveletet, és nem szükséges hálózati vezérlés beállítás | Nagyobb költség és összetettsége kezdeti telepítés |

A magas szintű lépéseket az AD FS üzembe ebben az esetben a következők:

1. Hozzon létre egy virtuális hálózati határokon helyszíni kapcsolat, a virtuális Magánhálózati vagy a [készült ExpressRoute](http://azure.microsoft.com/services/expressroute/).

2. A virtuális hálózaton tartomány vezérlők üzembe. Ez a lépés nem kötelező, de javasolt.

3. Telepítse az AD FS-kiszolgálók a tartományhoz a virtuális hálózaton.

4. Hozzon létre egy [belső terheléselosztás beállítása](http://azure.microsoft.com/blog/internal-load-balancing/) , amely tartalmazza az AD FS-kiszolgálók, és a virtuális hálózaton (dinamikus IP-cím) belül új személyes IP-címet használja.

  1. A teljesen minősített tartománynév, mutasson a saját (dinamikus) IP-címére a belső terheléselosztása készlet létrehozásához a DNS frissítése.

5. Hozzon létre egy felhőalapú szolgáltatásba (vagy egy különálló virtuális hálózaton) a webes alkalmazás Proxy csomópontok.

6. A webes alkalmazás Proxy csomópontok a felhőalapú szolgáltatásba vagy egy virtuális hálózati terjesztése

  1. Hozzon létre egy külső terheléselosztása, amely tartalmazza a webes alkalmazás Proxy csomópontok.

  2. Frissítse a külső tartománynév-re mutasson a felhőalapú szolgáltatás nyilvános IP-címet (a virtuális IP-cím) (FQDN).

  3. Állítsa be az Active Directory összevonási szolgáltatások proxyk, amely megfelel a belső terheléselosztása az AD FS-kiszolgálók a teljesen minősített tartománynév használatára.

  4. A külső teljesen minősített tartománynév használható a követelések szolgáltató jogcímalapú webhelyek frissítése

7. Korlátozhatja a hozzáférést az Active Directory összevonási szolgáltatások virtuális hálózat gépi webes alkalmazás Proxy között.

Szeretné korlátozni a forgalmat, a terheléselosztás csoport a Azure belső terheléselosztó kell konfigurálni kell TCP-portok 80 és 443-as csak forgalmat, és minden más forgalom terheléselosztása megadása belső dinamikus IP-címére megszakad.

![ADFS hálózati hozzáférés-vezérlési listák TCP 443-as + 80 ábrája engedélyezett.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

Az AD FS-kiszolgáló-alapú forgalmat a csak a következő forrásokból által megengedett lenne:

- Az Azure belső terheléselosztó.
- Az IP-címe a rendszergazda a helyszíni hálózaton.

> [AZURE.WARNING] A tervezés kell megakadályozni, hogy webes szolgáltatásalkalmazás-Proxy csomópontok elérje a Azure virtuális hálózaton bármely más VMs vagy minden olyan helyre, a helyszíni hálózaton. Hogy a helyszíni készülék Express útvonal kapcsolatok vagy a webhely virtuális Magánhálózati kapcsolat VPN-eszköz tűzfalszabályokat konfigurálásával elvégezhető.

Ez a lehetőség hátránya a hálózat hozzáférés-vezérlési listák beállítása több eszközről, többek között belső terheléselosztó, a az AD FS-kiszolgáló és a rendszer hozzáadja a virtuális hálózat többi kiszolgáló kell. Ha bármilyen eszközről hálózati hozzáférés-vezérlési listák forgalmának korlátozása beállítása nélkül bekerül a telepítést, a teljes telepítést veszélyt jelent is. Ha minden eddiginél módosítása a webes alkalmazás Proxy csomópontok IP-címét, a hálózati hozzáférés-vezérlési listák alaphelyzetbe kell állítani (Ez azt jelenti, hogy a proxyk kell beállítania, hogy [statikus dinamikus IP-](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)címeket).

![A hozzáférés-vezérlési LISTÁK hálózattal Azure ADFS.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Egy másik, hogy a [Barracuda NG tűzfal](https://www.barracuda.com/products/ngfirewall) készülék használatával szabályozhatja az Active Directory összevonási szolgáltatások proxy és az AD FS-kiszolgálók között forgalmat. Ez a beállítás megfeleljenek a gyakorlati tanácsok a biztonságát és hozzáférhetőségét magas, és kevesebb felügyelete a kezdeti beállítás után igényel, mivel a Barracuda NG tűzfal készülék tartalmaz tűzfal felügyeleti whitelist módban, illetve közvetlenül az Azure virtuális hálózaton telepíthető. Amely szükségtelenné konfigurálása hálózati hozzáférés-vezérlési listák bármikor új kiszolgáló bekerül a telepítést. De ez a beállítás hozzáadja a kezdeti telepítés összetettsége és a költség.

Ebben az esetben két virtuális hálózatok telepítik egy helyett. VNet1 és VNet2 fogja hívása őket. VNet1 tartalmaz a proxyk lehetőséget, és VNet2 a STSs és a hálózati kapcsolat vissza a vállalati hálózathoz az tartalmazza. VNet1 vagyis fizikailag (jóllehet gyakorlatilag) elszigetelt VNet2 és, viszont a vállalati hálózathoz. VNet1 kapcsolódik VNet2 technológiával speciális közötti ismert, átviteli független hálózat architektúrája (BALÁZS). A BALÁZS alagutas csatolva van minden egyes Barracuda NG tűzfal használatáról virtuális hálózathoz – egy Barracuda minden virtuális hálózathoz.  Max-elérhetőségét célszerű, hogy minden virtuális hálózaton; két kígyókról rendszerbe egyetlen aktív, a többi passzív. Rendkívül gazdag firewalling funkciók, amelyek lehetővé teszik az Azure-ban egy hagyományos helyszíni DMZ működésének imitálása us kínálnak.

![A tűzfallal Azure ADFS.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

További tudnivalókért lásd: [Active Directory összevonási szolgáltatások: előtér-alkalmazás követelések szem előtt a helyszíni és az Internet kiterjesztése](#BKMK_CloudOnlyFed).

### <a name="an-alternative-to-ad-fs-deployment-if-the-goal-is-office-365-sso-alone"></a>Ha a cél, az Office 365-ÖS egyedül AD FS-példányhoz alternatív

Nincs a másik lehetőség, üzembe AD FS teljes, ha csak az Office 365-ben való bejelentkezés engedélyezése. Ebben az esetben is egyszerűen üzembe helyezéséhez DirSync jelszó szinkronizálása a helyszíni és az azonos végeredmény elérése a minimális telepítési összetettsége, mivel ezt a megközelítést nincs szükség az AD FS vagy Azure.

Az alábbi táblázat hasonlítja össze a bejelentkezési folyamat működéséről és AD FS telepítése nélkül.

| Az Office 365 egyszeri bejelentkezéses az Active Directory összevonási szolgáltatások és a Dirsync eszköz | Az Office 365 ugyanaz bejelentkezéses DirSync + jelszó-szinkronizálás használata |
| ------------- | ------------- |
| 1 a felhasználó bejelentkezik a vállalati hálózathoz, és ahhoz, hogy a Windows Server Active Directory. | 1 a felhasználó bejelentkezik a vállalati hálózathoz, és ahhoz, hogy a Windows Server Active Directory. |
| 2. a felhasználó megkísérel hozzáférni az Office 365-ben (vagyok @contoso.com). | 2. a felhasználó megkísérel hozzáférni az Office 365-ben (vagyok @contoso.com). |
| 3. az office 365 átirányítja a felhasználó Azure AD. | 3. az office 365 átirányítja a felhasználó Azure AD. |
| 4. mivel az Azure Active Directory nem csatlakozó felhasználók hitelesítését, és nincs megbízhatósági kapcsolat a helyszíni Active Directory összevonási szolgáltatások értelmez, átirányítja a felhasználó Active Directory összevonási szolgáltatások. | 4. a azure Active Directory közvetlenül nem lehet elfogadni Kerberos jegyeket, és nem megbízható kapcsolat áll fenn, úgy, hogy a felhasználó adja meg a hitelesítő adatokat kér. |
| 5 a felhasználó Kerberos-jegy küld az Active Directory összevonási szolgáltatások STS. | 5 a felhasználó beírja a helyszíni ugyanazt a jelszót, és Azure Active Directory ellenőrizte őket a felhasználónév és jelszó DirSync által szinkronizált szemben. |
| 6. AD FS átalakítások a szükséges jogkivonat formátum és követelések Kerberos-jegy és Azure Active Directory irányítja át a felhasználó. | 6. azure Active Directory átirányítja a felhasználót az Office 365-be. |
| 7. Azure Active Directory hitelesíti a felhasználó (egy másik átalakítása esetén). |  7. a felhasználó Office 365-ben és az Outlook Web App használatával az Azure Active Directory jogkivonathoz jelentkezhetnek be. |
| 8. azure Active Directory átirányítja a felhasználót az Office 365-be. |  |
| 9. a felhasználó lehetősége van jelentkezett be az Office 365-be. |  |

Az Office 365-ben a Dirsync használatakor és jelszó-szinkronizálás forgatókönyv (nem AD FS) az egyszeri bejelentkezés lecseréli az "ugyanaz bejelentkezéses" Ha "azonos" egyszerűen azt jelenti, hogy felhasználók újra meg kell adnia a ugyanazt a helyszíni hitelesítő adatait az Office 365 elérésekor. Figyelje meg, hogy ezeket az adatokat is-jegyzi a felhasználó böngészőjében csökkentésére a következő képernyőn megjelenő utasításokat.

### <a name="additional-food-for-thought"></a>További élelmiszer vonatkozó gondolkodási

- Ha egy AD FS-proxy Azure virtuális-gépen telepít, az AD FS-kiszolgálók kapcsolatot van szükség. Ebben az esetben a helyszíni javasoljuk, hogy a webhely virtuális Magánhálózati kapcsolatot a webes alkalmazás Proxy csomópontok a AD FS-kiszolgálók kommunikáció engedélyezése a virtuális hálózat által biztosított kihasználhatja.

- Ha telepíti az Active Directory összevonási szolgáltatások server Azure virtuális-gépen, Windows Server Active Directory tartományi vezérlők, attribútum tárolók és konfigurációs adatbázisok kapcsolata szükség, és kérheti egy készült ExpressRoute vagy a webhely virtuális Magánhálózati kapcsolat között az Azure virtuális hálózati és a helyszíni hálózaton.

- Minden forgalom díjak alkalmazza a program az Azure virtuális gépeken futó (kilépési forgalom). Ha a költség a vezetői tényező, tanácsos a webes alkalmazás Proxy csomópontok Azure, az Active Directory összevonási szolgáltatások kiszolgálók helyszíni elhagyása telepítése. Ha az AD FS-kiszolgálók Azure virtuális gépeken is van telepítve, akkor többletköltségek hitelesítést végezni a helyszíni felhasználóknak merülnek fel. Kilépési forgalmat a költség-e az áthaladó meg a készült ExpressRoute vagy a webhely virtuális magánhálózat függetlenül vonz.

- Ha úgy dönt, hogy az Azure a belső kiszolgáló terheléselosztási magas AD FS-kiszolgáló elérhetőségének lehetőségeit használni, vegye figyelembe, hogy terheléselosztás biztosít, amelyek alapján határozza meg a belül a felhőalapú szolgáltatást a virtuális gépeken futó állapotának szondákat. Azure virtuális gépeken futó (nem pedig az interneten vagy dolgozó szerepkörök), amíg egy egyéni vizsgálati kell használni, mivel a agent, az alapértelmezett szondákat válaszoló nem szerepel a Azure virtuális gépeken. Az egyszerűség kedvéért használhat fel egy egyéni TCP-vizsgálati – ehhez csak a TCP-kapcsolat (TCP SYN szegmens küldött és a SYN-ACK TCP szegmens válaszolt) sikeresen létrejött a virtuális gép állapotáról. Úgy is beállíthatja az egyéni vizsgálati minden olyan portot, amelyhez a virtuális gépeken futó aktívan figyeli használni.

> [AZURE.NOTE] Ugyanazok a portok közvetlenül az interneten (például a 80 és 443-as port) nézetéhez kell gépek nem oszthat meg ugyanazt a felhőalapú szolgáltatást. Ezért ajánlott dedikált felhő létrehozott potenciális elkerülése érdekében a Windows Server AD FS-kiszolgálók szolgáltatás átfedi portokon az alkalmazás és a Windows Server Active Directory összevonási szolgáltatások között.

## <a name="deployment-scenarios"></a>Telepítési esetek

A következő rész ismertet alkotómunkájának telepítési eseteket figyelembe kell venni fontos dolgot figyelemfelkeltőbbé. Minden esetben többet szeretne tudni a döntéseket és tényezők szempontok mutató hivatkozásokat tartalmaz.

1. [Active Directory tartományi szolgáltatások: A nem vállalati hálózati kapcsolat követelmény az Active Directory tartományi szolgáltatások tartsa szem előtt alkalmazások telepítése](#BKMK_CloudOnly)

    Ha például egy internetes SharePoint szolgáltatás telepítve van Azure virtuális-gépen. Az alkalmazás nem függőséget tartalmaz, a vállalati hálózathoz erőforrásokat. Az alkalmazás csak a Windows Server az Active Directory tartományi szolgáltatások, de nem szükséges a vállalati Windows Server Active Directory tartományi szolgáltatások.

2. [Active Directory összevonási szolgáltatások: Előtér-alkalmazás követelések szem előtt a helyszíni és az Internet bővítése](#BKMK_CloudOnlyFed)

    Például egy követelések-et alkalmazást sikeresen telepített a helyszíni és vállalati felhasználók által használt kell az internetről elérhetők lesznek. Az alkalmazás kell közvetlenül az interneten keresztül érhető el, használja a saját vállalati identitások üzleti partnereivel mind a meglévő vállalati felhasználóknak.

3. [Active Directory tartományi szolgáltatások: A Windows Server AD DS-et a vállalati hálózathoz kapcsolódási igénylő alkalmazás telepítése](#BKMK_HybridExt)

    Ha például egy LDAP-et alkalmazása, amely támogatja a Windows-hitelesítést, és használja a Windows Server AD DS összegyűjti konfigurálása és a felhasználóiprofil adatok telepítve van az Azure virtuális gépen. Célszerű az alkalmazás a meglévő vállalati Windows Server AD DS kihasználhatja, és adja meg az egyszeri bejelentkezés. Az alkalmazás nem követelések tudomása áll.

### <a name="BKMK_CloudOnly"></a>1. a Active Directory tartományi szolgáltatások: A nem vállalati hálózati kapcsolat követelmény az Active Directory tartományi szolgáltatások tartsa szem előtt alkalmazások telepítése

![Felhőben Active Directory tartományi szolgáltatások telepítési](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**ábra 1**

#### <a name="description"></a>Leírás

SharePoint-Azure virtuális gépen telepíti, és az alkalmazás nem a függőséget tartalmaz, a vállalati hálózathoz erőforrásokat. Az alkalmazás megkövetelése a Windows Server AD DS, de *nem* szükséges a vállalati Windows Server AD DS. Kerberos vagy nem szövetséges meghatalmazások szükség, mert a felhasználók is önálló kiépített az alkalmazást a Windows Server Active Directory tartományi szolgáltatások is Azure virtuális gépeken a felhőben tárolt keresztül.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Forgatókönyv szempontot, és hogyan technológia területek vonatkozik-e az alkalmazási példát

- [Hálózati topológiája](#BKMK_NetworkTopology): hozzon létre egy Azure virtuális hálózati (más néven hely közötti kapcsolat) határokon helyszíni kapcsolat nélkül.

- [Adatközpont telepítési beállítása](#BKMK_DeploymentConfig): egy új tartományvezérlőnek üzembe egy új, egyetlen tartományból, Windows Server Active Directory erdőbe. Ez a Windows DNS-kiszolgáló együtt kell telepítését.

- [Windows Server Active Directory-webhely topológia](#BKMK_ADSiteTopology): használja az alapértelmezett Windows Server Active Directory-webhely (összes számítógépre történő lesz alapértelmezett-első-webhely neve).

- [IP-címek és a DNS](#BKMK_IPAddressDNS):

 - A statikus IP-cím megadása az Adatközpont a Set-AzureStaticVNetIP Azure PowerShell-parancsmag használatával.
 - Telepítse és a tartomány vezérlő Azure a Windows Server DNS konfigurálása.
 - Virtuális hálózati tulajdonságainak beállítása a név és az Adatközpont és a DNS-kiszolgálói szerepkörök üzemeltető a virtuális IP-címét.

- [Globális katalógus](#BKMK_GC): az első Adatközpont erdőben globális katalógus kiszolgálónak kell lennie. További DCs kell is be kell állítani globális katalógusok mivel egyetlen tartományerdő a globális katalógus nem szükséges az az Adatközpont további munkáját.

- [A Windows Server AD DS adatbázis és SYSVOL elhelyezésének](#BKMK_PlaceDB): adatok lemezen hozzáadása DCs futtatott Azure VMs ahhoz, hogy a Windows Server Active Directory-adatbázist, naplók és SYSVOL tárolni.

- [Biztonsági mentési és visszaállítási](#BKMK_BUR): határozza meg, hogy hol tárolja a rendszerállapot biztonsági mentése. Ha szükséges, egy másik adatok lemezre hozzáadása a Adatközpont virtuális biztonsági másolatok tárolására szolgáló.

### <a name="BKMK_CloudOnlyFed"></a>2 az AD FS: előtér-alkalmazás követelések szem előtt a helyszíni és az Internet bővítése

![Idegen helyszíni kapcsolatot az összevonási](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**ábra 2**

#### <a name="description"></a>Leírás

Egy követelések szem előtt sikeresen telepített a helyszíni és vállalati felhasználók által használt alkalmazásnak az internetről közvetlenül elérhető lesz. Az alkalmazás egy webhelyen frontend, amelyben adatokat tárolja az SQL-adatbázishoz szolgál. Az alkalmazás által használt SQL-kiszolgálók is találhatók a vállalati hálózathoz. Két Windows Server Active Directory összevonási szolgáltatások STSs és egy terheléselosztó lett telepítve a helyszíni elérését a vállalati felhasználóknak. Az alkalmazás letöltése kell továbbá közvetlenül az interneten keresztül érhető el saját vállalati identitások használatáról üzleti partnereivel mind a meglévő vállalati felhasználóknak.

Annak érdekében, hogy egyszerűsítése, és ez az új követelmény telepítésének és konfigurációs igényeknek, azt az határozza meg, hogy két további webes frontends és két Azure virtuális gépeken futó telepíteni kell a Windows Server Active Directory összevonási szolgáltatások proxykiszolgálóiban. Az összes négy VMs fog kell kitenni közvetlenül az interneten, és nyújtanak a kapcsolat a helyszíni hálózaton Azure virtuális hálózati hely közötti VPN lehetőség használatával.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Forgatókönyv szempontot, és hogyan technológia területek vonatkozik-e az alkalmazási példát

- [Hálózati topológiája](#BKMK_NetworkTopology): Azure virtuális hálózati és [idegen helyszíni kapcsolatok konfigurálása](../vpn-gateway/vpn-gateway-site-to-site-create.md)hozhat létre.

 > [AZURE.NOTE] Az egyes a Windows Server AD FS-tanúsítványok győződjön meg arról, hogy az URL-címet a tanúsítvány sablon és az eredményül kapott tanúsítványok belül definiált szerint a Windows Server AD FS-példányok Azure futó érhető el. Ez a nyilvános kulcsú infrastruktúra infrastruktúra részeinek határokon helyszíni kapcsolódási megkövetelheti. A példában ha a CRL-végpontot LDAP-alapú és a kizárólag a helyszíni majd határokon helyszíni kapcsolódási frissítenie kell. Ha ez nem célszerű, amelynek CRL érhető el az interneten keresztül hitelesítésszolgáltató által kibocsátott tanúsítványok használatához szükséges lehet.

- [Cloud services konfigurálása](#BKMK_CloudSvcConfig): Győződjön meg róla, két felhőbeli szolgáltatásokkal sorrendben adja meg a két terheléselosztás – ezek olyan virtuális IP-címet. Az első felhőalapú szolgáltatás virtuális IP-címet a két Windows Server Active Directory összevonási szolgáltatások proxy VMs a 80-as és 443-as irányítja át. A Windows Server AD FS-proxy VMs mutasson a helyszíni terheléselosztó IP-címét, hogy a Windows Server Active Directory összevonási szolgáltatások STSs előlapot kell beállítani. A második felhőbeli szolgáltatástól virtuális IP-cím irányítja át azokat az operációs rendszert futtató a webes frontend ismét a 80-as és 443-as két VMs. Állítsa be az egyéni vizsgálati, annak érdekében, hogy a terheléselosztó csak irányítja a forgalmat működik Windows Server AD FS-proxy és webes frontend VMs.

- [Összevonási kiszolgáló konfigurálása](#BKMK_FedSrvConfig): Windows Server konfigurálása az Active Directory összevonási szolgáltatások összevonási kiszolgálók (STS) a Windows Server Active Directory erdő a felhőben létrehozott biztonsági tokenek létrehozásához. Összevonás követelések szolgáltató meghatalmazotti fogadja el a identitások szeretne más partnerekkel, és konfigurálása megbízó fél meghatalmazotti a tokenek létrehozásához használni kívánt különböző alkalmazásokkal.

    A legtöbb esetben a Windows Server Active Directory összevonási szolgáltatások proxykiszolgálók van telepítve biztonsági okokból internetes minőségben közben a Windows Server Active Directory összevonási szolgáltatások összevonási mint közvetlen internetkapcsolat elkülönítve marad. Telepítési igényektől függetlenül meg kell adnia a felhőalapú szolgáltatás egy virtuális IP-címet, amely a nyilvánosan elérhető IP-címet, és olyan portot, amely képes terheléselosztási – több a két Windows Server AD FS STS-példányok és a proxy-példányok nyújt.

- [A Windows Server Active Directory összevonási szolgáltatások elérhetősége magas konfigurációs](#BKMK_ADFSHighAvail): tanácsos telepítése a Windows Server Active Directory összevonási szolgáltatások farm feladatátvételi legalább két kiszolgálókkal és a terheléselosztás. Előfordulhat, hogy a érdemes használni a Windows belső adatbázis (WID) a Windows Server AD FS-konfigurációs adatok, és a belső betöltés Azure terheléselosztó szolgáltatásának használata a farm kiszolgálóin található a osztania a bejövő felkérést.

További tudnivalókért lásd: az [Active Directory tartományi szolgáltatások telepítési útmutatóját](https://technet.microsoft.com/library/cc753963).


### <a name="BKMK_HybridExt"></a>3. a Active Directory tartományi szolgáltatások: A Windows Server AD DS-et a vállalati hálózathoz kapcsolódási igénylő alkalmazás telepítése

![Idegen helyszíni Active Directory tartományi szolgáltatások példányhoz](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**ábra 3**

#### <a name="description"></a>Leírás

LDAP-et alkalmazás telepítve van az Azure virtuális gépen. Támogatja a Windows-hitelesítést és a Windows Server az Active Directory tartományi szolgáltatások konfigurálása és a felhasználói profiladatok összegyűjti használja. A cél, az alkalmazás kihasználhatja a Windows Server meglévő vállalati AD DS, és adja meg az egyszeri bejelentkezés. Az alkalmazás nem követelések tudomása áll. Felhasználók kell is elérhetik az alkalmazást közvetlenül az internetről. Optimalizálhatja a teljesítmény és a költség, dönt, hogy a két további tartományt vezérlők a vállalati tartomány részét képező telepíthető-e összegzik az Azure alkalmazást.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Forgatókönyv szempontot, és hogyan technológia területek vonatkozik-e az alkalmazási példát

- [Hálózati topológiája](#BKMK_NetworkTopology): hozzon létre egy Azure virtuális hálózati [határokon helyszíni kapcsolatot](../vpn-gateway/vpn-gateway-site-to-site-create.md).

- [Telepítési mód](#BKMK_InstallMethod): a vállalati Windows Server Active Directory tartományi replika DCs telepítése. Kópia Adatközpont Windows Server AD DS telepítése a virtuális, és tetszés szerint a telepítse a Adathordozóról funkcióval Rövidítse le kell az új Adatközpont való telepítésekor replikált kell, hogy adatokat. Oktatóanyagot olvassa el a [egy replika az Active Directory tartományi vezérlő Azure telepítése](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)című témakört. Még ha adathordozóról, a virtuális Adatközpont helyszíni létre, és a teljes virtuális merevlemezt (virtuális Merevlemezt) áthelyezés a felhőbe, nem pedig a Windows Server AD DS a telepítés során hatékonyabb lehet. Biztonsági javasoljuk, hogy törli a virtuális a helyszíni hálózaton után Azure másolták.

- [A Windows Server az Active Directory-webhely topológiájának](#BKMK_ADSiteTopology): Azure új webhely létrehozása az Active Directory-helyek és szolgáltatások. Hozzon létre egy Windows Server Active Directory-alhálózat objektum jelenítik meg az Azure virtuális hálózati és az alhálózathoz felvétele a webhelyre. Hozzon létre egy új webhely hivatkozásra, amely tartalmazza az új Azure webhely és a-webhelyre, ahol az Azure virtuális hálózati VPN végpont szabályozhatja, és a Windows Server Active Directory forgalom az Azure optimalizálása.

- [IP-címek és a DNS](#BKMK_IPAddressDNS):

 - A statikus IP-cím megadása az Adatközpont a Set-AzureStaticVNetIP Azure PowerShell-parancsmag használatával.
 - Telepítse és a tartomány vezérlő Azure a Windows Server DNS konfigurálása.
 - Virtuális hálózati tulajdonságainak beállítása a név és az Adatközpont és a DNS-kiszolgálói szerepkörök üzemeltető a virtuális IP-címét.

- [GEO meghatározva DCs](#BKMK_DistributedDCs): további virtuális hálózatok konfigurálását, szükség szerint. Ha az Active Directory-webhely topológiát, amelyek megfelelnek különböző Azure régiókban, mint a létre szeretne hozni az Active Directory-helyek ennek megfelelően geographies vezérlői szükséges.

- [Csak olvasható DCs](#BKMK_RODC): telepít, előfordulhat, hogy egy írásvédett az Azure webhelyen, a elvégzésére vonatkozó követelmények függően írja be az Adatközpont elleni művelet és az alkalmazások és szolgáltatások a kompatibilitás írásvédett tartományvezérlőre a webhelyet. Alkalmazás kompatibilitással kapcsolatos további tudnivalókért lásd: a [csak olvasható tartomány vezérlők alkalmazás kompatibilitási útmutatójában](https://technet.microsoft.com/library/cc755190).

- [Globális katalógus](#BKMK_GC): globális katalógusok szükséges bejelentkezési szolgáltatáskérések erdőkben távolítja el. Ne telepítse az Azure webhelyen globális Katalógus, mint hitelesítési kérések lekérdezések globális katalógusok okozó más helyek merülnek fel fog kilépési forgalom költségeket. A forgalom minimalizálásához engedélyezheti univerzális csoporttagság gyorsítótárazása az Azure Active Directory-helyek és szolgáltatások-webhely számára.

    Ha telepíti a globális Katalógus, webhelyeken lévő hivatkozásokat és konfigurálása webhely hivatkozások költségek, hogy a globális Katalógus az Azure webhelyen ne által előnyben részesített adatforrásként Adatközpont más globális katalógusok való replikáció azonos részleges tartomány partíciók van szükség.

- [A Windows Server AD DS adatbázis és SYSVOL elhelyezésének](#BKMK_PlaceDB): adatok lemezen hozzáadása DCs Azure VMs futó ahhoz, hogy a Windows Server Active Directory-adatbázist, naplók és SYSVOL tárolni.

- [Biztonsági mentési és visszaállítási](#BKMK_BUR): határozza meg, hogy hol tárolja a rendszerállapot biztonsági mentése. Ha szükséges, egy másik adatok lemezre hozzáadása a Adatközpont virtuális biztonsági másolatok tárolására szolgáló.

## <a name="deployment-decisions-and-factors"></a>Telepítési döntéseket és tényezők

Az alábbi táblázat összefoglalja a Windows Server Active Directory technológia területeket, amely az előző jelenik meg, és a megfelelő döntéseket is célszerű figyelembe venni részletesebben az alábbi hivatkozásokkal hatással vannak. Bizonyos technológia területeken előfordulhat, hogy nem alkalmazható minden telepítési forgatókönyv, és bizonyos technológia területeken előfordulhat, hogy több kritikus telepítési eset, mint egyéb technológia funkcióival.

Például ha egy másolata Adatközpont virtuális hálózaton található rendszerbe, és a erdő csak egyetlen tartományneve van, lehetőséget, majd a globális katalógus kiszolgáló üzembe ebben az esetben csak akkor fontos, hogy a telepítéshez, mert nem hoz létre az esetleges további replikációs követelmények. Azonban ha az erdő több tartomány van, majd egy virtuális hálózaton globális katalógus üzembe döntés befolyásolhatja a sávszélességre, teljesítményét, hitelesítés, directory keresések és így tovább.

| Windows Server Active Directory-technológia terület | Határozatok | Tényezők |
| ---- | ---- | ---- |
| [Hálózati topológiája](#BKMK_NetworkTopology) | Hozhat létre virtuális hálózatot? | <li>Corp elérésére vonatkozó követelmények</li> <li>Hitelesítés</li> <li>Fiókok kezelése</li> |
| [Adatközpont telepítésének konfigurálása](#BKMK_DeploymentConfig) | <li>Egy másik erdőben anélkül, hogy minden olyan meghatalmazások üzembe?</li> <li>Egy új akinek összevonási üzembe?</li> <li>A Windows Server Active Directory erdő adatvédelmi új akinek vagy Kerberos üzembe?</li> <li>Kiterjesztése Corp erdő Adatközpont kópia telepítésével?</li> <li>A tartomány fa vagy egy új gyermek tartomány üzembe helyezésével erdő meghosszabbítása Corp?</li> | <li>Biztonsági</li> <li>Megfelelőség</li> <li>Költség</li> <li>Tűrőképessége és hibatűrést</li> <li>Kompatibilitási</li> |
| [Windows Server Active Directory-webhely topológiája](#BKMK_ADSiteTopology) | Hogyan azt be a alhálózat, webhelyek és webhelyeken lévő hivatkozásokat Azure virtuális hálózati forgalmat optimalizálása, és minimalizálhatja költség? | <li>Alhálózat és webhely-definíciók</li> <li>A webhely kapcsolat tulajdonságainak és értesítés módosítása</li> <li>A replikáció tömörítés</li> |
| [IP-címek és a DNS-](#BKMK_IPAddressDNS) | Hogy miként konfigurálható az IP-címek és névfeloldás? | <li>Statikus IP-cím használata a Set-AzureStaticVNetIP parancsmag használatával</li> <li>A Windows Server DNS server telepítése, és a nevet és a az Adatközpont és a DNS-kiszolgálói szerepkörök üzemeltető virtuális IP-címét tartalmazó virtuális hálózati tulajdonságainak konfigurálása</li> |
| [GEO elosztott DCs](#BKMK_DistributedDCs) | Hogyan lehet külön virtuális hálózatokon DCs való replikáció? | Ha az Active Directory-webhely topológiát, amely megfelel a különböző Azure területek, mint létre szeretne hozni az Active Directory-helyek megfelelően geographies vezérlői szükséges. [Virtuális hálózat konfigurálása virtuális hálózati kapcsolat](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) való replikáció tartomány vezérlői külön virtuális hálózatok között. |
| [Csak olvasható DCs](#BKMK_RODC) | Csak olvasható vagy írható DCs használni? | <li>HBI/adat attribútumok szűrése</li> <li>Szűrő titkos kulcsok</li> <li>Kimenő forgalmának korlát</li> |
| [Globális katalógus](#BKMK_GC) | Globális katalógus telepíteni? | <li>Egyetlen tartományerdő ellenőrizze az összes DCs globális katalógusok</li> <li>Az erdőhöz távolítja el a globális katalógusok szükségesek hitelesítéshez</li> |
| [Telepítési mód](#BKMK_InstallMethod) | Hogyan telepítheti az Adatközpont Azure-ban? | Bármelyik: <li>Telepítse az Active Directory tartományi szolgáltatások, a Windows PowerShell alrendszerrel vagy Dcpromo</li> <li>Virtuális Merevlemezt, egy helyszíni virtuális Adatközpont áthelyezése</li> |
| [A Windows Server AD DS adatbázis és SYSVOL elhelyezkedése](#BKMK_PlaceDB) | Hol tárolja a Windows Server AD DS adatbázis, naplók és SYSVOL? | Dcpromo.exe alapértelmezett értékek módosítása. A kritikus az Active Directory fájlok *kell* mutatnia Azure adatok lemezen operációs rendszer megvalósítása írási gyorsítótárazás lemezt helyett. |
| [Biztonsági mentési és visszaállítási](#BKMK_BUR) | Hogyan megóvása, és helyreállításáról? | Hozzon létre a rendszer rendszerállapot biztonsági mentése |
| [Összevonási kiszolgáló konfigurálása](#BKMK_FedSrvConfig) | <li>A felhőben összevonási egy új akinek üzembe?</li> <li>Helyszíni Active Directory összevonási szolgáltatások üzembe és jelenítik meg a felhőben proxy?</li> | <li>Biztonsági</li> <li>Megfelelőség</li> <li>Költség</li> <li>Access-alkalmazások üzleti partnerek számára</li> |
| [Cloud services konfigurálása](#BKMK_CloudSvcConfig) | Egy felhőalapú szolgáltatásba implicit módon virtuális gép létrehozása első alkalommal telepíti. Szükség van további cloud services telepítése? | <li>A virtuális vagy VMs kell-e közvetlen módon kapcsolódik az internethez?</li> <li> A szolgáltatás kell-e terheléselosztási?</li> |
| [Nyilvános és titkos IP-címek a (virtuális IP-összehasonlítás dinamikus IP) összevonási kiszolgálói követelményei](#BKMK_FedReqVIPDIP) | <li>Nem a Windows Server AD FS-példány kell érhető el az internetről közvetlenül?</li> <li>Az alkalmazás üzembe helyezéséhez a felhőben kell-e saját internetes IP-cím és a port?</li> | Virtuális IP-címekre van szüksége a telepítéshez egy felhőalapú szolgáltatás hozzon létre |
| [A Windows Server AD FS magas elérhetősége konfigurálása](#BKMK_ADFSHighAvail) | <li>A Windows Server Active Directory összevonási szolgáltatások kiszolgálófarm hány csomópontok?</li> <li>Hány csomópontok a Windows Server AD FS-proxy farm bevezetését tervezi?</li> | Tűrőképessége és hiba |

### <a name="BKMK_NetworkTopology"></a>Hálózati topológiája

Annak érdekében, hogy az IP-cím egységesebb és a Windows Server Active Directory tartományi szolgáltatások DNS igényeinek megfelelő, akkor először hozzon létre egy [Azure virtuális hálózatot](../virtual-network/virtual-networks-overview.md) , és a virtuális gépeken futó csatolása szükség. A létrehozás során döntse el, e tetszés szerint a kapcsolat a helyszíni vállalati hálózathoz, amely átlátszó kapcsolódik Azure virtuális gépeken futó helyszíni gépek terjeszteni – Ez a hagyományos VPN-technológiákkal érhető el, és van szükség az, hogy a virtuális Magánhálózati végpont érhetők el a vállalati hálózathoz szélén. Ez azt jelenti, hogy a virtuális Magánhálózati kezdeményezni Azure a vállalati hálózathoz, nem fordítva.

Látható, hogy további tarifa szerint a szabványos díjakat, amely az egyes virtuális alkalmazása túl a helyszíni hálózaton virtuális hálózaton bővítésekor. Pontosabban vannak díjak Processzor alkalommal az Azure virtuális hálózati átjáró és a kilépési forgalmához minden egyes helyszíni gépek kommunikál, a virtuális Magánhálózaton keresztül virtuális által generált. Hálózati forgalmának engedélyezésére díjak kapcsolatos további tudnivalókért olvassa el az [Azure árak,-a-adatábrázolás](http://azure.microsoft.com/pricing/)című témakört.

### <a name="BKMK_DeploymentConfig"></a>Adatközpont telepítésének konfigurálása

Konfigurálja az Adatközpont módja attól függ, hogy a szolgáltatás követelményei Azure futtatni szeretné. Például a saját vállalati erdőből egy vásárlási-a-fogalom, egy új alkalmazás vagy néhány egyéb rövid távú projekt címtárszolgáltatásaival, de a belső vállalati erőforrások nem meghatározott hozzáférést igénylő teszteléshez elszigetelt új erdő előfordulhat, hogy telepíteni.

Előny, mint az Adatközpont nem névkiszolgálóit elszigetelt erdő helyszíni DCs, kisebb kimenő hálózati forgalmának engedélyezésére magát, a rendszer hozza létre, így közvetlenül a költségek csökkentése. Hálózati forgalmának engedélyezésére díjak kapcsolatos további tudnivalókért olvassa el a [Azure árak,-a-adatábrázolás](http://azure.microsoft.com/pricing/)című témakört.

Másik példaként tegyük fel van a szolgáltatás adatvédelmi követelményei, de a szolgáltatás függ, hogy a belső Windows Server Active Directory való hozzáférést. A szolgáltatás a felhőben engedélyezettek host adatok, ha telepítenie előfordulhat, hogy a belső erdő Azure új gyermek tartományt. Ebben az esetben telepítheti egy Adatközpont az új gyermek tartomány (nélkül a globális katalógus annak érdekében, hogy a cím adatvédelmi aggályokat súgója). Ebben az esetben kópia Adatközpont telepítési, valamint egy virtuális hálózati kapcsolat a helyszíni DCs van szükség.

Ha új erdő hoz létre, válassza ki, hogy használja [az Active Directory bizalmi kapcsolatok](https://technet.microsoft.com/library/cc771397) vagy [összevonási megbízik](https://technet.microsoft.com/library/dd807036). A kompatibilitási, biztonság, megfelelőség, költség és tűrőképessége határozza követelmények egyenleg. Például kihasználhatja [a szelektív hitelesítés](https://technet.microsoft.com/library/cc755844) előfordulhat, hogy a Azure új erdő üzembe és választja létrehozása a Windows Server Active Directory megbízhatósági kapcsolat a helyszíni erdő és a felhő erdő között. Ha az alkalmazás követelések szem előtt, azonban lehet, hogy telepítenie helyett erdőszintűek az Active Directory összevonási meghatalmazások. Másik tényező akár több adat a helyszíni Windows Server Active Directory a felhőbe időtartamának vagy bizonyos eredményeként hitelesítési és a lekérdezés betöltése több kimenő forgalmának készítése költség lesz.

Elérhetőség és hibafa hibatűrést követelményei is befolyásolhatja a választási lehetőségek. Például ha megszakad a kapcsolat feljebb helyezése vagy egy Kerberos adatvédelmi, vagy egy összevonási alkalmazások megbízható összes valószínűleg teljesen megszakadnak hacsak nem telepítette a megfelelő infrastruktúra Azure. Alternatív telepítési konfigurációjában például replika DCs (írható vagy írásvédett tartományvezérlőre) való hivatkozás kimaradások elviselni valószínűsége növelése.

### <a name="BKMK_ADSiteTopology"></a>Windows Server Active Directory-webhely topológiája

Helyesen megadása a helyek és webhelyeken lévő hivatkozásokat optimalizálása a forgalmat, és a költség összezárása szüksége. Webhelyek, a webhely-hivatkozások és alhálózat befolyásolja a topológiát DCs és a hitelesítési forgalmat között. Fontolja meg a következő forgalom díjakat, majd üzembe helyezéséhez és DCs konfigurálása a telepítéshez előírásainak megfelelően:

- Az átjáró magát óránként névleges díjat van:

 - Azt is elindítható és leállítható tetszés szerint

 - Ha leállítja, Azure VMs elkülönülnek a vállalati hálózathoz

- Bejövő forgalom az ingyenes

- Kimenő forgalmának megfelelően [Azure árak,-a-adatábrázolás](http://azure.microsoft.com/pricing/)terheli. Optimalizálhatja a helyhivatkozás tulajdonságait a helyszíni webhelyek és a felhő webhelyek között az alábbi képlettel történik:

 - Virtuális hálózatok összevonása használatakor állítsa be a webhely-hivatkozások és a költségeket megfelelően meg, hogy a Windows Server AD DS az Azure webhely rangsorolása egyike ingyenesen szolgáltatás azonos szintű biztosíthat fölé. Is érdemes letiltása a híd gomb (Ez alapértelmezés szerint engedélyezve van) hivatkozásra (BASL) lehetőséget. Ezzel biztosíthatja, hogy csak közvetlenül-csatlakoztatott webhelyek névkiszolgálóit egymással. Tranzitív csatlakoztatott webhelyek vezérlői már nem tudja közvetlenül egymással való replikáció, de kell készítenie egy közös webhelyet vagy webhelyeket keresztül. Ha valamilyen oknál fogva az közvetítő webhelyek válnak elérhetetlenné, vezérlői tranzitív csatlakoztatott webhelyek közötti replikáció nem történik meg, még akkor is, ha a webhelyek közötti kapcsolatot érhető el. Végül a tranzitív replikáció működését szakaszok célszerű továbbra is fennáll, ahol webhely létrehozása a megfelelő webhelyhivatkozások és webhelyeken, például a helyszíni vállalati hálózati helyek tartalmazó hivatkozás hidakat.

 - [Konfigurálása helyhivatkozás költségét](https://technet.microsoft.com/library/cc794882) megfelelően elkerülése érdekében a várt forgalmat. Például ha **Következő legközelebbi helyen próbálja** beállítás engedélyezve van, ellenőrizze, hogy a virtuális hálózati helyek amelyek nem a következő legközelebb a helyhivatkozás objektum, amely az Azure webhely vissza csatlakozik a vállalati hálózathoz költsége növelésével.

 - Konfigurálja a webhely hivatkozásra [intervallumok](https://technet.microsoft.com/library/cc794878) és [Ütemezés](https://technet.microsoft.com/library/cc816906) szerint konzisztencia követelmények és objektum megváltozik mértéke. Időtartam-eltérés az ütemezésben igazítása DCs érték, csak az utolsó állam készítenie, így a replikációs időköz csökkenő mentheti, költségek ha van egy megfelelő objektum sebességének módosítása.

- Ha a kis méretre költségek kiemelt, győződjön meg róla, replikációs van ütemezve, és nincs engedélyezve a módosítási értesítést. Ez a beállítás az alapértelmezett verzióazonosítójának webhelyek között. Ez nem fontos: Ha egy írásvédett egy virtuális hálózaton telepít, mert az írásvédett nem fog bizonyos kimenő módosításokat. De ha telepít egy írható Adatközpont, gondoskodnia kell arról frissítések névkiszolgálóit szükségtelen gyakoriság nincs beállítva a webhely hivatkozásra. A globális katalógus server (globális Katalógus) telepít, győződjön meg arról, hogy minden más webhely, amely tartalmazza a globális Katalógus replikálja a tartomány partíciót egy webhelyet, amely a webhely-hivatkozás vagy a globális Katalógus-nél kisebb számol van az Azure-webhelyen webhely hivatkozások csatlakozik az Adatközpont forrásból származó.


- Akkor lehet, hogy továbbra is tovább csökkentse a hálózati forgalmat a replikáció tömörítés algoritmus módosításával webhelyek közötti replikáció által generált. A tömörítési algoritmus REG_DWORD rendszerleíró bejegyzés HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator tömörítési algoritmus szabályozza. Az alapértelmezett értéke 3, amely a Xpress tömörítés algoritmus hibához. Módosíthatja az érték 2, amely algoritmus módosításaira MSZip. A legtöbb esetben ez megnöveli a tömörítés, de költségére Processzor kihasználtsági jelent. További tudnivalókért olvassa el a [topológiát az Active Directory hogyan működik a](https://technet.microsoft.com/library/cc755994)című témakört.

### <a name="BKMK_IPAddressDNS"></a>IP-címek és a DNS-

Azure virtuális gépeken futó "DHCP bérbe címek" van rendelve, alapértelmezés szerint. Azure virtuális hálózati dinamikus címek továbbra is fennáll a virtuális gép a élettartamot a virtuális gép, mert a Windows Server AD DS követelményeinek teljesül.

Eredményeként Azure dinamikus címet használ, amikor éppen effektus használata egy statikus IP-címet, mert a átirányítható a bérleti ideje, és a bérleti ideje megegyezik a felhőbeli szolgáltatástól élettartama.

A dinamikus cím felszabadítása azonban a virtuális leállítás esetén. Az IP-cím megakadályozásához felszabadítása készül, használhatja a [Set-AzureStaticVNetIP hozzá szeretné rendelni a statikus IP-címet](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

A névfeloldás, üzembe a saját (vagy kihasználhatja a meglévő) DNS-kiszolgáló infrastruktúra; Azure által biztosított DNS nem felel meg a Windows Server AD DS speciális neve felbontás igényeinek. Ha például azt nem támogatása dinamikus SRV-rekordokat, és így tovább. Névfeloldás DCs és a tartományhoz tartozó ügyfelek kritikus konfigurációs elem. DCs képes erőforrásrekordok rögzítése és más Adatközpont erőforrásrekordok megoldása kell lennie.
Hibafa hibatűrést és a teljesítmény érdekében célszerű a Windows Server DNS-szolgáltatás telepítése az Azure futó DCs optimális. Ezután állítsa be az Azure virtuális hálózati tulajdonságai nevét és a DNS-kiszolgáló IP-címét. A virtuális hálózaton más VMs indításakor a DNS-ügyfél feloldó beállításait a DNS-kiszolgáló IP-címek dinamikus kiosztása részeként konfigurálhatók.

> [AZURE.NOTE] A helyszíni számítógépek egy Windows Server AD DS az Active Directory-tartományra Azure közvetlenül az interneten tárolt nem tud csatlakozni. Portokon az Active Directory és a tartomány-join művelet jeleníti meg ezt a praktikus közvetlenül az jelenítik meg a szükséges portokat és hatása, egy teljes Adatközpont az internethez.

VMs regisztrálhatja a saját tartománynév automatikus indítás vagy név módosítás esetén.

Ebben a példában és egy másik példa, hogy miként kiépítése a első virtuális, és telepítse az Active Directory tartományi szolgáltatások rajta kapcsolatos további tudnivalókért olvassa el a [telepítse a Microsoft Azure Active Directory új erdő](active-directory-new-forest-virtual-machine.md)című témakört. A Windows PowerShell használatával kapcsolatos további tudnivalókért olvassa el az [Azure PowerShell telepítése](../powershell-install-configure.md) és [Azure-kezelő parancsmagok](https://msdn.microsoft.com/library/azure/jj152841)című témakört.

### <a name="BKMK_DistributedDCs"></a>GEO elosztott DCs

Azure előnyökkel jár, ha több DCs különböző virtuális hálózatok szolgáltatónál:

- Több elem webhely hibatűrést

- Fizikai közelében ág irodák (alsó időtartama)

Közvetlen közlemény virtuális hálózatok között beállításával kapcsolatos további tudnivalókért lásd [virtuális hálózat konfigurálása virtuális a hálózati kapcsolat](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>Csak olvasható DCs

Válassza ki, hogy csak olvasható vagy írható DCs üzembe szüksége. Előfordulhat, hogy ferde, írásvédett tartományvezérlőre bevezetését tervezi, mert nem lesz fizikai szabályozni kell őket, de írásvédett tartományvezérlőre célja, hogy hol található a saját fizikai biztonsági kockázatot, például ág irodák a helyek telepíthetők.

Azure sem jelenik meg egy másik iroda fizikai biztonsági kockázatot jelentenek, de az írásvédett tartományvezérlőre előfordulhat, hogy továbbra is igazolni költség hatékonyabb lesz, mert a funkciók nyújtanak jól illeszkedik ezen környezetekben jóllehet nagyon különböző okokból. Írásvédett tartományvezérlőre például nincs replikáció van, és így tudják szelektív kitöltéséhez titkos kulcsok (jelszó). Mi a hátrányuk előfordulhat, hogy a titkos adatok hiányát igény szerinti kimenő forgalmának felhasználóként érvényesítéséhez vagy a számítógép hitelesíti. De titkos kulcsok szelektív módon lehet előre és gyorsítótárazott.

Írásvédett tartományvezérlőre tartalmaz egy további előnye és környékén HBI és adat aggályokat, mivel a is hozzáadhat az írásvédett bizalmas adatokat tartalmazó attribútumok szűrt attribútum beállítása (eszközök). Az eszközök olyan attribútumok nem írásvédett tartományvezérlőre a replikált testre szabható csoportja. Is használhatja az eszközök védelmet abban az esetben nem engedélyezett, vagy nem szeretne a Azure adat vagy HBI tárolja. További tudnivalókért lásd: [írásvédett szűrt attribútum beállítása [(https://technet.microsoft.com/library/cc753459)].

Győződjön meg arról, hogy alkalmazások írásvédett tartományvezérlőre használni kívánt kompatibilis lesz. Sok Windows Server Active Directory-alkalmazásokban is dolgozhat írásvédett tartományvezérlőre, de egyes alkalmazások töredezetté végrehajtása, vagy a sikertelen, ha nincs olyan írható Adatközpont való hozzáférést. További információ a [írásvédett DCs alkalmazás kompatibilitási útmutató](https://technet.microsoft.com/library/cc755190)című témakörben.

### <a name="BKMK_GC"></a>Globális katalógus

Válassza ki, hogy a globális katalógus telepítéséhez szükséges. Az egyetlen tartományerdő konfigurálnia kell az összes DCs globális katalógus kiszolgálók. Költségek nem nő, mert nem fog nem lehet további replikációs forgalmat.

Egy távolítja el erdőben globális katalógusok szükségesek a hitelesítési folyamat során univerzális csoporttagság kibontásához. Ha nem telepíti a globális Katalógus, a virtuális hálózaton munkaterhelésekből, amely egy Adatközpont Azure a hitelesítést közvetve készítése a kimenő hitelesítést forgalmat a helyszíni globális katalógusok lekérdezése az egyes hitelesítési kísérlet során.

Globális katalógusok társított költségek azért kisebb kiszámítható, mert azok szolgáltató minden tartományt (a részből álló). Ha a terhelést a egy olyan internetes szolgáltatás üzemelteti, és végzi a felhasználók hitelesítését Windows Server az Active Directory tartományi szolgáltatások, a költségek teljesen járhat lehet. Csökkentésére a felhőben webhelyet partnerlistámba globális Katalógus lekérdezések hitelesítés során, azt is megteheti [gyorsítótárazása univerzális csoport tagságát](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Telepítési mód

Meg kell adnia a DCs telepítése a virtuális hálózaton:

- Új DCs előléptetése További tudnivalókért lásd: a [egy új Active Directory-erdőt Azure virtuális hálózaton telepítése](active-directory-new-forest-virtual-machine.md)elemre.

- A virtuális egy helyszíni virtuális Adatközpont az áthelyezés a felhőbe. Ebben az esetben előbb gondoskodnia kell róla, hogy a helyszíni virtuális Adatközpont "helyez át," nem "másolt" vagy "Klónozott."

Használjon csak Azure virtuális gépeken futó DCs (nem pedig az Azure "web" vagy "dolgozó" szerepkör VMs). Tartós és állapotának egy Adatközpont tartósságát szükség. Azure virtuális gépeken futó munkaterhelésekből, például DCs készültek.

Ne használjon SYSPREP üzembe vagy DCs klónozhatja. A szúrhatók DCs klónozhatja, a Windows Server 2012 csak elérhető elején. A klónképző funkció VMGenerationID támogatás az alapul szolgáló hipervizor igényel. A Windows Server 2012 és Azure virtuális hálózatok a Hyper-V egyaránt használható VMGenerationID, külső virtualizációs szoftver szállítók.

### <a name="BKMK_PlaceDB"></a>A Windows Server AD DS adatbázis és SYSVOL elhelyezkedése

Jelölje be a hol helyezi el a Windows Server AD DS adatbázis naplók és SYSVOL. Azok a kell Azure Datamarket-lemezen telepítését.

> [AZURE.NOTE] Azure adatok lemez és 1 TB korlátozott.

Adatok lemezmeghajtók alapértelmezés szerint nem gyorsítótár írások teheti meg. Adatok egy virtuális használata író gyorsítótárazás csatolt meghajtót. Író gyorsítótárazás teszi, hogy az írás elkötelezte magát, tartós Azure tároló előtt a tranzakció befejeződött a virtuális operációs rendszer szemszögéből. Tartóssági költségére kissé lassabban írások biztosít.

Ennek oka fontosak a Windows Server AD DS írási mögöttes merevlemez-gyorsítótár érvényteleníti feltételezések alapján az Adatközpont. A Windows Server az Active Directory tartományi szolgáltatások letiltása írási gyorsítótárazás megpróbálja, de legfeljebb a lemez IO rendszer elfogadja azt. Hiba írási gyorsítótárazás letiltása bizonyos körülmények között vezethetnek USN visszaállítás, így lingering, objektumok és egyéb problémák.

Virtuális DCs a legjobb módszer tegye a következőket:

- Állítsa be a Host gyorsítótár preferencia-sorrend az Azure adatok lemezen a Nincs lehetőséget. Megakadályozza, hogy a írási gyorsítótárazás Active Directory tartományi szolgáltatások műveletekhez kapcsolatos problémákat.

- Az adatbázis naplók és SYSVOL tárolhatja vagy azonos adatokat lemez vagy a különböző adatokhoz lemezt. Ez általában a lemezről az operációs rendszer önmagában használható külön lemezen. A fő takeaway, hogy a Windows Server AD DS adatbázis és SYSVOL nem kell tárolni az Azure operációs rendszer lemezen típusát. Az Active Directory tartományi szolgáltatások telepítési folyamatot telepíti, az összetevők Azure nem ajánlott % rendszergyökér % mappában.

### <a name="BKMK_BUR"></a>Biztonsági mentési és visszaállítási

Tartsa szem előtt mi van, és nem támogatott biztonsági mentése és visszaállítása a Adatközpont általános, valamint a egy virtuális konstrukciókban, pontosabban. [Biztonsági mentési és visszaállítási szempontjai virtualizált DCs](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers)látható.

Hozzon létre rendszer rendszerállapot biztonsági mentése csak kifejezetten ismeri a Windows Server AD DS, például a Windows Server biztonsági másolat biztonsági követelményeinek biztonsági szoftver használatával.

Másolja vagy ne virtuális fájlok replikakészletében klónozhatja rendszeres biztonsági mentés végrehajtása helyett. A visszaállítás minden eddiginél kell, így használata a Windows Server 2012-ben és a támogatott hipervizor nélkül klónozott vagy másolt VHD bevezetésére USN buborékok módon.

### <a name="BKMK_FedSrvConfig"></a>Összevonási kiszolgáló konfigurálása

A Windows Server Active Directory összevonási szolgáltatások összevonási kiszolgálóihoz (STSs) konfigurálása függ részben e Azure a telepíteni kívánt alkalmazást kell a helyszíni hálózaton forrásokat.

Az alkalmazások teljesíti az alábbi követelményeket, ha az alkalmazások önmagában sikerült telepítenie a helyszíni hálózaton.

- SAML biztonsági tokenek elfogadása

- Azok az internethez exposable

- Ezek nem fér hozzá a helyszíni

Ebben az esetben állítsa be a Windows Server Active Directory összevonási szolgáltatások STSs az alábbi képlettel történik:

1. Azure-elszigetelt egyetlen tartományerdő konfigurálhatja.

2. Az erdő szövetséges hozzáférést biztosít a Windows Server Active Directory összevonási szolgáltatások összevonási kiszolgálófarm konfigurálásával.

3. A helyszíni erdőben beállítása a Windows Server Active Directory összevonási szolgáltatások (összevonási kiszolgálófarm és összevonási kiszolgálófarm proxy).

4. Összekapcsolására összevonási megbízhatósági kapcsolat a helyszíni és a Windows Server AD FS Azure példányát.

Azonban ha az alkalmazásokat a helyszíni erőforrásokhoz való hozzáférést kér, is beállítható a Windows Server Active Directory összevonási szolgáltatások a Azure alkalmazással az alábbi képlettel történik:

1. Állítsa be a helyszíni hálózatok és Azure közötti kapcsolatot.

2. A Windows Server Active Directory összevonási szolgáltatások összevonási kiszolgáló farm konfigurálása a helyszíni erdőben.

3. A Windows Server Active Directory összevonási szolgáltatások összevonási kiszolgáló proxy farm konfigurálása Azure.

Ez a beállítás az az előnye, hogy a helyszíni erőforrások, hasonlóan, mint a Windows Server Active Directory összevonási szolgáltatások konfigurálása peremhálózat-alkalmazásokkal való csökkentése tartalmaz.

Figyelje meg, hogy mindkét esetben futtatva létesíthet bizalmi kapcsolatok további Identitásszolgáltatók, az üzleti vállalati verzió együttműködési van szükség.

### <a name="BKMK_CloudSvcConfig"></a>Cloud services konfigurálása

Felhőszolgáltatások szükség, ha azt szeretné, hogy kattintva jelenítse meg a virtuális közvetlenül az interneten vagy kattintva jelenítse meg az internetes terheléselosztás alkalmazások. Ez a lehetőség, mivel minden felhőalapú szolgáltatást biztosít a egyetlen konfigurálható virtuális IP-címet.

### <a name="BKMK_FedReqVIPDIP"></a>Nyilvános és titkos IP-címek a (virtuális IP-összehasonlítás dinamikus IP) összevonási kiszolgálói követelményei

Azure virtuális gépeken megkapja a dinamikus IP-címet. A dinamikus IP-cím csak az Azure belül hozzáférhető egy saját címet. A legtöbb esetben azonban lesz a Windows Server AD FS telepítésekhez egy virtuális IP-cím beállításához szükséges. A virtuális IP kattintva jelenítse meg a Windows Server Active Directory összevonási szolgáltatások az Internet végpontok szükség, és felhasználja a szövetséges partnerek és ügyfelek hitelesítési és a mindennapi felügyeleti feladatokhoz. A virtuális IP-cím egy felhőalapú szolgáltatásba, amely tartalmaz egy vagy több Azure virtuális gépeken futó tulajdonsága. Internetes és a megosztás gyakran portokat az Azure és a Windows Server Active Directory összevonási szolgáltatások rendszerbe követelések-et alkalmazás esetén minden egyes esetén kell egy virtuális IP-címének a saját, és ezért lesz egy felhőalapú szolgáltatásba, az alkalmazás és a Windows Server Active Directory összevonási szolgáltatások számára egy második létrehozása.

A kifejezések meghatározása a virtuális IP-cím vagy dinamikus IP-címet, olvassa el [fogalmak és definíciók](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>A Windows Server AD FS magas elérhetősége konfigurálása

Különálló Windows Server Active Directory összevonási szolgáltatások összevonási szolgáltatásainak üzembe lehetőség, miközben ajánlott üzembe helyezése a farm legalább két csomópontok Active Directory összevonási szolgáltatások STS és a gyártási környezetekben proxyval.

Lásd az [AD FS 2.0-s alkalmazással topológia kapcsolatos megfontolások](https://technet.microsoft.com/library/gg982489) az [AD FS 2.0-s tervezési útmutató](https://technet.microsoft.com/library/dd807036) döntse el, melyik telepítési beállítások ajánlott az adott igényeinek.

> [AZURE.NOTE] Ahhoz, hogy a Windows Server Active Directory összevonási szolgáltatások végpontjait Azure terheléselosztás, állítsa be a Windows Server AD FS-farm minden tagjának ugyanazt a felhőalapú szolgáltatást, és a betöltés terheléselosztó szolgáltatásának Azure http (alapértelmezett 80) és HTTPS-portok (alapértelmezett 443-as). További tudnivalókért lásd: [Azure terheléselosztó vizsgálati](https://msdn.microsoft.com/library/azure/jj151530).
A Windows Server hálózati betöltés terheléselosztás Terheléselosztási Azure a nem támogatott.
