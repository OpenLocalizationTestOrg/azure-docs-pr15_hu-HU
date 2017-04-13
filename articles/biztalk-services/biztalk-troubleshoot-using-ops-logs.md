<properties 
    pageTitle="Művelet naplók használata BizTalk szolgáltatások elhárítása |} Microsoft Azure" 
    description="A művelet naplók használata BizTalk szolgáltatások problémáinak elhárításához. MABS, WABS" 
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


# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk szolgáltatások: A művelet naplók használata – problémamegoldás

## <a name="what-are-the-operation-logs"></a>Mik azok a művelet naplókat
A művelet naplók az Azure klasszikus portálon, amely lehetővé teszi, hogy az Azure szolgáltatást, többek között a BizTalk szolgáltatások végrehajtott műveletek korábbi naplók megtekintése igénybe vehető szolgáltatások szolgáltatás. Ez lehetővé teszi, hogy a as far back as 180 nap BizTalk szolgáltatás előfizetés adatkezelési műveletek kapcsolódó korábbi adatok megtekintése.

> [AZURE.NOTE]Ez a funkció csak rögzíti a naplók management műveletekhez BizTalk szolgáltatások, például amikor a szolgáltatás elindult, biztonsági fel, és így tovább. Ezeket a műveleteket követik nyomon függetlenül, hogy végeznek az Azure klasszikus portálról, vagy a [BizTalk szolgáltatás REST API -khoz](http://msdn.microsoft.com/library/azure/dn232347.aspx)használatával. Műveletek nyomon követett szolgáltatások használatával teljes listáját című témakör tartalmaz [műveletek nyomon követett használatával Azure kezelése](#bizops).<br/><br/>
A rögzítés a naplókat, a tevékenységekhez kapcsolódó BizTalk szolgáltatás futtatókörnyezet (például hidakat, és így tovább által feldolgozott üzenet.) nem. Ezek a naplók megtekintéséhez a nyomon követés nézetben a BizTalk szolgáltatások portálon használja. További tudnivalókért lásd: az [Üzenetek nyomon követése](http://msdn.microsoft.com/library/azure/hh949805.aspx).

## <a name="view-biztalk-services-operation-logs"></a>A művelet szolgáltatásnaplók BizTalk megtekintése
1. Az Azure klasszikus portálon válassza a **Kezelés szolgáltatások**, és válassza az **Művelet naplók** lehetőséget.
2. A naplók előfizetés, dátumtartományt, szolgáltatás típusa (pl. BizTalk szolgáltatások), szolgáltatás neve és a művelet (sikerült, nem sikerült) állapotának különböző paraméterek alapján szűrhető.
3. Jelölje ki a jelet a szűrt listájának megtekintéséhez. Az alábbi képen látható testbiztalkservice kapcsolódó tevékenységek:  ![művelet naplók megtekintése][ViewLogs] 
4. Egy adott működésével kapcsolatos további megtekintése, jelölje ki a sort, és az alsó eszköztáron kattintson a **Részletek** gombra.


## <a name="bizops"></a>Műveletek nyomon Azure szolgáltatások használatával
A következő táblázat felsorolja a műveleteket, amelyek követik nyomon az Azure szolgáltatások használatával:

Művelet neve | Tevékenység
--- | ---
CreateBizTalkService | Hozzon létre egy új BizTalk művelet
DeleteBizTalkService | A művelet BizTalk szolgáltatás törlése
RestartBizTalkService | A művelet BizTalk szolgáltatás újraindítása
StartBizTalkService | A művelet BizTalk szolgáltatás indítása
StopBizTalkService | A művelet BizTalk szolgáltatás leállítása
DisableBizTalkService | A művelet BizTalk szolgáltatás letiltása
EnableBizTalkService | A művelet BizTalk szolgáltatás engedélyezése
BackupBizTalkService | Készítsen biztonsági másolatot BizTalk szolgáltatás művelet
RestoreBizTalkService | A művelet BizTalk szolgáltatás visszaállítása biztonsági másolatból megadott
SuspendBizTalkService | A művelet kell függeszteni BizTalk szolgáltatás
ResumeBizTalkService | A művelet folytatásához BizTalk szolgáltatás
ScaleBizTalkService | Ha át kívánja méretezni BizTalk szolgáltatás, felfelé vagy lefelé a művelet
ConfigUpdateBizTalkService | A művelet a beállítások egy BizTalk szolgáltatás frissítése
ServiceUpdateBizTalkService | Művelet frissítése vagy egy másik verzióját BizTalk szolgáltatás vissza léptetheti
PurgeBackupBizTalkService | A művelet a BizTalk szolgáltatás kívül az adatmegőrzési időszak biztonsági másolatok törlése


## <a name="see-also"></a>Lásd még:
- [Biztonsági másolat BizTalk szolgáltatás](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [BizTalk szolgáltatás visszaállítása biztonsági másolatból](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [BizTalk szolgáltatások: Fejlesztőeszközök, Basic, normál és Premium kiadásában diagram](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalk szolgáltatások: Kiépítési használatával Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [BizTalk szolgáltatások: Kiépítési állapot diagram](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk szolgáltatások: Irányítópult, a Monitor és a méret lapon](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [BizTalk szolgáltatások: szabályozásának](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalk szolgáltatások: Kibocsátó neve és a kibocsátó billentyűk](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Hogyan tudom használatával indítsa el az Azure BizTalk szolgáltatások SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png
 
