<properties
    pageTitle="Azure tároló számla lista"
    description="A tároló fiókbeállításai Holdas az Azure eszközkészlet használata"
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->

# <a name="azure-storage-account-list"></a>Azure tároló számla lista #

Azure tároló fiókok a JDK alkalmazáskiszolgáló és tetszőleges összetevők, valamint gyorsítótárazás használatakor állam tárolásához használandó letöltési helyek engedélyezése. Holdas egy tipplistát ismert tároló fiókok elérhető a Holdas munkaterület projektjét. Nyissa meg a **Tárterület-fiókok** párbeszédpanelen, hogy a listán belül Holdas, kezelésére szolgál kattintson az **ablak**, kattintson a **Beállítások**gombra, bontsa ki az **Azure**, és válassza a **Tárterület-fiókok**.

Az alábbi példában a **Tárterület-fiókok** párbeszédpanelen látható.

![][ic719496]

Ez a párbeszédpanel- **partnerek** hivatkozása által használt tárterület-fiókokkal, például a következő párbeszédpanelek is megnyitható:

* A **Kiszolgáló beállítása** párbeszédpanel a **JDK** fülre.
* A **Kiszolgáló beállítása** párbeszédpanel a **kiszolgáló** fülre.
* A **Összetevő hozzáadása** párbeszédpanel.
* A **gyorsítótár** tulajdonságai párbeszédpanel.

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>Importálhatja a tárterület-fiókok-fájlból a Közzététel beállításai ##

1. **Tárterület-fiókok** párbeszédpanelen kattintson az **Importálás a közzététel beállításfájl**gombra.
2. (Ugorja át ezt a lépést, ha a helyi számítógépre már mentette a közzététel beállításfájl.) Az **Előfizetés-adatok importálása** párbeszédpanelen kattintson a **Közzététel-BEÁLLÍTÁSOKAT tartalmazó fájl letöltése**. Ha még nem jelentkezik az Azure-fiókjába, a rendszer kéri, jelentkezzen be a. Ezután kérni fogja az Azure mentés beállításfájl közzététele. (Az eredményül kapott utasításokat jelenik meg a bejelentkezési oldal - figyelmen kívül hagyhatja azokat az Azure portal által nyújtott és a Visual Studio felhasználóknak.) Mentse a helyi számítógépre.
3. Továbbra is az **Előfizetés-adatok importálása** párbeszédpanelen kattintson a **Tallózás** gombra, jelölje ki a helyi meghajtóra mentett korábban a Közzététel beállításai fájlt, és kattintson a **Megnyitás**gombra.
4. Kattintson az **OK gombra** az **Előfizetés-adatok importálása** párbeszédpanel bezárásához.

## <a name="to-create-a-new-storage-account"></a>Tárterület új fiók létrehozása ##

1. Belül a **Tárterület-fiókok** párbeszédpanelen kattintson a **Hozzáadás**gombra.
2. **Tárterület-fiók felvétele** párbeszédpanelen kattintson az **Új**gombra.
3. Belül **Új tárterület-fiók** párbeszédpanelen adja meg a következő értékeket:
    * Tárterület-fiók nevére.
    * A tároló fiók helyét.
    * A tároló számla leírását.
    * Az előfizetést, amelyhez a tárterület-fiók tartozik.
4. Kattintson **az OK gombra** kattintva zárja be az **Új tárterület-fiók** párbeszédpanelen.

Eltarthat néhány percig, amíg a tárhely fiók létrehozását. Létrehozás, után kattintson **az OK gombra** kattintva zárja be a **Tárterület-fiók hozzáadása** párbeszédpanelen, és a tárterület-fiókja, hozzáadódik a rendelkezésre álló tárhely fiókok listáját.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Tárterület meglévő fiók hozzáadása a listához ##

1. Ha nem rendelkezik Azure tárterület-fiókkal, hozzon létre egy újat a **Hozzon létre egy új tároló szakaszt a** fenti lépéseket követve. (Azt is megteheti, hozhat létre egy új tárterület-fiókot az [Azure Kezelőportálja segítségével][]a.)
2. Belül a **Tárterület-fiókok** párbeszédpanelen kattintson a **Hozzáadás**gombra.
3. Belül a **Tárterület-fiók hozzáadása** párbeszédpanelen adja meg **nevét** és értékeket **Hívóbetű**. A fiók nevét, és az access kulcs egy meglévő Azure tárterület-fiókot kell lennie. A **tároló** szakaszában az [Azure Kezelőportálja][] segítségével a fióknevét tárhely és a billentyűk megtekintése. A **Tárterület-fiók hozzáadása** párbeszédpanel az alábbihoz hasonlóan fog kinézni.

    ![][ic719497]

4. Kattintson az **OK gombra** a **Tárterület-fiók hozzáadása** párbeszédpanel bezárásához.

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>Egy új access termékkulccsal tárterület-fiók módosítása ##

1. Belül a **Tárterület-fiókok** párbeszédpanelen kattintson a Szerkesztés, és kattintson a **szerkeszteni**kívánt tároló fiókra.
2. Belül **Tároló fiók hívóbetű szerkesztése** párbeszédpanelen módosítsa a **Hívóbetű** értéket.
3. Kattintson az **OK gombra** a **Tárhely fiók hívóbetű szerkesztése** párbeszédpanel bezárásához.

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>A tárterület-fiók eltávolítása a listából, Holdas karbantartása ##

1. **Tárterület-fiókok** párbeszédpanelen kattintson az a tárterület-fiókot szerkesztéséhez, és kattintson az **Eltávolítás**gombra.
2. Amikor a rendszer kéri a tárterület-fiók eltávolítása **az OK** gombra.

>[AZURE.NOTE] A **Tárhely fiókok** párbeszédpanel – a tárhely fiók eltávolításával csak eltávolítja a megtekinthető belül Holdas tárterület-fiókok listáját. Nem az Azure előfizetéséből távolítja el a tárterület-fiókot. Emellett a tárterület-fiókot is után jelennek meg újra a listában Holdas betölti az előfizetés részleteit.

## <a name="see-also"></a>Lásd még: ##

[Azure eszközkészlete Holdas][]

[Az Azure eszközkészlet Holdas telepítése][] 

[A Holdas Azure egy helló világ alkalmazás létrehozása][]

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center][]című témakört.

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure eszközkészlete Holdas]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Kezelőportálja segítségével]: http://go.microsoft.com/fwlink/?LinkID=512959
[A Holdas Azure egy helló világ alkalmazás létrehozása]: http://go.microsoft.com/fwlink/?LinkID=699533
[Az Azure eszközkészlet Holdas telepítése]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png
