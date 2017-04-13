<properties
    pageTitle="Azure API kezelésével szabályozásának speciális kérés"
    description="Megtudhatja, hogy miként hozhat létre és rugalmas kvóta és ráta korlátozása Azure API adatkezelési házirendeket alkalmazhatja."
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="advanced-request-throttling-with-azure-api-management"></a>Az Azure API-kezelés szabályozásának speciális kérés

Azure API-kezelés szerepet bejövő felkérést szabályozása is. Akár kéréseket vagy a teljes kéréseket/átadott mértéke ellenőrzésével API-kezelés lehetővé teszi a API-szolgáltatók az API-khoz védeni abuse, és hozhat létre a különböző API-termék rétegek értékét.

## <a name="product-based-throttling"></a>Termék alapú szabályozása
Dátum, az árfolyam szabályozásának funkciók lett korlátozott, hogy az adott termék előfizetést (lényegében egy kulcsot) korlátozódik, definiált az API kezelése a publisher-portálon. Ez a API-szolgáltató korlátozásai vonatkoznak a fejlesztők számára, akik használni a API regisztráltak a hasznos, azonban ez nem segít, például az egyéni végfelhasználók a API szabályozásának. Lehetséges, hogy a felhasználó a fejlesztői alkalmazás teljes kvóta felhasználása és a többi ügyfél adataitól a fejlesztő megakadályozása alkalmazással való egyszeri. Is előfordulhat, hogy készítése kérések nagy mennyiségű több felhasználókat korlátozhatják a alkalmi felhasználók való hozzáférést.

## <a name="custom-key-based-throttling"></a>Egyéni kulcsú szabályozása
Az új [ráta-korlát-szerint – kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) és [a billentyű-kvóta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) házirendeket forgalom vezérlő jelentősen rugalmasabb megoldást nyújtanak. Ezek a házirendek azonosítja a billentyűket, amelyek a forgalom használatának nyomon követése használt kifejezések megadását teszi lehetővé. Ez számos módon easiest szemlélteti, például a. 

## <a name="ip-address-throttling"></a>IP-cím szabályozása
Az alábbi házirendeket alkalmazhatja korlátozása egy egyetlen ügyfél IP-címe a hívások csak 10 percenként 1,000,000 hívások összesen és 10 000 KB havonta sávszélességet. 

    <rate-limit-by-key  calls="10"
              renewal-period="60"
              counter-key="@(context.Request.IpAddress)" />

    <quota-by-key calls="1000000"
              bandwidth="10000"
              renewal-period="2629800"
              counter-key="@(context.Request.IpAddress)" />

Összes ügyfelet az interneten használt egy egyedi IP-címet, lehet, hogy a felhasználó által használat korlátozása hatékony módszer. Azt azonban nem igazán valószínű, hogy több felhasználó fog megosztása miatt őket az interneten keresztül a hálózati Címfordítást eszköz elérése egyetlen nyilvános IP-címet. Ezzel az illetéktelen hozzáférést lehetővé tevő API-khoz ellentétben a `IpAddress` lehet, hogy a legjobb megoldást.

## <a name="user-identity-throttling"></a>Felhasználói azonosító szabályozása
Ha a végfelhasználó hitelesített, majd a szabályozási kulcs jön létre, amely az, hogy egyedi módon azonosító alapján felhasználói.

    <rate-limit-by-key calls="10"
        renewal-period="60"
        counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />

Ez a példa azt az engedélyezési fejléc kibontása, átalakíthatja `JWT` objektum és a token tárgya azonosíthatja a felhasználót, és azt használja kulcs korlátozása kamatráta segítségével. Ha a felhasználó személyazonosságára van tárolva a `JWT` , az egyik a másik követelések majd, hogy érték felhasználható elfoglalt helyét.

## <a name="combined-policies"></a>Kombinált házirendek
Az új szabályozási házirendeket biztosít a meglévő szabályozási házirendek-nél több szabályozási, de nincs továbbra is érték mindkét funkciók egyesítésével. Ahhoz, hogy egy API úgy használatát szintek alapján monetizing remekül előfizetés termékkulcsot ([előfizetés korlát hívás arányt](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) és [előfizetés keretében használati kvóta beállítása](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) szerint szabályozásának. A felhasználó által szabályozása való táblázatrácsot nyomtatott vezérlő kiegészítő, és megakadályozza, hogy egyetlen felhasználó viselkedése a felület egy másik elrontása. 

## <a name="client-driven-throttling"></a>Ügyfél-alapú szabályozása
A szabályozási billentyűt van beállítva a [házirend-kifejezést](https://msdn.microsoft.com/library/azure/dn910913.aspx), majd esetén az API-szolgáltató, amely megválasztása hogyan a szabályozásának megfelelően változik. Azonban egy fejlesztő szabályozhatja, hogy hogyan azok ráta korlát saját érdemes ügyfelek. A sikerült engedélyezni az API-szolgáltató által egy egyéni élőfejet bevezetésével a fejlesztői ügyfélalkalmazás az API-nak a kulcs kommunikáció engedélyezése.

    <rate-limit-by-key calls="100"
              renewal-period="60"
              counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>

A Fejlesztőeszközök ügyfélalkalmazás adja meg, hogyan azokat a kulcs korlátozása ráta létrehozása lehetővé teszi. Egy kis nyújt az ügyfél fejlesztői lehetett létrehozni, saját rétegek kulcsok készletek lefoglalása a felhasználók és forgatása a fontosabb használatát.

## <a name="summary"></a>Összefoglalás
Azure API-kezelés itt rátát és védelme és az értékkel adja hozzá az API-szolgáltatás szabályozásának árajánlat. Az új, egyéni tartalmazó szabályok szabályozási házirendeket táblázatrácsot nyomtatott szabályozni kell ahhoz, hogy ügyfelei számára még jobb alkalmazások házirendekhez engedélyezése. A jelen cikkben szereplő példák bemutatják, ezek a házirendek felhasználása gyártási mérték korlátozása billentyűk ügyfél IP-címek, felhasználói identitások és generált ügyfél értékek szerint. Vannak azonban olyan sok más részei az üzenetet, amely felhasználható például felhasználói ügynököt, az URL-cím elérési út töredékek, az üzenet mérete.

## <a name="next-steps"></a>Következő lépések
Adjon visszajelzést a Disqus szálhoz tartozó üzenet az ebben a témakörben. Más potenciális fő érték, amely egy logikai választási lehetőségek, a következő helyzetekben lett szóló értesítésekre nagyszerű lenne.

## <a name="watch-a-video-overview-of-these-policies"></a>Ezek a házirendek áttekintését bemutató videó megtekintése
A cikkben szereplő [ráta-korlát-szerint – kulcs](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) és [a billentyű-kvóta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) házirendek megtekintés a következő videót.

> [AZURE.VIDEO advanced-request-throttling-with-azure-api-management]
