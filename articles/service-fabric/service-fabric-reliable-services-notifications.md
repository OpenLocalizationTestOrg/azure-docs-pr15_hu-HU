<properties
   pageTitle="Megbízható szolgáltatások értesítések |} Microsoft Azure"
   description="Magyarázó rész dokumentáció szolgáltatást háló megbízható az értesítésekhez"
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="reliable-services-notifications"></a>Megbízható szolgáltatások értesítések

Értesítések, hogy engedélyezi-objektumra, amely érdekli a végrehajtott a változások követése ügyfeleknek. Kétféle típusú objektumok értesítések támogatása: *Megbízható állam Manager* és a *Megbízható szótár*.

Értesítések használatával gyakori okai a következők:

- Épület nézeteket, például a másodlagos indexek eseményre vagy összesíti a replika állam szűrt nézetben. Példa az összes kulcs megbízható szótárban rendezett index létrehozása.
- Küldő nyomon adatokat, például az utolsó órában hozzáadott felhasználók számát.

Értesítések műveletek alkalmazása részeként van indítva. Amely miatt értesítések gyors lehetséges és szinkron események bármely drága műveletekkel kerülni a Webhelyfiókok együtt kell kezelni.

## <a name="reliable-state-manager-notifications"></a>Megbízható állam Manager értesítések

Megbízható állam Manager értesítésekkel a következő eseményekre vonatkozóan:

- Transaction
    - Jóváhagyás
- Állapot manager
    - Újbóli létrehozása
    - Összeadás egy megbízható állam
    - Egy megbízható állapot megszüntetése

Megbízható állam Manager követi nyomon az aktuális inflight tranzakciók. Csak módosítása való értesítés okozó tranzakció állapotban alatt álló vállalt tranzakció.

Megbízható állam Manager tárolja megbízható államok, például a megbízható szótár és megbízható várólista gyűjteménye. Megbízható állam Manager értesítések alkalommal elindul, amikor ebben a gyűjteményben változik: egy megbízható állam van, illetve el, vagy a teljes gyűjteményt az újonnan létrehozott.
A megbízható állam Manager gyűjtemény van az újonnan létrehozott három esetben:

- Helyreállítási: Kópia indításakor azt állít helyre az eredeti állapotába a lemezről. A helyreállítási végén használ **NotifyStateManagerChangedEventArgs** egy eseményt, amely tartalmazza a helyreállított megbízható államok megadása.
- Teljes másolás: kópia is csatlakozni tudnak a konfigurációs készletet, mielőtt létrehozása rendelkezik. Időnként előfordulhat ehhez teljes példányával megbízható állam Manager állam eltávolította a elsődleges az üresjárati másodlagos replika való alkalmazásra. A másodlagos replikakészlettagon megbízható állam Manager **NotifyStateManagerChangedEventArgs** használja egy eseményt, amely tartalmazza azt eltávolította a elsődleges szerezte be megbízható állapotok csoportja.
- Visszaállítás: Katasztrófa helyreállítási esetekben a replika visszaállítható egy biztonsági másolatból **RestoreAsync**keresztül. Ezekben az esetekben az elsődleges replikakészlettagon megbízható állam Manager segítségével **NotifyStateManagerChangedEventArgs** egy eseményt, amely azt visszaállítása biztonsági másolatból megbízható állapotok készlete tartalmazza.

Regisztrációhoz az tranzakció értesítések, illetve állam manager értesítések regisztrálnia kell a megbízható állam Manager **TransactionChanged** vagy **StateManagerChanged** eseményekhez. A regisztrációhoz az alábbi eseménykezelők közös hely az állapot-nyilvántartó szolgáltatás konstruktorának. Amikor regisztrál az konstruktort, nem kihagy **IReliableStateManager**élettartama során megváltoztatásával okozó értesítést.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

Az **TransactionChanged** eseménykezelő **NotifyTransactionChangedEventArgs** segítségével adja meg az esemény részleteit. Annak megadása, módosítása típusú művelet tulajdonság (például a **NotifyTransactionChangedAction.Commit**) tartalmaz. A tranzakciók tulajdonság, ahol a módosított tranzakció hivatkozás is tartalmaz.

>[AZURE.NOTE] Ma **TransactionChanged** esemény csak akkor, ha a tranzakció vált. A művelet **NotifyTransactionChangedAction.Commit**majd megegyezik. De a jövőben események előfordulhat, hogy léptethető más típusú tranzakció állapotának megváltozásakor. Javasoljuk, hogy a művelet ellenőrzése és az esemény feldolgozása, csak akkor, ha egy várt.

Az alábbiakban példa **TransactionChanged** eseménykezelő.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

Az **StateManagerChanged** eseménykezelő **NotifyStateManagerChangedEventArgs** segítségével adja meg az esemény részleteit.
**NotifyStateManagerChangedEventArgs** van két alosztályok: **NotifyStateManagerRebuildEventArgs** és **NotifyStateManagerSingleEntityChangedEventArgs**.
Segítségével a művelet tulajdonság **NotifyStateManagerChangedEventArgs** a megfelelő alosztályra **NotifyStateManagerChangedEventArgs** típusra:

- **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
- **NotifyStateManagerChangedAction.Add** és **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Az alábbiakban egy példa **StateManagerChanged** értesítés kezelő.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Megbízható szótár értesítések

Megbízható szótár értesítésekkel a következő eseményekre vonatkozóan:

- Újraépítő: Ha **ReliableDictionary** helyreállt állapotába egy helyreállított vagy másolt helyi állapot vagy a biztonsági másolat neve.
- Törlés: Végrehajtásra, ha **ReliableDictionary** állapotának ki lett ürítve a **ClearAsync** módszerrel.
- Adja hozzá: Nevű elem hozzáadva **ReliableDictionary**.
- A frissítés: **IReliableDictionary** elem frissítésekor neve.
- Eltávolítás: A **IReliableDictionary** elemének törlésekor neve.

Megbízható szótár értesítést kap, meg kell a **IReliableDictionary** **DictionaryChanged** eseménykezelő regisztrálása. A regisztrációhoz az alábbi eseménykezelők közös hely szerepel-e a **ReliableStateManager.StateManagerChanged** értesítés hozzáadása.
Regisztrálás **IReliableDictionary** **IReliableStateManager** való hozzáadásakor biztosítja, hogy nem kihagy értesítések.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

>[AZURE.NOTE] **ProcessStateManagerSingleEntityNotification** módszerrel a minta, amely az előző példában **OnStateManagerChangedHandler** hív meg.

Az előző kód állítja be a **IReliableNotificationAsyncCallback** felület **DictionaryChanged**együtt. Mivel **NotifyDictionaryRebuildEventArgs** tartalmaz **IAsyncEnumerable** felületet – mely igények aszinkron – kell felsorolásos újraépítéséhez értesítések **RebuildNotificationAsyncCallback** helyett **OnDictionaryChangedHandler**keresztül van indítva.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

>[AZURE.NOTE] Előző kódban az Újraépítés értesítés feldolgozása részeként először karbantartott összesített állapota nincs bejelölve. A megbízható gyűjtemény újraépíti az új állapot, mert az előző értesítések rá.

Az **DictionaryChanged** eseménykezelő **NotifyDictionaryChangedEventArgs** segítségével adja meg az esemény részleteit.
**NotifyDictionaryChangedEventArgs** öt alosztályok tartalmaz. **NotifyDictionaryChangedEventArgs** művelet tulajdonsággal a megfelelő alosztályra **NotifyDictionaryChangedEventArgs** típusra:

- **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
- **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
- **NotifyDictionaryChangedAction.Add** és **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
- **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
- **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Javaslatok

- Értesítési események ugyanolyan gyorsan befejezéséhez *végezze el* .
- Bármely drága műveletek (például a bemeneti és kimeneti műveletet) egyidejű események részeként *nem* hajtható végre.
- *Akkor* kell bejelölni a művelettípus előtt, az események feldolgozása. Új tevékenységtípusok előfordulhat, hogy a jövőben hozzá.

Íme néhány dolog, amit érdemes észben tartania:

- Értesítések művelet végrehajtása részeként van indítva. Ha például visszaállítása értesítést nézetnek visszaállítási művelet utolsó lépéseként. A visszaállítás mindaddig, amíg az értesítési esemény feldolgozása nem fejeződik be.
- Értesítések alkalmazása műveletek részeként van indítva, mert ügyfelek csak helyileg végrehajtott műveletekhez értesítés jelenik meg. Mivel a műveleteket csak a helyi meghajtóra véglegesítése garantált és (más szóval bejelentkezett), előfordulhat, hogy, vagy előfordulhat, hogy nem lehet visszavonni a jövőben.
- A művelet ismétlése útján egyetlen értesítést nézetnek minden egyes alkalmazott művelet. Ez azt jelenti, hogy tranzakció T1 Create(X) Delete(X) és Create(X) tartalmaz, akkor kap egy értesítést létrehozását, az X, az egyet a Törlés és újbóli létrehozása egy abban a sorrendben.
- Több műveletet tartalmazó tranzakciók a műveletek abban a sorrendben, amelyben beérkezett a felhasználótól elsődleges replikakészlettagon veszi.
- Részeként hamis előrehaladását feldolgozása bizonyos műveletek előfordulhat, hogy vonható vissza. Értesítések következik be, például visszavonás műveletekhez, az állam másolat visszaállítása stabil pontra. Egy fontos visszavonás értesítés különbség, hogy a rendszer az ismétlődő Billentyűparancsok események összesíti. Például ha tranzakció T1 van folyamatban visszavont, értesítés jelenik meg egy egyetlen Delete(X) szeretne.

## <a name="next-steps"></a>Következő lépések

- [Megbízható gyűjtemények](service-fabric-work-with-reliable-collections.md)
- [Megbízható szolgáltatások – első lépések](service-fabric-reliable-services-quick-start.md)
- [Megbízható szolgáltatások biztonsági mentése és visszaállítása (Vészhelyreállítás)](service-fabric-reliable-services-backup-restore.md)
- [Fejlesztői segédlet megbízható gyűjtemény](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
