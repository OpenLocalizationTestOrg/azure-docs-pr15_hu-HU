<properties 
    pageTitle="Értékáram-elemzés: Elforgatása a bejelentkezési hitelesítő adatok ráfordítások és a kimeneti értékeket |} Microsoft Azure" 
    description="Megtudhatja, hogy miként frissítse a hitelesítő adatait megjelenítő Értékáram-elemzés ráfordítások és a kimeneti értékeket."
    keywords="a bejelentkezési adatok"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>A ráfordítások és adatfolyam Analytics feladatok a kimeneti értékeket a bejelentkezési adatok elforgatása

##<a name="abstract"></a>Absztrakt
Azure Értékáram-elemzés ma nem támogatja a hitelesítő adatokat, a bemeneti és kimeneti cseréjével kapcsolatban, a feladat futtatása közben.

Azure Értékáram-elemzés nem támogatja a feladat utolsó eredményéből folytatása, miközben azt megy végbe az eltérés között a megállás minimalizálása és a projekt kezdési és a bejelentkezési adatok elforgatása a teljes folyamat megosztása.

##<a name="part-1---prepare-the-new-set-of-credentials"></a>1 rész - előkészítése az újonnan létrehozott hitelesítő adatok:
Ebben a részben megfelel az alábbi bemenetben/kimeneti értékeket:

* BLOB-tárolóhoz
* Esemény hubok
* SQL-adatbázis
* Táblatároló

Az egyéb bemeneti adatok alapján/kimeneti értékeket folytassa a kijelző 2.

###<a name="blob-storagetable-storage"></a>BLOB-tároló/táblatároló
1.  Nyissa meg a tárterület-kiterjesztés az Azure adatkezelési portálon:  
![graphic1][graphic1]
2.  Keresse meg a projekt által használt tárolására és mappába érkeznek meg:  
![graphic2][graphic2]
3.  Kattintson a hívóbetűk kezelése parancsra:  
![graphic3][graphic3]
4.  Az Access elsődleges kulcs és között a másodlagos hívóbetű, **Válasszon a lehetőségek, a feladatok nem használja**.
5.  Találati újragenerálása:  
![graphic4][graphic4]
6.  Másolja az újonnan létrehozott billentyűt.  
![graphic5][graphic5]
7.  Folytassa a 2.

###<a name="event-hubs"></a>Esemény hubok
1.  Nyissa meg a szolgáltatás Bus bővítmény az Azure adatkezelési portálon:  
![graphic6][graphic6]
2.  Keresse meg a szolgáltatás Bus Namespace a projekt által használt, és lépjen be:  
![graphic7][graphic7]
3.  Ha a feladat egy megosztott hozzáférési házirend használ, kattintson a szolgáltatás Bus Namespace, Ugrás a 6 lépés  
4.  Nyissa meg az esemény hubok lapon:  
![graphic8][graphic8]
5.  Keresse meg az esemény-központban, a feladatok által használt, és lépjen be:  
![graphic9][graphic9]
6.  Nyissa meg a beállítás lapon:  
![graphic10][graphic10]
7.  A házirend neve legördülő listában keresse meg a megosztott hozzáférési házirend a projekt által használt:  
![graphic11][graphic11]
8.  Az elsődleges kulcs és között a másodlagos billentyűt, és **Válasszon a lehetőségek, a feladatok nem használja**.  
9.  Találati újragenerálása:  
![graphic12][graphic12]
10. Másolja az újonnan létrehozott billentyűt.  
![graphic13][graphic13]
11. Folytassa a 2.  

###<a name="sql-database"></a>SQL-adatbázis

>[AZURE.NOTE] Megjegyzés: szüksége lesz az SQL-adatbázis szolgáltatás csatlakozni. Ennek módjáról az adatkezelési folyamatok az Azure adatkezelési portál használatával megjelenítése fogjuk, de néhány ügyféloldali eszközzel például SQL Server Management Studio is választhatja.

1.  Nyissa meg az SQL-adatbázisait bővítmény az Azure adatkezelési portálon:  
![graphic14][graphic14]
2.  Keresse meg a ugyanabban a sorban a feladatot, és **kattintson a kiszolgálón** kapcsolat által használt SQL-adatbázishoz:  
![graphic15][graphic15]
3.  Kattintson a kezelés parancsot:  
![graphic16][graphic16]
4.  Írja be a fő adatbázis:  
![graphic17][graphic17]
5.  Írja be a felhasználónevét és jelszavát, és kattintson a Log:  
![graphic18][graphic18]
6.  Kattintson az új lekérdezést:  
![graphic19][graphic19]
7.  Írja be az alábbi lekérdezés < login_name > helyettesítve a felhasználónevét és a csere <enterStrongPasswordHere> az új jelszavával:  
`CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8.  Kattintson a Futtatás gombra:  
![graphic20][graphic20]
9.  Térjen vissza lépés 2, és ez idő kattintson az adatbázis:  
![graphic21][graphic21]
10. Kattintson a kezelés parancsot:  
![graphic22][graphic22]
11. Írja be a felhasználónevét és jelszavát, és kattintson a Bejelentkezés gombra:  
![graphic23][graphic23]
12. Kattintson az új lekérdezést:  
![graphic24][graphic24]
13. Írja be az alábbi lekérdezés < felhasználónév > helyettesítve azonosítsa a bejelentkezési ebben az adatbázisban (az azonos értékkel < login_name >, például adott lehet nyújtani) környezetében kívánt nevet, és az új felhasználó nevét helyettesítve a < login_name >:  
`CREATE USER <user_name> FROM LOGIN <login_name>`
14. Kattintson a Futtatás gombra:  
![graphic25][graphic25]
15. Kell megadnia most az új felhasználó azonos szerepkörök és jogosultságok az eredeti felhasználónál meg volt.
16. Folytassa a 2.

##<a name="part-2-stopping-the-stream-analytics-job"></a>2 rész: A adatfolyam Analytics feldolgozás leállítása
1.  Nyissa meg az adatfolyam Analytics-bővítmény a Azure adatkezelési portálon:  
![graphic26][graphic26]
2.  Keresse meg a feladatot, és lépjen be:  
![graphic27][graphic27]
3.  Nyissa meg a bemeneti adatok alapján vagy a kimeneti értékeket lapon e vannak elforgatása a bemenetként, vagy olyan eredménye a hitelesítő adatok alapján.  
![graphic28][graphic28]
4.  Kattintson a Leállítás parancsot, és erősítse meg, a feladat leállt:  
![graphic29][graphic29] megvárja, amíg a feladat leállítása.
5.  Keresse meg a bemeneti és kimeneti hitelesítő adatok elforgatása a és a mappába érkeznek meg:  
![graphic30][graphic30]
6.  Folytassa a 3.

##<a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Rész 3: A adatfolyam Analytics projektre vonatkozóan a hitelesítő adatok szerkesztése

###<a name="blob-storagetable-storage"></a>BLOB-tároló/táblatároló
1.  Keresse meg a tárhely Fiókkulcs mezőbe, és az újonnan létrehozott kulcs beillesztése:  
![graphic31][graphic31]
2.  Kattintson a Mentés parancsra, és erősítse meg, ha a módosítások mentése:  
![graphic32][graphic32]
3.  A kapcsolat tesztelése automatikusan elindul, amikor menti a módosításokat, győződjön meg arról, azaz sikeresen ment.
4.  Folytassa a 4.

###<a name="event-hubs"></a>Esemény hubok
1.  Keresse meg az esemény központi házirend kulcs mezőjét, és az újonnan létrehozott kulcs beillesztése:  
![graphic33][graphic33]
2.  Kattintson a Mentés parancsra, és erősítse meg, ha a módosítások mentése:  
![graphic34][graphic34]
3.  A kapcsolat tesztelése automatikusan elindul, amikor menti a módosításokat, győződjön meg arról, hogy azt sikeresen ment.
4.  Folytassa a 4.

###<a name="power-bi"></a>A Power BI
1.  Kattintson a megújítás engedélyezése:  
* ![graphic35][graphic35]
* A következő megerősítés jelenik meg:  
* ![graphic36][graphic36]
2.  Kattintson a Mentés parancsra, és erősítse meg, ha a módosítások mentése:  
![graphic37][graphic37]
3.  A kapcsolat tesztelése automatikusan elindul, amikor a módosítások mentéséhez, győződjön meg arról, hogy azt sikeresen ment.
4.  Folytassa a 4.

###<a name="sql-database"></a>SQL-adatbázis
1.  Keresse meg a felhasználónevét és jelszavát a mezőkben, és az újonnan létrehozott beállítása a hitelesítő adatok beillesztése:  
![graphic38][graphic38]
2.  Kattintson a Mentés parancsra, és erősítse meg, ha a módosítások mentése:  
![graphic39][graphic39]
3.  A kapcsolat tesztelése automatikusan elindul, amikor menti a módosításokat, győződjön meg arról, hogy azt sikeresen ment.  
4.  Folytassa a 4.

##<a name="part-4-starting-your-job-from-last-stopped-time"></a>4 rész: A feladatok kezdve leállítva legutóbbi
1.  Nyissa meg a bemeneti távolabb:  
![graphic40][graphic40]
2.  Kattintson a Start parancsot:  
![graphic41][graphic41]
3.  Válassza ki a legutóbbi leállt idejét, és kattintson az OK gombra:  
 ![graphic42][graphic42]
4.  Folytassa az 5.  

##<a name="part-5-removing-the-old-set-of-credentials"></a>5 rész: A régi készlete hitelesítő adatok eltávolítása
Ebben a részben megfelel az alábbi bemenetben/kimeneti értékeket:
* BLOB-tárolóhoz
* Esemény hubok
* SQL-adatbázis
* Táblatároló

###<a name="blob-storagetable-storage"></a>BLOB-tároló/táblatároló
Ismételje meg a korábban használt által a feladatot a most még nem használt hívóbetű megújítása hívóbetű rész 1.

###<a name="event-hubs"></a>Esemény hubok
Ismételje meg a kulcs, amely a korábban használt által a feladatot a most még nem használt kulcs megújításához rész 1.

###<a name="sql-database"></a>SQL-adatbázis
1.  A kijelző 1 lépés 7, és írja be az alábbi lekérdezés < previous_login_name > helyettesítve a felhasználó nevét a projekt által korábban használt térjen vissza a lekérdezés ablak:  
`DROP LOGIN <previous_login_name>`  
2.  Kattintson a Futtatás gombra:  
    ![graphic43][graphic43]  

A következő megerősítés kapja: 

    Command(s) completed successfully.

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png
 
