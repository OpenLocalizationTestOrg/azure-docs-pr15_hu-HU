<properties 
    pageTitle="Értesítések és az e-mail sablonok konfigurálása Azure API kezelése" 
    description="Megtudhatja, hogy miként értesítések konfigurálása és e-mail sablonok a Azure API-kezelés." 
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

# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Értesítések és az e-mail sablonok konfigurálása Azure API kezelése

API-kezelés lehetővé teszi a megadott eseményekkel kapcsolatos értesítések konfigurálása és állítsa be az e-mail sablonok, amelyekkel a rendszergazdák és a fejlesztők az API-kezelési-példányok kommunikálni. Ez a témakör bemutatja, hogyan a rendelkezésre álló eseményekkel kapcsolatos értesítések konfigurálása, és áttekintést nyújt az alábbi események használt e-mail sablonok konfigurálása.

## <a name="publisher-notifications"> </a>Publisher-értesítések konfigurálása

Értesítések beállítása, kattintson a **kezelés** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz elemre. Ekkor megjelenik az API kezelése a publisher-portálra.

![Publisher-portál][api-management-management-console]

>Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés – első lépések][] oktatóprogram során.

**Értesítések** kattintson a **API-kezelés** menüből elérhető az értesítések megjelenítése a bal.

![Publisher-értesítések][api-management-publisher-notifications]

Az alábbi listában szereplő események beállítható az értesítésekhez.

-   **Előfizetés kérések (jóváhagyást)** – a megadott e-mail címzettek és a felhasználók az értesítő e-mailek jóváhagyást API-termékek előfizetés kérések kapcsolatos kap.
-   Az **új előfizetések** – a megadott e-mail címzettek és a felhasználók e-mailben értesítéseket API-termék új előfizetések kap.
-   **Alkalmazás gyűjtemény kérések** – a megadott e-mail címzettek és a felhasználók értesítő e-mailek kap, amikor az alkalmazás tárába új kérelmeket.
-   **Titkos** – a megadott e-mail címzettek és a felhasználók e-mailek Titkos másolatot a fejlesztők küldött összes e-mailt kap.
-   **Új probléma vagy megjegyzést** – a megadott e-mail címzettek és a felhasználók kapnak értesítő e-mailek, amikor egy új probléma, vagy a Megjegyzés elküldése az developer Portal.
-   **Bezárás fiók üzenet** - a megadott e-mail címzettek és a felhasználók kap értesítő e-mailek, fiók bezárásakor.
-   **Approaching előfizetés kvótakorlátja** – az alábbi e-mail címzettek és a felhasználók értesítő e-mailek kap, amikor közelébe használati kvótája megkapja előfizetés használatát.

Az egyes események megadhatja, hogy e-mail címzettek használata az e-mail cím mezőbe, vagy választhat a felhasználók listájából.

Az e-mail címét, ha értesítést szeretne kapni, írja be őket az az e-mail cím mezőbe. Ha több e-mail címre, külön őket vesszőkkel.

![Értesítés a címzettek][api-management-email-addresses]

Adja meg a felhasználóknak, ha értesítést szeretne kapni, kattintson a **címzettek hozzáadása**gombra, jelölje be a felhasználóknak, ha értesítést szeretne kapni melletti jelölőnégyzetet, és kattintson az **OK gombra**.

>Figyelje meg, hogy csak a rendszergazdák jelennek meg a listában.

Után állítja be az értesítési címzettek, kattintson a **Mentés** alkalmazása a frissített értesítés címzettek gombra.

>Ha elhagyja a **Publisher értesítések** lap a publisher portál figyelmeztetés nem mentett vannak változások.

## <a name="email-templates"> </a>Konfigurálása e-mail sablonok

API kezelése során felügyelete és a szolgáltatással küldött e-mailek nyújt a e-mail sablonok. Az alábbi e-mail sablonok állnak rendelkezésre.

-   Alkalmazás gyűjtemény Beküldési jóváhagyott
-   Fejlesztőeszközök farewell betű
-   Értesítés közelít Developer kvótakorlátja
-   Felhasználók meghívása
-   Egy problémához hozzáadott új megjegyzés
-   Új probléma kapott
-   Új előfizetés aktiválása
-   Az előfizetés megújítása megerősítése
-   Előfizetés lemondott
-   Fogadott előfizetés kérés

Ezek a sablonok módosíthatók a kívánt módon.

Megtekintheti, és állítsa be az e-mail sablonok a API-kezelés az előfordulásra vonatkozóan, **értesítések** kattintson a bal oldali menüjében **API-kezelés** , és válassza az **E-mail sablonok** fület.

![E-mail sablonok][api-management-email-templates]

Megtekintéséhez, vagy módosíthatja az adott sablont, jelölje ki a **sablonok** legördülő listából.

![E-mail sablonok listájában][api-management-email-templates-list]

Egyes e-mail sablont tartalmaz, a tárgyat, egyszerű szöveg formátumban, és egy szervezet definition HTML formátumban. Testre szabható a minden elem a kívánt módon.

![E-mail sablon szerkesztőben][api-management-email-template]

A **Paraméterek** listában felsorolja azokat a paramétereket, amikor a tárgy vagy a szöveg szúrt lesz cserélni a a kijelölt érték, ha az e-mailt küldi. Paraméter beszúrásához vigye a kurzort, ahol kívánt szeretne lépni a paraméter, és kattintson a nyílra a bal oldalán a paraméter neve.

Kattintson a **kép** vagy **a Teszt küldése** hogyan az e-mailt fog keresse meg vagy próba e-mail küldése.

>Figyelje meg, hogy a paraméterek nem cseréli a tényleges értékek, amikor témakört, vagy elküldheti a vizsgálatot.
>
>A módosítások mentése az e-mailek sablon, kattintson a **Mentés**gombra, vagy a megszakításhoz a módosításokat a **Mégse**gombra.



[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Azure API-kezelés – első lépések]: api-management-get-started.md
[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance