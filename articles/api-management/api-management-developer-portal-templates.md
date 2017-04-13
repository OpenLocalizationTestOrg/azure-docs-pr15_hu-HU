<properties 
    pageTitle="Sablonok használata Azure API-kezelés developer Portal segítségével testreszabása |} Microsoft Azure" 
    description="Megtudhatja, hogy miként szabhatja testre a Azure API-kezelés developer Portal segítségével sablonok használatával." 
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


# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Sablonok használata Azure API-kezelés developer Portal segítségével testreszabása

Azure API-kezelés funkcióival több testreszabási rendszergazdák [testre szabhatja a megjelenését és működését a developer Portal segítségével](api-management-customize-portal.md), valamint testre szabhatja a tartalom egy sor olyan sablonokat, állítsa be a tartalom a saját maguk lapok használata developer portal lapot. Megvan [DotLiquid](http://dotliquidmarkup.org/) szintaxisát, és a egy megadott honosított karakterlánc erőforrások, ikon és lap vezérlők használata esetén a konfigurálása a lapok tartalmát, ezek a sablonok igényei szerint kiváló lehetősége.

## <a name="developer-portal-templates-overview"></a>Developer portal sablonok – áttekintés

Developer portal sablonok a developer Portal segítségével a rendszergazdák az API Management szolgáltatás példányának kezeli. Fejlesztőeszközök sablonok kezeléséhez, nyissa meg a API Management szolgáltatás példányt az Azure klasszikus portálon, majd kattintson a **Tallózás gombra**.

![Developer Portal segítségével][api-management-browse]

Ha már a publisher-portálon, a developer Portal segítségével elérheti a **Developer portal**gombra kattintva.

![Developer portal menü][api-management-developer-portal-menu]

Developer portal sablon használatához kattintson a Testreszabás menü megjelenítése a bal oldalon a testreszabása ikonra, és kattintson a **sablonok**.

![Developer portal sablonok][api-management-customize-menu]

A sablonok listájában a developer Portal segítségével a különböző lapokon kiterjedő sablonok számos kategóriába jeleníti meg. Minden sablon eltérő, de módosításainak közzététele és szerkeszthessék azokat a lépéseket megegyeznek. Sablon szerkesztéséhez kattintson a sablon nevét.

![Developer portal sablonok][api-management-templates-menu]

Sablon kattintás megnyitja a fejlesztői portáloldalon, testre szabható sablon alapján. Ebben a példában a **terméklistában** sablon jelenik meg. A **termék** listasablon a képernyőn a piros téglalap jelölt területe szabályozza. 

![Termékek listasablon][api-management-developer-portal-templates-overview]

Egyes sablonok, például a **Felhasználói profil** sablonokkal, testre szabhatja a Navigálás a ugyanazon a lapon. 

![A felhasználói profil sablonok][api-management-user-profile-templates]

A szerkesztő minden developer portal sablon két részből áll jelenik meg az oldal alján. A bal oldalon megjeleníti a szerkesztési ablakot a sablon, és a jobb szélen jeleníti meg a sablon az adatmodellbe. 

A sablon szerkesztése ablakban a korrektúrát megjelenését és az developer Portal az adott oldal működését szabályozó tartalmazza. A sablonban a korrektúrát [DotLiquid](http://dotliquidmarkup.org/) szintaxissal. Egy népszerű szerkesztője DotLiquid [tervezőknek DotLiquid](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Valós idejű módosításai a sablonba szerkesztés közben jelennek meg a böngészőben, de nem láthatók az ügyfelekkel, [mentése](#to-save-a-template) és [közzététele](#to-publish-a-template) a sablon-ig.

![Sablon jelölés][api-management-template]

A **sablon adatok** ablaktáblában az adatmodellhez útmutató biztosít a szervezetek, amelyek egy adott sablon használható. Ez az útmutató biztosít az élő adatokat a developer Portal segítségével az aktuálisan megjelenített jelennek meg. A **sablon adatok** ablak jobb felső sarkában a téglalap gombra kattintva kibonthatja a sablon ablaktáblák.

![Sablon adatmodell][api-management-template-data]

Az előző példában a developer Portal segítségével jelenik meg, hogy az adatok ablaktáblában jelenik meg a **sablon adatokat** , az alábbi példában látható módon beolvasott két termékek vannak.

    {
        "Paging": {
            "Page": 1,
            "PageSize": 10,
            "TotalItemCount": 2,
            "ShowAll": false,
            "PageCount": 1
        },
        "Filtering": {
            "Pattern": null,
            "Placeholder": "Search products"
        },
        "Products": [
            {
                "Id": "56ec64c380ed850042060001",
                "Title": "Starter",
                "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
                "Terms": "",
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            },
            {
                "Id": "56ec64c380ed850042060002",
                "Title": "Unlimited",
                "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
                "Terms": null,
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            }
        ]
    }

A **terméklistában** sablonban a korrektúrát az adatokat a kívánt kimeneti nyújtása a webhelycsoport minden egyes termék információk és a hivatkozás megjelenítendő termékek keresztül léptetés dolgozza fel. Megjegyzés: a `<search-control>` és `<page-control>` elemeinek a korrektúrát. Ezek a Keresés és vezérlőelemek a lapon személyhívó megjelenítésének szabályozása `ProductsStrings|PageTitleProducts`honosított karakterlánc hivatkozás, amely tartalmazza a `h2` fejlécszöveg laphoz. Karakterlánc listáját erőforrások, oldal vezérlők és ikonok érhető el a developer portal sablonok című témakörben olvashat [API-kezelés developer portal sablonok hivatkozást](https://msdn.microsoft.com/library/azure/mt697540.aspx).

    <search-control></search-control>
    <div class="row">
        <div class="col-md-9">
            <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
        {% if products.size > 0 %}
        <ul class="list-unstyled">
        {% for product in products %}
            <li>
                <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
                {{product.description}}
            </li>   
        {% endfor %}
        </ul>
        <paging-control></paging-control>
        {% else %}
        {% localized "CommonResources|NoItemsToDisplay" %}
        {% endif %}
        </div>
    </div>

## <a name="to-save-a-template"></a>A sablon mentéséhez

A sablon mentéséhez kattintson a Mentés gombra a sablon szerkesztőben.

![Sablon mentése][api-management-save-template]

Mindaddig, amíg a rendszer teszi közzé, amelyek mentett módosítások nem élő a developer Portal segítségével.

## <a name="to-publish-a-template"></a>Sablon közzététele

Mentett sablonok akár egyenként, akár az összes közös tehetők közzé. Ha közzé szeretné egy egyéni sablont, kattintson a Közzététel gombra a sablon szerkesztőben.

![Űrlapsablon közzététel][api-management-publish-template]

Kattintson az **Igen gombra** , és erősítse meg a sablon élő developer Portal.

![Erősítse meg közzététele][api-management-publish-template-confirm]

Jelenleg nem közzétett sablon az összes verzió közzétételéhez kattintson a **Közzététel** gombra a sablonok listájában. Közzé nem tett sablonok a sablon neve követő csillag jelölik. Ebben a példában a **terméklistában** és a **termék** sablonok vannak közzétéve.

![Sablonok közzététele][api-management-publish-templates]

Kattintson a **Közzététel testreszabások** megerősítéséhez.

![Erősítse meg közzététele][api-management-publish-customizations]

Újonnan közzétett sablonok közvetlenül a developer Portal segítségével hatékonyak.

## <a name="to-revert-a-template-to-the-previous-version"></a>Vissza a korábbi verzióra sablon

Sablon közzétett korábbi verziójának visszaállításához kattintson a sablon szerkesztőben visszavált.

![Vissza a sablon][api-management-revert-template]

Kattintson az **Igen gombra** kattintva erősítse meg.

![Erősítse meg][api-management-revert-template-confirm]

A korábban közzétett sablon verziószáma élő az developer Portal a visszaállítási művelet befejezése után.

## <a name="to-restore-a-template-to-the-default-version"></a>Ha vissza szeretne állítani egy sablont az alapértelmezett verzióra

Sablonok visszaállítása az alapértelmezett verzióra két lépésből áll. Először lehet visszaállítani a sablonok, és kattintson a visszaállított verziók közzé kell tennie.

Az alapértelmezett verzióra egyetlen sablon kattintson visszaállítás lehetőségre a sablon szerkesztőben.

![Vissza a sablon][api-management-reset-template]

Kattintson az **Igen gombra** kattintva erősítse meg.

![Erősítse meg][api-management-reset-template-confirm]

Az alapértelmezett verzióra összes sablont visszaállításához kattintson a sablon lista **visszaállítása az alapértelmezett sablonok** gombra.

![Sablonok visszaállítása][api-management-restore-templates]

A visszaállított sablonokat kell majd tehet közzé egyenként vagy egyszerre [sablon közzététele](#to-publish-a-template)utasításait követve.

## <a name="developer-portal-templates-reference"></a>A portál sablonok fejlesztői segédletben

Hivatkozás információt developer portal sablonok, oldal vezérlők, ikonok és karakterlánc erőforrásokhoz tartozó című témakörben talál [API-kezelés developer portal sablonok hivatkozást](https://msdn.microsoft.com/library/azure/mt697540.aspx).

## <a name="watch-a-video-overview"></a>Megtekintés az egy videó – áttekintés

A következő videóból megtudhatja, hogy miként vitafórum és a minősítés hozzáadása a developer Portal segítségével sablonok használata az API-val és a művelet oldalakat.

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







