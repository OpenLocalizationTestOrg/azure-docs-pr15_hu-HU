<properties
    pageTitle="Megbízható gyűjtemények használata |} Microsoft Azure"
    description="Ismerje meg, hogy a gyakorlati tanácsok a megbízható gyűjtemények használata."
    services="service-fabric"
    documentationCenter=".net"
    authors="JeffreyRichter"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="03/28/2016"
    ms.author="jeffreyr" />

# <a name="working-with-reliable-collections"></a>Megbízható gyűjtemények használata

Szolgáltatás háló felajánlja a rendelkezésre álló állapot-nyilvántartó programozási modell .NET fejlesztők megbízható gyűjtemények keresztül. Konkrétan a szolgáltatás háló megbízható szótár és megbízható várólista osztályok biztosít. Ha használja ezek az osztályok, a állapotát (a méretezhetőség) particionálva, replikált (az elérhetőség), és belül egy partíciót (savas szemantikáját). Lássunk egy tipikus használatát egy megbízható szótár objektum tekintse meg, és látható, hogy milyen a ténylegesen módon.

~~~
retry:
try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to  
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) { 
   await Task.Delay(100, cancellationToken); goto retry; 
}
~~~

A megbízható szótár objektumok (kivéve a ClearAsync amely nem vonható vissza), az összes művelet ITransaction objektum szükség. Az objektum van társítva bármely és minden módosítás, hogy minden megbízható szótárba és/vagy megbízható próbál várólista egyetlen partíciót belüli objektumok. Egy ITransaction finomítása hívja fel a partíciót objektum StateManager's CreateTransaction módszer van.
 
A fenti kód a ITransaction objektum egy megbízható szótár AddAsync módszer átadott. Belső szótár módszerek, amely egy kulcsot fogad el, hogy kulcshoz tartozó olvasó/író lakat. A módszer módosítja a kulcs érték, ha a módszer a kulcs írási lakat veszi, és ha a módszer csak beolvassa a kulcs értékét, majd olvasási zárolás kérdezi kulcs. Mivel AddAsync módosítja a kulcs értékét az új, átadott érték, a kulcs írási zárolása venni. Igen 2 (vagy több) szálak megpróbálnak értékek összeadása ugyanazzal a kulccsal egyszerre, több szálon fog szerezni írási zárolás, és blokkolja a többi beszélgetésekben. Alapértelmezés szerint a zárolás; 4 másodpercet módszerek tiltása 4 másodperc után a módszerek throw egy TimeoutException. Módszer túlterhelések létezik, amely lehetővé teszi az explicit időkorlát átadni, ha inkább.
 
Általában írhat a kód egy TimeoutException reagálni kifogására azt, és a teljes művelet újra próbálkozik, (ahogy a fenti kódot). Az egyszerű kód Task.Delay áthaladó 100 ezredmásodperc minden alkalommal, amikor csak hívandó. De a valóságban, akkor célszerűbb lehet kikapcsolása az exponenciális vissza kikapcsolása késleltetés bizonyos típusú használja.
 
Miután a zárolás szerezte be van, a AddAsync hozzáadása egy belső ideiglenes szótárba a ITransaction objektummal társított kulcs és érték objektumhivatkozások. Ez történik, hogy biztosítson olvasási-a-Öné-írások szemantikáját. Ez azt jelenti, hogy hívja AddAsync, miután TryGetValueAsync (azonos ITransaction objektumot használva) egy újabb hívást ad vissza az érték, ha még nem véglegesített a tranzakció. Ezután AddAsync serializes a termékkulcsot, és érték bájt tömbök objektumokat, és ezek bájt a tömbök hozzáfűzi a helyi csomóponton naplófájlt. Végezetül AddAsync küld a bájt tömbök a másodlagos replikák, hogy kulcs/érték is ugyanezeket az adatokat. Annak ellenére, hogy a kulcs/érték információkat íródott egy naplófájlban, az adatok nem számít a szótár részét mindaddig, amíg a tranzakció, amely vannak társítva követtek. 

A fenti kód CommitAsync hívás véglegesíti a tranzakció műveleteket. Kifejezetten jóváhagyás információk hozzáfűzi a naplófájl a helyi csomópontra, és a jóváhagyás rekordot is küld a másodlagos replikák. A replikák határozatképességének (többség) válaszolt, miután összes adatmódosítás számít állandó, és bármely billentyűket, amelyek a ITransaction objektum keresztül voltak választéka társított zárolások kiadott, más szálak tranzakciók is kezelheti a megegyező billentyűk és értékeik.

Ha CommitAsync nem neve (általában miatt kivételt alatt), a ITransaction objektum kap távolítva. Nem véglegesített ITransaction objektum értékesítésekor szolgáltatás háló hozzáfűzi a megszakítás információkat a helyi csomópont naplófájl, és semmi sem kell lehet küldeni a másodlagos replikák bármelyike. És ezt követően bármilyen billentyűket, amelyek keresztül a tranzakció volt választéka társított zárolások kiadott.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Gyakori csapdák és őket kiküszöbölése
Most, hogy megismeri a megbízható gyűjtemények működése belső, nézzük meg néhány gyakori előreláthatók őket. Olvassa el az alábbi kódot:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes, 
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
~~~

Normál .NET szótárhoz használatakor kulcs/érték felvétele a szótárba, és módosítsa úgy (például LastLogin) a tulajdonság értékét. Jó helyen jár a kód nem működnek megfelelően megbízható szótárhoz. Ne feledje, hogy a korábbi vitafórum a AddAsync a hívást a kulcs/érték objektumok bájt tömbök serializes majd helyi fájlba menti a tömbök és is a másodlagos replikák küldjön nekik. Ha később egy tulajdonság, ez módosítja a tulajdonság értékét a memóriában csak; a helyi fájl vagy az adatok elküldve a nincs hatással. Ha a folyamat összeomlik, mi az a memóriában elő nem vagyok a gépnél. Új folyamat indításakor, vagy ha replikává elsődleges, a régi tulajdonság értéke nem, mi érhető el. 

E hangsúlyozza, nem elég megtudhatja, milyen egyszerű, hogy milyen fent látható hibát. És csak megtanulhatja a hiba kapcsolatos Ha a folyamat megszakad. Írja be a kódot a megfelelő úgy egyszerűen visszavonása a két vonalat:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync(); 
}
~~~

Az alábbiakban a gyakori hibát egy másik példa:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync(); 
   }
}
~~~

Ismét a normál .NET szótárak, a fenti kód jól működik, és közös mintát: a fejlesztői kulccsal egy érték. Ha az érték szerepel-e, a a Fejlesztőeszközök módosítja a tulajdonság értékét. A megbízható gyűjtemények, hogy ezt a kódot azonban mutat azonos, már tárgyalt problémát: __nem módosítania kell objektum után egy megbízható webhelycsoport adandó.__
 
Egy értéket az egy megbízható webhelycsoport frissítése a megfelelő úgy, hogy a hivatkozás a meglévő értékre, és vegye figyelembe az objektum megváltoztatható ez a hivatkozás által megjelölt. Ezután hozzon létre egy új objektum, amely az eredeti objektum pontos másolatát. Most módosítsa az új objektum állapotát, és írja be a gyűjteménybe az új objektum, hogy akkor kap szerializálásának bájt tömbök, hogy a helyi fájlt csatolni, és elküldve a. Után a módosítás(ok) elvégzése, a memóriában objektumok, a helyi fájl és az összes kópiák van azonos pontos állapotát. Célszerű az összes!

Az alábbi kód egy értéket az egy megbízható webhelycsoport frissítése megfelelő módon jeleníti meg:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire 
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync(); 
   }
}
~~~

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Programmer hibák elkerülése megváltoztatható adattípusok meghatározása

Ideális esetben azt szeretné, a fordító jelentés hibák Ha véletlenül konzerv kódot, amely egy objektumot, amelyeknek szempontok megváltoztatható állapotának mutates. De a C# fordító nem rendelkezik az azt jelenti, hogy a művelet. Igen, potenciális programmer hibák elkerülése érdekében azt javasoljuk, hogy a fájltípusok meghatározása kell megváltoztatható típusokra megbízható gyűjtemények használata. Kifejezetten Ez azt jelenti, hogy Ön szigorúan core értéktípusok (például számok [Int32, UInt64 stb.], DateTime, globálisan egyedi azonosítója, időszak vagy hasonló). És természetesen is használhatja karakterlánc. Webhelycsoport-tulajdonságai, szerializálása elkerülése érdekében célszerű és deszerializálása azokat is gyakran is sérti a teljesítmény. Jó helyen jár Ha a webhelycsoport-tulajdonságai használni kívánt, azt javasoljuk e. A nettó megváltoztatható gyűjtemények tár (System.Collections.Immutable). A tár http://nuget.org letölthető érhető el. Azt is órájához lezárása, és a mezők írásvédetté tétele minden lehetséges esetben javasoljuk.

Az alábbi UserInfo típusa bemutatja, hogyan lehet a fenti javaslatok nyújtotta előnyök kihasználása megváltoztatható típusának megadásához.

~~~
[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;
 
   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }
 
   [DataMember]
   public readonly String Email;
 
   // Ideally, this would be a readonly field but it can't be because OnDeserialized 
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
~~~

A ItemId típusa is megváltoztatható típus nem itt ismertetett módon:

~~~
[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
~~~

## <a name="schema-versioning-upgrades"></a>Séma verziószámozás (frissítés)

Belső megbízható gyűjtemények szerializálni az objektumok használata. A nettó DataContractSerializer. A szerializált objektumok csoportosítások megmaradnak az elsődleges replika helyi lemezre, és a másodlagos replikák is küldött. A szolgáltatás lejáratkori, mint valószínű, bizonyára tudni szeretné, hogy milyen típusú adatokat (séma) a szolgáltatáshoz módosítása. Az adatok remek körültekintéssel verziószámozás kell megközelíthető. Mindenekelőtt és elsősorban mindig kell tudni a régi adatokat. Kifejezetten, ez azt jelenti az deszerializálás kódot kell végtelen visszamenőleges kompatibilitás működik: verzió 333, a szolgáltatáskód adatot egy megbízható gyűjtemény a szolgáltatáskód 1-es verziójú 5 évvel ezelőtti megjelenésével vonatkozni fognak tudni kell lennie.

Ezenkívül a szolgáltatáskód az frissített egy frissítési tartomány egyszerre. Igen frissítéskor, ha a szolgáltatáskód futó egyidejűleg két különböző verziói. Az új verzióját a szolgáltatáskód használja a szolgáltatáskód régi verziói előfordulhat, hogy nem tud az új séma kezelése az új séma problémákat kell elkerülése érdekében. Ha lehetséges, tervezheti meg a szolgáltatás előre kompatibilisek 1 verziója minden verzió. Kifejezetten Ez azt jelenti, hogy a szolgáltatás kód V1 látnia kell egyszerűen csak figyelmen kívül minden sémaelemek explicit módon nem kezeli. Legyen jó helyen jár, minden adatot, nem kifejezetten tudnivalók és a egyszerűen ismét meg egy szótár kulcs vagy érték frissítésekor írási mentheti. 

>[AZURE.WARNING] Miközben a séma kulcs módosításához győződjön meg arról, hogy a kulcs kivonat kódot és algoritmusok egyenlő stabil is. Módosíthatja, hogy hogyan működnek ezek algoritmusok közül választhat, ha nem tudja újra minden eddiginél keresheti meg a megbízható szótár belül billentyűt.
  
Másik lehetőségként végezheti el mit általában nevezik a 2-fázis frissítése. A 2-fázis verziófrissítéssel frissíti a szolgáltatás V1 V2: V2 kódot tartalmazza, hogy tudja, hogy hogyan szeretné kezelni az új séma módosítása, de ez a kód nem hajtja végre. A V2 kódot V1 adatokat olvas be, amikor rajta működik, és V1 adatot ír. Ezután a frissítés befejezése után minden frissítési tartományok, akkor is valami jel V2 példánya fut az, hogy helyesek-e a frissítés. (Jel egyik módja Ez egy konfigurációs frissítés bevezetésére, miből ezt a 2-fázis frissítése: az.) Most a V2 példányok is V1 adatokat olvasni, átalakíthatja V2 adatok, működik rajta és V2 adatként írni. Más példányok V2 adatok olvasása, nem szükséges konvertálja a fájlt, csak működésük rá, és írja meg V2 adatok.

## <a name="next-steps"></a>Következő lépések
Előre kompatibilis adatok szerződéseket létrehozásával kapcsolatos további tudnivalókért lásd: a [Kompatibilitás működik adatok szerződéseket](https://msdn.microsoft.com/library/ms731083.aspx).

Ajánlott eljárások a verziószámozás adatok szerződéseket című témakörben [Adatok szerződés verziószámozás](https://msdn.microsoft.com/library/ms731138.aspx). 

Verzió alternatív adatok szerződéseket megvalósításáról című témakörben talál [Verzió tervezett szerializálási visszahívást](https://msdn.microsoft.com/library/ms733734.aspx). 

Megtudhatja, hogy miként adatstruktúra, amely több verziója között együttműködhetnek megadására, olvassa el a [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx)című témakört.
