Az alábbi táblázat a korlátok társított a különböző szolgáltatási rétegek (S1, S2, S3, F1). A költség tudni az egyes *egység* minden réteg olvassa el a [IoT központi árak](https://azure.microsoft.com/pricing/details/iot-hub/)című témakört.

| Erőforrás | S1 Standard | Normál s2 | S3 Standard | Ingyenes F1 |
| -------- | ----------- | ----------- | ----------- | ------- |
| Üzenetek/nap | 400000 | 6,000,000   | 300,000,000 | 8000 dollár   |
| Maximális mennyiség | 200    | 200         | 200         | 1       |

> [AZURE.NOTE] Ha várhatóan sok legfeljebb 200 egységek használata egy S1 vagy S2 vagy S3 réteg hubhoz, lépjen kapcsolatba a Microsoft-támogatást.

A következő táblázat felsorolja a IoT központi erőforrások vonatkozó korlátozások:

| Erőforrás | Határérték |
| -------- | ----- |
| Maximális kifizetett IoT hubok Azure előfizetésenként | 10 |
| Maximális ingyenes IoT hubok Azure előfizetésenként | 1 |
| Eszköz identitások maximális száma<br/>  egyetlen hívás visszaadott | 1000 |
| IoT központi üzenet maximális adatmegőrzési eszköz a felhőbe üzenetek | 7 nap |
| Eszköz a felhőbe üzenet maximális mérete | 256 KB |
| Eszköz a felhőbe köteg maximális mérete | 256 KB |
| Az eszköz a felhőbe köteg üzenetek maximális száma | 500 |
| Felhőalapú-eszköz üzenet maximális mérete | 64 KB |
| Felhőalapú-eszköz üzenetek maximális TTL (élettartam) | 2 nap |
| Felhőalapú-eszközökre készült kézbesítési maximális száma <br/> üzenetek | 100 |
| Visszajelzés üzenetek kézbesítési maximális száma <br/> a válasz az üzenetre cloud-eszköz | 100 |
| Üzenet, visszajelzést maximális TTL (élettartam) <br/> válasz az üzenetre cloud-eszköz | 2 nap |

> [AZURE.NOTE] Ha 10-nél több fizetett IoT hubok az Azure-előfizetésben van szüksége, forduljon a Microsoft támogatási.

A központi IoT szolgáltatás lehetővé kérelmeket, a következő kvóták túllépésekor:

| Szabályozása | Egy központi érték |
| -------- | ------------- |
| Identitás beállításjegyzék műveletek <br/> (létrehozása, beolvasásához, lista, frissítése és törlése), <br/> egyéni vagy tömeges importálás/exportálás | 5000/min/egység (S3) <br/> 100/min/egység (S1 és S2). |
| Eszköz kapcsolatok | 6000/sec/egység (S3), 120/sec/egység (S2), 12/sec/egység (S1). <br/>100/sec legalább. |
| Küldje eszközt a felhőbe | 6000/sec/egység (S3), 120/sec/egység (S2), 12/sec/egység (S1). <br/>100/sec legalább. |
| Felhőalapú-eszköz küldése | 5000/min/egység (S3), 100/min/egység (S1 és S2). |
| Felhőalapú-eszköz kapja | 50000/min/egység (S3), 1000/min/egység (S1 és S2). |
| Fájl feltöltése műveletek | 5000 fájlfeltöltési értesítések/min/egység (S3), a 100 fájl feltöltése értesítések/min/egység (az S1 és S2). <br/> Társítások URL-címe 10000 lehet ki az Azure tároló ügyfélhez egy időben.<br/> 10 Társítások URL-címe/eszköz használható-e ki egy időben. |
