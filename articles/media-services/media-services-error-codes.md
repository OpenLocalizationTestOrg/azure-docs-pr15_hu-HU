<properties
    pageTitle="Azure Media Services hibakódok |} Microsoft Azure"
    description="A témakör áttekintést nyújt az Azure Media Services hibakódok."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>

# <a name="azure-media-services-error-codes"></a>Azure Media Services hibakódok esetén

Microsoft Azure Media Services használatakor, attól függően, hogy problémákat, például a Media Services nem támogatott műveletek hamarosan lejár hitelesítési tokenek szolgáltatásból jelenhet meg a HTTP-hibakódok. Az alábbiakban megtalálja őket a Media Services és a lehetséges okok által visszaadott **HTTP hibakódok** listáját.  
  
## <a name="400-bad-request"></a>400 – Hibás kérés

A kérelem érvénytelen adatokat tartalmaz, és elutasítják, az alábbi okok valamelyike miatt:

- Nem támogatott verzióját API van megadva. A legújabb verziót olvassa el a [Media Services REST API -val fejlesztési beállítása](media-services-rest-how-to-use.md)című témakört.
- Nincs megadva a Media Services API verziója. Adja meg az API-verzió tudnivalókért olvassa el a [Csatlakozás Media Services a Media Services REST API-val](media-services-rest-connect-programmatically.md). 
   
    >[AZURE.NOTE] Alkalmazás használatakor a .NET vagy Java SDK való csatlakozáshoz a Media Services, az API-verzió van megadva, valahányszor próbálja meg, és néhány Media Services elleni művelet végrehajtása.
- Egy meghatározott tulajdonság lett megadva. A tulajdonság neve a hibaüzenet tartalmaz. Csak azokat a tulajdonságokat, amelyek egy adott személy tagjai adható meg. Lásd: az [Azure Media Services REST API-hivatkozás](http://msdn.microsoft.com/library/azure/hh973617.aspx) személyek és tulajdonságaik listáját.
- Az érvénytelen tulajdonság értéke nincs megadva. A tulajdonság neve a hibaüzenet tartalmaz. Lásd: a érvénytelen tulajdonság-típusok vagy az értékeikre előző hivatkozására.
- Tulajdonságérték hiányzik, és szükség.
- A megadott URL-cím része egy hibás értéket tartalmazza.
- Kísérlet történt WriteOnce tulajdonság frissítéséhez.
- Kísérlet történt, amelynek a bemeneti tárgyi eszköz egy elsődleges AssetFile, amely nincs megadva vagy nem kell meghatározni a feladat létrehozásához.
- Kísérlet történt a Társítások megnevezés frissítéséhez. Társítások Locator csak létrehozása vagy törlése. A folyamatos átvitelű Locator frissíthető. További tudnivalókért olvassa el a [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx)című témakört.
- Az nem támogatott művelet vagy lekérdezés elindították. 

## <a name="401-unauthorized"></a>401 nem engedélyezett

A kérés nem sikerült hitelesíteni (előtt engedélyezhető) az alábbi okok valamelyike miatt:

- Hiányzik a hitelesítési fejléc.
- Hibás hitelesítési élőfej érték.
    - A token lejárt. Ha közvetlenül az a REST API-hoz, olvassa el a [Csatlakozás a Media Services a Media Services REST API-val](media-services-rest-connect_programmatically.md) megtudhatja, hogy miként egy új hitelesítési jogkivonat létrehozásához. A .NET vagy Java SDK használja, ha CloudMediaContext vagy MediaContract-objektum létrehozása a token készítése. Ennek módjáról a további tudnivalókért olvassa el a [Csatlakozás a .NET rendszerhez, a Media Services SDK a Media Services](media-services-dotnet-connect-programmatically.md).
    - A token érvénytelen aláírással tartalmazza.</li></ul></li></ul>

## <a name="403-forbidden"></a>Tiltott 403

Az alábbi okok valamelyike miatt nem engedélyezi a kérelmet:

- A Media Services-fiók nem található, vagy törölték.
- A Media Services fiók le van tiltva, és a kérelem típus nem HTTP GET. Szolgáltatási műveletek, valamint 403 választ ad eredményül.
- A hitelesítési jogkivonat nem tartalmaz a felhasználói hitelesítő adatokkal: fióknév és/vagy SubscriptionId. Ezt az információt a Media Services Felhasználóifelület-bővítmény a az adatkezelési portálon Azure Media Services fiókjának található.
- Az erőforrás nem érhető el.
    - Egy kísérlet történt egy MediaProcessor, amely nem érhető el a Media Services-fiók használata.
    - Kísérlet történt a Media Services által meghatározott JobTemplate frissítéséhez.
    - Kísérlet történt meg, hogy más Media Services fiók néhány megnevezés felülírásához.
    - Kísérlet történt meg, hogy más Media Services fiók néhány ContentKey felülírásához.

- Az erőforrás a Media Services-fiók elérésének szolgáltatás kvóta miatt nem hozható létre. A szolgáltatás kvótákat kapcsolatos további tudnivalókért lásd: a [kvótáinak és korlátai](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 – nem található

A kérés nem engedélyezett egy erőforrás az alábbi okok valamelyike miatt:

- Kísérlet történt, amely nem létezik entitás frissítéséhez.
- Egy kísérlet történt, amely nem létezik entitás törlése.
- Egy kísérlet történt létrehozása egy egyed, amely nem létezik entitás mutató hivatkozást.
- Egy kísérlet történt, amely nem létezik entitás első.
- Egy kísérlet történt adjon meg egy tárterület-fiókot, amely nem a Media Services fiókjához társított.  

## <a name="409-conflict"></a>409 ütközés

Az alábbi okok valamelyike miatt nem engedélyezi a kérelmet:

- Egynél több AssetFile belül az eszköz a megadott név tartalmaz.
- Egy kísérlet történt hozzon létre egy második elsődleges AssetFile belül az eszközt.
- Egy kísérlet történt hozzon létre egy ContentKey már használja a megadott azonosítóval.
- Egy kísérlet történt hozzon létre egy megnevezés már használja a megadott azonosítóval.
- Egynél több IngestManifestFile a megadott név belül a IngestManifest tartalmaz.
- Egy kísérlet történt a tárhely titkosított tárgyi eszköz egy második tároló titkosítási ContentKey csatolása.
- Egy kísérlet történt az azonos ContentKey csatolása az eszközt.
- Egy kísérlet történt hozzon létre egy megnevezés egy eszközre, amelynek tároló tároló hiányzik, vagy már nem társított az eszköz.
- Kísérlet történt a megnevezés egy eszköz, amely már használatban lévő 5 Locator való létrehozásához. (Azure tároló kényszeríti a legfeljebb öt átengedése házirendek egy tároló tároló.)
- Tárterület-fiók tárgyi eszköz egy IngestManifestAsset csatolása nincs ugyanaz, mint a szülő IngestManifest által használt tárterület fiókot.  

## <a name="500-internal-server-error"></a>500 – belső kiszolgálóhiba

A kérelem feldolgozása közben a Media Services a folyamatos feldolgozási akadályozó valamilyen hiba lép fel. Ez lehet az alábbi okok valamelyike miatt:

- Egy eszköz vagy a projekt létrehozása meghiúsul, mert a Media Services fiók kvóta szolgáltatásadatok átmenetileg nem érhető el.
- Mivel a fiók tároló fiókadatok átmenetileg nem érhető el egy digitáliseszköz- vagy IngestManifest blob tároló tároló létrehozása sikertelen lesz.
- Más váratlan hiba történt. 

## <a name="503-service-unavailable"></a>503 – a szolgáltatás nem érhető el

A kiszolgáló nem jelenleg nem tudja kérések fogadása. Ez a hiba a szolgáltatás fölösleges kérések is okozhatja. Mechanizmusa szabályozásának Media Services korlátozza az Erőforrás kihasználtsága alkalmazások, amelyek a fölösleges kérelem a szolgáltatás.

>[AZURE.NOTE]Ellenőrizze a hibaüzenet és hiba kód karakterlánc, ha részletesebb információkra az OK a 503 hiba lépett. Ez a hiba nem mindig jelenti szabályozásának.

Lehetséges állapot leírások a következők:

- "A kiszolgáló túlterhelt. Előző futtatja az ilyen típusú kérelem tartott {0} másodpercnél."
- "A kiszolgáló túlterhelt. Több mint {0} kérés / szekundum is lehet szabályozott."
- "A kiszolgáló túlterhelt. Több mint {0} kérések {1} másodpercek is lehet szabályozott."

A hiba kezelését használata ajánlott exponenciális vissza ki ismét összefüggés. Ez azt jelenti, hogy egymást követő hibaüzenetek próbálkozások közötti fokozatosan hosszabb vár használatával.  További tudnivalókért lásd: az [Ideiglenes (tranziens) hiba kezelésének alkalmazás blokkokból álló](https://msdn.microsoft.com/library/hh680905.aspx). 

>[AZURE.NOTE][Azure Media Services SDK a .NET rendszerhez](https://github.com/Azure/azure-sdk-for-media-services/tree/master)használatakor az újraküldés logikája 503 hibának még nem hajtotta végre a SDK csomagjában talál.  
  
## <a name="see-also"></a>Lásd még:  

[Media Services kezelése hibakódok esetén](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Visszajelzés küldése

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
