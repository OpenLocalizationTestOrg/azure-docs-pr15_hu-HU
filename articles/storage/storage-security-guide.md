<properties
    pageTitle="Azure tároló biztonsági útmutató |} Microsoft Azure"
    description="Részletek biztonságossá tétele Azure tárolására, beleértve, de nem korlátozódik RBAC, tároló titkosítását, ügyféloldali titkosítás, 3.0 kis-és Középvállalatok és Azure lemezre titkosítás sok módszereket."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="robinsh"/>

#<a name="azure-storage-security-guide"></a>Azure tároló biztonsági útmutató

##<a name="overview"></a>– Áttekintés

Biztonsági funkciók, amelyek közös engedélyezése a fejlesztők számára biztonságos alkalmazások átfogó Azure tárhelyet biztosít. A tároló fiókon védhetők szerepköralapú hozzáférés-vezérlés és Azure Active Directory. A védett adatok között egy alkalmazást, és Azure [Ügyféloldali titkosítást](storage-client-side-encryption.md), a HTTPS vagy a kis-és Középvállalatok 3.0 használatával. Adatok beállítható, hogy automatikusan titkosítása Azure tárolóhoz írásakor [Tároló szolgáltatás titkosítási (SSE)](storage-service-encryption.md)segítségével. Virtuális gépeken futó által használt operációs rendszer és az adatok lemez adhatja meg az [Azure lemez](../security/azure-security-disk-encryption.md)titkosítással titkosítva. A data objects Azure-tárolóban lévő delegált eléréséhez lehet adni a [Megosztott Access aláírások](storage-dotnet-shared-access-signature-part-1.md)használata.

Ez a cikk áttekintést minden Azure adathordozós használható biztonsági funkciókról nyújt. Hivatkozások találhatók cikkekre mutató képet ad szolgáltatások részleteit, egyszerűen végezhető további vizsgálat minden témában.

Az alábbiakban nem vonatkozik a jelen cikkben a témakörök:

-   [Adatkezelési sík biztonsági](#management-plane-security) – tárterület-fiókja biztonságossá tétele

    A kezelő sík áll, az erőforrások használt tárterület-fiókjának kezelését. Ebben a részben fogja megvitatjuk, az erőforrás-kezelő Azure telepítési modell és szerepköralapú hozzáférés szerepalapú használatáról a tárterület-fiók elérésének szabályozása. A tároló fiók kulcsok, és hogyan újragenerálása őket kezelésével kapcsolatban is előadás.

-   [Adatok biztonsági sík](#data-plane-security) – az adatokhoz való hozzáférés biztonságossá tétele

    Ebben a részben áttekintjük a tárterület-fiókjában BLOB, a fájlokat, a sorok és a táblázatok, például a tényleges data objects hozzáférést megosztott Access aláírások használatával és az Access-házirendek tárolt. Témákat vesszük sorra szolgáltatói Társítások és a fiók szintű Társítások. Azt is megtalálja, egy adott IP-cím (vagy az IP-címek) való hozzáférés korlátozása, hogy miként szeretné korlátozni a használt HTTPS protokollt, illetve vissza szeretné vonni az Access megosztott aláírás nem várja meg, hogy lejárjon.

-   [A hálózaton átvitt titkosítás:](#encryption-in-transit)

    Ez a szakasz ismerteti a védik az adatokat, amikor azt át és elhalványítása Azure tároló. Megvitatjuk fogja HTTPS és Azure fájlmegosztások a kis-és Középvállalatok 3.0 által használt titkosítással ajánlott használatát. Azt a fog is ajánljuk figyelmébe az ügyféloldali titkosítás, amely lehetővé teszi az adatok titkosítása, mielőtt tárolóba ügyfélalkalmazás át és visszafejteni az adatokat, után átkerül tároló ki.

-   [A többi titkosítás:](#encryption-at-rest)

    Az előadás tároló szolgáltatás titkosítási (SSE), és hogyan kapcsolhatja be tároló fiókot, a továbbfejlesztett fájlblokkolás BLOB, oldal BLOB, így és Azure tárolóhoz írásakor az automatikusan titkosított BLOB Hozzáfűzés. Azt is néznének hogyan Azure merevlemez-titkosítás használatára, és ismerje meg a alapvető különbség és a lemez titkosítást és SSE ügyféloldali titkosítást és eseteit. Megnézi fog röviden, az Amerikai Egyesült Államok kormányzati számítógépek a FIPS.

-   Azure-tárterületét hozzáférés naplózása [Tároló Analytics](#storage-analytics) használatával

    Ez a szakasz ismerteti a információk keresése a tárhely analytics naplókban kérésekben. Hogy miként ajánljuk figyelmébe az valós tároló analytics naplóadatokat, és megtudhatja, hogy miként discern e kérelem érkezik, a tárhely fiók termékkulccsal, egy megosztott Access aláírással ellátott vagy névtelenül és kell-e sikeres vagy sikertelen volt.

-   [Böngészőalapú ügyfelek CORS használatával engedélyezése](#Cross-Origin-Resource-Sharing-CORS)

    Ez a szakasz folytatott beszélgetést, arról, hogy miként engedélyezése határokon származási erőforrások megosztása (CORS). Fogja megvitatjuk-tartományok hozzáférést, és azt a CORS funkciókhoz épített Azure-tároló kezelése.

##<a name="management-plane-security"></a>Adatkezelési sík biztonsági

A kezelő sík, ahol a műveleteket, amelyek befolyásolják a tárhely fiókon. Például, létrehozhat vagy tároló fiók törlése, tároló fiókok lista beszerzése az előfizetés, tároló fiók kulcsok, vagy a tárhely fiók billentyűk újragenerálása.

Új tároló fiók létrehozásakor klasszikus vagy az erőforrás-kezelő telepítési modell jelölje ki. A Klasszikus modell erőforrások létrehozása az Azure-ban csak lehetővé teszi, hogy a mindent access, az előfizetést, és kapcsolja be, a tárterület-fiókot.

Ez az útmutató az erőforrás-kezelő modell, amely a tárterület-fiókok létrehozása az ajánlott eszköze koncentrál. Az erőforrás-kezelő tároló fiókok, helyett a teljes előfizetés amely eléréséhez az access egy további függvényt szintet úgy, hogy a kezelő sík szerepköralapú hozzáférés szerepalapú használatával szabályozhatja, hogy.

###<a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Szerepköralapú hozzáférés szerepalapú tároló fiókját védelme

Vegyük beszélni RBAC van, és hogyan használhatja azt. Minden egyes Azure előfizetés az Azure Active Directory tartalmaz. Felhasználók, csoportok és alkalmazások e könyvtárból is hozzáférési jogosultsággal erőforrások Azure-előfizetésben, amelyek az erőforrás-kezelő telepítési modell. Ez a szerepköralapú hozzáférés szerepalapú nevezik. A hozzáférés kezelése, az [Azure portál](https://portal.azure.com/), az [Azure CLI eszközök](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)vagy az [Azure tároló erőforrás szolgáltató REST API -khoz](https://msdn.microsoft.com/library/azure/mt163683.aspx)is használhatja.

Az erőforrás-kezelő modellel helyezi el a tárterület-fiók egy erőforrás csoport és a vezérlő hozzáférést, hogy adott tárterület-fiók használata az Azure Active Directory kezelése síkban. Ha például úgy adhat bizonyos felhasználók az azt jelenti, hogy a hívóbillentyűk a tárhely fiók, míg mások a tárterület-fiókkal kapcsolatos információk megtekintése, de nem tud hozzáférni a tárhely fiók billentyűk.

####<a name="granting-access"></a>Hozzáférés engedélyezése

Hozzáférés a megfelelő RBAC szerepkör hozzárendelése a felhasználók, csoportok és alkalmazásokat, a jobb oldali hatókör alapján. Való hozzáférést a teljes előfizetést, akkor rendeljen egy szerepkört, az előfizetés szintjén. Hozzáférés az összes az erőforrások erőforráscsoport magára a erőforrás csoportra engedélyek megadásával adhat meg. Adott erőforrások, például a tárterület-fiókok szerepköröket rendelhet.

Az alábbiakban a fő címzett pontok kell tudnia a kezelési műveletek Azure tárterület-fiók eléréséhez RBAC használatáról:

-   Amikor kioszt access, amelyen alapvetően rendeljen egy szerepkört annak a fióknak, amelyet el szeretne érni. Megadhatja, hogy hozzáférést a fiókhoz társított tároló kezelésére szolgáló műveletek, de nem a data objects fiók. Ha például az engedélyeket a tárterület-fiók (például a redundancia), a Tulajdonságok beolvasásához, de nem egy tároló vagy belül Blob-tárolóhoz tároló adatait.

-   Mások a data objects tároló fiók eléréséhez szükséges engedélyek akkor is engedélyt adhat nekik a tárterület-fiók felolvastatása, és a felhasználó számára használhatja ezeket a billentyűk a BLOB, sorok, táblázatok és fájlok eléréséhez.

-   Szerepkörök rendelhetők egy adott felhasználói fiók, a felhasználók egy csoportja, illetve egy adott alkalmazást.

-   Mindegyik szerepkör műveletek és a műveletek nem listáját tartalmazza. Például a virtuális gép munkatársi szerepkörök tartalmaz-e a "listKeys" művelet, amely lehetővé teszi, hogy a fiók tároló billentyűk olvasható. A közös munka "Nem műveletek" tartalmaz, például frissítése az Active Directory számára a hozzáférést.

-   Tárterület-szerepkörei belefoglalható (azonban nem korlátozódik) a következő:

    -   Tulajdonos – ezek kezelheti mindent, beleértve a hozzáférést.

    -   Közös munka – ezek bármit a tulajdonos is hozzáférés hozzárendelése kivételével. A szerepkör rendelkező tekintheti meg és a tárhely fiók billentyűk újragenerálása. A tároló fiók billentyűkkel a data objects elérhetik azt.

    -   Olvasót – tárterület-fiókkal, kivéve titkos kulcsok kapcsolatos információk megtekintése. Például ha, rendeljen egy szerepkört a tárterület-fiók olvasó engedélyekkel rendelkező valaki, a tárhely fiók tulajdonságainak megtekintése, de nem bármilyen módosítást végez az a tulajdonságokat és a tárterület-fiók billentyűk megtekintéséhez.

    -   Tárterület-fiók közös munka – azok is a tárhely fiók kezelése – olvasnak az előfizetést az erőforrás-csoportok és erőforrások, és hozzon létre és előfizetési resource csoport telepítések kezelése. A fiók az azt jelenti, hogy az adatok sík elérhetik tároló billentyűk azokat is elérheti.

    -   Felhasználói hozzáférés rendszergazda – ezek is felhasználói hozzáférés kezelése a tárterület-fiókjába. Például azok is hozzáférést olvasót egy adott felhasználó számára.

    -   Virtuális gép közreműködői – ezek kezelhetik a virtuális gépeken futó, de nem a tárhely fiókot, amelyhez csatlakozott. A szerepkör felsorolhatja a tárhely fiók kulcsok, ami azt jelenti, hogy a felhasználót, akinek a szerepkör hozzárendelése frissítheti az adatokat sík.

        Ahhoz, hogy a felhasználó virtuális gép létrehozása engedélyezni szeretné a megfelelő virtuális fájl létrehozása a tárterület-fiókkal rendelkeznek. Ehhez szükségük engedélyezni szeretné a tárhely fiókkulcs beolvasásához, és adja át az API-t a virtuális létrehozása. Ezért így azok felsorolhatja a tárhely fiók billentyűk ezzel az engedéllyel kell rendelkezniük.

- Egyéni szerepkörök definiálása szúrhatók, egy funkció, amely lehetővé teszi, hogy egy sor a listából az Azure-erőforrások elvégezhető elérhető műveletek összeállítása.

- A felhasználó rendelkezik, be kell állítani a az Azure Active Directory előtt hozzárendelheti egy szerepkört őket.

- Kik nyújtott/visszavont access szeretne vagy akitől, és milyen PowerShell alrendszerrel vagy az Azure CLI hatókörén milyen típusú jelentést hozhat létre.

####<a name="resources"></a>Erőforrások

-   [Azure Active Directory-szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)

    Ez a cikk ismerteti, hogy az Azure Active Directory szerepköralapú hozzáférés-vezérlés és működéséről.

-   [RBAC: Szerepkörök beépített](../active-directory/role-based-access-built-in-roles.md)

    Ez a cikk részletes összes RBAC elérhető beépített szerepkört.

-   [Erőforrás-kezelő-telepítés és üzembe klasszikus ismertetése](../resource-manager-deployment-model.md)

    Ez a cikk ismerteti, hogy az erőforrás-kezelő telepítési és klasszikus telepítési modellek, és bemutatja az erőforrás-kezelő és a erőforrás csoport használatának előnyei

-   [Azure számítási, hálózati és tároló szolgáltatók csoportban az Azure erőforrás-kezelő](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

    Ez a cikk ismerteti, hogy az erőforrás-kezelő modell alatt az Azure számítja ki, és hálózati tároló szolgáltatók működéséről.

-   [A REST API-val irányító szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-manage-access-rest.md)

    Ez a cikk bemutatja, hogyan a REST API-lel való kezelése RBAC.

-   [Azure tároló erőforrás szolgáltató REST API-hivatkozás](https://msdn.microsoft.com/library/azure/mt163683.aspx)

    Ez a tárterület-fiókjának kezelését programozás útján is használhatja az API-k hivatkozását.

-   [Azure erőforrás-kezelő API-val auth fejlesztői útmutató](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

    Ebből a cikkből megtudhatja, hogyan kívánja hitelesíteni a az erőforrás-kezelő API-k használata.

-   [Microsoft Azure Ignite a szerepköralapú hozzáférés-vezérlés](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Ez a 2015-ös MS Ignite konferencia el egy csatorna 9 a hivatkozást. Ebben a munkamenetben azok beszélni feltárása ajánlott eljárások megalkotása Azure előfizetések Azure Active Directory használatával való hozzáférés biztonságossá tétele és a hozzáférés kezelése és jelentéskészítési lehetőségek az Azure-ban.

###<a name="managing-your-storage-account-keys"></a>A tároló fiók kulcsok kezelése

Tárterület-fiók billentyűk 512 bites karakterláncok, hogy a tárterület-fiók nevét, valamint a data objects tárolt tároló fiók elérésére használható, például BLOB, belüli táblázat üzenetek, fájlok, és az Azure-fájlok megosztása a Azure által létrehozott. A tároló fiók billentyűk szabályozza a hozzáférést az adatok sík tárterület-fiókhoz tartozó való hozzáférés szabályozása.

Minden tároló "Kulcs 1" és "Főbb 2" az [Azure-portálra](http://portal.azure.com/) , és a PowerShell-parancsmagok néven két billentyűk rendelkezik. Ezek helyreállíthatja, manuálisan több módszerek valamelyikével, beleértve a, de nem korlátozódik az [Azure portál](https://portal.azure.com/), PowerShell, az Azure CLI, vagy a programozás útján használata a .NET tároló ügyfél tárat vagy az Azure tároló szolgáltatások REST API-val.

Vannak olyan érvek a tárolás fiók kulcsok újbóli tetszőleges számú.

-   Előfordulhat, hogy újragenerálása őket, rendszeresen biztonsági okokból.

-   A tároló fiók billentyűk volna újragenerálása, ha valaki felügyelt ellophatja alkalmazásba, valamint a szoftveresen kötött és a kereséskonfigurációs fájlban mentett kulcs biztosítva teljes hozzáférést biztosít a tárterület-fiók.

-   Kulcs egy másik eset, ha a csapat van, amely megőrzi a tárhely fiókkulcs tároló Intéző alkalmazást használ, majd egy, a csapattagok. Az alkalmazás folytatni a munkát, hozzáférést biztosítva a őket a tárterület-fiók után megszűnt is legyenek. Ez ténylegesen az oka elsődleges fiók szintű megosztott Access aláírások létrehozza őket – -fiók szintű Társítások helyett a hívóbetűk tárolása konfigurációs fájl is használhatja.

####<a name="key-regeneration-plan"></a>Kulcs terv

Ha nem szeretné, csak újbóli az egyes tervezés nélkül esetén billentyűt. Az ehhez szükséges lépéseket, ha meg, hogy tárterületet fiókjához okozhatják a fő zavarok lemarad sikerült összes access. Az oka annak, hogy két kulcsa van. Több billentyűt egyszerre kell újragenerálása.

A hívóbetűk követező létrehozásakor, mielőtt kell róla, hogy az alkalmazásokat, amelyek a tárterület-fiók típusától függően változnak, valamint esetén Azure-ban bármely más szolgáltatások listája. Például ha az Azure Media Services függő a tárterület-fiók használata esetén kell újra szinkronizálja a hívóbetűk a media szolgáltatás után a kulcs követező létrehozásakor. Ha bármelyik alkalmazásokhoz, például egy tároló explorer használata esetén szüksége lesz adja meg azokat az alkalmazásokat, valamint új billentyűk. Figyelje meg, hogy VMs, amelynek virtuális fájlok vannak tárolva a tárterület-fiók esetén azok nem érinti a tárhely fiók billentyűk újragenerálása.

Helyreállíthatók a billentyűparancsok az Azure-portálon. Billentyűk vannak újbóli létrehozása után azok készíthet legfeljebb 10 perc kell szinkronizálni a tárhely szolgáltatásban.

Ha készen áll, az alábbiakban az általános folyamat részletező, hogy módosítania kell a termékkulcsot. Ebben az esetben a azon a feltételezésen az, hogy jelenleg a kulcs 1 használ, és fogja módosítása minden kulcs 2 használja helyette.

1.  Kulcs 2 annak érdekében, hogy az biztonságos újragenerálása. Ehhez az Azure-portálon.

2.  Összes a tárhely kulcs tároló alkalmazást módosítsa a tárhely kulcs kulcs 2 új érték használatához. Tesztelése és az alkalmazás Közzététel gombra.

3.  Után az alkalmazások és szolgáltatások felfelé, és sikeresen fut, újragenerálása 1 billentyűt. Ez biztosítja, hogy bárki, akinek Ön nem kifejezetten adott az új kulcs többé nem lesz hozzáférésük a tárterület-fiókjába.

Ha jelenleg a kulcs 2 használ, ugyanazt az eljárást használhatja, de a fő neveket fordított.

Áttelepítheti néhány nap alatt, akkor az új billentyűvel egyes alkalmazások módosítása és közzétéve. Miután mindegyiket után kell majd térjen vissza és újbóli létrehozása a régi billentyűt, így a már nem működik.

Egy másik, hogy a tárhely fiókkulcs helyezi el egy, a titkos [Azure kulcs tárolóból elemre](https://azure.microsoft.com/services/key-vault/) , és az alkalmazások beolvasni a kulcsot a tárból van. Kattintson a kulcs újragenerálása, és frissítse az Azure kulcs tárolóból elemre, ha az alkalmazások nem kell újratelepítésre, mert az illető fog felveszi az új kulcs a az Azure kulcs tárolóból elemre automatikusan. Ne feledje, hogy az alkalmazást, olvassa el a billentyűt minden alkalommal, amikor szüksége van rá lehet, vagy a memóriában a gyorsítótárban, és ha, használata esetén nem sikerül beolvasni a kulcsot ismét a az Azure kulcs tárolóból elemre.

A tárhely kulcsok biztonsági egy másik szintjének hozzáadja is használatával Azure kulcs tárolóból elemre. Ezt a módszert, ha soha nem lesz a tárhely kulcs szoftveresen kötött a konfigurációs fájl, amely, hogy valaki hozzáférést a billentyűk külön engedélye nélkül az út eltávolítja.

Egy másik Azure kulcs tárolóra előnye a hozzáférés vezérléséhez az Azure Active Directory használata billentyűk. Ez azt jelenti, hogy az alkalmazások, a billentyűk lekérése az Azure kulcs tárolóból elemre, és egyéb alkalmazások nem lesznek elérheti a billentyűk engedélyt őket kifejezetten anélkül, hogy kell néhány hozzáférést is.

Megjegyzés: ajánlott egyszerre minden, az alkalmazás csak a billentyűk egyikének használatára. Kulcs 1 néhány helyen és a kulcs 2 más használata esetén nem tudja elforgatni a billentyűk néhány alkalmazás hozzáférést adatvesztés nélkül.

####<a name="resources"></a>Erőforrások

-   [Azure tároló fiókokról](storage-create-storage-account.md#regenerate-storage-access-keys)

    Ez a cikk áttekintést nyújt a tárterület-fiókok, és azt ismerteti, hogy a megtekintés, másolása és újragenerálása tároló hívóbetűk.

-   [Azure tároló erőforrás szolgáltató REST API-hivatkozás](https://msdn.microsoft.com/library/mt163683.aspx)

    Ez a cikk beolvasása a tárhely fiók kulcsok és a tárhely fiók billentyűk bezárhatja a REST API-Azure-fiók adott cikkekre mutató hivatkozásokat tartalmaz. Megjegyzés: Ez az erőforrás-kezelő tároló fiókok.

-   [Tárterület-fiókok műveletek](https://msdn.microsoft.com/library/ee460790.aspx)

    Ez a cikk a tárhely szolgáltatáskezelő REST API-val hivatkozás beolvasása és bezárhatja a tárhely fiók kulcsok a REST API adott cikkekre mutató hivatkozásokat tartalmaz. Megjegyzés: Ez az a klasszikus tároló fiókokhoz.

-   [Főbb felügyeleti – használatával az Azure Active Directory Azure tároló adatokhoz való hozzáférés kezelése a vásárolnia](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

    Ez a cikk bemutatja, hogyan használja az Active Directory való hozzáférés korlátozása a Azure tároló hívóbetűi Azure kulcs tárolóból elemre. Azt is megtudhatja, hogy hogyan egy Azure automatizálási feladat újbóli óránkénti kombinálásával a billentyűket.

##<a name="data-plane-security"></a>Adatok sík biztonsági

A módszereket, amellyel biztonságossá a data objects Azure – a BLOB, sorok, táblázatok és fájlok tárolóban lévő adatok sík biztonsági hivatkozik. Láthatta, hogy módszerek az adatok és a biztonsági titkosítása hálózaton átvitt adatok során, de hogyan megtekinti az objektumokhoz való hozzáférés engedélyezése?

Kétféleképp alapvetően maguk adatobjektumok való hozzáférés szabályozása. Az első által a tárhely fiók billentyűk való hozzáférés szabályozása, és a második használja a megosztott Access aláírások hozzáférést bizonyos adatobjektumok az egy adott idő mennyiségét.

Egy kivétellel jegyezze fel, hogy engedélyezheti, nyilvános hozzáférés a BLOB szeretné a hozzáférési szint beállítása a tároló, amely a BLOB megfelelően. Ha az access Blob vagy tároló tároló beállított, nyilvános olvasási hozzáférést a eszközébe BLOB-lehetővé. Ez azt jelenti, hogy bárki, akinek van egy URL-címe a tároló blob megnyithatja a böngészőben egy megosztott Access aláírás használatának és a tárhely fiók billentyűk problémákat nélkül.

###<a name="storage-account-keys"></a>Tárterület-fiók kulcsok

Tárterület-fiók billentyűk 512 bites karakterláncok használható, a tárhely fiók nevét, valamint elérheti a tárterület-fiókjában tárolt data objects Azure által létrehozott.

Például akkor is BLOB olvasni, írni sorban várakozó, táblázatok létrehozása és módosítása a fájlok. Az Azure portal végezhető el az alábbi műveletek közül számos vagy közül számos tároló Intéző alkalmazást. Ezek a műveletek elvégzéséhez a REST API-t vagy a tároló ügyfél tárai egyik használandó kód is írhat.

Az [Adatkezelési sík biztonsági](#management-plane-security)tárgyalt szakaszában, a tárhely kulcsok klasszikus tároló fiókot is hozzáférést teljes hozzáférést biztosítva a az Azure előfizetés. Tárterület-fiókját az Azure erőforrás-kezelő modell tároló billentyűparancsai a hozzáférést a szerepköralapú hozzáférés szerepalapú keresztül szabályozható.

###<a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Hogyan kell a fiók elérése az Access aláírások megosztott és az Access-házirendek tárolt objektumok hozzáférés átadása

A megosztott Access aláírás egy olyan biztonsági jogkivonat kapcsolódhat URI, amely lehetővé teszi, hogy a tárterület-objektumok hozzáférés átadása, és adja meg a kényszerek, például az engedélyek és hozzáférés a dátum/idő tartomány tartalmazó karakterlánc.

BLOB, tárolók, üzenetek, fájlok és táblák hozzáférést adhat meg. Táblázatok, a személyek a táblázat egy cellatartomány, amelyet a felhasználó hozzáférjen partíciót, illetve a sor kulcs tartományok megadásával eléréséhez szükséges engedély ténylegesen biztosíthat. Ha például földrajzi helye partíciót használatával tárolt adatok esetén nem ad valakinek Kalifornia csak az adatok való hozzáférés.

A példában egy másik, előfordulhat, hogy adjon meg egy webalkalmazás Társítások jogkivonat, amely lehetővé teszi a bejegyzések írni egy várólista, és adjon egy dolgozó szerepkört alkalmazás üzenetek lekérése a sor-és feldolgozni azokat a Társítások jogkivonat. Vagy ha nem ad egy ügyfél Társítások jogkivonat őket tároló Blob-tárolóban lévő képek feltöltése használja, és engedélyezheti a webes alkalmazás olvassa el a képek. Mindkét esetben nem vonatkozik elkülönítése – is a minden alkalmazás csak annak érdekében, hogy a feladat végrehajtásához igénylő hozzáférést kap. Ez a lehetőség való hozzáférés aláírások megosztott.

####<a name="why-you-want-to-use-shared-access-signatures"></a>Miért kívánt megosztott Access aláírások használata

Miért lehet szükség egy Társítások használata helyett a tárhely fiókkulcs, amelyek így sokkal egyszerűbb csak jelzéseket? A tároló fiókkulcs jelzéseket szeretné megosztani a billentyűket a tárhely Királyság van. Ez a teljes hozzáférést biztosít. Mások esetleg a nyílbillentyűkkel, és töltse fel a teljes zenét tár a tárterület-fiókjába. Azt is is vírusfertőzött verziók cserélje ki a fájlok, vagy az adatok ellopására alkalmas. Hozzáférést biztosítva a nem vagyok a gépnél korlátlan tárterület-fiókja, amit nem kell venni enyhe.

Az Access aláírások megosztott úgy adhat egy ügyfél az idő korlátozott mennyiségű szükséges engedélyeket. Például ha valaki feltöltése folyamatban van blob fiókját, biztosíthat őket írási csak elég ideig töltse fel a blob (attól függően, az blob méretének természetesen). És Ha meggondolja magát, hogy hozzáférést vonhatja.

Továbbá megadhatja, hogy egy Társítások használatával kérések bizonyos IP-cím vagy Azure kívüli IP-címtartományokat korlátozott. Hogy kérések (HTTPS- vagy a HTTP-/ HTTPS) egy adott protokoll használatával is szükséges. Ez azt jelenti, hogy ha csak HTTPS-forgalom engedélyezni szeretné, beállíthatja a szükséges protokoll HTTPS csak és azzal blokkolhatja a HTTP-forgalmat.

####<a name="definition-of-a-shared-access-signature"></a>A megosztott Access aláírás meghatározása

A megosztott Access aláírás egy olyan hozzáfűzi az URL-címe az erőforrás a lekérdezés paraméterei

az access engedélyezett és az időtartamot, amelynek az access engedélyezett (permitted) információt, amely tartalmaz. Íme egy példa; a URI blob olvasási hozzáférést biztosít a öt perc. Megjegyzés: a lekérdezés paraméterei Társítások kell kódolva URL-CÍMÉT, például % 3A kettőspont (:) vagy % 20 használatára.

    http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL to the blob)
    ?sv=2015-04-05 (storage service version)
    &st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
    &se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
    &sr=b (resource is a blob)
    &sp=r (read access)
    &sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
    &spr=https (only allow HTTPS requests)
    &sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)

####<a name="how-the-shared-access-signature-is-authenticated-by-the-azure-storage-service"></a>Hogyan hitelesíti a megosztott Access aláírást az Azure Tárhelyszolgáltatáshoz

Amikor a tároló szolgáltatás a kérelem érkezik, a bemeneti lekérdezés paraméterei, és létrehoz egy aláírást, ugyanazzal a módszerrel a hívó programnak. Kattintson a két aláírások hasonlítja össze. Ha elfogadja, a tároló szolgáltatás a tárhely verzióját, hogy érvényes, győződjön meg róla, hogy az aktuális dátum és idő a megadott ablakon belül, ellenőrizze, hogy az access kért felel meg a kérelme stb ellenőrizheti.

Például a fenti URL, ha az URL-címe helyett blob,-fájlba lett mutató a kérelem volna sikertelen lesz, mert azt jelzi, hogy az Access megosztott aláírás blob. Ha a többi parancs a hívott blob frissítése volt, mivel a megosztott Access aláírás határozza meg, hogy engedélyezve van-e a csak olvasási hozzáférést szeretne sikertelen.

####<a name="types-of-shared-access-signatures"></a>Megosztott Access aláírások típusai

-   A szolgáltatás szintű Társítások használható adott forrásokat tárterület-fiókjában. Néhány példa a keres BLOB-tárolóban, blob letöltése, egy táblázatban entitás frissítése, üzenetek hozzáadása egy sorba vagy fájl feltöltése a fájlmegosztás listáját.

-   A fiók szintű Társítások mindent, ami egy szolgáltatói Társítások használható eléréséhez használható. Ezenkívül adhat a beállítások információforrások, amelyek egy szolgáltatói Társítások, például a tárolók, táblák, sorok és fájlmegosztások lehetősége nem engedélyezettek. Több szolgáltatásai egyszerre is megadhatja. Ha például meg lehet adni a jegyzetfüzethez hozzáférést BLOB- és a fájlokat a tárterület-fiókjában.

####<a name="creating-an-sas-uri"></a>Egy Társítások URI létrehozása

1.  Létrehozhat egy alkalmi URI, igény definiálása az összes lekérdezésparaméter minden alkalommal, amikor.

    Ez nagyon rugalmas, de ha logikai csoportja, amelyek hasonlítanak minden alkalommal paramétereit, az Access tárolt házirenddel ötlet jobb.

2.  Egy teljes tároló, fájlmegosztás, táblázat vagy várólista tárolt hozzáférési házirendet hozhat létre. Ezután-et is ez alapjául létrehozhat Társítások URL-címe. Access-házirendek tárolt alapján engedélyek egyszerűen visszavont is. Beállíthatja, hogy minden tároló, várólista, táblázat vagy fájlmegosztás definiált legfeljebb 5 házirendek.

    Például ha, amelyeket lesz több személynek, olvassa el az adott tárolóban BLOB van, létrehozhat egy tárolt hozzáférési házirendet, amely szerint a "olvasási hozzáférést" és az egyéb beállításokat, amelyek azonos lesz minden alkalommal, amikor. Kattintson egy Társítások URI a tárolt hozzáférési házirend beállításainak használatával, és a lejárati dátum/idő megadása hozhat létre. Az az előnye, hogy nem kell minden alkalommal adja meg a lekérdezés mindegyikének.

####<a name="revocation"></a>Visszavonási

Tegyük fel, hogy a Társítások napvilágra kerülhetett, vagy meg szeretné változtatni azt a vállalati biztonsági vagy a szabályozási megfelelőségről követelményeknek. Hogyan ezt csak egy adott Társítások használatával erőforráshoz való hozzáférést? Ez attól függ, hogy a Társítások URI létrehozását.

Alkalmi URI használ, akkor három lehetőség közül választhat. Rövid jelszólejárati házirendekről Társítások tokenek kibocsátó, és egyszerűen megvárja, amíg a Társítások lejár. Átnevezheti és törölheti az erőforrás (feltételezve, hogy a token egyetlen objektum lett hatóköre). Módosíthatja a tárhely fiók billentyűket. Utolsó beállítás a nagy befolyásolhatják, attól függően, hogy hány szolgáltatások adott tároló fiókot használ, és valószínűleg nem valamit, amit szeretne tenni bizonyos tervezés nélkül.

Egy Access tárolt házirend származás Társítások használja, ha az access távolíthatja a tárolt hozzáférési házirend visszavonása – a csak módosíthatja azt, már lejárt vagy teljesen eltávolíthatja azt. Azonnal érvénybe lép, és minden Access tárolt házirendhez használatával létrehozott Társítások érvényteleníti. Frissítése vagy eltávolítása a tárolt hozzáférési házirend is hatással mások adott tároló, megosztott fájl megnyitása táblázat vagy várólista keresztül Társítások, ha az ügyfelek kerülnek, így azok kérése egy új Társítások, amikor a régit válik érvénytelen, de ez működik jól.

Mivel az egy Access tárolt házirend származás Társítások használatával lehetővé teszi, hogy Társítások azonnal visszavonása, mindig a tárolt Access házirendek amikor csak lehetséges használata ajánlott célszerű is.

####<a name="resources"></a>Erőforrások

További információ a megosztott Access aláírások használatával és tárolt Access házirendek teljes példákat olvassa el az alábbi cikkekben:

-   Ezek a útmutatót.

    -   [Társítások szolgáltatás](https://msdn.microsoft.com/library/dn140256.aspx)

        Ez a cikk példákat BLOB, az üzenetek, a táblázat tartományok és a fájlok használata egy szolgáltatói Társítások.

    -   [A szolgáltatás Társítások megépítése.](https://msdn.microsoft.com/library/dn140255.aspx)

    -   [Fiók Társítások megépítése.](https://msdn.microsoft.com/library/mt584140.aspx)

-   Ezek a .NET-ügyfél tárat a hozhat létre Access aláírások megosztott és a hozzáférési szabályok tárolt oktatóanyagok.

    -   [Megosztott Access aláírások (Társítások) használata](storage-dotnet-shared-access-signature-part-1.md)

    -   [Megosztott Access aláírások, 2 rész: Létrehozása és a Blob-szolgáltatás használatához a Társítások](storage-dotnet-shared-access-signature-part-2.md)

        Ez a cikk tartalmazza a Társítások modell megosztott Access aláírások példák magyarázatot, és Társítások célszerű javaslatok használata. Azt is ismertetjük, a visszavonás, a megadott engedélyeket.

-   A hozzáférés korlátozása IP-cím (IP hozzáférés-vezérlési listák) alapján

    -   [Mi az hozzáférés-vezérlési lista (hozzáférés-vezérlési listák) zárólap?](../virtual-network/virtual-networks-acl.md)

    -   [A szolgáltatás Társítások megépítése.](https://msdn.microsoft.com/library/azure/dn140255.aspx)

        Ez a szolgáltatás szintű Társítások; a hivatkozás cikk Példa IP ACLing tartalmazza.

    -   [Fiók Társítások megépítése.](https://msdn.microsoft.com/library/azure/mt584140.aspx)

        Ez a fiók szintű Társítások; a hivatkozás cikk Példa IP ACLing tartalmazza.

-   Hitelesítés

    -    [Az Azure tároló szolgáltatások hitelesítés](https://msdn.microsoft.com/library/azure/dd179428.aspx)

-   A megosztott Access aláírások oktatóanyag első lépések

    -   [Társítások oktatóanyag első lépések](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

##<a name="encryption-in-transit"></a>A hálózaton átvitt titkosítás:

###<a name="transport-level-encryption--using-https"></a>Átviteli szintű titkosítás – HTTPS használatával

Egy másik kell tennie az Azure tároló adatok biztonsága érdekében lépésként titkosíthatja az adatokat az ügyfél és Azure tároló között. Az első ajánlást környezetbe mindig a [HTTPS](https://en.wikipedia.org/wiki/HTTPS) protokollt, amely biztonságos kommunikáció biztosítja az nyilvános interneten keresztül.

Amikor a tárterület-beli objektumokkal hívja fel a REST API-k vagy elérése HTTPS mindig kell használni. **A megosztott Access aláírások**használható meghatalmazotti hozzáférést Azure tárterület-objektumok, amelyek is megadhatja, hogy csak a HTTPS protokollt a megosztott Access aláírások, biztosítva, hogy bárki, hivatkozásokat Társítások alapelemeket küldése fogja használni a megfelelő protokoll használata esetén használható beállítást.

####<a name="resources"></a>Erőforrások

-   [HTTPS-alkalmazás Azure alkalmazás szolgáltatás engedélyezése](../app-service-web/web-sites-configure-ssl-certificate.md)

    Ez a cikk bemutatja, hogyan HTTPS ahhoz, hogy az Azure-webappokban.

###<a name="using-encryption-during-transit-with-azure-file-shares"></a>Az Azure fájlmegosztások titkosítással továbbítás során

Azure fájltároló támogatja a HTTPS, amikor a REST API, de több mint egy kis-és Középvállalatok fájlmegosztás gyakran használt csatolva van a virtuális. Kis-és Középvállalatok 2.1 nem támogatja a titkosítást, így kapcsolat csak engedélyezve van az Azure-ban ugyanazon régió belül. Azonban kis-és Középvállalatok 3.0 támogatja a titkosítást, és a Windows Server 2012 R2 kínál, a Windows 8, Windows 8.1 és a Windows 10 lehetővé tevő határokon-régió elérése és még az asztali access.

Figyelje meg, hogy Azure fájlmegosztások Unix kínál, miközben a kis-és Középvállalatok Linux ügyfél egyelőre nem támogatják a titkosítást, így az Azure régión belüli csak akkor engedélyezett az access. Titkosítás támogatása Linux be van kapcsolva a kis-és Középvállalatok funkciók felelős Linux fejlesztők tudnivalókat foglaltuk össze. Titkosítás hozzáadása őket, akkor egy Linux Azure fájlmegosztás elérésére, mint a Windows ugyanazon lehetősége.

####<a name="resources"></a>Erőforrások

-   [Azure fájltároló használata Linux](storage-how-to-use-files-linux.md)

    Ez a cikk bemutatja, hogyan egy Azure fájlmegosztás csatlakoztatásához Linux rendszerben és a fájlok feltöltése és letöltése.

-   [A Windows Azure fájltároló – első lépések](storage-dotnet-how-to-use-files.md)

    Ez a cikk áttekintést nyújt Azure fájlmegosztások és csatlakoztatni, és használhatja őket a PowerShell és a .NET használatával.

-   [Sárga ceruzák Azure tárhely](https://azure.microsoft.com/blog/inside-azure-file-storage/)

    Ez a cikk Azure fájltároló az általános elérhető bejelenti, és ismerteti a kis-és Középvállalatok 3.0-titkosítás műszaki részleteit.

###<a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Ügyféloldali titkosítással tárolóhoz küldött adatok védelme

Egy másik lehetőség, amellyel győződjön meg arról, hogy az adatok biztonságos közben ügyfélalkalmazások és tárolására között átadott ügyféloldali titkosítás. Az adatok átvitele Azure tárolóba előtt titkosítva. Adatok beolvasása az Azure-tárhelyről, ha az adatok visszafejteni, után őket az ügyféloldalon.. Annak ellenére, hogy az adatok titkosítva végig a vezetékes használni, azt javasoljuk HTTPS, is használhatja, mert az adatok integritását érintő hálózati hibák csökkentésében súgó beépített adatok integritását szempontú vizsgálatok.

Ügyféloldali titkosítás megegyezik is a többi, az adatok titkosítására titkosított formájában tárolja az adatokat. Fogja megvitatjuk, ez a szakaszban részletesebb a [többi a titkosítást](#encryption-at-rest).

##<a name="encryption-at-rest"></a>A többi titkosítás:

Vannak, amelyekkel a többi titkosítási három Azure szolgáltatás. Azure lemez titkosítási titkosítása a IaaS virtuális gépeken futó operációs rendszer és az adatok lemezt használják. A másik két – ügyféloldali titkosítást és SSE – is mindkét Azure-tárolóban lévő adatok titkosítására használt. Nézzük meg, majd végezze el a összehasonlítása és látható, ha egyenként már nem használható.

Miközben ügyféloldali titkosítás segítségével titkosítsa az adatokat a hálózaton átvitt (amely a titkosított formájában tárolója tárolja is), előfordulhat, hogy inkább egyszerűen HTTPS használata közben a továbbított, és valamilyen módon automatikusan titkosítása tárolt adatokhoz van. Kétféleképpen ehhez – Azure lemezre titkosítást és SSE. Közvetlenül a az adatok VMs által használt operációs rendszer és az adatok lemezen titkosítására használt egyik, és a többi Azure Blob-tárolóhoz írt adatok titkosítására szolgál.

###<a name="storage-service-encryption-sse"></a>Tárterület titkosítását (SSE)

SSE lehetővé teszi, hogy a tároló szolgáltatás automatikusan titkosíthatja az adatokat, ír be Azure tárolóhoz kérése. Azure-tárhelyről olvasni az adatokat, ha azt fogja fejthető vissza a tárterület-szolgáltatás által visszaküldött előtt. Ez lehetővé teszi, hogy az adatok biztonságos kódot módosítsa vagy adja hozzá a kódot és más alkalmazások nélkül.

Ez a beállítás, amely a teljes tárterület fiók vonatkozik. Engedélyezése, és a beállítás értékének módosításával tiltható le a ezt a szolgáltatást. Ehhez az Azure-portálra, PowerShell, az Azure CLI, a tárhely erőforrás szolgáltató REST API-t vagy a .NET-tároló ügyfél tár is használhatja. SSE alapértelmezés szerint ki van kapcsolva.

Ekkor a billentyűket a titkosítást használja a Microsoft kezeli. Azt a billentyűk eredetileg készítése és kezelése a billentyűket, valamint a normál Forgatás biztonságos tárolása, belső Microsoft házirend szerint meghatározott formátumban. A jövőben használni az azt jelenti, hogy kezelése a saját titkosítási kulcs első, és adja meg a Microsoft által kezelt billentyűk ügyfél által kezelt billentyűk áttelepítési elérési.

Ez a funkció az erőforrás-kezelő telepítési modell használatával létrehozott normál és prémium tároló fiókok érhető el. SSE csak BLOB, oldal BLOB, letiltása és a Hozzáfűzés BLOB vonatkozik. A más típusú adatokat, többek között a táblázatok, a sorok és a fájlok, nem lesz titkosítva.

Adatok csak titkosítva van, ha engedélyezve van a SSE, és az adatok Blob-tárolóhoz íródott. Engedélyezés vagy letiltás SSE nincs hatással a meglévő adatok. Más szóval ha engedélyezi ezt a titkosítást, nem térjen vissza, és nem titkosítani az adatokat, amely már létezik; sem fog visszafejteni az adatokat, amely már létezik SSE letiltásakor.

Ha azt szeretné, a szolgáltatás használatához a klasszikus tároló fiókkal, hozzon létre egy új erőforrás-kezelő tárterület-fiókot, és AzCopy segítségével másolja az adatokat az új fiók. 

###<a name="client-side-encryption"></a>Ügyféloldali titkosítás:

Ügyféloldali titkosítás azt említett, amikor a titkosítást, a hálózaton átvitt adatok tárgyal. Ezzel a funkcióval programozás útján titkosítsa az ügyfélalkalmazás végig a vezetékes elküldése előtt ahhoz, hogy Azure tárolóhoz, és az adatok programozás útján visszafejteni az Azure-tárhelyről beolvasása azt követően az adatokat.

Ezzel adja meg a hálózaton átvitt titkosítást, de a titkosítási a többi szolgáltatást is tartalmaz. Megjegyzés: a, hogy a hálózaton átvitt adatok titkosított, de továbbra is használata ajánlott HTTPS kihasználhatja a beépített adatintegritás ellenőrzi, hogy az adatok integritását érintő hálózati hibák csökkentésében súgó.

Hol használható ez a példa, ha egy webes alkalmazást, amely BLOB tárolja, és olvassa be a BLOB azt szeretné, hogy az alkalmazás és az adatok kell a lehető legnagyobb biztonságban. Ebben az esetben ügyféloldali titkosítás kellene használnia. A forgalom az ügyfél és az Azure Blob-szolgáltatás között a titkosított erőforrás, és senki sem a hálózaton átvitt adatok értelmezése, és azt be a magánjellegű BLOB pótlására.

Ügyféloldali titkosítás beépített a Java- és a .NET-tároló ügyfél tárai, viszont használó az Azure kulcs tárolóból elemre API-k, így meglehetősen egyszerű feladat végrehajtásához Önnek. Az adatok titkosítása és visszafejtése folyamata a boríték módszerrel, és tárolja a titkosítást, az egyes tárolási objektum által használt metaadatok. Például az BLOB, tárolja azt a blob-metaadatokat, a sorok, az azt felveszi azt minden várólista üzenet.

Önmagában titkosításhoz készíthet, és kezelheti a saját titkosítási kulcs. Is használhatja az Azure tároló ügyfél tár által generált kulcsok, illetve lehet, hogy a kulcs generálása Azure kulcs tárolóból elemre. A titkosítási kulcs tárolhatja a helyszíni-tárolóban lévő, vagy az Azure kulcs tárolóból elemre a tárolhatja őket. Azure kulcs tárolóból elemre a titkos kulcsok Azure kulcs tárolóból elemre a hozzáférést az adott felhasználókhoz Azure Active Directory használatával teszi lehetővé. Ez azt jelenti, hogy bárki, nem csak olvasható az Azure kulcs tárolóból elemre, és az ügyféloldali titkosítás használ kulcsok.

####<a name="resources"></a>Erőforrások

-   [Titkosítása és Azure kulcs tárolóra használata a Microsoft Azure-tárolóban lévő BLOB visszafejtése](storage-encrypt-decrypt-blobs-key-vault.md)

    Ez a cikk bemutatja, hogyan Azure kulcs tárolóból elemre, így a KEK létrehozása, és tárolhatja a PowerShell használatá tárolóra ügyféloldali titkosítást használ.

-   [Ügyféloldali titkosítást és Azure kulcs tárolóból elemre a Microsoft Azure-tárterületre](storage-client-side-encryption.md)

    Ez a cikk ügyféloldali titkosítási magyarázatot ad, és a tárhely ügyfél-tár használata titkosítása és a négy tároló szolgáltatást erőforrásainak visszafejtése talál példákat. Azt is bemutat Azure kulcs tárolóból elemre.

###<a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Azure lemez titkosítással lemezt a virtuális gépeken futó által használt titkosítása

Azure lemez titkosítás lehetővé teszi az új, amely jelenleg előzetes verzióban. Ez a szolgáltatás lehetővé teszi, hogy titkosítsa az operációs rendszer lemezre és adatok lemezre egy IaaS virtuális gép által használt. A Windows a meghajtók titkosított szabványos BitLocker titkosítási technológia használata. Linux rendszerhez, a lemezt titkosított DM – a titkosítási technológia használatával. Ez integrálva van lehetővé teszi a kezelése és kezelése a lemez titkosítási kulcsok Azure kulcs tárolóból elemre.

A merevlemez-titkosítás Azure megoldást támogatja az alábbi három ügyfél titkosítási jelenik meg:

-   Ügyfél-kódolt virtuális fájlok és az ügyfél által biztosított titkosítási kulcsok, amelyeket tárolni az Azure kulcs tárolóból elemre létrehozott új IaaS VMs titkosításának engedélyezése.

-   A Microsoft Azure piactéren lévő létrehozott új IaaS VMs titkosításának engedélyezése.

-   Engedélyezze a meglévő IaaS VMs Azure-ban már fut a titkosítást.

>[AZURE.NOTE] Azure-ban már fut Linux VMs, vagy az Azure piactéren elérhető képek alapján készített új Linux VMs esetén az operációs rendszer lemez titkosítás jelenleg nem támogatott. Az operációs rendszer kötet Linux VMs titkosítási csak, amelyeket a titkosított a helyszíni, és Azure feltöltött VMs támogatott. Ezt a korlátozást csak az operációs rendszer lemezre; egy Linux virtuális adaton titkosítását támogatja.

A megoldás támogatja a következő IaaS VMs a nyilvános előzetes verzióval, ha engedélyezve van a Microsoft Azure-ban:

-   Integrálása az Azure kulcs tárolóból elemre

-   Normál [A, D és G sorozat IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)

-   [Erőforrás-kezelő Azure](../azure-resource-manager/resource-group-overview.md) modell használatával létrehozott IaaS VMs titkosításának engedélyezése

-   Az összes Azure nyilvános [területek](https://azure.microsoft.com/regions/)

Ez a funkció biztosítja, hogy a virtuális gép merevlemezeken lévő összes adatot az Azure-tárolóban lévő többi titkosítva van-e.

####<a name="resources"></a>Erőforrások

-   [A Windows és Linux IaaS virtuális gépeken futó Azure lemez titkosítás](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

    Ez a cikk ismerteti az Azure lemez titkosítási előzetes kiadása és letöltése a tanulmány mutató hivatkozást.

###<a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Azure lemez titkosítást, a SSE és az ügyféloldali titkosítás összehasonlítása

####<a name="iaas-vms-and-their-vhd-files"></a>IaaS VMs és a hozzájuk virtuális fájlok

A lemez IaaS VMs által használt használata ajánlott Azure lemez titkosítást. Bekapcsolhatja a virtuális fájlok e lemezre Azure-tárolóban lévő biztonsági használt titkosításához SSE, de csak titkosítja a újonnan írt adatok. Ez azt jelenti, ha egy virtuális létrehozása és engedélyeznie kell a tároló számlán a virtuális fájlt tároló SSE, csak a változásokat titkosítva lesznek, nem az eredeti virtuális fájlt.

Ha egy virtuális hoz létre egy kép felhasználásával a Microsoft Azure piactéren lévő Azure Azure-tárolóban lévő a tárterület-fiókjába a kép [részleges másolatok](https://en.wikipedia.org/wiki/Object_copying) hajt végre, és nem titkosítva van még akkor is, ha SSE engedélyezve van. Miután a virtuális hoz létre, és elindítja a kép módosítása, SSE elkezdenek titkosítja az adatokat. Emiatt célszerű az Azure merevlemez-titkosítás használatára a VMs, ha azt szeretné, hogy azok teljesen titkosítva létrehozott képek használata a Microsoft Azure piactéren.

Ha egy előre titkosított virtuális összhangba Azure a helyszíni, töltse fel a titkosítási kulcs Azure kulcs tárolóból elemre, és használja a titkosítást, hogy virtuális, hogy a helyszíni használta továbbra is fogja. Azure lemez titkosítás engedélyezve van a ebben az esetben kezelheti.

Ha a helyszíni titkosítatlan virtuális, töltse fel az egyéni képként a gyűjtemény, és egy virtuális belőle kiépítése. Ha ezek az erőforrás-kezelő sablonok használata, megkérheti bekapcsolásához Azure lemez titkosítást, a virtuális felfelé indításakor.

Ha adatok lemez hozzáadása, majd azt a virtuális csatlakoztatni, a adatok lemezen bekapcsolhatja Azure lemez titkosítást. Azt először titkosítja helyi adatok lemezről, és a szolgáltatás felügyeleti réteg fog végezze el a tárolás ellen Lusta írási, a tárhely tartalom titkosítva van.

####<a name="client-side-encryption"></a>Ügyféloldali titkosítás:####

Ügyféloldali titkosítás a legbiztonságosabb lehetőség az, hogy titkosítja a az adatok, mert titkosítja a hálózaton átvitt előtt, és az adatokat a többi titkosítja. Azonban kell hozzá kód hozzáadása a alkalmazások tárolására, előfordulhat, hogy nem szeretne tenni. Ezekben az esetekben HTTPs használhatja az adatoknak a hálózaton átvitt és SSE titkosítása többi adatait.

Ügyféloldali titkosítással titkosíthatja táblázat szervezetek, az üzenetek és a BLOB. A SSE csak titkosíthatja BLOB. Ha a tábla- és várólista adatok titkosítva van szüksége, ügyféloldali titkosítás kell használnia.

Ügyféloldali titkosítás teljesen az alkalmazás kezeli. Ez a legbiztonságosabb megoldás, de szükség programozott módosítsa az alkalmazást, és helyezze a kulcskezelő folyamatok helyen. Ha azt szeretné, a további biztonsági továbbítás során, és azt szeretné, hogy a gyorsítótárban tárolt adatokat titkosítása ez használna.

Ügyféloldali titkosítás több terhelést az ügyfélszámítógépen, és számlája Ez a méretezhetőség tervek, különösen akkor, ha titkosítása és átvitele az adatokat kell.

####<a name="storage-service-encryption-sse"></a>Tárterület titkosítását (SSE)

Azure tároló SSE kezeli. SSE használatával nem adja meg a biztonsági a hálózaton átvitt adatok, de az adatok titkosítása, mint Azure tárolóhoz van írva. Nincs semmilyen hatása a teljesítményre gyakorolt Ez a funkció használata esetén.

Csak blokk BLOB titkosítása, BLOB hozzáfűző és lap BLOB SSE használatával. Ha titkosítani várólista vagy táblázatban lévő adatok kell figyelembe ügyféloldali titkosítást használ.

Archiválási vagy virtuális fájlokat hozhat létre új virtuális gépeken futó alapjaként használt tárban van, ha hozzon létre egy új tárterület-fiókot, SSE engedélyezése és szeretné a fiókhoz társított töltse ki a virtuális fájlokat. A virtuális fájlokat Azure tároló titkosítja.

Ha engedélyezve van egy virtuális és a tároló fiókot a virtuális fájlokat tartsa bekapcsolva SSE lemezt Azure lemez titkosítás, működik jól; kétszer titkosítását újonnan írt adatok eredményez.

##<a name="storage-analytics"></a>Tárterület-elemzés

###<a name="using-storage-analytics-to-monitor-authorization-type"></a>Tárterület Analytics használata a Lync-hitelesítés típusa

Az összes tárterület-fiókhoz Azure tároló Analytics végezze el a naplózási és mérőszámok adattárolásra engedélyezheti. Ez a kiváló eszköz használni, amikor a tárterület-fiók teljesítménymutatók ellenőrizni szeretné, illetve kell elhárítani egy tárterület-fiókkal, mert teljesítménybeli problémákat tapasztal.

A tároló analytics naplókban megjelenik egy másik adatot a mások által használt tárterület elérésekor kérni hitelesítési módszer. Például a Blob-tárolóhoz megtekintheti azokat a megosztott Access aláírás vagy a a tárhely fiókot is használni, vagy ha a blob elérhető nyilvános volt.

Lehet, hogy ez nagyon hasznos, ha szorosan esetlegesen korán tároló való hozzáférést. Ha például Blob-tárolóban lévő is személyes beállítása az összes a tárolók és Társítások szolgáltatásainak használata végrehajtása során az alkalmazások. Ellenőrizze a naplókat rendszeresen szeretné látni, ha a BLOB érhetők el a fiók jelezheti a biztonság megsértése, tároló billentyűkkel, vagy ha nyilvánosak a BLOB, de ne lehetnek.

####<a name="what-do-the-logs-look-like"></a>Hogyan néznek a naplókat?

Miután beállította a fiók tárolási mértékek, és a naplózás az Azure portálon keresztül, analytics adatainak a program ekkor elkezdi gyűjteniük gyorsan. A naplózás az egyes szolgáltatásokhoz mértékek pedig külön; a naplózás csak írni, ha van, akkor tevékenység az adott tárterület-fiókban, amikor a mértékek fog van bejelentkezve, percenként, óránként vagy minden nap beállításaitól függően.

A naplók blokk BLOB-tároló fiók $logs nevű tárolóban tárolja. A tároló automatikusan létrejön, ha engedélyezve van a tárhely Analytics. Ez a tároló hoz létre, nem törölhetők, bár a tartalmát.

Az $logs tároló alatt van az egyes szolgáltatásokhoz egy mappát, és ezután vannak almappák az év és hónap/nap/óra. Az óra a naplókat egyszerűen számozásának. Ez a címtár-struktúra hasonlóan fog kinézni:

![A naplófájlok megtekintése](./media/storage-security-guide/image1.png)

Azure tároló minden kérés be van jelentkezve. Íme egy naplófájlban, az első néhány űrlapmezőket ábrázoló pillanatképét.

![A naplófájl pillanatképét](./media/storage-security-guide/image2.png)

Láthatja, hogy a naplók bármilyen típusú tárterület-fiókhoz hívások nyomon követésére használhatja.

####<a name="what-are-all-of-those-fields-for"></a>Mik azok az adott mezők mindegyikének?

Van egy szerepel a források alatt, amely a cikkben a naplókat és a használatuk sok mezőinek listáját. Az alábbiakban a mezők, sorrendben:

![Pillanatkép egy naplófájlban mezők](./media/storage-security-guide/image3.png)

Sajnos érdekli bejegyzéseit GetBlob, és hogyan azok hitelesítése, így szükség keresse meg a művelettípus "Get-Blob" rendelkező bejegyzéseket, és ellenőrizze a kérelem állapota (4<sup>edik</sup> oszlop) és a hitelesítés típusa (8<sup>edik</sup> oszlop).

Például a nevére a partnerlistában a fenti az első néhány sor kérelem-állapota "Sikeres" és a hitelesítés típusa "hitelesített". Ez azt jelenti, hogy a kérelem ellenőrzése a tárhely fiókkulcs használatával.

####<a name="how-are-my-blobs-being-authenticated"></a>Hogyan vannak a BLOB hitelesíteni kívánt?

Van még három olyan esetek, amelyek érdeklik, az azt.

1.  A blob nyilvános és érhető el, a megosztott Access aláírás nélküli URL-cím használatával. Ebben az esetben a kérelem állapot "AnonymousSuccess" pedig a hitelesítés típusa "névtelen".

    1.0; 2015-11-17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**; 200; 124; 37. **Névtelen**; mystorage...

2.  A blob privát és használta a megosztott Access aláírás. Ebben az esetben a kérelem állapot "SASSuccess" pedig a hitelesítés típusa "társítások".

    1.0; 2015-11-16T18:30:05.6556115Z; GetBlob; **SASSuccess**; 200; 416; 64; **társítások**; mystorage...

3.  A blob privát, és a tárhely kulcs használt elérheti őket. Ebben az esetben a kérelem állapot "**sikeres**" pedig a hitelesítés típusa "**hitelesített**".

    1.0; 2015-11-16T18:32:24.3174537Z; GetBlob; **A siker**; 206; 59; 22. **hitelesített**; mystorage...

A Microsoft üzenet Analyzer megtekintéséhez és elemzéséhez ezeket a naplókat is használhatja. Azt a keresésre és szűrésre funkciókat tartalmazza. Például érdemes lehet keresni megtudhatja, ha a használatát mi várható, azaz győződjön meg arról, hogy valaki nem éri el a tárterület-fiók jogosulatlanul GetBlob példányát.

####<a name="resources"></a>Erőforrások

-   [Tárterület-elemzés](storage-analytics.md)

    Ez a témakör az tároló analytics és ahhoz, hogy miként áttekintése.

-   [Tárterület Analytics napló formázása](https://msdn.microsoft.com/library/azure/hh343259.aspx)

    Ez a cikk azt mutatja be, a Analytics napló formátumot, és adatait megjelenítő rendelkezésre álló mezők ott, beleértve a hitelesítés típusa, amely jelzi a kérelem használt hitelesítés típusa.

-   [A tároló fiókot az Azure-portálon figyelése](storage-monitor-storage-account.md)

    Ez a cikk bemutatja, hogyan a mértékek figyelhető és naplózható tárterület-fiók beállítása.

-   [Azure tárolási mértékek és a naplózás, a AzCopy és az üzenet Analyzer végpont – hibaelhárítás](storage-e2e-troubleshooting.md)

    Ez a cikk elhárításáról a tárhely Analytics használatával folytatott beszélgetést, és bemutatja, hogyan használhatja a Microsoft üzenet Analyzer.

-   [A Microsoft üzenet Analyzer működő útmutató](https://technet.microsoft.com/library/jj649776.aspx)

    Ez a cikk a Microsoft üzenet Analyzer hivatkozását, és egy oktatóprogram, a rövid útmutató az első és a funkció összefoglalása mutató hivatkozásokat tartalmaz.

##<a name="cross-origin-resource-sharing-cors"></a>Idegen származási erőforrás (CORS) megosztása

###<a name="cross-domain-access-of-resources"></a>Az access tartományok erőforrások

Ha egy tartományban található webböngésző egy HTTP kérést egy erőforrás egy másik tartományból, azaz határokon származási HTTP kérelmet. Ha például egy HTML-lapot, akkor a contoso.com a felszolgált fabrikam.blob.core.windows.net is jpeg kér. Biztonsági okokból böngészők korlátozása határokon származási HTTP-kérések belül parancsfájlok, például JavaScript kezdeményezni. Ez azt jelenti, hogy bizonyos JavaScript-kód, a contoso.com címen weblapra kéri, hogy a fabrikam.blob.core.windows.net jpeg, amikor a böngésző nem teszi lehetővé a kérést.

Mit jelent ez van Azure adathordozós? Jól, ha statikus eszközök, például a JSON vagy XML-adatfájlok tárolja Blob-tárolóhoz a tárterület-fiókja nevű Fabrikam, a tartomány eszközök lesz fabrikam.blob.core.windows.net, és a contoso.com webes alkalmazás nem tudja érni őket az JavaScript, mert a tartományok eltérők. Ez akkor is érvényes, ha szeretné hívni egyik az Azure tároló szolgáltatások – például Táblatároló –, amely visszatérési JSON adatok feldolgoztatni a JavaScript-ügyfél.

####<a name="possible-solutions"></a>Lehetséges megoldások

Megoldásához egy úgy, hogy egy egyéni tartományt, például "storage.contoso.com" hozzárendelése fabrikam.blob.core.windows.net. A probléma oka, hogy csak rendelhet hozzá egyéni tartomány egy tárterület-fiókjába. Mi történik, ha az eszközök tárolódnak több tárterület-fiók?

Megoldásához úgy, hogy a használni kívánt a tárhely hívások proxy webalkalmazást. Ez azt jelenti, ha egy fájl feltöltése Blob-tárolóhoz, a webalkalmazás volna vagy helyileg írja be azt, és ezután másolja a vágólapra Blob-tárolóhoz vagy azt szeretné, hogy az összes olvasson memória és írni Blob-tárolóhoz. Másik megoldás egy saját webalkalmazás (például a webes API-val), amely feltölti a fájlok helyi meghajtóra, és beírja őket a Blob-tárolóhoz is írhat. Mindkét módon, hogy amikor igények meghatározása a méretezhetőség a függvény figyelembe.

####<a name="how-can-cors-help"></a>Hogyan segíthet a CORS?

Azure tárolás engedélyezése CORS – Cross Origin erőforrás-megosztás teszi lehetővé. Az összes tárterület-fiókhoz megadhatja az erőforrásokat, a tárterület-fiók elérésére jogosult tartományok. Például a fent ismertetett lehetőséget választja, hogy CORS engedélyezése a fabrikam.blob.core.windows.net tárterület-fiók és állította be, akkor a contoso.com való hozzáférés engedélyezése. Kattintson a webes alkalmazás contoso.com közvetlenül érheti el az erőforrások fabrikam.blob.core.windows.net.

Megjegyzés egyvalamihez, hogy CORS lehetővé teszi, hogy az access azonban nem nyújt hitelesítés szükséges az összes nem – nyilvános hozzáférés tároló-erőforrások. Ez azt jelenti, hogy csak akkor tud hozzáférni BLOB, ha az azok nyilvános vagy vesz fel, biztosítva megfelelő engedélye a megosztott Access aláírás. Táblázatok, a sorok és a fájlok nem nyilvános férhetnek, és csak egy Társítások.

Alapértelmezés szerint CORS le van tiltva a szolgáltatások. CORS engedélyezheti a REST API-t vagy a tárterület-ügyfél tár segítségével hívja fel a szolgáltatás házirendek beállítása a lehetőségek közül választhat. Az ehhez szükséges lépéseket, ha meg kell adnia egy CORS szabályt, amely az XML-ben. Íme egy példa egy CORS szabályt, amely a szolgáltatás tulajdonságainak beállítása a művelet a Blob-szolgáltatás használata a tárterület-fiók van beállítva. A tároló ügyfél tár vagy a REST API-k használata Azure tároló művelet végrehajtása

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Az alábbiakban minden egyes sorára jelenti:

-   **AllowedOrigins** Ez azt jelenti, hogy mely nem egyező tartományok kérhetik és a tárhely szolgáltatásból származó adatok fogadásához. Ez arról tájékoztat, hogy a contoso.com és a fabrikam.com is kérhet adatok Blob-tárolóhoz adott tároló fiókot. Azt is beállíthatja a egy helyettesítő karaktert (\*) engedélyezése minden tartományok kérések eléréséhez.

-   **AllowedMethods** Ez akkor is használható, amikor a kérés módszerek (HTTP-kérés műveletek) listája. Ebben a példában csak helyezése és az engedélyezett. Beállíthatja, hogy ez egy helyettesítő karaktert szeretne (\*) engedélyezése minden módszer használható.

-   **AllowedHeaders** Ez az a kérelem fejlécek, amely a kérés origin tartomány lehetőségeiről. Ebben a példában az x-ms-metaadatok, kezdődő összes metaadatok fejlécét x-ms-metaadat-tároló és az x-ms-metaadat-abc kijelölésének engedélyezése A helyettesítő karaktert (\*) jelzi, hogy engedélyezve van-e minden élőfej kezdődő a megadott előtagot.

-   **ExposedHeaders** Ez azt jelenti, hogy melyik válasz fejlécek kell lehet téve a kérelem kibocsátó számára a böngészőben. Ebben a példában, minden élőfej kezdődő "x-ms - metaadat-" fog érhetők el.

-   **MaxAgeInSeconds** Az idő, hogy a böngésző gyorsítótár-ellenőrzési beállítások kérésére legnagyobb mennyiségű. (Többet szeretne tudni az ellenőrzés kérést, jelölje be az első cikk az alábbi.)

####<a name="resources"></a>Erőforrások

További információt a CORS és ahhoz, hogy miként kérjük, nézze meg ezek az erőforrások.

-   [Idegen származási erőforrás megosztása Azure.com Azure tároló szolgáltatást (CORS) támogatása](storage-cors-support.md)

    Ez a cikk áttekintést nyújt a különböző tároló szolgáltatást a szabályok beállításáról és CORS.

-   [Idegen származási erőforrás megosztása MSDN Azure tároló szolgáltatást (CORS) támogatása](https://msdn.microsoft.com/library/azure/dn535601.aspx)

    Ez a hivatkozás dokumentációjában találhat CORS támogatása a Azure tároló szolgáltatást. Ez alkalmazása az egyes tárhelyszolgáltatáshoz cikkekre mutató hivatkozásokat tartalmaz, és azt szemlélteti, és ismerteti a CORS fájlt minden elemének.

-   [Microsoft Azure tárterület: CORS bemutatása](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

    Ez az a kezdeti blog cikk CORS kihirdetése és használatához ábrázoló mutató hivatkozást.

##<a name="frequently-asked-questions-about-azure-storage-security"></a>Gyakori kérdések a Azure tároló biztonsági

1.  **Hogyan tudom ellenőrizni a BLOB-e és Azure tároló elhalványítása vagyok átvitele a Ha nem tudja használni a HTTPS protokollt integritását?**

    Ha valamilyen okból, akkor használja a HTTP helyett HTTPS, és blokk okkal dolgozik, használhatja a MD5 ellenőrzése segítségével tudja ellenőrizni a továbbított BLOB integritását. Ez a hálózati/átviteli réteg hibák védekezéshez, de nem feltétlenül közvetítő támadások segítséget nyújt.

    Ha a HTTPS, amely biztosítja az átviteli szintű biztonság, majd MD5 ellenőrzése használata a felesleges és szükségtelen is használhatja.
    
    További információt kérjük, nézze meg az [Azure Blob-MD5 áttekintése](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).

2.  **Mit kell tudni az Amerikai Egyesült Államok részéről a FIPS?**

    Az Amerikai Egyesült Államok Federal információk Processing Standard (FIPS) kényes adatok védelméhez amerikai szövetségi kormányzati számítógép rendszerek által jóváhagyott titkosítási algoritmust határozza meg. FIPS engedélyezése mód a Windows server vagy az asztalon azt közli az operációs rendszer titkosítási algoritmust csak FIPS jóváhagyással kell használni. Ha egy alkalmazás nem kompatibilis algoritmusok használ, az alkalmazások megszakad. With.NET Framework verziók 4.5.2 vagy nagyobb, az alkalmazás automatikusan vált titkosítási algoritmusok FIPS-kompatibilis algoritmus használata, ha a számítógép FIPS üzemmódban van.

    A Microsoft elhagyja kívánja-e a FIPS mód engedélyezése ügyfelek felfelé. Úgy véli, hogy az ügyfelek, akik nem fizetnie kormányzati szabályzatát FIPS üzemmód engedélyezése alapértelmezés szerint nincs lenyűgöző OK.

    **Erőforrások**

-   [Miért azt esetén nem ajánlja "FIPS üzemmód" szolgáltatáshoz](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

    Ez a cikk blog FIPS áttekintést, és megtudhatja, hogy miért nem tagjaik FIPS üzemmód alapértelmezés szerint.

-   [FIPS 140 érvényesítése](https://technet.microsoft.com/library/cc750357.aspx)

    Ez a cikk információt nyújt hogyan Microsoft-termékek és a titkosítási modulok megfelel-e az amerikai szövetségi kormányzati FIPS szabványát.

-   ["Rendszer titkosítás: használata FIPS megfelelő algoritmusok titkosításhoz, a kivonatolás, és bejelentkezés" biztonsági beállítások effektusok a Windows XP és Windows újabb verzióiban](https://support.microsoft.com/kb/811833)

    Ez a cikk folytatott beszélgetést, a régebbi Windows számítógépek FIPS üzemmód használatával kapcsolatban.
