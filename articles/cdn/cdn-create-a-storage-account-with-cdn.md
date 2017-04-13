<properties
    pageTitle="A tároló fiók integrálása CDN |} Microsoft Azure"
    description="Megtudhatja, hogy miként nagy sávszélességű tartalmak olvasása által Azure-tárhelyről BLOB gyorsítótárazás Azure tartalom kézbesítési hálózati (CDN) segítségével."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="integrate-a-storage-account-with-cdn"></a>A tároló fiók integrálása CDN

CDN gyorsítótár tartalom engedélyezhető az Azure tárhelyről. Nagy sávszélességű tartalom kipróbálására által BLOB- és a fizikai csomópontok az Amerikai Egyesült Államokban, Europe, Ázsia, Ausztrália és Dél-Amerika számítási példányainak statikus tartalommá gyorsítótárazás globális megoldást is kínál a fejlesztők.


## <a name="step-1-create-a-storage-account"></a>Lépés: 1: A tárterület-fiók létrehozása

A következő eljárással hozzon létre egy új tároló fiókot Azure előfizetéshez. A tároló fiók Azure tárolása hozzáférést biztosít. A tároló fiók jelöli a legmagasabb szintű egyes Azure tárterület-szolgáltatás összetevők eléréséhez a névtér: Blob-szolgáltatások, a várólista szolgáltatások és a táblázat szolgáltatásokat. További tudnivalókért olvassa el a [Bevezetés a Microsoft Azure-tárolóhoz](../storage/storage-introduction.md).

Tárterület-fiók létrehozása, vagy a szolgáltatás rendszergazdája, vagy egy közös rendszergazda társítva az előfizetéshez kell lennie.

> [AZURE.NOTE] Több módon is használhatja az Azure-portál és a Powershell tárterület-fiók létrehozása.  Az ebben az oktatóanyagban fogjuk használni az Azure-portálra.  

**Az Azure előfizetéshez tartozó tárterület fiók létrehozása**

1.  Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2.  A bal felső sarokban válassza az **Új**elemre. **Az **Új** párbeszédpanelen válassza az **adatok + tároló**tároló fiók**parancsára.

    A **tárterület-fiók létrehozása** lap jelenik meg.

    ![Tárterület-fiók létrehozása][create-new-storage-account]

4. A **név** mezőbe írja be a altartomány nevét. Ez a bejegyzés 3-24 kisbetűket és számokat is tartalmazhat.

    Ez az érték, amely az előfizetéshez tartozó Blob, várólista vagy táblázat erőforrások cím a URI belül az állomásnév lesz. A Blob szolgáltatásban tároló erőforrás megoldására az alábbi formátumban szeretné használni URI hol * &lt;StorageAccountLabel&gt; * az **Írja be az URL-CÍMEK**beírt érték hivatkozik:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Fontos:** Az URL-cím címke az altartomány a tárterület-fiók URI-űrlapok és Azure-ban tárolt szolgáltatások között egyedinek kell lennie.

    Ehhez a fiókhoz programozás útján elérésekor ezt az értéket is lesz a portálon vagy a tárterület-fióknak a nevét.

5. Az alapértelmezett hagyja **telepítési modell**, a **fiók típusát**, a **Teljesítmény**és a **replikáció**. 

6. Válassza ki a tárterület-fiókhoz használt az **előfizetést** .

7. Jelölje be, vagy hozzon létre egy **Erőforráscsoport**.  További tájékoztatást az erőforrás csoportok az [erőforrás-kezelő Azure áttekintése](azure-resource-manager/resource-group-overview.md#resource-groups)című témakörben találhat.

8. Válasszon egy helyet a tárterület-fiókjához.

8. Kattintson a **létrehozása**gombra. A tároló fiók létrehozásának folyamata néhány percet igénybe vehet.


## <a name="step-2-create-a-new-cdn-profile"></a>Lépés: 2: Hozzon létre egy új CDN-profil

CDN-profil CDN-végpontok gyűjteménye.  Az egyes profilok tartalmaz egy vagy több CDN-végpontok.  Előfordulhat, hogy használni kívánt több profil a CDN-végpontok internetes tartomány, webalkalmazás, vagy néhány egyéb feltétel szerinti rendszerezésére.

> [AZURE.TIP] Ha már van egy CDN-profilt, ebben az oktatóanyagban használni kívánt, folytassa [a 3](#step-3-create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="step-3-create-a-new-cdn-endpoint"></a>Lépés 3: Hozzon létre egy új CDN-végpontot

**Tárterület-fiókjának új CDN végpont létrehozása**

1. Nyissa meg az [Azure Kezelőportálja](https://portal.azure.com)CDN-profilját.  Előfordulhat, hogy rendelkezik a kiemelt azt az előző lépésben az irányítópult.  Ha nem, akkor megtalálja: kattintson a **Tallózás gombra**, majd a **CDN-profilokat**, és a profiljában szeretne felvenni a végpontot.

    A CDN-profil lap jelenik meg.

    ![CDN-profil][cdn-profile-settings]

2. **Végpont hozzáadása** gombra.

    ![Végpont gomb felvétele][cdn-new-endpoint-button]

    A **Hozzáadás zárólap** lap jelenik meg.

    ![Végpont lap hozzáadása][cdn-add-endpoint]

3. Írja be egy **nevet** a CDN-végpont.  Ez a név szolgálnak a tartományban a gyorsítótárban tárolt forrásokat `<endpointname>.azureedge.net`.

4. Jelölje ki az **Origin típusa** legördülő *tároló*.  

5. A legördülő **Origin hostname (állomásnév)** jelölje ki a tárterület-fiókját.

6. Az alapértelmezett hagyja az **Origin elérési útját**, **Origin host élőfej**és **protokoll/Origin port**.  Meg kell adnia egy protocol (HTTP vagy HTTPS).

    > [AZURE.NOTE] Ez a beállítás lehetővé teszi, hogy az összes a nyilvánosan látható tárolók az a CDN-gyorsítótárazás a tárterület-fiókjában.  Ha a hatókörének szűkítésére szolgáló egyetlen tárolóhoz, használja az **Origin elérési útját**.  Megjegyzés: a tárolót kell rendelkeznie a cím láthatóságát, beállítása, hogy nyilvános.

7. A **Hozzáadás** gombra kattintva hozza létre az új végpontot.

8. Amikor létrejött a végpontot, jelennek meg a profil végpontok listáját. A lista nézet eléréséhez a gyorsítótárban tárolt tartalmat, valamint az origin tartományt használni az URL-cím látható.

    ![CDN-végpontok][cdn-endpoint-success]

    > [AZURE.NOTE] Az endpoint nem azonnal lesz használható.  A regisztrációhoz az a CDN-hálózaton keresztül szülőtől 90 percig is eltarthat. A felhasználók, akik tartománynevet szeretne használni a CDN azonnal próbálja állapotkód 404 jelenhet meg mindaddig, amíg a tartalom a CDN keresztül érhető el.


## <a name="step-4-access-cdn-content"></a>Lépés: 4: Access CDN-tartalom

A CDN-t a gyorsítótárban tárolt tartalom eléréséhez használja az a CDN megadott URL-cím a portálon. A gyorsítótárban tárolt blob címe a következőhöz hasonló lesz:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [AZURE.NOTE] Miután CDN a tárterület-fiók elérésének engedélyezése vagy szolgáltatás üzemeltetett, nyilvánosan elérhető összes objektumát aktiválhatják CDN él mentén-gyorsítótárazás. Ha módosítja egy objektumot, amely jelenleg tárolja a CDN-t, az új tartalom nem lesz a CDN kapható mindaddig, amíg a CDN frissíti a tartalmát, a gyorsítótárban tárolt tartalom time-to-live időszak lejártakor.

## <a name="step-5-remove-content-from-the-cdn"></a>5 lépés: A CDN tartalom eltávolítása

Ha már nem szeretne a gyorsítótárban lévő az Azure tartalom kézbesítési hálózati (CDN) objektum, hajthatók végre a következő lépések egyikét:

-   A tároló fel, hogy magánjellegű helyett. [Kezelés névtelen olvasási hozzáférést tárolók és BLOB](../storage/storage-manage-access-to-resources.md) talál további információt.
-   Az adatkezelési portálon az CDN-végpontot törlése vagy letiltása.
-   A szolgáltatott szolgáltatás már nem válaszol az objektum kérelmekre módosíthatók.

Egy objektum már szerepel a gyorsítótárban az a CDN gyorsítótárazott marad, amíg az objektum a time-to-live időszak lejárta vagy a végpont törölve van. A time-to-live időszak lejártakor a CDN fog ellenőrizze, hogy e az a CDN-végpontot is érvényes, és az objektum névtelenül is elérhető. Ha nem, majd az objektum lesz már nem gyorsítótárazott.


## <a name="additional-resources"></a>További források

-   [Hogyan CDN-tartalom hozzárendelése az egyéni tartomány](cdn-map-content-to-custom-domain.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png

[cdn-profile-settings]: ./media/cdn-create-a-storage-account-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-a-storage-account-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-a-storage-account-with-cdn/cdn-endpoint-success.png
