<properties 
    pageTitle="Hozzon létre és visszaállítása a BizTalk szolgáltatások biztonsági |} Microsoft Azure" 
    description="BizTalk szolgáltatások biztonsági mentési és visszaállítási tartalmazza. Megtudhatja, hogy miként hozhat létre és visszaállítása biztonsági másolatból, és állapítsa meg, mi kapja a biztonsági mentésben. MABS, WABS" 
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


# <a name="biztalk-services-backup-and-restore"></a>BizTalk szolgáltatások: Biztonsági mentése és visszaállítása

Azure BizTalk szolgáltatások biztonsági mentési és visszaállítási funkciókat tartalmazza. Ez a témakör ismerteti, hogyan biztonsági mentése és visszaállítása az Azure klasszikus portálon BizTalk szolgáltatások.

Biztonsági a [BizTalk szolgáltatások REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584)-BizTalk szolgáltatásokat is. 

> [AZURE.NOTE] Hibrid kapcsolatok nem készül, függetlenül a Edition. A hibrid kapcsolatokat újra létre kell hoznia.

## <a name="before-you-begin"></a>Mielőtt elkezdené

- Biztonsági mentési és visszaállítási nem feltétlenül vehető igénybe az összes verzió. Lásd: [BizTalk szolgáltatások: kiadásai diagram](biztalk-editions-feature-chart.md).

- Az Azure klasszikus portálon használja, az igény szerinti biztonsági másolat létrehozása vagy ütemezett biztonsági másolatot készíteni. 

- Biztonsági másolat tartalom ahonnan BizTalk ugyanezt vagy egy új BizTalk szolgáltatáshoz. Vissza szeretné állítani a BizTalk szolgáltatást használ, ugyanazt a nevet, törölni kell a meglévő BizTalk szolgáltatás, és név legyen elérhető. Miután BizTalk szolgáltatás törli, számára elérhetővé szeretné tenni az azonos nevű megy végbe, mint hosszabb időt vehet. Ha nem tudja várja meg szeretné elérhetővé tenni a meglévő nevet, majd visszaállíthatja az új BizTalk szolgáltatás.

- Az azonos edition vagy egy újabb kiadására BizTalk szolgáltatások állíthatók vissza. Visszaállítása BizTalk szolgáltatások alsó kiadásra, amikor a biztonsági mentés származik, a nem támogatott.

    Ha például prémium kiadásának a Basic Edition használata biztonsági feltölthet. A Standard Edition használata prémium kiadásának biztonsági nem állíthatók vissza.

- A vezérlő szerkesztése számok megőrzéséhez a vezérlő számok folytonosságot másolat készül. A legutóbbi biztonsági mentés után dolgozza fel az üzeneteket, ha ez a tartalom visszaállítása okozhatják ismétlődő vezérlő számokat.

- Ha egy köteg aktív üzeneteket, dolgozza fel a köteg **előtt** biztonsági másolat készítése. Biztonsági másolat létrehozása (mint szükséges vagy ütemezett), az üzenetek egy csoportba soha ne tárolja. 

    **Ha biztonsági egy köteg aktív leveleit, az alábbi üzenetek nem készített biztonsági másolatot, és ezért elvesznek.**

- Nem kötelező: A BizTalk Services portál leállítása kezelési műveletek.


## <a name="create-a-backup"></a>Biztonsági másolat létrehozása

A biztonsági másolatot bármikor lehet venni, és teljesen határozza meg. Ez a szakasz részletez létrehozása az Azure klasszikus portálon biztonsági másolatok együtt:

[A igény szerinti biztonsági mentése](#backupnow)

[A biztonsági mentés ütemezése](#backupschedule)

#### <a name="backupnow"></a>A igény szerinti biztonsági mentése
1. Az Azure klasszikus portálon, jelölje ki a **BizTalk szolgáltatások**, és válassza ki a kívánt BizTalk szolgáltatás biztonsági másolatot készíteni.
2. Az **Irányítópult** lapon jelölje ki a **biztonsági másolatot** az oldal alján.
3. Adja meg a biztonsági másolat nevét. Ha például írja be a *myBizTalkService*BU*dátum*.
4. Egy blob-tároló fiókot, és válassza a be van jelölve a biztonsági mentés indításához.

Ha a biztonsági mentés befejeződött, a tárterület-fiókot, adja meg a biztonsági másolat nevű tároló jön létre. Ez a tároló a BizTalk biztonsági konfigurációjának tartalmazza.

#### <a name="backupschedule"></a>A biztonsági mentés ütemezése

1. Az Azure klasszikus portálon jelölje ki a **BizTalk szolgáltatások**, jelölje ki a szeretne ütemezni a biztonsági mentés BizTalk szolgáltatás nevét, és válassza a **beállítás** fülre.
2. **Az automatikus** **biztonsági mentés állapota** állítsa. 
3. Jelölje ki a **Tárterület-fiók** tárolni a biztonsági másolatot, írja be a **gyakoriság** hozhat létre a biztonsági másolatok, és hogy mennyi ideig kell tartani a biztonsági másolatok (**Adatmegőrzési nap**):

    ![][AutomaticBU]

    **Jegyzetek**   
    - Az **Adatmegőrzési nap**az adatmegőrzési időszak biztonsági mentési gyakoriságának nagyobbnak kell lennie.
    - Válassza a **mindig legalább egy biztonsági másolatot**, esetén is lejárt az adatmegőrzési időszak.
    

4. Válassza a **Mentés**.


Ütemezett biztonságimásolat-feladat futtatásakor megadott tároló fiók létrehoz tároló (az adatok biztonsági másolatának tárolni). A tároló neve *nevet – a dátum / idő BizTalk szolgáltatás*neve. 

Ha a BizTalk szolgáltatás irányítópult állapotjelzője **sikertelen** :

![Utolsó ütemezett biztonsági állapota][BackupStatus] 

Hivatkozás a kezelési műveletet szolgáltatásnaplók hibaelhárítási nyitja meg. Lásd: [BizTalk szolgáltatások: a művelet naplók használata – problémamegoldás](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Visszaállítása

Biztonsági másolatok visszaállíthatja az Azure klasszikus portálról vagy a [Visszaállítás BizTalk szolgáltatás REST API -t](http://go.microsoft.com/fwlink/p/?LinkID=325582). Ez a szakasz lépéseit az klasszikus portálon visszaállítása sorolja fel.

#### <a name="before-restoring-a-backup"></a>Mielőtt visszaállítása biztonsági másolatból

- Új nyomon követése, archiválás és tárolók figyelése BizTalk szolgáltatás visszaállítása közben meg lehet adni.

- Egyező adatok szerkesztése futtatókörnyezet vissza. A biztonsági másolat szerkesztése futtatókörnyezet a vezérlő számokat tárolja. A visszaállított vezérlő számokat a biztonsági mentés óta sorrendben vannak. A legutóbbi biztonsági mentés után dolgozza fel az üzeneteket, ha ez a tartalom visszaállítása okozhatják ismétlődő vezérlő számokat.

#### <a name="restore-a-backup"></a>A biztonsági mentés visszaállítása

1. Az Azure klasszikus portálon válassza az **Új** > **Alkalmazás szolgáltatások** > **BizTalk szolgáltatás** > **visszaállítása**:

    ![A biztonsági mentés visszaállítása][Restore]

2. **Biztonsági URL-címet**jelölje be a mappa ikonra, és bontsa ki a Azure tároló fiókot, amely a BizTalk szolgáltatás konfigurálása biztonsági mentés tárolja. Bontsa ki az elemet, és a jobb oldali ablaktáblán jelölje be a megfelelő biztonsági másolat .txt fájlból. 
<br/><br/>
Kattintson a **Megnyitás**.

3. A **Visszaállítás BizTalk szolgáltatás** lapon írjon be egy **BizTalk szolgáltatás neve** , és ellenőrizze a a **Tartományi URL-cím**, **Edition**és **terület** a visszaállított BizTalk szolgáltatás. **Új SQL-adatbázis példány létrehozása** a nyomon követés adatbázis esetén:

    ![][RestoreBizTalkService]

    Jelölje be a következő nyílra.

4.  Ellenőrizze az SQL-adatbázis nevét, adja meg a kiszolgáló a fizikai hol hozható létre az SQL-adatbázis-kiszolgáló és felhasználónevével és jelszavával.


    Ha azt szeretné, az SQL-adatbázis edition konfigurálása, méretét és más tulajdonságait, válassza a **Speciális adatbázis-beállítások megadása**. 

    Jelölje be a következő nyílra.

5. Hozzon létre egy új tárterület-fiókot, vagy adjon meg egy meglévő tárterület-fiókot a BizTalk szolgáltatás.

7. Jelölje ki a jelet a visszaállítás indításához.

Ha a visszaállítás sikeresen befejeződött, új BizTalk szolgáltatás szerepel egy függesztve a BizTalk-szolgáltatások lapon az Azure klasszikus portálon.



### <a name="postrestore"></a>Miután visszaállítása biztonsági másolatból

A BizTalk szolgáltatás mindig visszaáll **felfüggesztett** állapotba kerül. Ezt az állapotot a konfigurációs módosításokat funkcionális, beleértve az új környezet előtt teheti meg:

- Ha létrehozott az Azure BizTalk szolgáltatások SDK használatával BizTalk szolgáltatásalkalmazások, ezek az alkalmazások használata a visszaállított környezet az Access vezérlő (ACS) hitelesítő adatok frissítése, előfordulhat, hogy kell.

- Visszaállíthatja az BizTalk szolgáltatás való replikáció egy meglévő BizTalk szolgáltatási környezetben. Ebben az esetben a rendelkezést az eredeti BizTalk Services portál konfigurálva, hogy egy adatforrás FTP-mappa használata esetén szüksége lehet frissíteni egy másik forrásból FTP-mappa használata az újonnan visszaállított környezetben a rendelkezést. Egyéb esetben lehet két különböző rendelkezést ugyanezt a hibaüzenetet csoportosítani kívánt.

- Ha szeretné, hogy több környezetek BizTalk visszaállította, ellenőrizze, jelölje ki a megfelelő környezet a Visual Studio alkalmazások, a PowerShell-parancsmagok, a REST API-hoz vagy a kereskedési Partner Management OM API-k.

- Célszerű az automatikus biztonsági mentések beállítása a az újonnan visszaállított BizTalk szolgáltatási környezetben.

Az Azure klasszikus portálon a BizTalk szolgáltatás elindításához jelölje ki a visszaállított BizTalk szolgáltatást, és válassza a **Önéletrajz** meg a tálcán. 



## <a name="what-gets-backed-up"></a>Mi a biztonsági másolat módja

Biztonsági másolat készül, a rendszer mentésben az alábbiakat:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>A biztonsági mentésben elemek</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Azure BizTalk Services portál</strong></td>
</tr> 
<tr>
<td>Konfigurációs és Runtime</td> 
<td>
<ul>
<li>Partner és a profil Részletek</li>
<li>Partner rendelkezést</li>
<li>Egyéni összeállítások rendszerbe</li>
<li>Hidakat rendszerbe</li>
<li>Tanúsítványok</li>
<li>Átalakítások rendszerbe</li>
<li>Folyamatok</li>
<li>Sablonok létrehozása és mentése a BizTalk Services portál</li>
<li>X12 ST01 és GS01 hozzárendelések</li>
<li>Vezérlőelem számokat (szerkesztése)</li>
<li>AS2 üzenet Mikrofon értékek</li>
</ul>
</td>
</tr> 
 
<tr>
<td colspan="2">
 <strong>Azure BizTalk szolgáltatás</strong></td>
</tr> 
<tr>
<td>Az SSL-tanúsítvány</td> 
<td>
<ul>
<li>SSL-tanúsítvány adatok</li>
<li>SSL-tanúsítvány jelszava</li>
</ul>
</td>
</tr> 
<tr>
<td>BizTalk szolgáltatás beállításai</td> 
<td>
<ul>
<li>Méretezés egység száma</li>
<li>Edition</li>
<li>A termék verziója</li>
<li>Régió/adatközponthoz</li>
<li>Access-vezérlő szolgáltatást (ACS) névtér és a billentyűk</li>
<li>A nyomon követés adatbázis kapcsolat-karakterlánc</li>
<li>Az archiválás tároló fiókkal kapcsolat-karakterlánc</li>
<li>Tárterület-fiók kapcsolati karakterlánc figyelése</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>További elemek</strong></td>
</tr> 
<tr>
<td>Adatbázis nyomon követése</td> 
<td>A BizTalk szolgáltatás létrehozásakor kerülnek a követés adatbázis adatait, beleértve az Azure SQL-adatbázis kiszolgálója és a követés adatbázis neve. A követés adatbázist nem automatikusan biztonsági másolat.
<br/><br/>
<strong>Fontos</strong><br/>
Ha a követés adatbázis törlődik, és az adatbázis igényeinek helyreállított, léteznie kell, az előző biztonsági másolatot. Ha a biztonsági másolatot nem létezik, a nyomon követése és az adatok nem lesznek helyreállítható. Ebben az esetben a követés új adatbázis létrehozása az azonos nevű adatbázist. A replikáció GEO ajánlott.</td>
</tr> 
</table>

## <a name="next"></a>Következő

Azure BizTalk Services létrehozása az Azure klasszikus portálon, használja a [BizTalk szolgáltatások: klasszikus portal segítségével Azure kiépítési](http://go.microsoft.com/fwlink/p/?LinkID=302280). Indítsa el az alkalmazások létrehozása, keresse fel az [Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=235197).

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

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png
 
