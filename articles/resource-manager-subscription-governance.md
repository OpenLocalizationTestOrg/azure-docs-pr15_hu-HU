<properties
   pageTitle="Gyakorlati tanácsok áthelyezése az Azure nagyvállalatoknak |} Microsoft Azure"
   description="Egy scaffold használó vállalkozások ahhoz, hogy a biztonságos és kezelhető környezet ismerteti."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure vállalati scaffold - elfogadott előfizetés irányítási

Nagyvállalatoknak egyre bevezetett agility és rugalmasság a nyilvános felhőben. Azok is felhasználásával készítése a bevétel, vagy a vállalati erőforrások optimalizálása a felhőben erősségek. Microsoft Azure biztosít a szolgáltatások rengeteg, hogy a vállalatok például építőelemek munkaterhelésének és alkalmazások széles tömbje címet is gyűjtenie. 

De, arra, hogyan kezdjek hozzá gyakran nehéz. Miután Azure használja, néhány kérdésre gyakran merülnek fel:

- "Hogyan megfelelnek az egyes országokban adatszuverenitás jogi követelményeknek?"
- "Hogyan győződhetek meg arról, hogy valaki nem véletlenül változtatja meg a kritikus rendszert?"
- "Hogyan állapíthatom meg, hogy mi minden erőforrás támogat, e számlája, és ismét pontosan számla?"

A potenciális egy üres előfizetés az nincs védőkorlátokat tűnhet. A szóköz is akadályozzák az Áthelyezés ide: Azure.

Ebben a cikkben a cím irányítási van szükség, és azt meg agility egyenlege technikai szakembereknek kiindulási pont. Azt fogalmának egy vállalati scaffold, amely végigvezeti a szervezetek végrehajtási és azok az Azure előfizetések kezelése. 

## <a name="need-for-governance"></a>Segítségre van szüksége az irányítási elvek

Amikor Azure helyez át, meg kell oldania témájának irányítási korai a felhőben, a vállalaton belül a sikeres használata érdekében. Sajnos az időt és a bürokráciával létrehozása a teljes irányítási rendszert azt jelenti, hogy bizonyos üzleti csoportok közvetlen megnyitásához szállítók vállalati informatikai érintő nélkül. Ezt a megközelítést hagyja a vállalati megnyitott biztonsági, ha az erőforrások nem megfelelően kezelt. A nyilvános felhő - agility rugalmasság és felhasználási-alapú árak - jellemzői fontosak üzleti csoportok kell gyorsan igényeit kiszolgáljuk (belső és külső). De vállalati informatikai kell, hogy adatokat és a rendszer hatékony védelmét.

A valós életben, állványon létrehozására való felhasználása esetén a struktúra alapján. A scaffold az általános szerkezeti segédvonalak és horgony pontokat csatlakoztatni állandóbb rendszerekhez. Egy vállalati scaffold megegyezik a: rugalmas vezérlők, és a környezet és a horgonyok struktúrát épül a nyilvános cloud Services Azure lehetőségekkel. A szerkesztők biztosít (IT és üzleti csoportok) a alap hozhat létre, és csatolja az új szolgáltatásokat.

A scaffold azt a különféle méretű ügyfelekkel sok Előjegyzések összegyűjtötte eljárások alapul. Ezeket a kis szervezetekben a felhőben Fortune 500 nagyvállalatoknak és független gyártók, akik áttelepítése és a felhőben megoldások fejlesztésére megoldások fejlesztésével ügyfelek tartományban. A vállalati scaffold "purpose-built" rugalmas hagyományos informatikai munkaterhelésekből és a Agilis munkaterhelésekből; támogatása a fejlesztők szoftver-mint-a-(szoftver) Szolgáltatásalkalmazások létrehozása, például: Azure funkciók alapján.

A vállalati scaffold Azure belül minden új előfizetést alapját szolgálja. Lehetővé teszi a rendszergazdáknak munkaterhelésekből megfeleljen a minimális irányítási egy szervezet anélkül, hogy megakadályozza, hogy a vállalati csoportok és a fejlesztők gyorsan értekezlet saját célok érdekében.

> [AZURE.IMPORTANT] Irányítási elengedhetetlen Azure egyik. Ez a cikk a technikai végrehajtása egy vállalati scaffold célként, de csak úgy elérje a szélesebb folyamat és az összetevők közötti kapcsolatokat. Házirend irányítási átfolyása az felülről lefelé, és határozzák meg a vállalkozás szeretne elérni. Természetesen az Azure az irányítási modell kibocsátása tartalmazza-e a rendeléseket informatikai, de fontosabb rendelkeznie kell az üzleti csoport vezető és biztonsági és kockázatkezelés erős ábrázolása. Végül egy vállalati scaffold az üzleti kockázatcsökkentő megkönnyítése érdekében a szervezet misszió és céljait.

Az alábbi képen a scaffold összetevői ismerteti. A foundation részlegek, partnerek és előfizetések egyszínű tervet támaszkodik. Az oszlopok erőforrás-kezelő házirendek és erős elnevezési szabályai állnak. A többi scaffold Azure funkciók core származik, és szolgáltatásainak adott engedélyezése a biztonságos és kezelhető környezet.

![scaffold összetevők](./media/resource-manager-subscription-governance/components.png)

> [AZURE.NOTE] Azure gyorsan bővült 2008 bevezetése óta. Ez a legjobb exponenciális mérnöki csapatok Microsoft kezelésére és szolgáltatások üzembe helyezése a megközelítés rethink szükséges. Az erőforrás-kezelő Azure modell a 2014-es jelent meg, és a klasszikus telepítési modell helyettesíti. Erőforrás-kezelő lehetővé teszi, hogy könnyebben telepíthető, rendszerezése és szabályozhatja az Azure erőforrások szervezetek. Erőforrás-kezelő parallelization tartalmaz, az összetett, egymástól megoldások gyorsabb telepítéshez erőforrások létrehozásakor. Részletes hozzáférés-vezérlés és az azt jelenti, hogy a metaadatok címke erőforrások is tartalmaz. Azt javasoljuk, hogy az erőforrás-kezelő használatával összes erőforrás hozza létre. A vállalati scaffold kifejezetten az erőforrás-kezelő modell készült.

## <a name="define-your-hierarchy"></a>A hierarchia meghatározása

A scaffold alapját az Azure vállalati regisztrációs (és a vállalati portál). A vállalati tagság határozza meg az alakzatot, és Azure szolgáltatások, a vállalaton belül, és az alapvető irányítási struktúra. A vállalati szerződés belül ügyfelek képesek további fel a környezetet részlegek, partnerek és végül előfizetések be. Az Azure-előfizetés a hol találhatók a összes erőforrás alapegység. Még több korlátai belül Azure, például a magmintákat, erőforrásokat stb határozza meg.

![hierarchia](./media/resource-manager-subscription-governance/agreement.png)

Minden vállalati eltér, és az előző képen a hierarchia lehetővé teszi, hogy hogyan vannak rendezve Azure a vállalaton belüli jelentős rugalmasság. Végrehajtási a dokumentumban található útmutatást, mielőtt a hierarchia modell és számlázás, erőforrás-elérés és összetettsége hatással megértéséhez.

Az Azure való belépés három közös mintázatok a következők:

- A **funkcionális** mintával

    ![funkcionális](./media/resource-manager-subscription-governance/functional.png)

- A **részleg** mintával 

    ![üzleti](./media/resource-manager-subscription-governance/business.png)

- A **földrajzi** mintával

    ![földrajzi](./media/resource-manager-subscription-governance/geographic.png)

Az irányítási a vállalati követelményeinek be az előfizetés meghosszabbítása előfizetés szintre scaffold alkalmazhat.

## <a name="naming-standards"></a>Elnevezési szabályai

Az első oszlop, a scaffold van elnevezési szabványoknak. Jól megtervezett elnevezési szabályai lehetővé teszi a portálon, a számlán és parancsfájlok belül erőforrások azonosítása. Nagy valószínűséggel már rendelkezik követését ellátó helyszíni infrastruktúra. A környezet Azure ad, amikor e elnevezési szabályai az Azure erőforrások kiterjedjen. Normál elnevezési megkönnyítése a környezet minden szintjén hatékonyabb kezeléséhez.

> [AZURE.TIP]
>
> - Tekintse át, és ha elfogadja lehetséges a [minták és eljárások útmutatást](./guidance/guidance-naming-conventions.md). Ez az útmutató segítségével eldöntheti, hogy a értelmes elnevezési.
> - Használjon camelCasing erőforrás (például myResourceGroup és vnetNetworkName). Megjegyzés: Vannak bizonyos erőforrások, például a tárterület-fiók esetén csak kisbetű (és egyéb különleges karaktereket) használata esetén.
> - Fontolja meg a hivatkozási elnevezési szabályai Azure erőforrás-kezelő házirendek (a következő szakaszban ismertetett) használatát.
 
## <a name="policies-and-auditing"></a>Szabályok és naplózás

A második oszlop a scaffold a csapattól kell beszerezni az [Azure erőforrás-kezelő szabályok](resource-manager-policy.md) és a [naplózás a tevékenységnapló](resource-group-audit.md). Erőforrás-kezelő házirendeket biztosít az azt jelenti, hogy kockázatkezelés Azure-ban. Határozhatja meg, amely biztosítja az adatszuverenitás korlátozása, kényszerítése vagy bizonyos műveletek naplózási házirendek. 

- Házirend rendszer alapértelmezett **engedélyezése** . Műveletek létrehozása és információforrások, amelyek megtagadhatja vagy erőforrásokra gyakorolt műveletek naplózási házirendek hozzárendelése szabályozható.
- Házirendek megkötésekről által házirend-definíciók nyelven házirend definition (if-then feltételek).
- Hoz létre a JSON (Javascript Objektumjelrendszer) házirendeket formátumú fájlokat. Házirend definiálta, hozzárendelheti egy adott hatókört: előfizetési, erőforráscsoport. vagy erőforrást.

Szabályok, amelyek lehetővé teszik az esetet külön megközelítésre több művelet van. A műveletek a következők:

-   **Megtagadás**: az erőforrás-igénylést akadályozza
-   **Naplózás**: lehetővé teszi, hogy a kérést, de elhelyezi a vonalat a tevékenység naplója (amely is használható, adja meg a riasztások vagy runbooks indíthatja el)
-   **Hozzáfűző**: meghatározott adatokat ad hozzá az erőforrás. Ha nincs "CostCenter" címke erőforrás, például adja meg, amely a címke alapértelmezett értékű.

### <a name="common-uses-of-resource-manager-policies"></a>Erőforrás-kezelő házirendek gyakori céljára

Azure erőforrás-kezelő-házirendek az Azure eszközkészlet hatékony eszköz olyan. Lehetővé teszik a váratlan költségek, a költséghely keresztül címkézés erőforrások azonosítása, és győződjön meg arról, hogy teljesülnek-e compliancy követelmények elkerülése érdekében. Ha házirendek egyesíti a beépített naplózási szolgáltatásaival, összetett és rugalmas megoldások is fashion. Házirendek lehetővé cégek "Hagyományos informatikai" munkaterhelésének és a "Agile" munkaterhelésekből; a vezérlők szükséges például: ügyfél-alkalmazások fejlesztésével. A leggyakoribb mintázatok házirendek láthatja a következők:

- **Geo-megfelelőségi/adatszuverenitás** - Azure régiók világszerte ismertetése Nagyvállalatoknak gyakran szeretné szabályozni, ahol erőforrások jönnek létre (akár adatszuverenitás biztosítja, vagy hogy erőforrások létrehozott közelébe az end vonzóbbak lehetnek az erőforrások biztosítására).
- **Költség-kezelés** – az Azure-előfizetés számos típusú és skála erőforrások is tartalmazhat. Vállalatok gyakran szeretné biztosítani, hogy szabványos előfizetések való használatának kerülése feleslegesen erőforrásokat, amelyek is költség dollárban egy hónap vagy annál több száz.
- **Alapértelmezett irányítási jelölői keresztül** – címkék igénylő az egyik azok a leggyakoribb és a nagyon kívánt funkció. Azure erőforrás-kezelő házirendekkel nagyvállalatoknak képesek annak érdekében, hogy egy erőforrás megfelelően címkézett. A leggyakoribb címkék: részleg, az erőforrás-tulajdonos és a környezet típusa (például - gyártási, próba, fejlesztési)

**Példák**

"Hagyományos informatikai" előfizetés a vállalati verziós alkalmazások

-   A hivatkozási részleg és a tulajdonos címkéket az összes erőforrás
-   Észak-amerikai régió erőforrás létrehozási korlátozása
-   G-sorozat VMs és HDInsight fürt létre korlátozása

"Agilis" környezetben, egy részleg felhő alkalmazások létrehozása

- Adatok felségterületéhez követelményeknek, az erőforrások létrehozásának engedélyezése csak egy adott tartomány.
- A hivatkozási összes erőforrás címke környezetben. Egy erőforrás egy címkét nélkül jön létre, ha a Hozzáfűzés a **környezet: Ismeretlen** az erőforrás címkére.
- Naplózási erőforrások Észak-Amerikán kívül létrehozásakor, de nem akadályozza meg.
- Naplózási magas költségerőforrások létrehozásakor.

> [AZURE.TIP]
>
> A leggyakoribb erőforrás-kezelő házirendek keresztül szervezetek használata vezérlőre *Hol* erőforrások hozhatja létre, és *milyen erőforrások* hozhatók létre. Nemcsak a vezérlők *Hol* és *milyen*kezeléséről, sok nagyvállalatoknak szolgáltatás részét képező házirendekkel erőforrások biztosan a megfelelő metaadatokat vissza felhasználási számára. Azt javasoljuk, hogy az előfizetés szintjén házirendek alkalmazása:
>
> - GEO-megfelelőségi/adatszuverenitás
> - A költség kezelése
> - Jelölői (üzleti segítségre van szüksége, például BillTo, alkalmazás tulajdonosa azzal)
>
> Hatókör alacsonyabb szinten további házirendeket alkalmazhatja.
 
### <a name="audit---what-happened"></a>Naplózási – Mi történt?

Ha szeretné megtekinteni, hogy hogyan működik-e a környezetben, kell felhasználói műveletek naplózása. A legtöbb erőforrástípus belül Azure létrehozása a diagnosztikai naplók, amely a napló eszközzel vagy az Azure műveletek Management alkalmazásokkal elemezheti. Tevékenység naplók gyűjthető több előfizetéseket, adja meg a részlegszintű vagy vállalati megtekintése. Naplóbejegyzések olyan fontos diagnosztikai eszközök és a kiváltó események az Azure környezetben való kulcsfontosságú mechanizmusa is.

Az erőforrás-kezelő telepítések engedélyezése alapján megállapíthatja, hogy a **Műveletek** beállításikon tevékenység naplók helyezze, és elvégzett ki őket. Tevékenység naplók gyűjthetők össze, és összesíti, például a napló Analytics eszközeivel.

## <a name="resource-tags"></a>Erőforrás-címkék

A szervezet felhasználói erőforrások hozzáadása az előfizetéshez, mint az lesz az erőforrások hozzárendelése a megfelelő szervezeti, az ügyfél és a környezet egyre fontos. Metaadat-alapú csatolhat erőforrások [címkék](resource-group-using-tags.md)között. Címkék segítségével adja meg az erőforrásra vagy a tulajdonos kapcsolatos adatokat. Címkék lehetővé teszi nemcsak összesítése és csoportosítása többféleképpen erőforrásokat, de adatok visszaterhelési céljából. Legfeljebb 15 kulcs: érték párokká rendelkező erőforrások is címkézni. 

Erőforrás-címkék rugalmas, és a legtöbb erőforrásokat kell kapcsolni. Általános erőforrás-címkék példák:

- BillTo
- Részleg (vagy részleg)
- A környezet (a termelési, szakaszban fejlesztés)
- Réteg (webes szint, alkalmazás réteg)
- Alkalmazás-tulajdonos
- Projektnév

![címkék](./media/resource-manager-subscription-governance/resource-group-tagging.png)

Címkék további példákat olvassa el a [elnevezési szabályai Azure erőforrások ajánlott](./guidance/guidance-naming-conventions.md)című témakört.

> [AZURE.TIP]
>
> Hozzon létre egy címkézési stratégia, amely azonosítja a előfizetésekben milyen metaadatok van szükség az üzleti, pénzügy, biztonsági, kockázatkezelés és a környezet általános kezelését. Egy házirendet, amely csak a címkézés tervezhet:
>
> - Erőforrás-csoportok
> - Tárhely
> - Virtuális gépeken futó
> - Alkalmazás szolgáltatási környezetben/webkiszolgálón

## <a name="resource-group"></a>Erőforráscsoport 

Erőforrás-kezelő lehetővé teszi, hogy helyezi a források kezelése, számlázási vagy természetes affinitás értelmes csoportokba. A korábban említett az Azure két telepítési modellek tartalmaz. A korábbi Klasszikus modell az alapegység kezelése az előfizetés volt. Erőforrások belüli nagyszámú előfizetések kialakulásához vezető előfizetés lebontva nehezen volt. Az erőforrás-kezelő modellel bekerül az erőforrás csoportok bevezetésével. Az erőforrás csoportok információforrások, amelyek egy közös életciklus van, vagy egy tulajdonság, például "az összes SQL-kiszolgálók" megosztása a tárolók vagy a "A program".

Az erőforrás csoportok nem található a egymással, és erőforrások csak egy erőforrás csoporthoz is tartoznak. Bizonyos műveleteket összes erőforrásában található erőforrás csoport is alkalmazhat. Például az erőforráscsoport belül minden erőforrás egy erőforrás csoport törlése eltávolítja. Általában elhelyeztem egy teljes alkalmazás vagy kapcsolódó rendszer ugyanazt a erőforráscsoport. Például a Contoso webalkalmazás nevű három réteg alkalmazás a webkiszolgáló, az application server és a azonos erőforráscsoport SQL server tartalmaz.

> [AZURE.TIP]
>
> Az erőforrás-csoportokat elrendezése változhat "Hagyományos informatikai" munkaterhelésekből "Agilis informatikai" munkaterhelésekből
>
> - "Hagyományos informatikai" munkaterhelésekből leggyakrabban a azonos életciklusáról, például az alkalmazások elemeire csoportosítva. Csoportosítás alkalmazás lehetővé teszi, hogy az egyes alkalmazások kezelése.
> - "Agilis informatikai" munkaterhelésekből gyakran külső ügyfélkapcsolati felhő alkalmazások összpontosíthat. Az erőforráscsoport tükrözze a rétegeket, a telepítés (például a webes réteg, alkalmazás réteg) és kezelése.

## <a name="role-based-access-control"></a>Szerepköralapú hozzáférés-vezérlés

Valószínűleg olya saját maga "kiknek kell erőforrásokhoz?" és "hogyan szabályozhatja a hozzáférés?" Engedélyezése vagy letiltása az access az Azure-portálra, és a portálon erőforrásokhoz való hozzáférés szabályozása elengedhetetlen. 

Azure kezdetben jelent meg, amikor hozzáférésének előfizetéshez azok egyszerű: vagy közös rendszergazdája. A klasszikus implicit modell hozzáférést a portálon összes erőforrás előfizetés a hozzáférést. Egyedi vezérlő hiánya megfelelő hozzáférési szintet nyújt az Azure tagság vezetett az száma korlátok, az előfizetések között.

Ez száma korlátok között maradjon előfizetések már nincs szükség. Szerepköralapú hozzáférés-vezérlés az (például szerepkörök közös "olvasó" és "író" típusú) szabványos szerepkörökhöz adhatnak a felhasználóknak. Egyéni szerepkörök is meghatározhat.

> [AZURE.TIP]
>  
> - Csatlakozás a vállalati identitás tárolására (általában az Active Directory) Azure Active Directory, a AD Connect eszközzel.
> - A felügyeleti/további-rendszergazdai használata egy felügyelt identitás előfizetés szabályozható. **Nem** rendelhet egy új előfizetés tulajdonosa felügyeleti/további-rendszergazda. Ehelyett RBAC szerepkörök segítségével adja meg a **tulajdonosi** engedélyekkel, vagy egyéni csoportba.
> - Azure felhasználók hozzáadása csoporthoz (például az alkalmazás X tulajdonosok) az Active Directoryban. A szinkronizált csoport segítségével adja meg a csoporttagok a megfelelő engedélyekkel az erőforráscsoport az alkalmazást tartalmazó kezelése.
> - Kövesse a megadása a várt munkához szükséges a **legalacsonyabb jogosultság** elve. Példa:
>     - Telepítési csoport: Egy csoportot, amely csak akkor tudja telepíteni az erőforrások.
>     - Virtuális gép kezelése: Egy csoportot, amely képes újraindítása VMs (műveletek)

## <a name="azure-resource-locks"></a>Azure erőforrás zárolás:

A szervezet core services hozzáadása az előfizetéshez, mint lesz egyre fontosabb annak érdekében, hogy azokat a szolgáltatásokat elérhető vállalati zavarok elkerülése érdekében. [Erőforrás zárolás:](resource-group-lock-resources.md) lehetővé teszi korlátozása műveletek értékes erőforrások, amelyben módosítása vagy törlése a őket alkalmazások vagy felhőalapú infrastruktúrájának jelentős hatást tartalmaz. Zárolások alkalmazhat egy előfizetés, erőforráscsoport vagy erőforrás. Általában zárolások alkalmazása eligazodást erőforrások, például a virtuális hálózatok, átjárókat és tároló fiókokat. 

Erőforrás-zárolások jelenleg támogatja a két érték: CanNotDelete és a csak olvasható. CanNotDelete azt jelenti, hogy a felhasználók (megfelelő jogosultságokkal) továbbra is olvasni vagy módosítani az erőforrást, de nem törölhetők. Csak olvasható azt jelenti, hogy jogosult felhasználók nem tudja törölni egy erőforrás módosítása.

Hozhat létre, és zárolásainak kezelése törölhet, rendelkeznie kell a hozzáférést `Microsoft.Authorization/*` vagy `Microsoft.Authorization/locks/*` műveletek.
A beépített szerepkörök csak a tulajdonos és a felhasználói hozzáférés rendszergazda megkapja ezeket a műveleteket.

> [AZURE.TIP] Alapvető hálózati beállítások a zárolás kell védeni. Az átjárók véletlen törlését, webhely VPN lenne katasztrofális Azure-előfizetésbe. Azure nem engedélyezi a használatban lévő virtuális hálózat törlése, de további korlátozások alkalmazása egy hasznos okokból. Házirendek is fontos a megfelelő vezérlőelemeket fenntartása. Azt javasoljuk, hogy egy **CanNotDelete** zárolása házirendeket használt telepítését.
>
> - Virtuális hálózati: CanNotDelete
> - Hálózati biztonsági csoport: CanNotDelete
> - Házirendek: CanNotDelete

## <a name="core-networking-resources"></a>Alapvető hálózati erőforrások

Erőforrások eléréséhez lehet belső (belül a hálózatán), vagy külső (az internetről). Nagyon egyszerűen véletlenül elhelyezni erőforrásokat a nem megfelelő, és a potenciálisan kártékony Access megnyitja a szervezet felhasználói számára. Helyszíni eszközök, mint a vállalatok kell vezérlőelemek megfelelő annak érdekében, hogy a felhasználók Azure a megfelelő döntések. Az előfizetés irányítási azonosítjuk alapvető erőforrások, amelyekkel az access egyszerű vezérlőelem. Az alapvető erőforrásokat áll:

- **Virtuális hálózatok** alhálózat tároló objektumok. Bár nem feltétlenül szükséges, akkor gyakran használatos belső vállalati erőforrások alkalmazásokat kapcsolódáskor.
- **Hálózati biztonsági csoportok** hasonlóak a tűzfalat, és az hogyan erőforrás "beszélgethet" a hálózaton keresztül szabályok adja meg. Hogyan szabályozható részletes biztosítanak /, ha egy alhálózat (vagy a virtuális gép) csatlakozhat az internethez vagy más alhálózat virtuális ugyanabba a hálózatba.

![alapvető hálózati](./media/resource-manager-subscription-governance/core-network.png)

> [AZURE.TIP]
>  
> - Hozzon létre virtuális hálózatok külső elérésű munkaterhelésének és a belső elérésű munkaterhelésekből hozzárendelve. Ezt a megközelítést csökkenti véletlenül úgy, hogy a virtuális gépeken futó, amely egy külső szemben lévő térközt a belső munkaterhelésekből szánt lehetőségét.
> - Hálózati biztonsági csoportok olyan fontos, hogy ez a beállítás. Legalább letiltása az access az internethez belső virtuális hálózatok, és letiltása az access a vállalati hálózathoz virtuális külső hálózatokhoz.

### <a name="automation"></a>Automatizálási

Az erőforrások kezelése egyenként is időigényes és jobban, bizonyos műveletek esetleg hibát. Azure Azure automatizálási, összefüggés-alkalmazások és Azure függvények automatizálási szolgáltatásainak ismertetése [Azure automatizálási](./automation/automation-intro.md) lehetővé teszi, hogy a rendszergazdák hozhat létre és kezelje a gyakori feladatok a források kezelése runbooks megadása. Egy PowerShell-kód szerkesztése vagy egy, grafikus szerkesztő runbooks hoz létre. Összetett többszakaszos munkafolyamatok hozhat létre. Azure automatizálási gyakran használatos a gyakori műveletek például nem használt erőforrások leállítása vagy erőforrások létrehozása egy adott eseményindító válaszként erre szolgáló emberi beavatkozás nélkül kezelheti.

> [AZURE.TIP]
>
> - Azure automatizálási fiók létrehozása, és tekintse át a rendelkezésre álló runbooks (grafikus és a parancs vonal) a [Runbook gyűjtemény](./automation/automation-runbook-gallery.md)érhető el.
> - Importálása, és testre szabhatja a saját használatra kulcs runbooks.
>
> A leggyakoribb helyzet indítása és leállítása virtuális gépeken futó időközönként lehetősége. Vannak olyan példa runbooks, a gyűjtemény rendelkezésre álló, és, ebben az esetben kezelésére és való nagyméretűre is.


## <a name="azure-security-center"></a>Azure biztonság otthon

Esetleg közül a legnagyobb blokkolók elfogadása cloud túllépte az aggályokat biztonsági. Informatikai kockázat vezetők és biztonsági részlegek van szüksége annak érdekében, hogy a biztonságos erőforrások Azure-ban. 

Az [Azure biztonság otthon](./security-center/security-center-intro.md) biztonsági állapotát az előfizetések erőforrásainak központi nézetben, és javaslatok, megakadályozva biztonságos erőforrások biztosít. Engedélyezheti, hogy a finomabb házirendek (például alkalmazása szabályok, amelyek lehetővé teszik a vállalat szabja testre a testtartását szeretné a kockázatot, azok is megcímezheti az adott erőforrás-csoportokat). Végezetül Azure biztonság otthon, amely lehetővé teszi a Microsoft-partnerek és a független gyártók hozhat létre, melyet az Azure biztonság otthon tartalmasabbá teszi a képességei szoftver nyílt platform. 

> [AZURE.TIP]
>
> Azure biztonság otthon az egyes előfizetések alapértelmezés szerint engedélyezve van. Adatok gyűjtése virtuális gépeken futó Azure biztonság otthon a ügynököt, és kezdje el az adatok összegyűjtése engedélyezni azonban engedélyeznie kell.
>
> ![adatok gyűjtése](./media/resource-manager-subscription-governance/data-collection.png)

## <a name="next-steps"></a>Következő lépések

- Most, hogy meg van értesült az előfizetés irányítási, pedig lásd a fenti ajánlást, a gyakorlatban. Példák [Azure előfizetés irányítási végrehajtása](resource-manager-subscription-examples.md).

*[Karl Kuhnhausen](https://github.com/karlkuhnhausen) járult ezt a témakört.*
