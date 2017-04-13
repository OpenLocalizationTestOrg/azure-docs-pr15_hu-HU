<properties
   pageTitle="Szolgáltatás háló fürt erőforrás-kezelő - affinitás |} Microsoft Azure"
   description="Szolgáltatás háló szolgáltatások affinitás beállítását."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Szolgáltatás háló szolgáltatás affinitás használata és konfigurálása

Affinitás az biztosított elsősorban a vezérlőelem nagyobb egységes alkalmazások áttérés megkönnyítése az a felhő és microservices világ érdekében. Azt is használható bizonyos esetekben a jogos optimalizálása mint gördülékenyebbé tételéhez szolgáltatást, bár ez lehet hatásai említett.

Tegyük fel, hogy éppen hárul, egy nagyobb alkalmazást, vagy amelyik közvetlenül nem lett megtervezni szem előtt, a szolgáltatás háló microservices. Ez a változás ténylegesen közös, és azt a lyncen több ügyfelek (belső és külső) ebben az esetben. Először be a teljes alkalmazás környezet első meg a csomagolni és futó feloldása. Ezután megkezdése be, hogy az összes beszélgetni egymással különböző kisebb szolgáltatások bontásban.

Akkor "Oops...". A "Oops" általában sorolhatók ezekben a kategóriákban:

1. Az egységes alkalmazásban néhány összetevő X Y összetevő egy dokumentált függőség volt, és azt csak most kapcsolta be a szolgáltatások külön azokat. Mivel az ilyen letöltése forgalmi a fürt csomópontjai, legyenek hibás.
2.  Az alábbi tényezőket villámgyorsan lehet (nevesített csövek helyi |} memória megosztott |} lemezen fájlokat), de engedélyezni szeretné, hogy egymástól függetlenül frissítése sebesség elemeinek be egy kicsit igazán szüksége. A merevlemez függőség később fogja eltávolítok.
3.  A szolgáltatás nem kell aggódnia, de, hogy ezt a két összetevőt ténylegesen nagyon is vannak utóbb chatty/teljesítmény bizalmas. Amikor azok áthelyezi őket külön szolgáltatások teljes alkalmazás teljesítményének tanked vagy késés nő. Emiatt a teljes alkalmazás nem megfelel-e elvárásainak.

Ebben az esetben azt szeretné, hogy a újrabontási munka elveszíti, és nem szeretné, térjen vissza az monolit, de szükség bizonyos településen értelmezhető. Ez csak akkor lehet átalakítani a természetes módon szolgáltatások működnek-összetevőket, vagy ha lehetséges orvosolható azt a teljesítmény várakozásoknak néhány más módszer, amíg áll fenn.

Mi a teendő? Jól sikerült próbálja, affinitás bekapcsolása.

## <a name="how-to-configure-affinity"></a>Affinitás konfigurálása
Affinitás beállításához határozhatja meg egy affinitás kapcsolatot, két különböző szolgáltatásai között. Érdemes elképzelnie affinitás "mutató" egy szolgáltatás egy másik és kapja meg: "Ez a szolgáltatás csak futtatható hol, hogy fut-e szolgáltatás." Előfordul, hogy hivatkozunk affinitás, a szülő – gyermek kapcsolat (ahol, mutasson a gyermek a szülő). Affinitás biztosítja, hogy a kópiák vagy szolgáltatás egy példányát a kópiák vagy egy másik példányát azonos csomóponton kerülnek.

``` csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

## <a name="different-affinity-options"></a>Különböző affinitás beállításai
Affinitás jeleníti meg több korrelációs rendszerek keresztül, és az két különböző módokban. A leggyakoribb mód affinitás nevezzük NonAlignedAffinity. NonAlignedAffinity másolaton, illetve a különböző szolgáltatások példányok ugyanazt a csomóponton helyezni. A többi módja AlignedAffinity. Igazított affinitás akkor hasznos, csak az állapot-nyilvántartó szolgáltatásaival. Van igazítva affinitás két állapot-nyilvántartó szolgáltatások konfigurálása biztosítja, hogy a szolgáltatások eredményezi egymással azonos csomóponton kerülnek. Azt is eredményezi, hogy minden pár formátumú másodlagos zónák a szolgáltatások ugyanazt a csomóponton helyét. Lehetőség arra is (bár kevésbé gyakori) NonAlignedAffinity konfigurálása az állapot-nyilvántartó szolgáltatások. Az NonAlignedAffinity a két állapot-nyilvántartó szolgáltatás különböző replikálását volna collocated ugyanazt a csomóponton, de nem szeretné kell kísérlet az elsődleges és formátumú másodlagos zónák igazítása.

![Affinitás módok és hatásaik][Image1]

### <a name="best-effort-desired-state"></a>Legjobb szükséges munkamennyiség állam
Néhány, kapcsolatok és egységes architektúrákban közötti különbségek vannak. Azok közül számos csak a mivel egy affinitás kapcsolatban a legjobb munkamennyiség. A szolgáltatások affinitás kapcsolatok történhet, és önállóan helyezhető alapvetően más szervezetek. Is oka az, hogy miért egy affinitás kapcsolat megszüntetése sikerült. Ha például kapacitás korlátozások, ahol a szolgáltatás csak néhány objektumok a kapcsolatban részt vevő affinitás ráfér egy adott csomópont. Ezekben az esetekben akkor is, ha egy affinitás kapcsolat áll fenn helyen, akkor nem érvényesíthetők miatt az érvényességi tartományán. Ha lehetséges, várjon egy kis időt, a affinitás korlátozás megsértése automatikusan kijavítja minden más kényszerek és affinitás érvényesítéséhez.  

### <a name="chains-vs-stars"></a>Lánccal csillagok összehasonlítása
Ma még ha nem tudja affinitás kapcsolatok a modellben lánccal. Ez azt jelenti, hogy a szolgáltatás, amely egy affinitás kapcsolatot a stílust, nem lehet a szülő a másik affinitás kapcsolat. Ha szeretné az ilyen típusú kapcsolat modell, hatékony modell azt egy lánc helyett egy csillag van. Ehhez a legalsó gyermek volna kell szülőjének a "középső" gyermek szülő helyette. Attól függően, hogy az elképzeléseinek megfelelően helyezkednek el a szolgáltatások ez szükség lehet a szülő-gyermek több szolgáló "helyőrző" szolgáltatás létrehozása.

![A kapcsolatok affinitás környezetében lánccal összehasonlítása csillagok][Image2]

Ma jegyezze fel a kapcsolatok affinitás másik megoldás,, hogy azok irányt jelző elemeket. Ez azt jelenti, hogy a "affinitás" szabály csak kényszeríti, hogy a gyermek-e a szülő esetén. Ha például a szülő váratlanul átvétele egy másik csomópontra majd fürt az erőforrás-kezelő nem ténylegesen érdemes tartalmaz valamit, amit nem megfelelő mindaddig, amíg észleli, hogy a gyermek nem jelenik meg a szülő értelmezése; helyen a kapcsolat nem azonnal érvénybe.

### <a name="partitioning-support"></a>Támogatás szétválasztás
A végleges dolog affinitás kapcsolatos figyelmeztetés, hogy kapcsolatok nem támogatottak, ahol a szülő particionált affinitás. Ez az, amit ahányat előfordulhat, hogy támogatjuk, de jelenleg nem engedélyezett.

## <a name="next-steps"></a>Következő lépések
- Az elérhető lehetőségekről további információt a témakör nézze át az erőforrás-kezelő fürt konfigurációk Services elérhető [szolgáltatások beállításával kapcsolatos további tudnivalók](service-fabric-cluster-resource-manager-configure-services.md)
- Számos oka lehet annak, ha használják, például egy kis szolgáltatások korlátozása affinitás gépek megadása, és próbálja a betöltés szolgáltatást, gyűjtemény összesítése jobban támogatott alkalmazás csoportokon keresztül. Nézze meg az [Alkalmazás-csoportok](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png
