
<properties
    pageTitle="Azure egységes tároló: különbségek és szempontok |} Microsoft Azure"
    description="Ismerje meg az Azure-tárhely és a más Azure egységes tároló telepítése során megfontolandó kérdések közötti különbségeket."
    services="azure-stack"
    documentationCenter=""
    authors="MChadalapaka"
    manager="siroy"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="mchad"/>

# <a name="azure-consistent-storage-differences-and-considerations"></a>Azure következetes tároló: különbségek és szempontok

Azure egységes tároló a Microsoft Azure egymást fedő tároló felhőszolgáltatásba megadása. Azure egységes tároló blob, a táblázat, a várólista és a fiók kezelése funkció az Azure egységes szemantikáját biztosít. Ez a cikk az Azure adathordozós ismert Azure egységes tároló különbségek foglalja össze. Is érdemes észben tartania, ha telepíti a Microsoft Azure Papírhalom jelenleg nyilvánosan elérhető előzetes verzióját egyéb szempontok foglalja össze.

<span id="Concepts" class="anchor"><span id="_Toc386544169" class="anchor"><span id="_Toc389466742" class="anchor"><span id="_Ref428966996" class="anchor"><span id="_Toc433223853" class="anchor"></span></span></span></span></span>
## <a name="known-differences"></a>Ismert különbségek

Azure egységes tároló technikai előzetes verziójában nincsenek 100 %-os funkció eltérés áll fenn Azure adathordozós támogatott API verziójához. Ismert szolgáltatásbeli különbségek a következők lehetnek:

-   Bizonyos tároló fióktípusok még nem érhetők el, ha például Standard\_RAGRS és a szokásos\_GRS.

-   Prémium\_LRS tároló fiókok kiépítése. Vannak azonban jelenleg nincs teljesítményét korlátai vagy garanciákkal.

-   Azure fájlok funkciót egyelőre nem érhető el.

-   Az első oldal csak jelszóval módosítható tartományok API nem támogatja a lekérés lapok, oldal BLOB egy adott időpontban érvényes alkalmazás közötti eltérő.

-   Az első oldal csak jelszóval módosítható tartományok API engedélyeknek 4 KB oldalakról adja eredményül.

-   Partition és Azure egységes tároló tábla végrehajtásában sor kulcs mindegyike 400 karaktereket (Ez azt jelenti, hogy 800 bájt) méretű korlátozódik.

-   Azure-következetes tároló Blob szolgáltatás végrehajtásához BLOB nevek 880 karaktereket (Ez azt jelenti, hogy 1760 bájt) méretű korlátozódik.

-   Bérlői tárterület használatát adatszolgáltatás biztosít, amelyek megegyeznek az Azure, a függvény szemantikáját és a mennyiség tároló használatát méter Azure egységes tároló végrehajtása. Jelenleg azonban a tárhely tranzakciók használatát állapotjelzője nem tartalmaz IaaS tranzakciók, és az adatok átvitele használatát állapotjelzője nem tesz különbséget a sávszélesség-használat belső és külső hálózati forgalmának engedélyezésére az Azure Papírhalom régió szerint.

-   Bizonyos különbségek vannak, a hatókör a tárhely kezelhetőség a használható funkciók körét. Ha például nem a fiók típusának módosítása vagy egyéni tartomány. Ezenkívül a prémium verzióban csak API-szintű konzisztencia felajánlott\_LRS tároló fióktípus.

## <a name="deployment-considerations"></a>Telepítéssel kapcsolatos szempontok

-   **Csak tesztet.** Telepítse az Azure egységes tároló éles környezetben, amely a Microsoft Azure Papírhalom Technical Preview jelenlegi kiadás használata. Ez a verzió csak kiértékelés célokra próba tesztkörnyezetben jelenti.

-   **Teljesítményét**. A Microsoft Azure Papírhalom Technical Preview Azure egységes tároló verziója nem teljesen teljesítményt optimalizálva.

-   **Méretezhetőség.** A Microsoft Azure Papírhalom Technical Preview verzióját Azure egységes tároló nem méretezhetőség van optimalizálva.

## <a name="version-support-considerations"></a>Verzió támogatási kapcsolatos szempontok

A következő verzióira jelen előzetes verziójában Azure egységes tároló támogatottak:

> [Microsoft Azure tároló 6.x .NET SDK](http://www.nuget.org/packages/WindowsAzure.Storage/6.2.0) Azure tároló ügyfél tár:
>
> Azure tároló adatszolgáltatások: [2015-04-05 REST API-verzió](https://msdn.microsoft.com/library/azure/mt705637.aspx)
>
> Azure tároló management szolgáltatást: [a Skype 2015-05-01-előnézet](https://msdn.microsoft.com/library/azure/mt163683.aspx)
> [2015-06-15](https://msdn.microsoft.com/library/azure/mt163683.aspx)
## <a name="next-steps"></a>Következő lépések

-   [Azure egységes tároló – bevezetés](azure-stack-storage-overview.md)
