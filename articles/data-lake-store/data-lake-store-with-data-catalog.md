<properties
   pageTitle="Adatok tó áruházból regisztrálása az Azure adatkatalógus |} Microsoft Azure"
   description="Adatok tó áruházból regisztrálása az Azure-adatkatalógus"
   services="data-lake-store,data-catalog" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Adatok tó áruházból regisztrálása az Azure-adatkatalógus

Ebben a cikkben megtanulhatja az Azure tó adattár integrálása az Azure adatkatalógus a tételére az adatok szervezeten belüli integrálja az adatkatalógusban. Kategorizálás adatok kapcsolatos további tudnivalókért olvassa el a [Azure adatkatalógus](../data-catalog/data-catalog-what-is-data-catalog.md)című témakört. Alkalmazási helyzetek, amelyben adatkatalógus használható, című cikkben talál részletes [Azure adatkatalógus esetei](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

- **Engedélyezze az Azure előfizetés** tó áruházból nyilvános Adatvillámnézet. [Útmutatást](data-lake-store-get-started-portal.md#signup)találhat.

- **Azure tó adattár fiókot**. Kövesse az utasításokat az [első lépések az Azure tó adattár az Azure-portálon](data-lake-store-get-started-portal.md). Az ebben az oktatóanyagban tudassa velünk **datacatalogstore**nevű tó adattár fiók létrehozásához. 

    Lépésben hozott létre a fiók, és töltse fel egy minta adathalmaz. Az ebben az oktatóanyagban tudassa velünk a [Azure adatok tó mely számjegy tárházba](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/) **AmbulanceData** mappán .csv fájl feltöltése. Különböző ügyfelek esetén például az [Azure tároló Explorer](http://storageexplorer.com/)blob-tárolóhoz feltölteni az adatokat is használhatja.

- **Azure adatok katalógus**. A szervezet már rendelkeznie egy a szervezet számára készült Azure adatkatalógus. Csak egy katalógus egyes a szervezet számára engedélyezett.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Tó adattár külső.FÜGGV adatkatalógus forrásaként

>[AZURE.VIDEO adcwithadl] 

1. Nyissa meg a `https://azure.microsoft.com/services/data-catalog`, és kattintson az **első lépések**.

2. Jelentkezzen be az adatkatalógusban Azure-portálra, és kattintson a **Közzététel adatok**.

    ![Adatforrás regisztrálása] (./media/data-lake-store-with-data-catalog/register-data-source.png "Adatforrás regisztrálása")

3. A következő lapon válassza az **Alkalmazás indítása**lehetőséget. A számítógépen a alkalmazás nyilvánvalóan fájl letöltése. A nyilvánvalóan fájlra duplán kattintva indítsa el az alkalmazást.

4. A Kezdőlap lapon kattintson a **Bejelentkezés**gombra, és írja be hitelesítő adatait.

    ![Üdvözlő képernyője] (./media/data-lake-store-with-data-catalog/welcome.screen.png "Üdvözlő képernyője")

5. A Válasszon egy adatforráshoz lapon jelölje ki az **Azure adatok tó**, és kattintson a **Tovább gombra**.

    ![Válassza az adatforrás] (./media/data-lake-store-with-data-catalog/select-source.png "Válassza az adatforrás")

6. A következő lapon adja meg regisztrálása az adatkatalógusban kívánt tó adattár fiókja nevét. Ezeket a más beállításokat alapértelmezettként, és kattintson a **Csatlakozás**gombra.

    ![Csatlakozás adatforráshoz] (./media/data-lake-store-with-data-catalog/connect-to-source.png "Csatlakozás adatforráshoz")

7. A következő oldalon a következő szegmensek lehet osztani.

    egy. A **Kiszolgáló hierarchia** mezőben a tó adattár fiók mappaszerkezet jelöli. **$Root** tó adattár fiók legfelső szintű, **AmbulanceData** pedig a létrehozott tó adattár fiók legfelső szintű mappa.

    b. A **rendelkezésre álló objektum** mező felsorolja a fájlokat és mappákat a **AmbulanceData** mappában találja.

    c billentyűkombinációt. **Objektumok regisztrált mezőben** a fájlokat és mappákat regisztrálása az Azure adatkatalógus kívánt listája.

    ![Nézet adatstruktúra] (./media/data-lake-store-with-data-catalog/view-data-structure.png "Nézet adatstruktúra")

8. Ebben az oktatóprogramban az összes fájlt regisztráljanak a címtárban. Az, hogy gombra a (![az objektumokhoz](./media/data-lake-store-with-data-catalog/move-objects.png "az objektumokhoz")) kattintva helyezze át az összes fájlt **regisztrálását objektumok** mezőbe. 

    Az adatok egy szervezeti szintű adatkatalógusában regisztrálva lesz, mert néhányuk, amelynek később használatával gyorsan megtalálhatja az adatok hozzáadása egy leronthatja megközelítés célszerű. Például adja meg egy e-mail címet (például egy ki a adatfeltöltés) adatok tulajdonosa, vagy az adatok azonosításához címkéket. A képernyőfelvétel alatt látható, hogy az adatok hozzáadunk címke.

    ![Nézet adatstruktúra] (./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Nézet adatstruktúra")

    Kattintson a **Regisztrálás**gombra.

8. A következő képernyőképet azt jelzi, hogy az adatok sikeres regisztrálva van a adatkatalógusában.

    ![Regisztráció kész] (./media/data-lake-store-with-data-catalog/registration-complete.png "Nézet adatstruktúra")

9. Kattintson a **Nézet portálon** lépjen vissza az adatkatalógusban-portálra, és ellenőrizze, hogy most már hozzáférhet a nyilvántartott adatokat a portálon. Az adatok keresése, a címkét, az adatok regisztrálásakor használt is használhatja.

    ![Katalógus adatkeresés] (./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Katalógus adatkeresés")

10. Most például vehet fel az adatokat a széljegyzetek és a dokumentáció műveleteket végezheti el. További információt az alábbi helyeken találhat.
    * [Jegyzetelés adatkatalógusában adatforrások](../data-catalog/data-catalog-how-to-annotate.md)
    * [Dokumentum-adatforrások adatkatalógusában](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Lásd még:

* [Jegyzetelés adatkatalógusában adatforrások](../data-catalog/data-catalog-how-to-annotate.md)
* [Dokumentum-adatforrások adatkatalógusában](../data-catalog/data-catalog-how-to-documentation.md)
* [Tó adattár integrálása más Azure szolgáltatások](data-lake-store-integrate-with-other-services.md)
