<properties
    pageTitle="A Blob-tároló végpont tartománynév beállítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként feleltesse meg egy egyéni tartományt a Blob tároló végpont az Azure tároló ügyfélhez az Azure klasszikus portálon."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Egyéni tartománynevet a Blob-tároló végpont beállítása

## <a name="overview"></a>– Áttekintés

Beállíthatja, hogy egy egyéni tartományt blob eléréséhez a Azure tárterület-fiókjában. Az alapértelmezett végpont az Blob-tárolóhoz `<storage-account-name>.blob.core.windows.net`. Ha leképezheti egy egyéni tartomány és altartomány például **www.contoso.com** – az blob-végpontot tárterület-fiókját, majd a felhasználók lehet is access blob-adatok, hogy a tartomány használata a tárterület-fiókjában.

>[AZURE.IMPORTANT] Azure tároló egyelőre nem támogatják a HTTPS az egyéni tartományok. Tartsa szem előtt, hogy a vevők érdeklik ezt a szolgáltatást, és lesz elérhető az elkövetkező kiadásokban vagyunk.

Két módon az egyéni tartomány mutasson az blob-végpontot, a tárterület-fiókjához. A legegyszerűbben az egyéni tartomány és altartomány hozzárendelése az blob-végpontot CNAME rekord létrehozásához. Egy CNAME rekordot a DNS-szolgáltatása, amely a forrástartomány rendel egy e-céltartományban. Ebben az esetben az adatforrások tartománya az egyéni tartomány és altartomány – ne feledje, hogy a altartomány mindig szükséges. A céltartományban a Blob-szolgáltatás végpontjának.

A folyamaton, az egyéni tartomány hozzárendelése a blob-végpontot, azonban eredményezhet rövid időtartam, a tartomány legrövidebb leállás közben regisztrál a tartományt az [Azure klasszikus portálon](https://manage.windowsazure.com). Ha az egyéni tartomány van jelenleg támogató szolgáltatói szerződést (SLA), amely szükségessé teszi, hogy nincs nincs legrövidebb leállás és az alkalmazások, majd is használhatja az Azure **asverify** altartomány köztes regisztrációs lépésnek számára, hogy a felhasználók hozzáadhassák a tartomány a DNS-leképezés kerül sor közben eléréséhez.

A következő táblázat mutatja a minta URL-címek egy **mystorageaccount**nevű tárterület-fiókban blob eléréséhez. Az egyéni tartomány regisztrált a tárterület-fiókom **www.contoso.com**:

Erőforrás típusa|URL-formátumok
---|---
Tárterület-fiók|**Alapértelmezett URL-címe:** http://mystorageaccount.blob.core.windows.net<p>**Egyéni tartományi URL-cím:** http://www.contoso.com</td>
BLOB|**Alapértelmezett URL-címe:** http://mystorageaccount.blob.core.windows.net/mycontainer/myblob<p>**Egyéni tartományi URL-cím:** http://www.contoso.com/mycontainer/myblob
Legfelső szintű tároló|**Alapértelmezett URL-címe:** http://mystorageaccount.blob.core.windows.net/myblob vagy http://mystorageaccount.blob.core.windows.net/$ legfelső szintű/myblob<p>**Egyéni tartományi URL-cím:** http://www.contoso.com/myblob vagy http://www.contoso.com/$ legfelső szintű/myblob

## <a name="register-a-custom-domain-for-your-storage-account"></a>Egyéni tartományt a tárterület-fiók regisztrálása

Ez az eljárás használatával az egyéni tartomány regisztrálni, ha nem rendelkezik a tartomány röviden érhető el a felhasználók problémákat kétségei, vagy az egyéni tartomány jelenleg nem tartalmazzák az alkalmazás.

Ha az egyéni tartomány van jelenleg támogató olyan alkalmazás, amely bármely legrövidebb leállás nem lehet, használja az <a href="#register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain">Egyéni tartomány használata a közvetítő asverify altartomány tároló fiókjának nyilvántartásban</a>leírt eljárást.

A saját tartománynév beállítása, létre kell hoznia egy új CNAME rekordot a tartományregisztrálója. A CNAME rekordot, adja meg a tartománynév; alias Ebben az esetben azt rendeli hozzá a címet az egyéni tartomány a Blob tároló végpont a tárterület-fiókjához.

Minden tartományregisztráló egy CNAME rekordot tartalmazó hasonló, de némileg eltérő módszer, de fogalma azonos. Figyelje meg, hogy sok egyszerű tartomány regisztrációs csomagok nem ajánlja fel a DNS-konfigurációja, így előfordulhat, hogy frissíteni a tartomány regisztrációs csomagot, a CNAME rekord létrehozása előtt.

1.  Az [Azure klasszikus portálon](https://manage.windowsazure.com)nyissa meg a **tárhely** fülre.

2.  A **tárhely** lapon kattintson a tárterület-fiókot, amelynek meg szeretné feleltesse meg az egyéni tartomány nevét.

3.  Kattintson a **beállítás** fülre.

4.  A képernyő alján kattintson a **Manage Domain** az **Egyéni tartomány kezelése** párbeszédpanel megjelenítéséhez. A párbeszédpanel tetején a szövegben látni fogja az adatokat a CNAME rekord létrehozásának lépéseiről. Ebben a példában figyelmen kívül hagyja a szöveget, amelyet utal, amelyet a **asverify** altartomány.

5.  Jelentkezzen be a DNS tartományregisztráló webhelyén, és nyissa meg a DNS kezelése lapot. Előfordulhat, hogy keresett elem, például a **Tartománynevet**, **DNS**vagy **Név kiszolgáló kezelése**szakaszban.

6.  Keresse meg a szakasz CNAME kezelésére szolgáló. Előfordulhat, hogy akkor nyissa meg a Speciális beállítások lapot, és keresse meg a **CNAME**, szavakat **Alias**vagy **altartományokat**.

7.  Hozzon létre egy új CNAME rekordot, és adja meg a egy altartomány aliast, például a **www-t** vagy a **fényképeket**. Állomásnév, amely a Blob szolgáltatás végpontjának (ahol **mystorageaccount** a tárterület-fiókja nevét) formátum **mystorageaccount.blob.core.windows.net** majd meg kell adnia. A host nevet, a rendszer létrehozza **Egyéni tartományt kezelése** párbeszédpanel a szöveg.

8.  Miután létrehozta a CNAME rekordot, térjen vissza az **Egyéni tartomány kezelése** párbeszédpanelen, és írja be a nevet az egyéni tartomány, a altartomány, beleértve az **Egyéni tartomány neve** mezőben. Ha például ha tartománya, **akkor a contoso.com** , és a altartomány: **www-t**, adja meg a **www.contoso.com**; Ha a altartomány **fényképeket**, írja be a **photos.contoso.com**. Figyelje meg, hogy szükség-e a altartomány.

9. A **regisztráció** gombra kattintva regisztrálhatja az egyéni tartomány.

    Ha a regisztráció sikeres, látni fogja az üzenetet, **az egyéni tartomány működik**. A felhasználók most view blob adatokat az egyéni tartomány mindaddig, amíg a megfelelő engedéllyel rendelkeznek.

## <a name="register-a-custom-domain-for-your-storage-account-using-the-intermediary-asverify-subdomain"></a>Egyéni tartomány használata a közvetítő asverify altartomány tároló fiókjának regisztrálása

Az alábbi eljárással regisztrálhatja az egyéni tartomány Ha az egyéni tartomány jelenleg támogat igénylő, amely egy SZOLGÁLTATÁSISZINT-alkalmazással, hogy nem nincs legrövidebb leállás. A CNAME, amely mutat a asverify létrehozásával. &lt;altartomány&gt;. &lt;customdomain&gt; asverify szeretne. &lt;storageaccount&gt;. blob.core.windows.net, előre regisztrálhatja a tartományt az Azure. A második CNAME mutat, majd létrehozhat &lt;altartomány&gt;. &lt;customdomain&gt; való &lt;storageaccount&gt;. blob.core.windows.net, mely pontján forgalom az egyéni tartomány irányítja át a blob-végpontot.

A asverify altartomány egy speciális Azure felismeri. Prepending **asverify** saját altartomány szeretne, tetszőleges a Azure felismerje az egyéni tartományát a tartomány DNS-rekordjainak módosítása nélkül. A tartomány DNS-rekordjainak módosítása, miután azt lesz rendelve a blob végpont nincs legrövidebb leállás.

1.  Az [Azure klasszikus portálon](https://manage.windowsazure.com)nyissa meg a **tárhely** fülre.

2.  A **tárhely** lapon kattintson a tárterület-fiókot, amelynek meg szeretné feleltesse meg az egyéni tartomány nevét.

3.  Kattintson a **beállítás** fülre.

4.  A képernyő alján kattintson a **Manage Domain** az **Egyéni tartomány kezelése** párbeszédpanel megjelenítéséhez. A párbeszédpanel tetején szöveg látni fogja az adatokat a használatával a **asverify** altartomány CNAME rekord létrehozásának lépéseiről.

5.  Jelentkezzen be a DNS tartományregisztráló webhelyén, és nyissa meg a DNS kezelése lapot. Előfordulhat, hogy keresett elem, például a **Tartománynevet**, **DNS**vagy **Név kiszolgáló kezelése**szakaszban.

6.  Keresse meg a szakasz CNAME kezelésére szolgáló. Előfordulhat, hogy akkor nyissa meg a Speciális beállítások lapot, és keresse meg a **CNAME**, szavakat **Alias**vagy **altartományokat**.

7.  Hozzon létre egy új CNAME rekordot, és adja meg a egy altartomány aliast, amely tartalmazza az asverify altartomány. Az altartomány megadhatja például a formátum **asverify.www** vagy **asverify.photos**lesz. Állomásnév, amely a Blob szolgáltatás végpontjának (ahol **mystorageaccount** a tárterület-fiókja nevét) formátum **asverify.mystorageaccount.blob.core.windows.net** majd meg kell adnia. A host nevet, a rendszer létrehozza **Egyéni tartományt kezelése** párbeszédpanel a szöveg.

8.  Miután létrehozta a CNAME rekordot, térjen vissza az **Egyéni tartomány kezelése** párbeszédpanelen, és az **Egyéni tartomány neve** mezőben adja meg az egyéni tartomány nevét. Ha például ha tartománya, **akkor a contoso.com** , és a altartomány: **www-t**, adja meg a **www.contoso.com**; Ha a altartomány **fényképeket**, írja be a **photos.contoso.com**. Figyelje meg, hogy szükség-e a altartomány.

9.  Jelölje be a jelölőnégyzetet, amely szerint **Speciális: preregister az egyéni tartomány használata a "asverify" altartomány**.

10. A **regisztráció** gombra kattintva az egyéni tartomány preregister.

    Ha az előzetes regisztrációja sikeres, látni fogja az üzenetet, **az egyéni tartomány működik**.

11. Ezen a ponton Azure ellenőrizte az egyéni tartományát, de a forgalmat a tartomány van még irányítása nem a tárterület-fiókjába. A folyamat befejezéséhez térjen vissza a DNS tartományregisztráló webhelyén, és még egy CNAME rekordot, amely a altartomány megfelelteti a Blob-szolgáltatás végpontjának létrehozása gombra. Az altartomány a **www-t** vagy a **fényképek**és a **mystorageaccount.blob.core.windows.net** (ahol **mystorageaccount** a tárterület-fiókja nevét) szerint hostname (állomásnév) például adja meg. Ezt a lépést az egyéni tartomány a regisztráció befejeződött.

12. Végül törölheti a CNAME rekord **asverify**, használatával létrehozott, csak közvetítő lépéseként szükséges volt.

A felhasználók most view blob adatokat az egyéni tartomány mindaddig, amíg a megfelelő engedéllyel rendelkeznek.

## <a name="verify-that-the-custom-domain-references-your-blob-service-endpoint"></a>Ellenőrizze, hogy az egyéni tartomány hivatkozik, a Blob-szolgáltatás végpontjának

Ha ellenőrizni szeretné, hogy az egyéni tartomány valóban van-e hozzárendelve a Blob-szolgáltatás végpontjának, létrehozhat blob nyilvános tároló belül a tárterület-fiók. Ezt követően egy webböngészőben URI a következő formátumban eléréséhez használt a blob:

-   http://<*subdomain.customdomain*>/<*mycontainer*>/<*myblob*>

Az alábbi URI használhatja például keresztül, amely a **myforms** tárolóban blob rendeli **photos.contoso.com** egyéni altartományt webes űrlap eléréséhez:

-   http://Photos.contoso.com/myforms/applicationform.htm

## <a name="unregister-a-custom-domain-from-your-storage-account"></a>Egyéni tartományt a tárterület-fiókjából unregister

Egyéni tartomány unregister, kövesse az alábbi lépéseket: 

1. Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com). 

2. A navigációs ablakban kattintson a **tárolás**. 

3. **Tárterület** lapján kattintson az irányítópult megjelenítése a tárhely fiók nevét. 

5. A menüszalagon kattintson a **Manage Domain**. 

6. **Egyéni tartomány kezelése** párbeszédpanelen kattintson a **Unregister**. 


## <a name="additional-resources"></a>További források

-   [Egyéni tartomány megfeleltetése tartalom kézbesítési hálózati (CDN) végpont módjáról](../cdn/cdn-map-content-to-custom-domain.md)
