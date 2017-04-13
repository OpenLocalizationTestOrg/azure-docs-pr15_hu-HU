<properties 
    pageTitle="Hogyan felhasználói fiókokat az Azure API-kezelés |} Microsoft Azure" 
    description="Megtudhatja, hogy miként hozhat létre, vagy a Azure API-kezelés felhasználók meghívása" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Azure API kezelése lapon a felhasználói fiókok kezelése

API-kezelése a fejlesztők az API-k, amely azt az jelenítik meg az API-kezelés segítségével a felhasználók. Ez az útmutató hogyan hozhat létre, és a fejlesztők meghívása API-k és termékek látható, hogy elérhetővé őket a felügyeleti API-példányt tartson. A felhasználói fiókok kezelését programozás útján további tudnivalókért lásd a [szervezet felhasználói](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentáció az [API-kezelés többi](https://msdn.microsoft.com/library/azure/dn776326.aspx) hivatkozás.

## <a name="create-developer"> </a>Hozzon létre egy új developer

Hozzon létre egy új fejlesztő, kattintson a **kezelése** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz. Ekkor megjelenik az API kezelése a publisher-portálra. Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés – első lépések][] oktatóprogram során.

![Publisher-portál][api-management-management-console]

A bal oldalon a **API-kezelés** menüben kattintson a **felhasználók** elemre, és kattintson a **felhasználó hozzáadása**gombra.

![Fejlesztőeszközök létrehozása][api-management-create-developer]

Az **e-mailek**, a **jelszót**és a **név** megadása az új fejlesztője, és kattintson a **Mentés**gombra.

![Fejlesztőeszközök létrehozása][api-management-add-new-user]

Alapértelmezés szerint az újonnan létrehozott Fejlesztőeszközök fiókok **aktív**, és a **fejlesztők** csoporthoz társított.

![Új developer][api-management-new-developer]

**Aktív** állapotú Fejlesztőeszközök fiókok összes az API-hoz, amelynek előfizetéssel rendelkezik eléréséhez használható. További csoportok társíthatja az újonnan létrehozott fejlesztője, megtudhatja, [hogy miként fejlesztők-csoportok társítani][].

## <a name="invite-developer"> </a>Fejlesztői meghívása

Ha meg szeretne hívni egy fejlesztő, a bal oldalon a **API-kezelés** menüben kattintson a **felhasználók** elemre, és válassza a **Felhasználók meghívása**.

![Fejlesztőeszközök meghívása][api-management-invite-developer]

Írja be a Fejlesztőeszközök nevét és e-mail címét, és kattintson a **Meghívás**.

![Fejlesztőeszközök meghívása][api-management-invite-developer-window]

A megerősítést kérő üzenet jelenik meg, de az újonnan meghívott fejlesztője nem jelenik meg a listában, amíg a meghívó elfogadása után. 

![Meghívás megerősítése][api-management-invite-developer-confirmation]

A Fejlesztőeszközök meghívására a Fejlesztőeszközök e-mailt küldi. Ez az e-mailek sablon használatával jön létre, és testre szabható. További tudnivalókért olvassa el a [konfigurálása e-mail sablonok][]című témakört.

Miután elfogadták a meghívást, a fiók aktívvá válik.

## <a name="block-developer"></a> Inaktiválás vagy a fejlesztői fiók újraaktiválása

**Alapértelmezés szerint újonnan létrehozott vagy a meghívott Fejlesztőeszközök fiókok aktívak.** **Továbbfejlesztett fájlblokkolás**kattintva Fejlesztőeszközök inaktiválása. A Fejlesztőeszközök letiltott fiók újraaktiválása, kattintson az **Aktiválás**gombra. A Fejlesztőeszközök letiltott fiók nem a developer Portal segítségével elérheti, vagy hívás bármely API-khoz. Ha törölni szeretne egy felhasználói fiókot, kattintson a **Törlés**gombra.

![Továbbfejlesztett fájlblokkolás developer][api-management-new-developer]

## <a name="reset-a-user-password"></a>Felhasználói jelszó alaphelyzetbe állítása

Felhasználói fiók jelszavának alaphelyzetbe állításához kattintson a fiók nevére.

![Jelszó alaphelyzetbe állítása][api-management-view-developer]

A mutató hivatkozást szeretne küldeni a felhasználó jelszavának alaphelyzetbe a **jelszó alaphelyzetbe állítása** gombra.

![Jelszó alaphelyzetbe állítása][api-management-reset-password]

Felhasználói fiókok programozás útján dolgozni, dokumentációja [felhasználói entitás](https://msdn.microsoft.com/library/azure/dn776330.aspx) a [API-kezelés többi](https://msdn.microsoft.com/library/azure/dn776326.aspx) hivatkozás. A felhasználói fiók jelszavának alaphelyzetbe egy adott értéket, használja a [felhasználó frissítése](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) műveletet, és adja meg a kívánt jelszót.

## <a name="pending-verification"></a>Függőben lévő ellenőrzése

![Függőben lévő ellenőrzése][api-management-pending-verification]

## <a name="next-steps"> </a>Következő lépések

A Fejlesztőeszközök fiók létrehozását követően társítása szerepkörök, és előfizetés-termékek és -API-khoz. További információért megtudhatja, [hogy miként csoportok létrehozása és használata][].


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
[]: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[Hogyan csoportok létrehozása és használata]: api-management-howto-create-groups.md
[Hogyan csoportok társítása fejlesztők számára]: api-management-howto-create-groups.md#associate-group-developer

[Azure API-kezelés – első lépések]: api-management-get-started.md
[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance
[E-mail sablonok konfigurálása]: api-management-howto-configure-notifications.md#email-templates