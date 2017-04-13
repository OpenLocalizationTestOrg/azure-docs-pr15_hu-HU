<properties
   pageTitle="Az adatok szolgáltatás létrehozása a piactér technikai előzetes követelmények |} Microsoft Azure"
   description="A szolgáltatás üzembe helyezéséhez és értékesítése a Microsoft Azure piactéren lévő adatok létrehozása követelményei ismertetése"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>A Microsoft Azure piactéren ajánlja fel a technikai előzetes követelmény az adatok szolgáltatás létrehozása

>[AZURE.IMPORTANT] **Adott időben azt már nem bevezetési minden új adatszolgáltatás közzétevők. Új dataservices nem első jóváhagyva listaelem.** Ha a szoftver üzleti alkalmazások AppSource a közzétenni kívánt talál további információt [Itt](https://appsource.microsoft.com/partners). Ha egy IaaS alkalmazások vagy Fejlesztőeszközök szolgáltatás szeretné közzé a Microsoft Azure piactéren, hogy több információt találhat [az alábbi](https://azure.microsoft.com/marketplace/programs/certified/).

A folyamat alapos megkezdése előtt és érthetőbbé, ahol, és miért minden egyes lépés végrehajtása. Szerint lehetséges, meg kell készítse el a vállalat adatainak és egyéb adatok, letöltéséhez szükséges eszközök és technikai összetevőket hozhat létre az ajánlat létrehozási folyamat megkezdése előtt.

Rendelkeznie kell készen áll a folyamat megkezdése előtt az alábbiakat:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Teheti meg, hogy milyen technológiát szolgálnak az adatok szolgáltatás ajánlat közzététele

A Publisher megadhatja, hogy az Azure piactéren elérhető adatok szolgáltatás közzétételekor több technológiák között. A fő technológiák által támogatott leírása alább. Függetlenül milyen technológiát közzétenni a adatok szolgáltatást használja, a végfelhasználói az adatok fogyasztása az **OData-adatcsatorna** Azure piactér szolgáltatás által elérhetővé tett keresztül. Az OData-szolgáltatás teljes körű tájékoztatást talál a [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>Az SQL Azure-adatbázis

Adatkészlet készen áll az SQL Azure-ban, akkor a Publisher felelősség. Azure előfizetés, kiépítése adatbázis megfelelő méretét, és töltse fel a az adatok az SQL Azure-adatbázis kell. A Publisher egyben felelős oktatók adatok mindig naprakész. Előfizetés a Microsoft Azure-szolgáltatások további információt találhat a [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)


Ha az adatok áthelyezése SQL Azure, a Microsoft Azure piactéren táblák és nézetek is fedheti fel. A Publisher is megadhat, mely táblák és nézetek és az oszlopokat a végfelhasználói kitenni. A tartalomszolgáltató további is megadhatja, melyik oszlopot a végfelhasználó által lekérdezhető, és melyeket csak a tartalomban adja vissza. Ezzel a magas fokú rugalmasság arról, hogy melyik az adatbázisban tárolt adatok elérhetővé tett kap. Oszlopokat, amelyeket lekérdezhető kell szerint egy vagy több adatbázis indexek készül.

## <a name="rest-based-web-service"></a>A többi alapú webes szolgáltatás

Támogatott protokollok: **csak HTTPS**

A Microsoft Azure piactéren keresztül is érhetők el a meglévő többi alapú szolgáltatásokat. Az adatkészlet értéke mindig csak akkor érhető el a végfelhasználói, mint egy OData-adatcsatorna, mert a az Azure piactéren elérhető szolgáltatásokra van szüksége, engedélyezni szeretné a szolgáltatás megfeleltetése OData-alapú szolgáltatást. A többi alapú végpontok ehhez szükséges kattintva jelenítse meg az összes paramétert HTTP paraméterként.

A tartalom kell lennie az űrlapokon, amely ATOM választ be csatolható. Ezért a válasza a szolgáltatások szót XML formátumban, és hogy csak egy ismétlődő elemet (például a rekord beállítása) terhelés értékeket tartalmazó tartalmazzák. A Microsoft Azure piactéren szolgáltatás leképezése az ismétlődő csomópontot a bejegyzés csomóponthoz ATOM és a tartalom értékeket be tulajdonság található csomópontok a bejegyzés csomópontot.

Hitelesítési információt (például API esetén hitelesítési jogkivonat, stb.) kell lennie, feltéve HTTP paraméterként vagy a HTTP-fejléc (elsődlegeskulcs-értékének pár) – az alapszintű hitelesítés is támogatott. Érvényes kulcsot kell ellátni, és minden keresztül Azure piactéren elérhető alatt álló kérések keresztül billentyűre. A felhasználó ellenőrzése és a számlázási az Azure piactéren elérhető rétegben történik.

A szolgáltatás által visszaadott hibák kell HTTP állapot kódok rendelhető hozzá. Abban az esetben, ha a szolgáltatás a HTTP-állapot kódokat az Azure piactéren elérhető szolgáltatás kell megfeleltetni megy, ezek a hibát tartalmazó XML adja eredményül.

## <a name="soap-based-web-services"></a>SOAP-alapú webes szolgáltatások

**Csak HTTPS** protokollt:

A követelmények esetén ugyanaz, mint a többi szolgáltatás szakaszban. Az egyetlen különbség, hogy paramétereket is biztosítható, hogy egy XML törzsébe közzétett alatt a Publisher szolgáltatás minden kérésével keresztül Azure piactérről. Ez azt jelenti, hogy a felhasználó biztosít az előtér-a HTTP paraméterek vannak éppen fordítani az XML-dokumentum és az értekezlet-összehívást a tartalomszolgáltató webszolgáltatás közzétett XML-elemek.

## <a name="odata-based-web-services"></a>Az OData-alapú webes szolgáltatások

**Csak HTTPS** protokollt:

Adatok az Azure piactéren elérhető OData szolgáltatások is elérhető. A rendszer a szolgáltatás keresztül átadni fog, és a szolgáltatás legfelső szintű cseréli a Microsoft Azure piactéren szolgáltatás legfelső szintű – minden további hívások Menjen végig a Microsoft Azure piactéren.

Az OData-szolgáltatások csak egy adatbázist, a kódmentes ellen lépjen nem szükséges. OData bármilyen típusú tároló vagy logikai meghajtóra a szolgáltatást támogat.


## <a name="next-steps"></a>Következő lépések
Most, hogy a előtti követelmények felül, és a szükséges műveletek végrehajtása, áthelyezheti előre a diagramok létrehozásának az adatok szolgáltatás ajánlat az [Adatok szolgáltatás közzétételi útmutató](marketplace-publishing-data-service-creation.md)ismertetett módon.

Vagy, ha szeretné, hogy az egész folyamatot, és a megfelelő cikkeket áttekinteni az egyes közzétételi fázisait, kérjük, látogasson el a cikk [első lépések: a Microsoft Azure piactéren felajánlás közzététele](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
