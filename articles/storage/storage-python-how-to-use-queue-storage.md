<properties
    pageTitle="Python várólista tárhelyet használata |} Microsoft Azure"
    description="Megtudhatja, hogy miként használhatja a Python Azure várólista szolgáltatást létrehozása és törlése a sorok, beszúrása, beszerzése és üzeneteket törölheti."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-python"></a>A Python várólista-tároló használata

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>– Áttekintés

Ez az útmutató megtudhatja, miként végezheti el a gyakori alkalmazási területek, az Azure várólista tároló szolgáltatással. A minták Python írt, és használja a [Microsoft Azure tároló SDK Python tartalmaz]. Az érintett esetek **beszúrása**, **Bepillantás**, **az első**és **Törlés,** üzenetek, valamint **létrehozásáról és a sorok törlése**. Sorok kapcsolatos további tudnivalókért tekintse át a [további lépések] szakasz hivatkozásait.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Útmutató: Várólista létrehozása

A **QueueService** objektum lehetővé teszi a sorok használata. A következő kódot létrehoz egy **QueueService** -objektumot. Adja hozzá a következő bármely Python fájlt, amelyben el szeretné érni programozás útján a Azure tároló tetején:

    from azure.storage.queue import QueueService

A következő kódot objektumot hoz létre **QueueService** a tárhely fiók nevét, és a fiók gombbal. Cserélje "saját fiók" és "SajátKulcs" a fiók nevét és billentyűt.

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## <a name="how-to-insert-a-message-into-a-queue"></a>Útmutató: Az üzenet beillesztése várólista

Üzenet beszúrni várólista, használja a **elhelyezni\_üzenet** módszert, hozzon létre egy új üzenetet, és vegye fel a várakozási sorban található.

    queue_service.put_message('taskqueue', u'Hello World')


## <a name="how-to-peek-at-the-next-message"></a>Útmutató: Bepillantás a következő üzenetbe

Akkor is betekintés levettük várólista-e az üzenet a anélkül, hogy eltávolítja a sor hívja fel a **betekintő\_üzenetek** módszert. Alapértelmezés szerint **betekintő\_üzenetek** lekéri egyetlen üzenetre.

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.content)


## <a name="how-to-dequeue-messages"></a>Útmutató: Az üzenetek feldolgozásához

A kód üzenet eltávolítása a két lépés várólista. Amikor felhívja **első\_üzenetek**, hibaüzenet a következő várólista alapértelmezés szerint. Egy üzenet, a visszaadott **első\_üzenetek** láthatatlanná válik a üzenetek olvasása a sorból bármely más kódot. Alapértelmezés szerint ez az üzenet maradjon láthatatlan 30 másodperces. Ha végzett, az üzenet eltávolítása a várólista, meg kell is hívni **törlése\_üzenet**. Az üzenet eltávolítása a két lépésből álló folyamat biztosítja, hogy a kód feldolgozása üzenet hardveres és szoftveres hiba miatt sikertelen, amikor a kód egy másik példányát is ugyanazon üzenet jelenik meg, és próbálkozzon újra. A kód hívások **törlése\_üzenet** jobb után az üzenet feldolgozása megtörtént.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)

Két módon testre szabhatja az üzenet lekérés a sorból.
Első lépésként elérheti a köteget, az üzenetek (legfeljebb 32). Második beállíthatja, hogy vagy lerövidítéséhez invisibility időtúllépés, több vagy kevesebb ideig teljesen feldolgozása minden üzenet, amely lehetővé teszi, hogy a kódot. Az alábbi példában kód a **első\_üzenetek** módszer 16 üzenetet kérhet egy hívásban vesz részt. Minden üzenet sablonnal feldolgozásával, majd a for ciklus. Azt is öt perc az egyes message invisibility időtúllépés állítja.

    messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)      


## <a name="how-to-change-the-contents-of-a-queued-message"></a>Útmutató: Az aszinkron üzenet tartalmának módosítása

Egy üzenet helyi a várakozási sorban található tartalmát módosíthatja. Ha az üzenet egy munkamennyiségű tevékenység jelöli, ez a szolgáltatás segítségével a munka feladat állapotának módosítása. A kód alatt használja a **frissítése\_üzenet** üzenet frissítési módjának. A láthatóság időtúllépés értéke 0, ami azt jelenti, azonnal megjelenik az üzenet, és a tartalom frissítésekor.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')

## <a name="how-to-get-the-queue-length"></a>Útmutató: Ismerkedés a sor hossza

Az üzenetek számát becslése várólista elérheti. A **első\_várólista\_metaadatok** módszer arra utasítja az való visszatéréshez a várakozási sorban található, és a **approximate_message_count**metaadatait a várólista szolgáltatást. Az eredmény oka csak közelítő üzeneteket, illetve is eltűnik, miután a kérelem válaszol a várólista szolgáltatást.

    metadata = queue_service.get_queue_metadata('taskqueue')
    count = metadata.approximate_message_count

## <a name="how-to-delete-a-queue"></a>Útmutató: Várólista törléséhez.

Várólista és a benne szereplő üzeneteket törléséhez hívja a **törlése\_várólista** módot.

    queue_service.delete_queue('taskqueue')

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta várólista-tároló alapjait, kövesse az alábbi hivatkozásokra kattintva további.

- [Python Developer Center](/develop/python/)
- [Azure tárolása REST API-val](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure tároló csapatának blogja]
- [Microsoft Azure tároló Python SDK]

[Azure tároló csapatának blogja]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure tároló Python SDK]: https://github.com/Azure/azure-storage-python
