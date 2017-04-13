<properties 
   pageTitle="Egyéni bejelentkezik a napló Analytics |} Microsoft Azure"
   description="Log Analytics események összegyűjtheti Windows és a Linux számítógépeken szövegfájlokból.  Ez a cikk ismerteti a definiálása az új egyéni naplózási és a rekordok hoznának létre a MOBILE adattárban részleteit."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-logs-in-log-analytics"></a>Egyéni részletek, a naplófájl Analytics

Az egyéni naplók adatforrás napló Analytics események gyűjt a Windows és a Linux számítógépeken szövegfájlokból teszi lehetővé. Sok alkalmazás információk naplózása szövegfájlba szabványos naplózási szolgáltatások, például a Windows eseménynaplójának vagy Syslog helyett.  Összegyűjtött, miután külön naplót Analytics az [Egyéni mezők](log-analytics-custom-fields.md) szolgáltatásával mezőkbe is elemzi a az egyes rekordok a naplóban.

![Egyéni napló gyűjtemény](media/log-analytics-data-sources-custom-logs/overview.png)

A naplófájlok gyűjtendő egyeznie kell az alábbi követelményeket.

- A napló kell soronként egy bejegyzés van, vagy használja a következő formátumokban mindegyik listaelem kezdetének egyező időbélyeg.

    YYYY-MM-DD ÓÓ <br>
  YYYY/M HH: MM: MM AM/PM <br>
  Hétfő nn éééé óó
    
- A naplófájl nem engedélyezniük körkörös frissítések, ahová a fájlt a rendszer felülírja az új bejegyzések. 

## <a name="defining-a-custom-log"></a>Egyéni naplózási meghatározása

Az alábbi eljárással definiáljon egyéni naplófájlt.  Görgessen le a minta hozzáadása egyéni naplózási ismertetését megtalálja a cikk végén.

### <a name="step-1-open-the-custom-log-wizard"></a>Lépés: 1. Nyissa meg az egyéni napló varázsló

Az egyéni napló varázsló a MOBILE portál fut, és lehetővé teszi, hogy gyűjthetők össze az új egyéni naplózási megadása.

1.  A MOBILE portálon válassza a **Beállítások**lehetőséget.
2.  Kattintson a **adatokat** , majd az **egyéni naplók**.
3.  Összes konfigurációs módosítást alapértelmezés szerint automatikusan tolni összes anyagokkal.  Konfigurációs fájl Linux anyagok, az Fluentd adatgyűjtő küldi.  Ha szeretné módosítani a fájlt kézzel minden Linux ügynök, majd törölje a jelet a *alkalmaz a Linux gépek konfigurációs alatti*mezőbe.
4.  Kattintson a **Hozzáadás +** nyissa meg az egyéni napló varázslót.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Lépés: 2. Töltse fel és egy mintanézet elemzése

Először egy mintája szerepel az egyéni napló feltöltéséről.  A varázsló elemezni, és a bejegyzések megjelenítése, ha ellenőrizni szeretné, a fájlban található.  Log Analytics az egyes rekordok azonosítása megadott határoló fogja használni.

**Új sor** az alapértelmezett elválasztó és soronként egy bejegyzés rendelkező naplófájlok szolgál.  Ha a sor egy dátumot és időpontot, az egyet a rendelkezésre álló formátumok kezdődik, amely támogatja a bejegyzéseket, amelyek egynél több sort átfogó **időbélyeg** elválasztó is megadhat. 

Időbélyeg elválasztó használata esetén kattintson az egyes rekordok MOBILE tárolt TimeGenerated tulajdonsága tölti fel a dátum/idő mezőt, a naplófájl megadott értékkel.  Ha új sort elválasztó használja, majd TimeGenerated kitölti dátuma és időpontja, hogy a napló Analytics a bejegyzés gyűjtött. 

>[AZURE.NOTE]Log Analytics jelenleg kezeli a dátum/idő időbélyeg elválasztó használata UTC naplózási gyűjtött.  Ez leggyorsabban változik az időzóna használni a Agent. 
 
1.  Kattintson a **Tallózás gombra** , és keresse meg a mintafájl.  Látható, hogy ez lehet gomb neve lehet **Válassza a fájl** bizonyos böngészőkben.
2.  Kattintson a **Tovább**gombra. 
3.  Az egyéni napló varázsló töltse fel a fájlt, és a listája, amely azonosítja a rekordokat.
4.  Az új rekord azonosítása, és válassza ki a legjobban azonosító a rekordok, a naplófájl elválasztó használt elválasztó módosítása.
5.  Kattintson a **Tovább**gombra.

### <a name="step-3-add-log-collection-paths"></a>3 a lépést. Log webhelycsoport elérési út hozzáadása

Meg kell adnia egy vagy több elérési út a Agent, ahol keresse meg az egyéni naplót.  Kétféleképpen lehet nyújtani egy adott elérési utat és a naplófájl neve, vagy egy helyettesítő karaktert a jelölőnégyzetét, adhatja meg az elérési.  Ez támogatja a naponta, vagy ha egy fájl elér egy bizonyos méretet, hozzon létre egy új fájlt alkalmazásokat.  Több út is biztosítanak egyetlen naplófájlt.

Például az alkalmazások előfordulhat, hogy a naplófájl létrehozásához naponta a dátuma, a neve, ahogy log20100316.txt tartalmazza. Lehet, hogy ilyen naplózási mintájának *log\*.txt* elnevezési séma adatait a amely volna alkalmazása bármely naplófájl alkalmazása után.

Az alábbi táblázat a különböző naplófájlok megadását érvényes minták példákat. 

| Leírás | Elérési út |
|:--|:--|
| *C:\Logs* lévő összes fájl a Windows-ügynök .txt kiterjesztésű | C:\Logs\\\*.txt |
| Naplókban és a Windows-ügynök .txt kiterjesztésű kezdődő nevű *C:\Logs* lévő összes fájl | C:\Logs\log\*.txt |
| Linux Agent */var/log/audit* .txt kiterjesztésű lévő összes fájl | /var/log/audit/*.txt |
| */Var/log/audit* naplókban és Linux Agent .txt kiterjesztésű kezdődő nevű lévő összes fájl | /var/log/audit/log\*.txt |
  

1.  Jelölje be a Windows vagy Linux megadhatja, melyik elérési út formátum hozzáadni.
2.  Írja be az elérési utat, és kattintson a **+** gombra.
3.  Ismételje meg a folyamat minden további elérési utak.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>Lépés: 4. Adja meg a nevét és leírását a napló

A megadott név használandó naplófájl típusa fent leírt módon.  Mindig a _CL megkülönböztetni a mint egyéni naplózási érjen véget.

1.  Írja be a napló nevét.  A ** \_törlése** utótag az automatikusan megjelenő.
2.  Tetszés szerint egy **leírást**.
3.  Kattintson a **Tovább gombra** az egyéni napló definition mentéséhez.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>5 a lépést. Az egyéni naplók begyűjtési ellenőrzése
Azt is eltarthat az eredeti adatok órává az új egyéni naplózási napló Analytics jelennek meg.  Bejegyzések gyűjtése a program ekkor elkezdi a naplókat, az elérési út található a megadott az, hogy az egyéni napló megadott ponttól.  Nem őrzik meg az egyéni napló létrehozása során feltöltött a bejegyzéseket, de azt összegyűjti a naplófájlok azt keresi, már meglévő elemeit.

Log Analytics elindul az egyéni napló gyűjti össze, miután rekordjai is elérhető lesz a napló keresést.  Használja, mint az egyéni naplófájl **típusa** a lekérdezés nevét.

>[AZURE.NOTE] Ha a RawData tulajdonság hiányzik a keresésből, előfordulhat, bezárja és újra megnyitja a böngészőben.

### <a name="step-6-parse-the-custom-log-entries"></a>6 a lépést. Az egyéni naplóbejegyzések elemzése

A teljes naplóbejegyzés az úgynevezett **RawData**egyetlen tulajdonság szeretne tárolni.  Valószínűleg kívánt választják el a különböző információkat vehet az egyes bejegyzések be a rekord tárolt egyes tulajdonságok.  A [Mezőnevek egyeztetése](log-analytics-custom-fields.md) az szolgáltatásával napló Analytics teheti meg.

Az egyéni naplóbejegyzést elemzése a lépések részletes itt nem találhatók.  Nézze meg ezt az információt az [Egyéni mezők](log-analytics-custom-fields.md) dokumentációját.

## <a name="disabling-a-custom-log"></a>Egyéni naplózási letiltása

Egyéni napló definícióját nem lehet eltávolítani, miután létrehozva van, de eltávolítja az összes a webhelycsoport javaslatok letiltásához.

1.  A MOBILE portálon válassza a **Beállítások**lehetőséget.
2.  Kattintson a **adatokat** , majd az **egyéni naplók**.
3.  Az egyéni napló definition letiltása mellett kattintson a **Részletek** gombra.
4.  Távolítsa el az egyes egyéni napló meghatározása webhelycsoport elérési útját.


## <a name="data-collection"></a>Adatok gyűjtése

Log Analytics összegyűjti az új bejegyzések minden egyéni naplófájlból körülbelül 5 percenként.  A agent minden naplófájlban, amely azt gyűjti össze a rögzíteni fogja elfoglalt helyét.  A agent időbeli offline állapotba kerül, ha majd napló Analytics fog gyűjt bejegyzések a, ahol az utolsó abbahagyta, még akkor is, ha azokat a tételeket létrehozott kapcsolat nélküli a agent állapotában.

A naplóbejegyzés teljes tartalmának **RawData**nevű egyetlen tulajdonság kerülnek.  Akkor is értelmezhető Ez több tulajdonságaik vannak, amelyek elemezni, és az egyéni napló létrehozása után [Egyéni mezők](log-analytics-custom-fields.md) megadásával külön-külön szeretne keresni.


## <a name="custom-log-record-properties"></a>Egyéni napló rekord tulajdonságai párbeszédpanelen

Egyéni naplóbejegyzések rendelkezik az alábbi táblázat a számukra nyújtott neve és a tulajdonságok egy típust.

| A tulajdonság | Leírás |
|:--|:--|
| TimeGenerated | Dátum és idő, amely a rekord gyűjtötte napló Analytics.  Ha a napló időpontokat elválasztó használ majd az a bejegyzés gyűjtött idő. |
| SourceSystem  | Írja be a rekord volt gyűjtött ügynök. <br> Csatlakozás OpsManager – Windows agent, vagy közvetlen vagy SCOM <br> Linux – az összes Linux ügynökök beállítása  |
| RawData             | Az összegyűjtött bejegyzés teljes szövegét. |
| ManagementGroupName | A kezelés csoport SCOM ügynökök nevét.  Más ügynökök, ez az AOI -\<munkaterület azonosító\> |


## <a name="log-searches-with-custom-log-records"></a>Az egyéni naplóbejegyzések napló keresések

Egyéni naplók rekordjaival a MOBILE tárat, mint bármely más adatforrásból származó rekordok tárolja.  A típus, a nevet, amelyet a naplóban, hogy használni tudja a típusa tulajdonság a keresésben adott naplózási gyűjtött rekordjával definiálása esetén adja meg a megfelelő rendelkeznek.

Az alábbi táblázat különböző példákat mutat be egyéni naplók rekordok beolvasó napló keresések.

| Lekérdezés | Leírás |
|:--|:--|
| Típus = MyApp_CL | Az egyéni összes eseményének elnevezett MyApp_CL jelentkezzen be. |
| Típus = MyApp_CL Severity_CF = hiba | A *hiba* az egyéni mező értékének MyApp_CL nevű egyéni naplózási összes eseményének nevű *Severity_CF*. |




## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Minta Útmutató: egyéni naplózási felvétele

Példa egyéni naplózási létrehozása az alábbiakban a következő szakaszt.  A minta napló gyűjtenek van egy bejegyzés soronként kezdési dátum és idő és majd vesszővel tagolt mezőinek kódot, az állapot és az üzenet.  Néhány példa bejegyzések alatt jelennek meg.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Töltse fel és egy mintanézet elemzése

Azt adja meg a naplófájlok közül, és láthatja a fog kell gyűjteni eseményeket.  Ebben az esetben az új sor elegendő elválasztó.  Ha egy bejegyzése a naplóban sikerült időtartamát többsoros bár időbélyeg elválasztó kellene használható.

![Töltse fel és egy mintanézet elemzése](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Log webhelycsoport elérési út hozzáadása

A naplófájlok *C:\MyApp\Logs*lesz található.  Új fájl létrejön a minta *appYYYYMMDD.log*dátumot tartalmazó néven naponta.  A napló elegendő mintájának lenne *C:\MyApp\Logs\\\*.log*.

![Napló webhelycsoport elérési útja](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Adja meg a nevét és leírását a napló

A **Leírás**használjuk *MyApp_CL* , és írja be a nevét.

![Bejelentkezési név](media/log-analytics-data-sources-custom-logs/log-name.png)


### <a name="validate-that-the-custom-logs-are-being-collected"></a>Az egyéni naplók begyűjtési ellenőrzése

Lekérdezés adott használjuk *típus = MyApp_CL* az összes rekordot ad vissza az összegyűjtött naplófájlból.

![Log lekérdezés nincs egyéni mezőkkel](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>Az egyéni naplóbejegyzések elemzése

Egyéni mezők használatával azt a *EventTime*, a *kód*, az *állapot*megadása, és a *üzenet* mezőket, és azt láthatja, a különbség a lekérdezés által visszaadott rekordokat.

![Log lekérdezést az egyéni mezőkkel](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Következő lépések

- [Egyéni mezők](log-analytics-custom-fields.md) használatával az egyéni napló elemeit értelmezhető egyes mezőket.
- [Log keresések](log-analytics-log-searches.md) megismerheti az adatforrások és megoldások gyűjtött adatok elemzése céljából. 