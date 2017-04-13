<properties 
   pageTitle="Adatforrások a napló Analytics |} Microsoft Azure"
   description="Adatforrások adja meg az adatokat, hogy a napló Analytics tárolja a ügynökök és más forrásokból csatlakoztatva.  Ez a cikk ismerteti, amely a hogyan napló Analytics az adatforrást, ebből a cikkből megtudhatja, hogy hogyan konfigurálhatók a részletek és összefoglalja a különböző forrásokból érhető el."
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

# <a name="data-sources-in-log-analytics"></a>Adatforrások a napló Analytics

Log Analytics által gyűjtött adatokat a MOBILE munkaterület csatlakoztatott adatforrásból, és a MOBILE adattárban tárolja.  Minden egyes gyűjtött adatokat az adatforrások konfigurálható határozza meg.  A MOBILE adattárban adatokat rekordkészlet tárolja.  Minden egyes adatforrás minden típusú, hogy a saját tulajdonságok csoportja hoz létre egy bizonyos típusú rekordot.

![Jelentkezzen be a Analytics-adatgyűjtés](./media/log-analytics-data-sources/overview.png)

Adatforrások MOBILE megoldások is adatgyűjtés csatlakoztatott adatforrásból, és a rekordok létrehozása a MOBILE adattárban eltérnek.  Megoldások lehet hozzáadni a munkaterület a megoldástárból, és a szokásos nyújt további elemzőeszközök a MOBILE portálon.  

## <a name="summary-of-data-sources"></a>Adatforrások áttekintése

Az alábbi táblázat az adatforrások jelenleg elérhető napló Analytics jelennek meg.  Minden egyes tartalmaz egy külön cikk kezeléséről az adatforrás részletei mutató hivatkozást.

| Adatforrás | Esemény típusa | Leírás |
|:--|:--|:--|
| [Egyéni naplók](log-analytics-data-sources-custom-logs.md) | \<Naplónév\>_CL | A Windows vagy Linux rendszerhez ügynökök naplóadatok tartalmazó szövegfájlokból. |
| [Windows-eseménynaplók](log-analytics-data-sources-windows-events.md) | Esemény | Események Windows rendszerű számítógépeken gyűjtött eseménynaplójában talál. |
| [A Windows teljesítmény számláló](log-analytics-data-sources-performance-counters.md) | Perf | Teljesítmény számláló Windows számítógépről gyűjtött. |
| [Számláló Linux teljesítmény](log-analytics-data-sources-performance-counters.md) | Perf | Teljesítmény számláló Linux számítógépről gyűjtött. |
| [IIS naplók](log-analytics-data-sources-iis-logs.md) | W3CIISLog | Az Internet Information Services naplózza a W3C formátumban. |
| [Syslog](log-analytics-data-sources-syslog.md) | Syslog | Windows vagy Linux rendszerhez számítógépeken események Syslog. |

## <a name="configuring-data-sources"></a>Adatforrások konfigurálása

Az **adatok** menü származó adatforrások napló Analytics- **Beállítások**beállíthatja.  A MOBILE munkaterület összes csatlakoztatott adatforrásához érkeznek beállításokat.  Bármely ügynökök éppen ez a beállítás nem kizárása.

![Windows-események beállítása](./media/log-analytics-data-sources/configure-events.png)

2. A MOBILE konzolban válassza a **Beállítások** csempére.
3. Jelölje ki az **adatokat**.
4. Kattintson az adatforrás konfigurálása.
5. A hivatkozás követése dokumentációjában találhat további információt a fenti táblázat minden egyes adatforrás a konfiguráción.

## <a name="data-collection"></a>Adatok gyűjtése

Adatok forrása konfigurációk érkeznek ügynökök, amelyek közvetlenül csatlakoznak MOBILE néhány percen belül.  A megadott adatok az ügynök gyűjtött, és minden egyes adatforrás meghatározott időközönként közvetlenül napló Analytics kézbesítve.  Olvassa el a következő részletekért minden adatforráshoz dokumentációját.

System Center műveletek Manager (SCOM) ügynökök egy csatlakoztatott kezelése csoportjában, az adatok forrása konfigurációk management csomagok alakítja, és kézbesítve a felügyeleti csoportba 5 percenként alapértelmezés szerint.  A agent letölti a felügyeleti csomag, mint bármely más, és a megadott adatokat gyűjt. Attól függően, hogy az adatforrás adatok lesznek, akár a napló Analytics továbbítja az adatokat, amelyek management kiszolgálóra küldött, vagy a agent az adatokat küld napló Analytics keresztül a management server nélkül. Keresse meg a [webhelycsoport Adatrészletek MOBILE funkcióit és megoldások](log-analytics-add-solutions.md#data-collection-details-for-oms-features-and-solutions) további információt.  Olvashat a csatlakozás SCOM és a MOBILE és a gyakoriság módosítása, hogy a konfigurációs érkezik, a [Rendszer központ Operations Manager konfigurálása integrációja](log-analytics-om-agents.md).

## <a name="log-analytics-records"></a>Naplóbejegyzések Analytics

Az összes, a naplófájl Analytics által gyűjtött adatokat a MOBILE tárházba rekordok tárolja.  Különböző forrásokból által gyűjtött rekordok lesz a saját tulajdonságainak beállítása és a **típus** tulajdonságban azonosítania kell magát.  A dokumentációt, ahol minden adatforráshoz és a megoldás részletekért látható egyes rekordtípusokat.


## <a name="next-steps"></a>Következő lépések

- Tudjon meg többet a [megoldások](log-analytics-add-solutions.md) funkciók felvétele a napló Analytics és is adatgyűjtés történő MOBILE tárházba.
- [Log keresések](log-analytics-log-searches.md) megismerheti az adatforrások és megoldások gyűjtött adatok elemzése céljából.  
- Állítsa be a [riasztások](log-analytics-alerts.md) ezzel kapcsolatban beérkező értesítést küld az adatforrások és megoldások gyűjtött fontos adatokat.
