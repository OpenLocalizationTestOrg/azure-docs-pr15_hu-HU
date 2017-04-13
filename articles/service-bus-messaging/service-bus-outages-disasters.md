<properties 
    pageTitle="Szolgáltatás Bus alkalmazások kimaradások és katasztrófák elleni szigetelő |} Microsoft Azure"
    description="Is használhatja az alkalmazások egy potenciális szolgáltatás Bus üzemszünetek elleni védelmét technikákat ismerteti."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/02/2016"
    ms.author="sethm" />

# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Gyakorlati tanácsok a szigetelő alkalmazások szolgáltatás Bus kimaradások és katasztrófák ellen

Kritikus fontosságú alkalmazások csak akkor működnek folyamatosan, akár a véletlen vagy katasztrófák jelenlétében. Ez a témakör ismerteti a technikákat szolgáltatás Bus alkalmazások egy potenciális szolgáltatás üzemszünetek vagy katasztrófa elleni védelmét is használhatja.

Egy üzemszünetek definíciója az Azure Service Bus ideiglenes elérhetetlenné. A üzemszünetek befolyásolhatják szolgáltatás Bus, például egy üzenetben tár, vagy akár a teljes adatközponthoz bizonyos összetevői. A ki lett javítva, miután szolgáltatás Bus ismét elérhetővé válik. Egy üzemszünetek általában nem hiúsul üzeneteket és más adatok elvesztése. Példa összetevő hiba egy adott üzenetben tároló elérhetetlenné. Példa egy adatközponthoz szintű üzemszünetek a adatközponthoz vagy hibás adatközponthoz hálózati kapcsoló áramszünet. Néhány nap egy üzemszünetek néhány percig is tarthat.

Katasztrófa definíciója az adatvesztést adatközponthoz vagy szolgáltatás Bus időosztás egységet. Előfordulhat, hogy az adatközpont, vagy előfordulhat, hogy nem áll rendelkezésre újra. A szokásos katasztrófa hatására néhány vagy összes üzeneteket és más adatok elvesztése. Katasztrófák példák fire, amelyet vagy földrengés.

## <a name="current-architecture"></a>Aktuális architektúra

Szolgáltatás Bus több üzenetben tárolja használja a sorok vagy témakörök küldött üzenetek tárolásához. Egy másik várólista vagy témakör van hozzárendelve egy üzenetben áruházból. Az üzenetben üzlet nem érhető el, ha a várólista vagy a témakör az összes művelet sikertelen lesz.

Az összes szolgáltatás Bus üzenetben személyek (sorban várakozó, témák, jelfogók) tárolnak szolgáltatás névteret, amely egy adatközponthoz viszonyban van. Szolgáltatás Bus nem teszi lehetővé az adatok automatikus geo-replikáció, és nem teszi lehetővé a névtér több adatközpontokkal időtartamát.

## <a name="protecting-against-acs-outages"></a>ACS kimaradások elleni védelem

Ha ACS hitelesítő adatok használja, és ACS elérhetetlenné válik, az ügyfelek nem kaphatnak tokenek. Ügyfelek, amelyek egy token ACS megszakad időben továbbra is használhatja a szolgáltatás Bus mindaddig, amíg a tokenek lejár. Az alapértelmezett jogkivonat élettartama 3 óra.

ACS kimaradások vírusvédelmet, használja a megosztott Access-aláírás (Társítások) tokenek. Ebben az esetben az ügyfél hitelesíti közvetlenül szolgáltatás Bus bejelentkezés önálló minted jogkivonat titkos kulccsal. Hívások átirányítása ACS már nem szükséges. További információt a Társítások tokenek lásd: a [hitelesítési szolgáltatás Bus][].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Sorok és a témakörök áruház hibáinak üzenetküldés ellen

Egy másik várólista vagy témakör van hozzárendelve egy üzenetben áruházból. Az üzenetben üzlet nem érhető el, ha a várólista vagy a témakör az összes művelet sikertelen lesz. Egy particionált várólista azonban, több töredékek áll. Minden részlet egy másik üzenetben áruházra vannak tárolva. Üzenet elküldésekor particionált várólista vagy témakör szolgáltatás Bus rendel az üzenetet a töredékek közül. A megfelelő üzenetben tároló nem érhető el, ha szolgáltatás Bus írja az üzenet egy másik részlet, ha lehetséges. Particionált szervezetek kapcsolatos további tudnivalókért olvassa el a [üzenetben szervezetek particionálva][]című témakört.

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Adatközponthoz kimaradások vagy katasztrófák elleni védelem

Között két adatközpontokkal feladatátvevő engedélyezéséhez létrehozhat szolgáltatás Bus szolgáltatás névtér az egyes adatközponthoz. A szolgáltatás Bus névtér **contosoPrimary.servicebus.windows.net** az Amerikai Egyesült Államok északi/központi területen található, és a **contosoSecondary.servicebus.windows.net** az US Dél/központi területen található. Ha egy személy üzenetküldési szolgáltatás Bus elérhető egy adatközponthoz üzemszünetek jelenlétében kell maradnia, mindkét névtér is létrehozhat, hogy a szervezet.

További információ című "Szolgáltatás Bus belül az Azure adatközponthoz hiba" [aszinkron üzenetben mintázatok][]és magas elérhetőségét.

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Továbbítási végpontok adatközponthoz kimaradások vagy katasztrófák elleni védelem

Továbbítás végpontok GEO replikációs lehetővé teszi, hogy a szolgáltatás, amely elérhetővé teszi a továbbítási végpontot jelenlétében szolgáltatás Bus kimaradások érhető el. A replikáció geo eléréséhez a szolgáltatás létre kell hoznia végpontokat továbbítási a különböző névtér. A névtér különböző adatközpont esetén kell lennie, és a végpontokat kell rendelkeznie a különböző neveket. Például egy elsődleges végpontot érhető el a **contosoPrimary.servicebus.windows.net/myPrimaryService**, miközben a másodlagos megfelelőjük érhető el a **contosoSecondary.servicebus.windows.net/mySecondaryService**.

A két végponton követően figyeli a szolgáltatás, és egy ügyfél hívhatja keresztül vagy végpontja a szolgáltatást. Ügyfélalkalmazás véletlenszerűen választja ki egyet a jelfogók elsődleges végpontjának, és a kérést küld az aktív végpontot. Ha nem sikerül a művelet egy kódszámú hiba jelenik meg, ezt a hibát azt jelzi, hogy a továbbítási végpont nem érhető el. Az alkalmazás megnyílik a biztonsági másolat végpont csatorna és reissues a kérést. Ezen a ponton az aktív, és a biztonsági másolat végpontok váltás szerepkörök: az ügyfélalkalmazás számításba veszi-e a régi aktív végpontot, az új biztonsági végpontot, és a régi biztonsági végpontot, az új aktív végpontot kell lennie. Mindkét Küldés művelet sikertelen, ha a szerepkörök, a két szervezetek változatlan marad, és a függvény hibát ad vissza.

A [szolgáltatás Bus továbbítását üzeneteinek Geo-replikáció][] minta bemutatja, hogyan való replikáció jelfogók.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Sorok és a témakörök adatközponthoz kimaradások vagy katasztrófák ellen

Szolgáltatás Bus elérése címtárfrissítések adatközponthoz kimaradások ellen, amikor brokered használ, üzenetben, támogatja a két megközelítés: *aktív* és *passzív* replikáció. Egyes megközelítésre Ha egy adott várólista vagy a témakör a adatközponthoz üzemszünetek jelenlétében elérhető maradhat akkor hozhatja létre a mindkét névtér. Mindkét személyek is lehet ugyanaz a neve. Például egy elsődleges várólista érhető el a **contosoPrimary.servicebus.windows.net/myQueue**, közben a másodlagos megfelelőjük a **contosoSecondary.servicebus.windows.net/myQueue**érhető el.

Ha az alkalmazás nem igényel állandó feladó a címzett való kommunikáció, az alkalmazás segítségével miként állíthat tartós ügyféloldali várólista üzenet az adatvesztés elkerülése érdekében, és az ideiglenes (tranziens) szolgáltatás Bus hibákat a feladó szolgálja.

## <a name="active-replication"></a>Aktív replikációs

Aktív replikációs szervezetek mindkét névtér az összes művelet használja. Bármilyen ügyfél által küldött üzenet két példánya ugyanazt az üzenetet küldi. Az első példányt küldi el az elsődleges személyhez (például a **contosoPrimary.servicebus.windows.net/sales**), és a második másolása az üzenet érkezik, a másodlagos személyhez (például a **contosoSecondary.servicebus.windows.net/sales**).

Egy ügyfél kap mindkét sorban várakozó üzeneteket. A címzett dolgozza fel egy üzenetet az első példányt, majd a második példányt le van tiltva. Ismétlődő üzenetek elrejtése, hogy a feladó kell nyomon követése egy egyedi azonosítót minden üzenet. Mindkét az üzenetből másolatot az azonos azonosítójú címkézve. A [BrokeredMessage.MessageId][] vagy [BrokeredMessage.Label][] tulajdonságait, vagy egy egyéni tulajdonság segítségével az üzenet nyomon követése. A címzett kell karbantartása: az üzenetek találhatók, amelyek már megkapta.

Az [üzenetekkel szolgáltatás Bus Brokered Geo replikációs][] szervezetek üzenetküldés az aktív replikációs mutatja be.

> [AZURE.NOTE] Az aktív replikációs megközelítés megduplázódnak műveletek, ezért újabb költség ezt a megközelítést vezethet.

## <a name="passive-replication"></a>Passive replikációs

Hibafa-mentes esetben passzív replikációs csak egyikét használja a két üzenetben entitás. Egy ügyfél az üzenetet küld, az aktív személyhez. Az aktív entitás a műveletet nem sikerül egy hibakóddal, amely jelzi, hogy az az aktív entitás üzemeltető adatközponthoz előfordulhat, hogy nem érhető el, ha az ügyfél küld az üzenet egy példányát a biztonsági másolat entitás. Ezen a ponton az aktív, és a biztonsági másolat szervezetek váltás szerepkörök: a küldő ügyfél a régi aktív entitás kell az új biztonsági entitás tekinti, és a régi biztonsági entitás az új aktív entitás. Mindkét Küldés művelet sikertelen, ha a szerepkörök, a két szervezetek változatlan marad, és a függvény hibát ad vissza.

Egy ügyfél kap mindkét sorban várakozó üzeneteket. Mivel a névjegyadatok, hogy a a címzett két példánya ugyanezt a hibaüzenetet kapja, a címzett az ismétlődő üzenetek elrejtése kell. Ismétlődő elemeket is mellőzése ugyanúgy aktív replikációs című témakörben leírt módon.

Az általános passzív replikációs oka gazdaságosabb, mint az aktív replikációs a legtöbb esetben csak egy műveletet végzi el. A nem replikált forgatókönyv késés, átviteli és a költség megegyeznek.

Passive replikációs használata esetén az alábbi esetekben üzenetek lehet elveszett vagy kétszer kapott:

-   **Üzenet késedelem vagy veszteség**: Tegyük fel, hogy a feladó sikerült elküldeni az elsődleges várólista egy üzenetet az m1, és kattintson a sor történjen a címzett kap m1 előtt. A feladónak a következő üzenetet m2 küld a másodlagos várakozási sorban található. Ha az elsődleges várólista átmenetileg nem érhető el, a címzett megkapja m1, után a várólista újra lesz elérhető. Abban az esetben, ha katasztrófa a címzett soha nem kap m1.

-   **Vételi ismétlődő**: Tegyük fel, hogy a feladó üzenetet küld m a elsődleges várólista. Szolgáltatás Bus sikeresen m folyamatok, de nem tud választ küldeni. Miután a Küldés művelet időtúllépés történik, a feladó m azonos példányának küld a másodlagos várakozási sorban található. Ha a címzett az első példányt m érkeznek, az elsődleges várólista válik nem érhető el, a címzett megkapja mindkét m példányainak körülbelül egy időben. Ha a címzett nem érkeznek m az első példányt, az elsődleges várólista válik nem érhető el, a címzett kezdetben a csak az m második másolatot kap, de majd m második másolatot kap, ha az elsődleges várólista elérhetővé válik.

Az [üzenetekkel szolgáltatás Bus Brokered Geo replikációs][] a szervezetek üzenetküldés passzív replikációs mutatja be.

## <a name="next-steps"></a>Következő lépések

További tudnivalók a vészhelyreállítás, az alábbi cikkekben talál:

- [Azure SQL-adatbázis üzemkészségének][]
- [Azure tűrőképessége technikai útmutató][]

  [Szolgáltatás Bus hitelesítési]: service-bus-authentication-and-authorization.md
  [Particionált üzenetben személyek]: service-bus-partitioning.md
  [Aszinkron üzenetben mintázatok és elérhetőséget]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
  [A szolgáltatás Bus GEO replikációs továbbítását üzenetek]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [BrokeredMessage.Label]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx
  [A szolgáltatás Bus GEO replikációs Brokered üzenetek]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
  [Azure SQL-adatbázis üzemkészségének]: ../sql-database/sql-database-business-continuity.md
  [Azure tűrőképessége technikai útmutató]: ../resiliency/resiliency-technical-guidance.md
