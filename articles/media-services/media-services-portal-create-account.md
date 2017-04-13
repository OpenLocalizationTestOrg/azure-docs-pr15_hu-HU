<properties
    pageTitle=" Az Azure Media Services-fiók létrehozása az Azure portálján |} Microsoft Azure"
    description="Ebben az oktatóanyagban végigvezeti az Azure Media Services fiók létrehozása az Azure portálján."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Az Azure portálon Azure Media Services fiók létrehozása

> [AZURE.SELECTOR]
- [Portál](media-services-portal-create-account.md)
- [A PowerShell](media-services-manage-with-powershell.md)
- [TÖBBI](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. A részletekért lásd: [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/). 

Az Azure portál oly módon, amellyel gyorsan létrehozhatja az Azure Media Services (AMS) fiókkal. Használhatja a fiókját, amelyek lehetővé teszik, hogy tárolja, titkosítása, kódolását, kezelése és az Azure media adatfolyamként Media Services eléréséhez. Media Services fiók létrehozása időben, Ön is társított tároló fiók létrehozása (vagy egy meglévőt használ) a Media Services fiókként ugyanabban a földrajzi régióban.

Ez a cikk ismerteti, néhány gyakori fogalmak, és bemutatja, hogyan Media Services-fiók létrehozása az Azure portálján.

## <a name="concepts"></a>Fogalmak

Két kapcsolódó fiókok elérése a Media Services van szükség:

- A Media Services fiók. A fiók férhet hozzá egy sor olyan felhőalapú Media Services az Azure használható. A Media Services-fiók nem tárolja a tényleges médiatartalom. A médiatartalom és a multimédiás feldolgozás feladatok metaadatait inkább a fiókban tárolja. A fiók létrehozásakor, válassza ki az elérhető Media Services régióját. A kiválasztott terület egy adatközpont a fiók a metaadat-alapú rekordok tároló.

    Rendelkezésre álló Media Services (AMS) régiók, többek között az alábbiak: Észak-Európa, nyugati Europe, nyugati Amerikai Egyesült Államok, kelet-Amerikai Egyesült Államok, Délkelet-ázsiai, kelet-ázsiai, japán nyugati japán keleti. Media Services affinitás csoportok nem használ.
    
    AMS jelenleg is elérhető a következő adatközpontokban: Brazília Dél, India nyugati, India déli és indiai központi. Most már használhatja az Azure-portálra Media szolgáltatás-fiókok létrehozása és az itt leírt változatos műveleteket hajthat végre. Jó helyen jár Live kódolást nincs engedélyezve a következő adatközpontokban. További nem az összes típusú fenntartott egységek kódolást érhetők el az alábbi adatközpontokban.
    
    - Brazília Dél: Csak a szokásos és kódolást fenntartott alapegység állnak rendelkezésre.
    - India nyugati indiai Dél: 

- Azure tároló fiókkal. Tárterület-fiókok a Media Services fiókként ugyanabban a földrajzi régióban kell lennie. Amikor létrehoz egy Media Services-fiókot, választhat a meglévő tároló fiók ugyanabban a régióban, vagy létrehozhat egy új tárterület-fiókot is ugyanabban a régióban. Ha töröl egy Media Services-fiókot, a kapcsolódó tároló fiókban BLOB nem törlődnek.

## <a name="create-an-ams-account"></a>AMS fiók létrehozása

Ez a szakasz lépései bemutatják, hogyan hozhat létre AMS fiókot.

1. Jelentkezzen be a az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **+ Új** > **webes + Mobile** > **Media Services**.

    ![Media Services létrehozása](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. A **MEDIA SERVICES-fiók létrehozása** adja meg a szükséges értékeket.

    ![Media Services létrehozása](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. A **Fiók neve**mezőbe írja be az új AMS fiók nevére. A Media Services fióknév összes kisbetűs számokat vagy betűket, szóközök nélkül, és 3-24 karakter hosszú.
    2. Az előfizetését a különböző Azure-előfizetések listáját van hozzáférése közül választhat.
    
    2. Válassza az új vagy meglévő erőforrás **Erőforráscsoport**.  Erőforráscsoport információforrások, amelyek életciklus, engedélyeket és házirendeket megosztása gyűjteménye. További tudnivalók [az alábbi](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Helyét**jelölje ki a tárolni a média és metaadatok rekordokat a Media Services-fiókjához használt földrajzi területhez tartozik. Ez a terület folyamat, és a multimédia-adatfolyam használatával. A rendelkezésre álló Media Services régiók jelennek meg a legördülő listából. 
    
    3. **Tárterület-fiókot**válassza ki a tárhely adja meg a Media Services fiókjából médiatartalom blob-tárolóhoz. Kijelölhet egy meglévő tárterület-fiókot a Media Services fiókként ugyanabban a földrajzi régióban, vagy létrehozhat egy tárterület-fiókkal. Új tároló fiók ugyanabban a régióban jön létre. Tárterület fióknevét szabályairól megegyeznek a Media Services fiókok.

        További tudnivalók a tárhely [Itt](storage-introduction.md).

    4. Válassza a **Irányítópult PIN-kódot** a fiók telepítési állapotának megtekintéséhez.
    
7. Kattintson a **Create** a képernyő alján.

    Miután sikeresen létrejön a fiók, az állapot **futó**változik. 

    ![Multimédia-szolgáltatások beállításai](./media/media-services-portal-vod-get-started/media-services-settings.png)

    AMS fiókjának kezelését szeretné (például videó feltöltése, kódolását eszközök, figyelheti a projekt előrehaladásának) a **Beállítások** ablakban.

## <a name="manage-keys"></a>Billentyűk kezelése

A fiók nevét és az elsődleges kulcs információkat a Media Services fiók programozás útján történő eléréséhez szükséges.

1. Jelölje ki a fiókját az Azure-portálon. 

    **Beállítások** ablakban a jobb oldalon megjelenik. 

2. A **Beállítások** ablakban jelölje ki a **billentyűk**. 

    A **kulcsok kezelése** windows jeleníti meg a fiók nevét, és az elsődleges és másodlagos kulcsok jelenik meg. 
3. A Másolás gombra kattintva másolja az értékeket.
    
    ![Media Services kulcsok](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="next-steps"></a>Következő lépések

Most már feltölthet fájlokat a AMS fiókba. További tudnivalókért olvassa el a [fájlok feltöltése a](media-services-portal-upload-files.md)című témakört.

## <a name="media-services-learning-paths"></a>Media Services tanulási javaslatot

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


