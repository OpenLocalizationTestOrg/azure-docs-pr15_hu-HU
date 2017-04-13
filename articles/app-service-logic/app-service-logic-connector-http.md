<properties
   pageTitle="A HTTP-figyelő és az összekötő használata összefüggés-alkalmazások |} Microsoft Azure alkalmazás szolgáltatás "
   description="Hogyan hozhat létre, és állítsa be a HTTP-figyelő és a HTTP művelet összekötő vagy API-alkalmazást, és vele az Azure alkalmazás szolgáltatás összefüggés-alkalmazásban"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/31/2016"
   ms.author="prkumar"/>


# <a name="get-started-with-the-http-listener-and-http-action-and-add-it-to-your-logic-app"></a>Első lépések a HTTP-figyelő és a HTTP-művelet, és adja hozzá az összefüggés-alkalmazás

> [AZURE.NOTE]Ez az összekötő támogatása befejezése azt esetén, mert szolgáltatásait van most alapértelmezés szerint látható mint a **kézi eseményindító** hoz létre új logika alkalmazások.  Azt javasoljuk, hogy frissítsen a logika minden olyan alkalmazás, ez az összekötő használják.  
> Ez a cikk verziójának logika alkalmazások 2014-es – 12-01-séma verziót vonatkozik.

Csatlakozás közvetlenül HTTP erőforrások HTTP kérések figyelése és a HTTP webszolgáltatás kérelmek beállítása. Vannak bizonyos esetekben, ahol szüksége lehet HTTP közvetlen kapcsolatok, beleértve a használata:

1.  Egy érték, amely támogatja a webhelyen vagy a mobilfelhasználót interaktív előtér alkalmazás fejlesztése.
2.  Adatok beolvasása és a folyamat, amely nincs a "Házon kívül" mezőben összekötő webszolgáltatásból.
3.  Az műveleteket, amelyek már elérhető, webes szolgáltatás, de nem érhető el, mint az API-alkalmazásokban.

Ez a helyzet a két lehetőség van:

1. **HTTP-figyelő**: Ez az összekötő működik-e az eseményindító, és figyeli a beállított végpont a HTTP-kérelmeket. Amikor hívása érkezik, a beállított végpont, a folyamat egy új példányát indítja, és adja a folyamat feldolgozásra az értekezlet-összehívást a kapott adatok. Is beállítható automatikusan a bejövő kérésre válaszolni, amikor a folyamat lépései, vagy Egyenletszerkesztővel munkafolyamat végrehajtása alapján választ, és a hívó választ küldeni.
2. **HTTP-művelet**: Ezzel minden elérhető végpontra webes kérelem konfigurálhatja az interneten, kapja vissza választ és számára elérhetővé teszi azt a további műveletek az használhatnak a folyamat.

Logika alkalmazások válthat a különféle adatforrásokat és ajánlat összekötők gyűjtéséhez és a folyamat részeként adatfeldolgozás alapján. A HTTP-összekötő is adhat az üzleti munkafolyamatok és a folyamat adatok ezzel a munkafolyamattal összefüggés-alkalmazásból részeként. 

## <a name="creating-an-http-listener-for-your-logic-app"></a>HTTP-figyelő az összefüggés-alkalmazás létrehozása
Összekötő összefüggés-alkalmazásból hozhatók létre, vagy közvetlenül a a Microsoft Azure piactéren lévő hozható létre. Összekötő létrehozása a piactérről:  

1. Válassza az Azure startboard **piactérről**.
2. "HTTP" keresni, jelölje be a HTTP-figyelő, és válassza a **Create**.
3.  Állítsa be a HTTP-figyelő a következőképpen:  
![][1]

4.  A csomagbeállítási beállításakor a következő beállítást, hogy a figyelő kell automatikus válasz vagy melyekhez fel explicit választ szeretne látni fogja. Ezeket a beállításokat **hamis** értékre a saját válasz küldése:  
![][2]

5.  Kattintson az **OK** hozhat létre.
6.  Amint az app API-példány jön létre, nyissa meg a beállításokat a biztonság beállítása. A HTTP-figyelő jelenleg támogatott alapszintű hitelesítés. Adhatja meg a biztonsági beállítással, a HTTP-figyelő megnyitásakor:  
![][3]
  
    **Ismert problémák** *a biztonsági beállítások megjelenítése "Nincs" az alapértelmezett érték azonban definiálva. Módosítania kell a beállítást, Basic, és nincs annak érdekében, hogy helyesen van-e beállítva a HTTP-figyelő mentése előtt vissza az.*  

7. Végezetül beállítása a biztonsági beállítások az API-alkalmazás nyilvános (névtelen) lehetővé teszi a külső ügyfelek végpontjának elérésére. Ez a beállítás részére elérhető a "minden elérhető beállítás > Alkalmazásbeállítások" HTTP figyelő API alkalmazás:![][10]

Miután befejeződött, amely, most már létrehozhat egy logikai alkalmazás használatához a HTTP-figyelő.

## <a name="using-the-http-listener-in-your-logic-app"></a>A HTTP-figyelő logika alkalmazás használata
Az API-alkalmazás létrehozása után is most már használhatja a HTTP-figyelő eseményindító az logika számára. Ehhez kell:

4.  Hozzon létre egy új logika alkalmazást.
5.  Nyissa meg a "Eseményindítók és a műveletek" nyissa meg a logika alkalmazások Designert, és a beállítása. A gyűjtemény a HTTP-figyelő szerepelnek. Jelölje ki.
6.  Most már beállíthatja, hogy a HTTP módszer, és amikor szüksége van a folyamat elindítása a értesülnie relatív URL-cím:  
![][4]  
![][5]

7.  Ha a teljes URI, kattintson duplán a HTTP-figyelő megtekintése a beállításokat, és másolja a vágólapra az URL-címet az API-alkalmazás "Host":  
![][6]
8.  Most már használhatja az adatokat, fogadja a HTTP-kérés az egyéb műveletek a folyamat az alábbi képlettel történik:  
![][7]  
![][8]
9.  Ha szeretné elküldeni a választ, végül pedig adja hozzá a másik HTTP-figyelő, és válassza ki a HTTP-válasz küldése. A kérelem azonosító beállítása a kapott a HTTP-figyelő kérelemazonosító, és a válasz szövegtörzséből, majd a HTTP-állapot térjen vissza szeretné feltölteni:  
![][9]

## <a name="using-the-http-action"></a>A HTTP-művelet használata
A HTTP-művelet logika alkalmazásokat támogatja natív módon, és használatához nincs szükség az API alkalmazás első szeretne használni, akkor hozható létre. HTTP-művelet beszúrása a logika alkalmazás bármely pontján, és válassza a URI, az élőfejek és az üzenet a híváshoz.
A HTTP-művelet ügyfél egymás biztonsági többféle módon támogatja. Az [ügyfél, oldal biztonsági beállítások](../scheduler/scheduler-outbound-authentication.md)megjelenítéséhez.

A kimenet a HTTP-művelet pedig a fejlécek használható további után a folyamat hasonlóan az egyéb műveletek és összekötők kimeneti felhasznált hogyan törzsébe.

## <a name="do-more-with-your-connector"></a>További műveletek az összekötő
Most, hogy az összekötő hoz létre, felveheti egy logikai alkalmazással üzleti munkafolyamat. Lásd: [logika alkalmazások melyek?](app-service-logic-what-are-logic-apps.md).

[Összekötők és az API-alkalmazások hivatkozás](http://go.microsoft.com/fwlink/p/?LinkId=529766)megtekintése a Swagger REST API hivatkozást.

Tekintse át a teljesítmény statisztika és a vezérlő biztonsági az összekötő is. Lásd: [kezelése, és figyelemmel követheti a beépített API alkalmazásait és összekötők](app-service-logic-monitor-your-connectors.md).

> [AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók Ismerkedés az Azure összefüggés-alkalmazások, [Próbálja meg logika alkalmazás](https://tryappservice.azure.com/?appservice=logic)megnyitásához. Létrehozhat egy rövid életű starter logika alkalmazás azonnal App szolgáltatásban. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

<!--Image references-->
[1]: ./media/app-service-logic-connector-http/1.png
[2]: ./media/app-service-logic-connector-http/2.png
[3]: ./media/app-service-logic-connector-http/3.png
[4]: ./media/app-service-logic-connector-http/4.png
[5]: ./media/app-service-logic-connector-http/5.png
[6]: ./media/app-service-logic-connector-http/6.png
[7]: ./media/app-service-logic-connector-http/7.png
[8]: ./media/app-service-logic-connector-http/8.png
[9]: ./media/app-service-logic-connector-http/9.png
[10]: ./media/app-service-logic-connector-http/10.png
