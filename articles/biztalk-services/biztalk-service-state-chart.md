<properties 
    pageTitle="Más államokból vagy BizTalk szolgáltatások állapotok engedélyezett feladatok |} Microsoft Azure" 
    description="A különböző MABS állapot engedélyezett műveletek/műveletek: leállítása, indítsa el, indítsa újra, felfüggesztheti, folytathatja, törlése, méretezni, frissítése konfigurálása és a biztonsági másolatot" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>



# <a name="biztalk-services-service-state-chart"></a>BizTalk szolgáltatások: Szolgáltatás állapota diagram
Attól függően, hogy a BizTalk szolgáltatás jelenlegi állapotának vannak, és nem tudja végrehajtani a BizTalk szolgáltatás műveletek.

Például az Azure klasszikus portál új BizTalk szolgáltatás kiépítése meg. Amikor befejeződik, a BizTalk szolgáltatás aktív állapotban van. Aktív állapotú akkor leállíthatja a BizTalk szolgáltatást. Sikeres leállítása esetén a BizTalk szolgáltatás leállt állapotba kerül. Nem sikerül a Leállítás, ha a BizTalk szolgáltatás StopFailed állapotba kerül. StopFailed állapotú indítsa újra a BizTalk szolgáltatást. Ha megpróbál egy művelet nem tartalmaz-e, például önéletrajz a BizTalk szolgáltatást, az alábbihoz hasonló hibaüzenet:

**A művelet nem engedélyezett**

BizTalk szolgáltatás kiépítése, használja a [BizTalk szolgáltatások: klasszikus portal segítségével Azure kiépítési](http://go.microsoft.com/fwlink/p/?LinkID=302280).

Az alábbi táblázatokban felsoroljuk a műveletek és a műveleteket, amelyeket tud végezni, amikor a BizTalk szolgáltatás egy adott állapotban van. Egy ✔, az azt jelenti, hogy állapotban a művelet engedélyezett. Egy üres bejegyzést azt jelenti, hogy a művelet nem hajtható végre, hogy állapotú.

## <a name="start-stop-restart-suspend-resume-and-delete-operations"></a>Indítsa el, leállítása, indítsa újra, felfüggesztheti, önéletrajz, és törlési műveletek
<table border="1">
<tr>
        <th colspan="15">A művelet vagy művelet</th>
</tr>

<tr>
        <th rowspan="18">BizTalk szolgáltatás állapota</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Indítása</th>
        <th>állj</th>
        <th>Indítsa újra a</th>
        <th>Felfüggesztése</th>
        <th>Önéletrajz</th>
        <th>Törlése</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktív</b></td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Letiltott</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Felfüggesztett</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Leállítva</b></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Nem sikerült Service frissítése</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
</table>
<br/>

## <a name="scale-update-configuration-and-backup-operations"></a>Méretezés, a frissítés konfigurálása és a biztonsági mentéssel kapcsolatos műveletek
<table border="1">
<tr>
        <th colspan="15">A művelet vagy művelet</th>
</tr>

<tr>
        <th rowspan="18">BizTalk szolgáltatás állapota</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Méretarány</th>
        <th>Konfiguráció frissítése</th>
        <th>Biztonsági másolat</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktív</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Letiltott</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Felfüggesztett</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Leállítva</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Nem sikerült Service frissítése</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
</tr>
</table>

## <a name="see-also"></a>Lásd még:
- [BizTalk szolgáltatások: Kiépítési használatával Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk szolgáltatások: Irányítópult, a Monitor és a méret lapon](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk szolgáltatások: Fejlesztőeszközök, Basic, normál és Premium kiadásában diagram](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk szolgáltatások: Biztonsági mentése és visszaállítása](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk szolgáltatások: szabályozásának](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalk szolgáltatások: Kibocsátó neve és a kibocsátó billentyűk](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [Hogyan tudom használatával indítsa el az Azure BizTalk szolgáltatások SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)


 
