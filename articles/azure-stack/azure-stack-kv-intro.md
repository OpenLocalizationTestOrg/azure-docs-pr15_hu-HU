<properties
    pageTitle="Azure Papírhalom kulcs tárolóra – bevezetés |} Microsoft Azure"
    description="Megtudhatja, hogyan kezeli az Azure Papírhalom kulcs tárolóból elemre a kulcsok és titkos kulcsok"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="introduction-to-key-vault-in-azure-stack"></a>Azure egymást fedő kulcs tárolóra – bevezetés #

## <a name="before-you-start"></a>Előzetes teendők

Ez a cikk azt feltételezi, hogy a következőket:

- Az olvasó, amelyen engedélyezett Azure kulcs tárolóra előfizetés hozzáférése van.
- Az Azure PowerShell SDK konfigurálva vannak és rendelkezésre.
- TP2 verzióval összes bérlői elérésű művelet csak a PowerShell hajtható végre.

## <a name="key-vault-basics"></a>Kulcs tárolóra alapjai

Azure egymást fedő kulcs tárolóra védi cryptographic billentyűkkel, és titkos kulcsok, az alkalmazások és szolgáltatások cloud használja. Kulcs tárolóra használatával titkosíthatja kulcsok és titkos kulcsok (például hitelesítési kulcsokat, tároló fiók kulcsok, adatok titkosítási kulcs, .pfx fájl és jelszavakat).

Kulcs tárolóból elemre a kulcskezelő folyamat egyszerűsítik, és lehetővé teszi, hogy a billentyűket, amelyek elérése és az adatok titkosítása irányításának karbantartása. A fejlesztők fejlesztéshez és percben teszteléshez kulcsok létrehozása, és majd zökkenőmentes térjen át ezekkel gyártási billentyűk. Biztonsági rendszergazdák is megadása (és megvonása) engedéllyel kulcsok, szükség szerint.

Bárki, az Azure Papírhalom előfizetéssel rendelkező létrehozhat és fő tárolókban használja. Bár a kulcs tárolóból elemre a fejlesztők és biztonsági rendszergazdák juttatások, végrehajtani, és a szervezet más Azure Papírhalom szolgáltatások felügyelő rendszergazda által felügyelt. A rendszergazdák az Azure Papírhalom előfizetéssel bejelentkezhetek létrehozhat például egy tárolóból elemre a szervezethez, amelyben kulcsok tárolására és majd felelős a műveleti feladatok:

- Indítson vagy importáljon egy billentyűt vagy a titkos kulcs
- Visszavonása vagy titkos kulcs vagy törlése
- Engedélyezheti a felhasználók és a fő tárolóra elérheti, így a azt kezelése, vagy használja a billentyűk és titkos kulcsok alkalmazások
- Állítsa be a fő használatát (például jelentkezzen vagy titkosítása)

A rendszergazda fejlesztők biztosítson segítségével hívhat fel saját alkalmazások URL-címe, majd küldje el a biztonsági rendszergazdája kulcs használatát naplózási adatait.

A fejlesztők is kezelheti a billentyűk közvetlenül, API-khoz használatával. További információ a a kulcs tárolóra fejlesztői útmutatójában.

## <a name="scenarios"></a>Felhasználási területei

Az alábbi táblázat néhány hol segíthetnek a kulcs tárolóból elemre az igényeknek megfelelően a fejlesztők és biztonsági rendszergazdák esetet tárgyal szemlélteti:


### <a name="developer-for-an-azure-stack-application"></a>Fejlesztőeszközök az Azure Papírhalom alkalmazáshoz

**Probléma**: írja be az alkalmazás az Azure Papírhalom, amely kulcsok használja az aláírás és titkosítás szeretném, de szeretnék ezeket az alkalmazás kívüli kell, hogy a megoldás, a megfelelő földrajzi oszlik alkalmazás.

**Kimutatás**: kulcsok URI szükség esetén által indított és tárolt a tárolóból elemre.


### <a name="developer-for-software-as-a-service-saas"></a>Fejlesztőeszközök szoftverek szolgáltatásként (szoftver)

**Problémát:** Nem szeretném megjeleníteni a felelős vagy egy potenciális kötelezettség a vevők bérlői kulcsok és titkos kulcsok.

**Utasítás:** Ügyfelek saját kulcsok importálása az Azure Papírhalom, és kezelheti azokat. Szeretném saját és igényelhetnek kezelése, így e koncentráljon módon, hogy mit tenni a legjobban, amely biztosítja az alapvető szoftver funkciók az ügyfelek számára.


### <a name="chief-security-officer-cso"></a>Chief biztonsági őr (CSO)

**Problémát:** Győződjön meg róla, hogy a szervezeten kulcs életciklusának vezérlő és figyelheti a főbb használatát szeretném.

**Kimutatás** Kulcs tárolóból elemre, hogy a Microsoft nem látható, vagy kinyerheti a billentyűk lett tervezve.  Ha egy alkalmazást kell a titkosítási műveleteket vevők használatával, kulcs tárolóból elemre végzi az alkalmazás nevében. Az alkalmazás nem jelenik meg a vevők billentyűk.  Bár használjuk, hogy több Azure Papírhalom szolgáltatásokat és erőforrásokat, szeretném kezelése a billentyűk Azure egymást fedő egyetlen helyről. A tárolóból elemre egyetlen, függetlenül attól, hány tárolókban van az Azure Papírhalom, amely régiók azok felületet biztosít a támogatási, és mely alkalmazások használhatja őket.

## <a name="next-steps"></a>Következő lépések

[Első lépések a fő tárolóból elemre](azure-stack-kv-getting-started.md)
