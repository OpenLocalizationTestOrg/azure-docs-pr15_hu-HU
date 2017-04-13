<properties
    pageTitle="Mi az Azure kulcs tárolóra? | Microsoft Azure"
    description="Azure kulcs tárolóból elemre a védelmét cryptographic billentyűkkel és titkos kulcsok felhő alkalmazások és szolgáltatások által használt segítségével. Azure kulcs tárolóra használatával ügyfelek titkosíthatja kulcsok és titkos kulcsok (például a hitelesítési kulcsokat, a tárhely fiók kulcsok, az adatok titkosítási kulcsok. PFX fájlok és a jelszavakat) hardver biztonsági modulokat (HSMs) eső billentyűk segítségével."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="cabailey"/>



# <a name="what-is-azure-key-vault"></a>Mi az Azure kulcs tárolóra?

Azure kulcs tárolóból elemre a legtöbb régióban érhető el. További tudnivalókért olvassa el a [kulcs tárolóra árak, oldal](https://azure.microsoft.com/pricing/details/key-vault/)című témakört.

## <a name="introduction"></a>– Bevezetés

Azure kulcs tárolóból elemre a védelmét cryptographic billentyűkkel és titkos kulcsok felhő alkalmazások és szolgáltatások által használt segítségével. Kulcs tárolóra használatával titkosíthatja kulcsok és titkos kulcsok (például a hitelesítési kulcsokat, a tárhely fiók kulcsok, az adatok titkosítási kulcsok. PFX fájlok és a jelszavakat) hardver biztonsági modulokat (HSMs) eső billentyűk segítségével. A hozzáadott garancia importálhat és kulcsok HSMs készítése. Ha úgy dönt, hogy a művelet, a billentyűk FIPS 140-2 szint 2 érvényesített HSMs (a hardver és a belső vezérlőprogram) Microsoft folyamatok.  

Kulcs tárolóból elemre a kulcskezelő folyamat egyszerűsítik, és lehetővé teszi, hogy a billentyűket, amelyek elérése és az adatok titkosítása irányításának karbantartása. A fejlesztők fejlesztéshez és percben teszteléshez kulcsok létrehozása, és majd zökkenőmentes térjen át ezekkel gyártási billentyűk. Biztonsági rendszergazdák is megadása (és megvonása) engedéllyel kulcsok, szükség szerint.

Használja az alábbi táblázat jobb ismerje meg, hogyan kulcs tárolóból elemre segíthet a fejlesztők és biztonsági rendszergazdák igényeinek megfelelően.





| Szerepkör        | A problémáról           | Végrehajtása után Azure kulcs tárolóból elemre  |
| ------------- |-------------|-----|
| Fejlesztőeszközök az Azure alkalmazáshoz      | "Szeretnék írni az alkalmazások az Azure által billentyűk az aláírás és titkosítás, de az alkalmazás kívüli kell, hogy a megoldás, földrajzi oszlik alkalmazás megfelelő billentyűk szeretném. <br/><br/>Szeretném is, ezek a billentyűk és titkos kulcsok védett, anélkül, hogy saját magam írja be a kódot. Is szeretnék ezek a billentyűk és titkos kulcsok kell, hogy használja az alkalmazásokat az optimális teljesítmény rendszerről." | Removed billentyűk tárolt a tárolóból elemre, és szükség esetén URI által indított.<br/><br/> Removed billentyűk személyének Azure, szabványos algoritmusok, a fő hossza és a hardver biztonsági modulokat (HSMs) szerint.<br/><br/> Az azonos Azure adatközpontokkal alkalmazásokkal tartományban található HSMs removed billentyűk feldolgozása. Ezzel a megoldással megbízhatóságot és jobb mint, ha a billentyűk egy külön helyen tárolnak, például a helyszíni csökkentett késés.|
| Fejlesztőeszközök szoftverek szolgáltatásként (szoftver)      |"Nem szeretném megjeleníteni a felelős vagy a potenciális felelősség a vevők bérlői kulcsok és titkos kulcsok. <br/><br/>Szeretném a és az ügyfelek számára saját igényelhetnek kezelése, így e koncentráljon módon, hogy mit tenni a legjobban, amely biztosítja az alapvető szoftver funkciók." | Removed ügyfelek saját kulcsok importálása az Azure, és kezelheti azokat. A szoftver alkalmazást kell a titkosítási műveleteket ügyfeleik használatával, kulcs tárolóból elemre az alkalmazás nevében ezeket a műveleteket hajtja végre. Az alkalmazás nem jelenik meg a vevők billentyűk.|
| Fő biztonsági őr (CSO) | "Szeretnék tudni, hogy az alkalmazások megfelelnek FIPS 140-2 szintet 2-HSMs biztonságos kulcskezelő. <br/><br/>Győződjön meg róla, hogy a szervezeten kulcs életciklusának vezérlő és figyelheti a főbb használatát szeretném. <br/><br/>És használjuk, hogy több Azure szolgáltatásokat és erőforrásokat, bár szeretném a billentyűk kezelése az Azure-ban egy helyen."     |Removed a HSMs 140-2 szintet 2 érvényesített FIPS.<br/><br/>Removed kulcs tárolóból elemre, hogy a Microsoft nem látható, vagy kinyerheti a billentyűk lett tervezve.<br/><br/>Valós idejű naplózási főbb felhasználási közelében removed.<br/><br/>Removed a tárolóból elemre egyetlen felületet, függetlenül attól, hány tárolókban van az Azure-azokat a területeket azok biztosít a támogatási, és mely alkalmazások használhatja őket. |


Bárki, az Azure előfizetéssel rendelkező létrehozhat és fő tárolókban használja. Kulcs tárolóból elemre a fejlesztők és biztonsági rendszergazdák juttatások, bár azt sikerült kell végrehajtani, és a szervezet rendszergazdája egyéb Azure szolgáltatás egy szervezet kezelő kezeli. Például volna, jelentkezzen be az Azure előfizetéssel, a rendszergazda hozzon létre egy tárolóból elemre, amelyben kulcsok tárolására és majd felelős műveleti feladatok, például a szervezet számára:

+ Indítson vagy importáljon egy billentyűt vagy a titkos kulcs
+ Visszavonása vagy titkos kulcs vagy törlése
+ Engedélyezheti a felhasználók és a fő tárolóra elérheti, így a azt kezelése, vagy használja a billentyűk és titkos kulcsok alkalmazások
+ Állítsa be a fő használatát (például jelentkezzen vagy titkosítása)
+ Fő használat figyelése

A rendszergazda, akinek majd fejlesztők biztosítson segítségével hívhat fel saját alkalmazások URL-címe, és küldje el a biztonsági rendszergazda kulcs használatát naplózási adatait. 

   ![Azure kulcs tárolóra áttekintése][1]

A fejlesztők is kezelheti a billentyűk közvetlenül, API-khoz használatával. További információ a [a kulcs tárolóra fejlesztői](key-vault-developers-guide.md)útmutatójában.

## <a name="next-steps"></a>Következő lépések

Rendszergazda számára beolvasása lépések oktatóanyagot lásd: [Első lépések az Azure kulcs tárolóból elemre](key-vault-get-started.md).

További információt a naplózás kulcs tárolóból elemre a használatát olvassa el a [Azure kulcs tárolóra naplózás](key-vault-logging.md)című témakört.

Kulcsok és titkos kulcsok használata Azure kulcs tárolóra kapcsolatos további tudnivalókért olvassa el a [billentyűk kapcsolatos, a titkos kulcsok, és a tanúsítványok](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx)című témakört.


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
