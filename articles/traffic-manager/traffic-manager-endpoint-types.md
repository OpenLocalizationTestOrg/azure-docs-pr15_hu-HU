<properties
    pageTitle="Forgalom Manager végpont típusok |} Microsoft Azure"
    description="Ez a cikk ismerteti a különböző típusú Azure forgalom Manager használható végpontok"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-endpoints"></a>Adatforgalom Manager végpontok

Microsoft Azure-forgalmat kezelő szabályozhatja, hogy hogyan hálózati forgalmának engedélyezésére különböző adatközpont esetén futó alkalmazás telepítések eljut teszi lehetővé. Egyes alkalmazások telepítési egy végpontjának konfigurálása az adatforgalom-kezelőben. Forgalom Manager DNS kérelem érkezik, amikor a DNS-válasz szerepelniük elérhető zárólap választja ki. Adatforgalom manager a végpontot aktuális állapotát és a forgalom útválasztás módszer a választási lehetőségek alapozza. További tudnivalókért olvassa el a [Forgalom Manager működése](traffic-manager-how-traffic-manager-works.md)című témakört.

Háromféle forgalom Manager által támogatott végpont van:

- **Azure végpontokat** az Azure-ban tárolt szolgáltatásokhoz használ.
- **Külső végpontok** kívüli Azure, vagy a helyszíni, vagy egy másik szolgáltató szolgáltatások segítségével.
- **Egymásba ágyazott végpontokat** forgalom Manager profilok létrehozása rugalmasabb forgalom útválasztás rendszerek támogatja az egyéni igényeknek nagyobb, összetett telepítések egyesítéséhez használ.

A különböző típusú végpontok egyetlen forgalom Manager profilban együttes hogyan nincs korlátozás nem. Az egyes profilok bármely végpont típusok vegyesen is tartalmazhatnak.

Az alábbi szakaszok ismertetik alaposabb végpont típusonként.

## <a name="azure-endpoints"></a>Azure végpontok

Azure végpontok szolgáltatások Azure-alapú forgalmat Manager segítségével. A következő Azure erőforrástípus támogatottak:

- Klasszikus' IaaS VMs és PaaS cloud services.
- Web Apps alkalmazások
- PublicIPAddress erőforrások (VMs csatlakoztatható vagy közvetlenül az Azure terheléselosztó keresztül). A publicIpAddress használhatók a forgalom Manager profil rendelt DNS nevét kell rendelkeznie.

PublicIPAddress erőforrások Azure erőforrás-kezelő erőforrások. Nem léteznek a klasszikus telepítési modell. Így csak a támogatott forgalom vezető Azure erőforrás-kezelő szolgáltatások legyenek is. A más végpont típusú erőforrás-kezelő és a a klasszikus telepítési modell támogatottak.

Azure végpontok használatakor a forgalom Manager észleli, ha egy "Classic" IaaS virtuális, felhőalapú szolgáltatásba vagy egy webalkalmazás leállt és lépések. Ezt az állapotot a végpont állapota is megjelenik. Lásd: a [forgalom Manager végpont figyelése](traffic-manager-monitoring.md#endpoint-and-profile-status) további információt. Az alapul szolgáló szolgáltatás leállítása esetén forgalom Manager nem végez végpont állapot-ellenőrzései vagy közvetlen forgalom az végpontot. Nincs forgalom Manager számlázási események leállítva példány fordul elő. A szolgáltatás újraindítása számlázási önéletrajzok, és a végpont esetén jogosult a forgalmat. Az észlelés PublicIpAddress végpontokhoz nem vonatkozik.

## <a name="external-endpoints"></a>Külső végpontok

Külső végpontokat az Azure-Ön kívüli szolgáltatásokkal használ. Például a szolgáltatás is a helyszíni, vagy egy másik szolgáltatónál. Külső végpontok lehet használt egyenként vagy a forgalom Manager profillal az Azure végpontok kombinálva. A külső végpontok Azure végpontok egyesítése lehetővé teszi, hogy a különböző forgatókönyvekben:

- A bármelyik aktív-aktív vagy aktív-passzív feladatátvevő modell Azure segítségével adja meg egy létező nagyobb redundancia helyszíni alkalmazást.
- Alkalmazás késése világszerte csökkentéséhez bővítése további földrajzi helyek Azure-ban egy meglévő helyszíni alkalmazást. További tudnivalókért lásd: [forgalom Manager "Teljesítmény" forgalom útválasztás](traffic-manager-routing-methods.md#performance-traffic-routing-method).
- Egy létező további kapacitásának megadására használata Azure helyszíni alkalmazást, folyamatosan vagy a Nyárs igény szerint az alkalmazást "burst felhő" megoldást.

Bizonyos esetekben célszerű használni a külső végpontok Azure szolgáltatások hivatkozni szeretne (Példák című témakörben talál a [Gyakori kérdések](#faq)). Ebben az esetben állapot-ellenőrzései a rendszer az Azure végpontok sebességét, nem külső végpontok a számlát kapni. Azonban eltérően Azure végpontok, ha leállítása vagy törlése az alapul szolgáló szolgáltatás állapotának ellenőrzése számlázási továbbra is fennáll, amíg az adatforgalom-kezelő végpontot törlése vagy letiltása.

## <a name="nested-endpoints"></a>Beágyazott végpontok

Beágyazott végpontok egyesítheti több forgalom Manager profilok hozhat létre rugalmas forgalom útválasztás rendszerek, és az egyéni igényeknek nagyobb, összetett telepítések támogatja. Egymásba ágyazott végpontok, a "child" profil megjelenik egy végpontjának "szülő" profil. A gyermek és a szülő profilok bármilyen típusú, beleértve a más beágyazott profilok más végpontokat is tartalmazhat. További információ című témakör tartalmaz [beágyazott forgalom Manager](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web Apps, a végpontok

Néhány további érvényesek, a végpontokat az adatforgalom-kezelőben Web Apps alkalmazások beállításakor:

1. A "Szabvány" Termékváltozat vagy ennél csak Web Apps használata forgalom Manager jogosultak. Alsó Termékváltozatot a webes alkalmazás felvétele a kísérletek sikertelenek. Visszalépés a Termékváltozat egy meglévő Web App-ról eredménye az adatforgalom-kezelőben már nem küld a forgalmat a Web App alkalmazásban.

2. Zárólap HTTP-kérelem érkezik, ha használ a 'host' fejléc, a kérelem megállapíthatja, melyik Web App kell szolgáltatás a kérést. A host fejléc kérni, például "contosoapp.azurewebsites.net" használt DNS nevét tartalmazza. Egy másik tartománynév használatára a Web App alkalmazásban, a DNS-név rendelkeznie kell regisztrált az alkalmazást az egyéni tartomány nevét. Egy webalkalmazás végpontot, Azure zárólap hozzáadásakor a forgalom Manager profilnév DNS automatikusan regisztrálva van az alkalmazás. Az endpoint törlésekor a rendszer automatikusan törli a regisztráció.

3. Minden forgalom Manager profil beállíthatja, hogy legfeljebb a Web App egyik végpontját az Azure területenként. Egy külső végpontot, amelyekkel megkerülheti ezt a korlátot, beállíthatja webalkalmazást. További tudnivalókért olvassa el a a [Gyakori kérdések](#faq)című témakört.

## <a name="enabling-and-disabling-endpoints"></a>Engedélyezése és letiltása a végpontok

Az adatforgalom-kezelőben zárólap letiltása forgalom ideiglenes eltávolítása, amely a karbantartási mód, vagy éppen újra nem telepítik zárólap hasznos lehet. Ha ismét az végpontot fut, újraengedélyezheti lehet.

Végpontok engedélyezhető és tiltható le a forgalom Manager portálon PowerShell, CLI vagy REST API-t, amelyek mindegyike támogatott az erőforrás-kezelő és a klasszikus telepítési modell keresztül.

>[AZURE.NOTE] Azure zárólap letiltása amelyen semmi sem a teendő az Azure környezetben állapotába. Az Azure service (például egy virtuális vagy a Web App marad fut, és a forgalmat, akkor is, ha a forgalom Manager letiltása érkeznek. Adatforgalom megoldhatók közvetlenül a szolgáltatás példánya helyett a forgalom Manager profilnév DNS keresztül. További információ a [forgalom Manager működése](traffic-manager-how-traffic-manager-works.md)című témakör tartalmaz.

Az aktuális jogosultság minden forgalom fogadására végpont attól függ, hogy az alábbi tényezőket:

- A profil állapota (engedélyezett/letiltott)
- Végpont állapotát (engedélyezett/letiltott)
- Az állapot eredményeinek ellenőrzi, hogy végpontra

A részletekért olvassa [forgalom Manager végpont figyelése](traffic-manager-monitoring.md#endpoint-and-profile-status).

>[AZURE.NOTE] Mivel a forgalom Manager DNS szintre működik, található nem befolyásolja a meglévő kapcsolatok bármely végpontot. Zárólap nem érhető el, ha a forgalom Manager egy másik elérhető végpont új kapcsolatokat irányítja. A host mögött a letiltott vagy sérült végpont azonban továbbra is forgalmat a meglévő kapcsolatok keresztül fogadni, amíg a megszakítja a munkamenetek. Alkalmazások korlátozza a munkamenet időtartama pedig a meglévő kapcsolatok forgalmának engedélyezésére.

Ha egy profilt az összes végponton le vannak tiltva, vagy ha magát a profil le van tiltva, forgalom Manager új DNS-lekérdezés "NXDOMAIN" választ küld.

## <a name="faq"></a>GYAKORI KÉRDÉSEK

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Használhatom-e forgalom Manager több előfizetésekről végpontok?

Több előfizetésekről végpontok használata esetén nem lehet az Azure Web Apps alkalmazásokkal. Azure Web Apps alkalmazások használatához szükséges szolgál, hogy minden olyan egyéni tartománynevet az Web Apps alkalmazások használatakor csak egyetlen előfizetés belül. Még nem lehet azonos nevű tartományban több előfizetésekről Web Apps alkalmazások használata.

Más végpont típusnál egynél több előfizetésből végpontok forgalom Manager használatára. Forgalom Manager beállításaitól attól függ, hogy használja a klasszikus telepítési modell vagy az erőforrás-kezelő élmény.

- Az erőforrás-kezelő bármely előfizetésből végpontok felvehetők a forgalom Manager mindaddig, amíg a forgalom Manager profil konfigurálásának személy olvasási hozzáférést az végpontot. Az engedélyek [Azure erőforrás-kezelő szerepköralapú hozzáférés - szerepalapú](../active-directory/role-based-access-control-configure.md)lehet adni.
- A klasszikus telepítési modell felületén a forgalom Manager szükséges, hogy Cloud Services vagy a Web Apps alkalmazások Azure végpontok konfigurált ugyanabban az előfizetésben, mint a forgalom Manager profil tárolnak. Más előfizetések felhőalapú szolgáltatás végpont felvehetők a forgalom Manager mint "külső" végpontok. Ezeket a külső végpontokat is számlát kapni, a külső ráta helyett Azure végpontok.

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Használhatom-e forgalom Manager Felhőszolgáltatásba átmeneti"helyek?

igen. "Átmeneti tárolására' helyek Felhőszolgáltatásba konfigurálhatók a forgalom Manager külső végpontok. Állapot-ellenőrzései előfizetést terhelő továbbra is az Azure végpontok díjazott. A külső végpont típus használatban van, mert az alapul szolgáló szolgáltatás módosításai vannak nem felvett automatikusan. A külső végpontok forgalom Manager nem észleli, ha a felhőalapú szolgáltatás leállítása vagy törölt. Emiatt a forgalom kezelő továbbra is az állapot-ellenőrzései számlázási mindaddig, amíg a végpont letiltották vagy törölték.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Támogatja a forgalom Manager IPv6-végpontot?

Forgalom Manager jelenleg nem nyújt az IPv6-addressible névkiszolgálók. Manager forgalmat továbbra is használható IPv6-ügyfelek csatlakoztatása az IPv6-végpontot. Egy ügyfél nem kéréseket DNS közvetlenül a forgalom Manager. Ehelyett az ügyfelet használ egy rekurzív DNS-szolgáltatás. Egy csak IPv6-ügyfél kérések küld a rekurzív DNS-szolgáltatás IPv6 keresztül. Kattintson a rekurzív szolgáltatás látnia kell a forgalom Manager névkiszolgálók IPv4 segítségével kapcsolatba lépni.

Forgalom Manager válaszol a végpont DNS-nevét. Támogatja az IPv6 zárólap, hogy a DNS AAAA rekordot a DNS-végpontjának neve mutasson az IPv6 address léteznie kell. Állapot-ellenőrzései forgalom Manager csak IPv4-címei támogatja. A szolgáltatás kattintva jelenítse meg a DNS-néven IPv4 zárólap igényeknek megfelelően.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Használhatom-e forgalom Manager ugyanabban a régióban egynél több Web Appban?

Általában a forgalom Manager irányítsa át a forgalom különböző régiókban telepített alkalmazások használatos. Azonban is használható azok az alkalmazás nem ugyanabban a régióban egynél több telepítési. A forgalom Manager Azure végpontokat nem teszik lehetővé egynél több webalkalmazás végpont megjelenjen a forgalom Manager profillal az azonos Azure területről.

Az alábbi lépésekkel adja meg, hogy a megoldás: a ezt a korlátot:

1. Ellenőrizze, hogy a végpontok egységekben különböző web app"skála". Egy adott időosztás egységet a webhely kell feleltesse meg a tartomány nevét. Emiatt a azonos időosztás egységet a két Web Apps alkalmazások nem oszthat meg forgalom Manager profil.
2. Vegye fel méltóság tartománynevét, egy egyéni hostname (állomásnév), minden Web App alkalmazásban. Minden egyes Web App be más időosztás egységet kell lennie. Az összes Web Apps alkalmazások ugyanabban az előfizetésben kell tartoznia.
3. Egy (és csak egy) webalkalmazás végpont hozzáadása forgalom Manager profilját, Azure zárólap.
4. Minden egyes további Web App végpont hozzáadása forgalom Manager profilját, külső zárólap. Csak felvehetők külső végpontok az erőforrás-kezelő telepítési modellt használja.
5. A CNAME DNS-rekord létrehozása a méltóság tartományban, amely a forgalom Manager profilnév DNS mutat (<> …. trafficmanager.net).
6. A webhely méltóság tartománynevet, nem a forgalom Manager DNS profilnév keresztül érhető el.

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, [Hogyan működik a forgalom Manager](traffic-manager-how-traffic-manager-works.md).
- Tudjon meg többet a forgalom felettes [végpont figyelése és automatikusan átveszi](traffic-manager-monitoring.md).
- Tudjon meg többet a forgalom kezelő [forgalom útválasztási módszereket](traffic-manager-routing-methods.md).
