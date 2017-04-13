<properties
   pageTitle="Csatlakozás adatforrásokhoz |} Microsoft Azure"
   description="Ennek a csatlakozás adatforrásokhoz Azure adatkatalógus kapcsolatban felmerült kiemelés cikk."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>


# <a name="how-to-connect-to-data-sources"></a>Csatlakozás adatforrásokhoz

## <a name="introduction"></a>– Bevezetés
**Microsoft Azure adatkatalógus** egy teljes körű felügyelt felhőalapú szolgáltatásba, amely a bejegyzés és a rendszer a vállalati adatforrások feltárás szolgál. Más szóval **Azure adatkatalógus** az összes ezzel megkönnyítve a személyek Fedezze fel, megértését és adatforrásokból, és használata segítve a szervezet több értéket a meglévő adatokból. Ebben az esetben a kulcsfontosságú használja a naplózott adatok után a felhasználó adatforrás találja, és értelmezésére képes célját, a következő lépés-e csatlakozni az adatforráshoz helyezze a használni kívánt adatokat.

## <a name="data-source-locations"></a>Adatforrás helye
Adatok forrása regisztráció, során **Azure adatkatalógus** kapja meg az adatforrás metaadatait. A metaadatok a az adatforrás helyének adatait tartalmazza. Adatforrás helyének részleteit változhatnak adatforrásból, de mindig való csatlakozás szükséges információkat tartalmaz. Például SQL Server-tábla helyét tartalmazza a kiszolgáló neve, az adatbázis neve, a séma neve és a táblázat neve közben az SQL Server Reporting Services-jelentés helyét is tartalmaz, a kiszolgáló nevét és elérési útját a jelentést. Más típusú adatforrásokra helynek a felépítés és a forrás rendszer funkciók lesz.

## <a name="integrated-client-tools"></a>Integrált ügyféleszközök elől
A Kapcsolódás egy adatforráshoz legegyszerűbben használata a "Megnyitás a..." menü az **Azure adatkatalógus** portálon. A menü a kijelölt adatok eszköz csatlakozhat beállítások listáját jeleníti meg.
Az alapértelmezett csempe nézet használata esetén az egyes hivatkozásra a menü érhető el.

 ![SQL Server-tábla megnyitása az Excelben az adatok eszköz csempe](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

A lista nézet használata esetén a menü érhető el a Keresés mezőbe a portál ablakának tetején.

 ![A keresőmező egy SQL Server Reporting Services-jelentés az Jelentéskezelő megnyitása](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Támogatott ügyfélalkalmazásai
Használata esetén a "Megnyitás..." az adatforrásokat az Azure adatkatalógus portálon a helyes ügyfélalkalmazás menü telepítenie kell az ügyfélgépen.

| Nyissa meg az alkalmazásban | Fájlkiterjesztést / protokoll | Támogatott alkalmazás verziók |
| --- | --- | --- |
| Excel | .odc | Az Excel 2010-es vagy újabb verzió |
| Excel (felső 1000) | .odc | Az Excel 2010-es vagy újabb verzió |
| A Power Query | .xlsx | Az Excel 2016-ban vagy az Excel 2010 és a a Power Query az Excel-bővítmény az Excel 2013 telepítve
| Power BI-asztal | .pbix | A Power BI Desktop július 2016 vagy újabb verzió |
| Az SQL Server Data Tools | vsweb: / / | Visual Studio 2013 frissítés 4-es vagy újabb verzió az SQL Server szerszámok telepítve |
| Jelentéskezelő | http:// | Lásd: [az SQL Server Reporting Services böngészőkövetelmények](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Az adatok, az eszközök
A menüjében elérhető beállítások a kijelölt adatok eszköz típusától függ. Természetesen nem az összes lehetséges eszközök fog szerepelni a "Megnyitás..." menü, de az továbbra is egyszerűen csatlakozhat az adatforrás bármilyen ügyfél-eszköz használata. Ha egy adatok eszköz ki van jelölve az **Azure adatkatalógus** portálon, a teljes hely megjelenik a Tulajdonságok ablaktáblában.

 ![SQL Server-tábla kapcsolati adatok](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

A kapcsolat információk fog különböznek az adatforrás típusa adatforrástípus, de az adatokat a portálon szereplő képet ad mindent szeretne csatlakozni az adatforráshoz bármilyen ügyfél eszközben. Felhasználók másolhatja a kapcsolat adatai az adatforrások van talált a **Azure adatkatalógus**, amely lehetővé teszi azok eszközben megválasztott adatokkal végzett munkához használatával.

## <a name="connecting-and-data-source-permissions"></a>Csatlakozás és az adatok forrása engedélyek
Bár **Azure adatkatalógus** készíti adatforrások felfedezhetőségét, magát az adatokhoz való hozzáférés a adatok forrása tulajdonosa és rendszergazdája felügyelete alatt továbbra is. **Azure** adatkatalógusában adatforrás felfedezése nem engedélyezhetik felhasználó bármely hozzáférése az adatforráshoz magát.

Megkönnyítik a felhasználók, akik adatforrás felfedezése, de nem rendelkezik az adatok eléréséhez szükséges engedély, a felhasználók adatokat szolgáltatja a hozzáférés kérése tulajdonságban amikor jegyzetet fűz az adatforrás. – Beleértve a folyamaton, illetve az adatok forrása elérésekor kapcsolattartó hivatkozásokat – itt leírt információk jelennek meg az adatforrás hely adatait a portálon mellett.

 ![A kérés access képernyőn megjelenő utasításokat a kapcsolat adatait](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

##<a name="summary"></a>Összefoglalás
Adatforrás regisztrálása **Azure** adatkatalógusban ellenőrzi, hogy adatok feltárható másolásával szerkezeti és leíró metaadatokat az adatforrásból a katalógus üzembe. Miután regisztrált egy adatforráshoz, és talált, felhasználók csatlakozhatnak az adatforrás az **Azure adatkatalógus** portálról "Megnyitás..." " menü vagy a választott Adateszközök használatával.

## <a name="see-also"></a>Lásd még:
- [Első lépések az Azure adatkatalógus](data-catalog-get-started.md) oktatóprogram csatlakozás adatforrásokhoz részletesen olvashat.
