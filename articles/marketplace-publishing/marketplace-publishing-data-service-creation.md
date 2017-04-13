<properties
   pageTitle="Útmutató a adatok szolgáltatás létrehozása a piactér |} Microsoft Azure"
   description="Részletes útmutatást, hogy miként hozhat létre, tanúsítása és az adatok szolgáltatás üzembe vásárolja meg a Microsoft Azure piactéren."
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

# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Útmutató a Microsoft Azure piactérről a közzétételi adatok szolgáltatás

>[AZURE.IMPORTANT] **Adott időben azt már nem bevezetési minden új adatszolgáltatás közzétevők. Új dataservices nem első jóváhagyva listaelem.** Ha a szoftver üzleti alkalmazások AppSource a közzétenni kívánt talál további információt [Itt](https://appsource.microsoft.com/partners). Ha egy IaaS alkalmazások vagy Fejlesztőeszközök szolgáltatás szeretné közzé a Microsoft Azure piactéren, hogy több információt találhat [az alábbi](https://azure.microsoft.com/marketplace/programs/certified/).

1, a [fiók létrehozását és a regisztrációs](marketplace-publishing-accounts-creation-registration.md), lépés végrehajtása után azt interaktív, [nem technikai jellegű általános](marketplace-publishing-pre-requisites.md) és egy adatszolgáltatás ajánlat [Műszaki követelmények](marketplace-publishing-data-service-creation-prerequisites.md) keresztül a Microsoft Azure piactéren. Most, hogy végigvezeti a lépéseket a adatszolgáltatás ajánlat létrehozásához a [Közzétételi portál] [ link-pubportal] az Azure piactér.

## <a name="1---login-to-the-publishing-portal"></a>1. bejelentkezési a közzétételi portál.

Nyissa meg a [https://publish.windowsazure.com](https://publish.windowsazure.com. )

**Közzétételi portál jelentkezni első alkalommal használja ugyanazt a fiókot, amellyel a vállalatprofil eladó regisztrálva volt Developer Center.**  (Később is hozzáadhat a vállalat bármely alkalmazott a közzétételi portál további-rendszergazdaként).

Ha ez az első alkalommal jelentkezik be a közzétételi portál, kattintson a **Közzététel a Data Services** csempére.

## <a name="2---choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2. a **Data Services** válassza a bal oldalán a navigációs menüben.

  ![rajz](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3---create-a-new-data-service"></a>3. Hozzon létre egy új adatok

A cím adja meg az új adatok szolgáltatás ajánlja fel, majd kattintson a "+" a jobb oldalon.

  ![rajz](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4---review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4. a alszint menüje a navigációs menüben az újonnan létrehozott adatok szolgáltatás áttekintése

Kattintson a **forgatókönyv** lapon, és tekintse át a megfelelően a szolgáltatást az Azure piactéren elérhető adatok közzététele szükséges az összes szükséges lépéseket.

> [AZURE.TIP] Mindig a hivatkozások "Forgatókönyv" lapon kattintson vagy lapok használata a adatszolgáltatás ajánlat almenüje bal oldalán.

## <a name="5---create-a-new-plan"></a>5. Hozzon létre egy új csomagban.

### <a name="offers-plans-transactions"></a>Kínál, tervek, tranzakciók.

Egyes ajánlatok beállíthatja, hogy több tervek, de kell legalább egy (1)-csomaggal rendelkezik-e. Amikor a végfelhasználók fizessen elő az ajánlat azokat az ajánlat terv egyik fizessen elő. Minden terv határozza meg, hogyan végfelhasználók tudják használni a szolgáltatást.

Microsoft Azure piactéren jelenleg csak a havi előfizetés tranzakció alapú modell támogatási Data Services, tehát a végfelhasználók aszerint, hogy az adott terv azok fizetett elő, az ár havi díjat kell fizetnie, és minden egyes hónap száma a terv által meghatározott tranzakció használhatnak.

Az adatok szolgáltatás által visszaadható rekordok számát általában tranzakciók a szolgáltatás elküldi a lekérdezés alapján. Az alapértelmezett érték 100. Minden lekérdezés vissza tranzakciók száma szám lesz a rekordok 100 és a legközelebbi egész számra felfelé kerekített osztva.

Feladata Azure piactér szolgáltatás réteg Lync-tranzakciók, mindkét lekérdezés elfogyasztott (állapotjelzője) száma.

> [AZURE.IMPORTANT] A szolgáltatás használatához a havi előfizetés ciklusának a végéig folyamatos, azzal blokkolhatja a hónapban tranzakció elérte végfelhasználók.

> A csomagot vagy a tervek az egyik is (de nem kell) tranzakciók tetszőleges számú tartalmazza.

### <a name="create-a-plan"></a>Terv létrehozása
1. Kattintson a **"+"** a "hozzáadása egy új terv" mellett.

2. Válasszon a lehetőségek közül: Ez terv **korlátlan** vagy **korlátozott** használatát.  Ha a korlátozott majd meg kell adnia a terv lehetővé teszi a hónapban használhatnak tranzakciók száma.

    ![rajz](media/marketplace-publishing-data-service-creation/step-5.1.png)  

    Közzétételi portál is javasolt a "Terv azonosító", amely kapcsolatba lépni a végfelhasználók számára a felhasználói felület előfizetésére használt és is használja a piactér szolgáltatás azonosítja a csomagot. Ha azt szeretné, módosíthatja a "terv azonosító".

    > [AZURE.NOTE] A "terv azonosító" belül az egyes ajánlatok köre egyedinek kell lennie. A közzétételi portál megtervezése azonosító használt számos más azonosítók zárolva lesz a első közzétételi gyártási, és nem tudja módosítani az azonosító után.

3. Fogadja el a választás gombra.

4. Ezután, megkéri néhány további kérdésekre kapcsolatban az újonnan létrehozott csomagot.

    ![rajz](media/marketplace-publishing-data-service-creation/step-5.2.png)


|Kérdés|Pontosság|
|----|----|
|**Ez a terv az ingyenes és elérhető világszerte?**|Teljesen szabad-a-ingyenesen terv létrehozása Ajánlatra – a csak csomag esetén az azt jelenti, hogy webkiszolgálón teszi közzé "Ingyenes ajánlat" a a piactéren. Ha csak az egyik (kevés) használ, az informatikai felkínálja végfelhasználók számára, ha többet szeretne tudni a tranzakciók havonta viszonylag kevés szolgáltatás kínálatát.  Ha a válasz "Igen", nincs további kérdéseket a rendszer kéri.|

> [AZURE.NOTE] A végfelhasználók folyamatos frissíthet a kifizetett csomag.

|Kérdés|Pontosság|
|----|----|
|**Van ingyenes kipróbálására?**|Válassza a "Nincs próbaverzió" közötti egyáltalán, vagy adjon meg egy csoportban az"egy" csomagja. A közzétevőknek szeretné ezt a beállítást használva a vonatkozó ingyenesen egy hónappal előnyei megértéséhez lehetőséget nyújt a végfelhasználók.|

> [AZURE.IMPORTANT] Végfelhasználók csak akkor tudja ingyenes próbaverziót vásárolni, ha az általuk létrehozott fizetési eszköz például hitelkártya vállalati szerződés.

> Után egy hónappal a az ingyenes próbaverziót Azure piactéren elérhető elkezdenek töltődik ügyfelek az ár kezdve a dátumot, az előfizetés, kivéve, ha az ügyfél által kezdeményezett az előfizetés lemondásáról. Speciális értesítésre nem biztosítja a végfelhasználók számára.

|Kérdés|Pontosság|
|----|----|
|**Ez a terv egy promóciós kódot kell vásárolnia van szükség?**| Közzétevők kell bizonyos felhasználók korlátozhatja a hozzáférést a szolgáltatás tervek, mert egy kódot, "A Promocode" nevű beállítást. Csak a Promocode lesz végfelhasználók tudják fizessen elő a csomagot. Ha úgy dönt, hogy a "Nem", majd Ön vállalja, hogy a terület, ahol az ajánlat érhető el (lásd: a [Piactér marketingtevékenység tartalmi útmutató](marketplace-publishing-push-to-staging.md) további részleteket) mindenkit fogja tudni e terv előfizetés. Nincsenek további kérdések kérni fogja.|
|**Ez a terv bárki, aki nem tartozik egy érvényes promóciós kódot az is elrejtése?**|Ha a válasz, az előző kérdésre "Igen" a Publisher tartalmaz a távolítsa el teljesen az e terv megjelenjenek a felhasználói felületen a a piactér beállítást. Az azt jelenti, ügyfelek nem jelennek meg ezt az ajánlatot Részletek lap csomagra. Végfelhasználók, amely megkapja kell vásárolnia, egy promocode tudják fizessen elő a promocode használatával.|

## <a name="6---create-your-marketplace-marketing-content"></a>6. a piactérről marketingtartalmak létrehozása
Adja meg a szükséges információt a **Marketing, a árak, a támogatási és a kategóriák** lapon kérjük, keresse fel az összes eltérések közös [Piactér marketingtevékenység tartalmi útmutató](marketplace-publishing-push-to-staging.md) az Azure piactéren elérhető közzétett témakört.  

## <a name="7---connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7. az ajánlat kapcsolódni a Service (SQL Azure-alapú vagy webszolgáltatás alapján).

Kattintson a **Data Services** alszint menüben.

A lap felső részén a kéri az ajánlat **Namespace**megadását.  

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.png)

A kérdés alatt határozza meg a Publisher fog hogyan elérhetővé tétele az újonnan létrehozott ajánlatot, hogy a Microsoft Azure piactéren. (Lásd: az [Adatok szolgáltatások technikai kapcsolatban előzetesen szükséges útmutató](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Közzététel az adatbázis-alapú szolgáltatás**

Kattintson a **adatbázis**. A megjelenő lap jelenik meg:

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.3.png)

Az adatkészlet az SQL Azure-adatbázis alapján CSDL megfeleltetés létrehozása

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.4.png)

Majd mindegyik táblázathoz

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.6.png)

Ha a webes szolgáltatás

  ![rajz](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [AZURE.IMPORTANT] Olvassa el a [meglévő webes szolgáltatásainak CSDL keresztül OData-leképezés](marketplace-publishing-data-service-creation-odata-mapping.md) részletes útmutatást és a példákat CSDL webszolgáltatás hozhat létre.

## <a name="next-steps"></a>Következő lépések
Most, hogy az adatok szolgáltatás ajánlat létrehozta, ellenőrizze, hogy az utasításokat a [Piactér marketingtevékenység tartalmi útmutató](marketplace-publishing-push-to-staging.md) végrehajtása előtt meg előrelépés [az adatok szolgáltatásba átmeneti tesztelése](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Lásd még:
- [Első lépések: Útmutató felajánlás közzététele az Azure piactérről](marketplace-publishing-getting-started.md)
- Ha érdeklik a teljes OData-megfeleltetés folyamat és célját ismertetése, olvassa el az Ez a cikk [Adatok szolgáltatás OData-hozzárendelés](marketplace-publishing-data-service-creation-odata-mapping.md) , tekintse át a definíciókat, szerkezetek és az utasításokat.
- Ha érdeklik a tanulási az adott csomópontok és a paraméterek, ebből a cikkből [Adatok szolgáltatás OData-leképezés csomópontok](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definíciók és magyarázatokat, példák és használatieset-környezetben használja.
- Ha érdeklik a példák megtekintésével, olvassa el a jelen cikk lásd: minta kód és értelmezhető a mezőkódok szintaxisa és a helyi [Adatok szolgáltatás OData-leképezés példák](marketplace-publishing-data-service-creation-odata-mapping-examples.md) .


[link-pubportal]:https://publish.windowsazure.com
