<properties
   pageTitle="A Power BI Azure biztonság otthon adatok beolvasása a háttérismeretek |} Microsoft Azure"
   description="A tartalom Azure biztonsági központ Power BI-csomag egyszerűen keresse meg a biztonsági figyelmeztetések, javaslatok, erőforrások elleni és trendeket, a jelentéskészítéshez létrehozott adatkészletet alapján."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>A Power BI Azure biztonság otthon adatok mélyebb beolvasása
Az [Irányítópult a Power BI](http://aka.ms/azure-security-center-power-bi) Azure biztonság otthon lehetővé teszi, hogy a ábrázolása, elemezheti és szűrheti a javaslatok és a biztonsági figyelmeztetések bárhonnan, beleértve a mobileszközén. A Power BI-irányítópult használatával jelenítse meg a trendek és a minták - nézet biztonsági figyelmeztetések, erőforrás vagy a forrás IP-címét, és címzés nélküli biztonsági kockázatot, erőforrás vagy kor szemben. 

Akkor is is egyesítését biztonság otthon javaslatok és más adatok biztonsági figyelmeztetések érdekes módon, például az [Azure naplókat](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) és [Azure SQL-adatbázis naplózási](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/)adatainak használata. Mindkét ajánlja fel a Power BI-irányítópultok, és akkor exportálhatja is a ezeket az adatokat az Excelbe egyszerűen jelentéskészítés az a felhő erőforrások biztonsági állapotát.

##<a name="using-azure-security-center-dashboard-to-access-power-bi"></a>Azure biztonság otthon irányítópult használata a Power BI eléréséhez
Az Azure biztonsági központ irányítópult a Power BI szolgáltatásban eléréséhez is használhatja. A feladat végrehajtásához kövesse: 

1. Az **Azure biztonsági központ** Irányítópultjára kattintson **a Power BI a Tallózás** gombra.

    ![Csatlakozás használata a Power BI Azure biztonság otthon](./media/security-center-powerbi/security-center-powerbi-fig1-new10.png) 

2. A **Power bi Tallózás** lap megnyitása a jobb oldalon, az alábbi képen látható módon:

    ![Csatlakozás használata a Power BI Azure biztonság otthon](./media/security-center-powerbi/security-center-powerbi-fig1-new2.png)

3. Ha első alkalommal hoz létre a Power BI-irányítópult, az alábbi lehetőségek közül választhat a **Power bi Tallózás** lap: 

    - **Biztonsági háttérismeretek irányítópult**: válassza ezt a lehetőséget, ha azt szeretné, amely tartalmazza a biztonsági állapotát, szálak és észlelési irányítópult létrehozása. Ez a beállítás akkor DevOps szerepkör, amely a felelős a védelem állapotuk elemzése és értesítések észlelt előfizetésekben további közös.
    - **Adatkezelési házirend az irányítópult**: válassza ezt a lehetőséget, ha azt szeretné, kezelése és a végrehajtás házirend feltárása.  Ez a beállítás akkor egy központi informatikai ki a irányítási pontosabban vonatkozó további közös. Az Irányítópult segítségével szerezhet a láthatóság és biztonsági házirend betartása az összefüggéseket a szervezeten belül.
    - Ha már van egy Power BI-irányítópult, kattintson az **Ugrás az aktuális Power BI-irányítópult**parancsra.

4. Ebben a példában a **biztonsági háttérismeretek irányítópult** választógombot. Ha most először, rákérdez, hogy a tartalom csomag telepítéséhez biztonság otthon a Power BI irányítópult készít. Gombra **beszerzése** a **tartalom, a Power BI csomagok** ablakban az alábbi képen látható módon:

    ![Azure biztonsági központ biztonsági háttérismeretek irányítópult](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)

5. Megjelenik a **Csatlakozás az Azure biztonsági központ biztonsági háttérismeretek** ablak. Ellenőrizze, hogy a **hitelesítési** módszer **oauth2 hitelesítési mód** alább látható módon, és kattintson a **Bejelentkezés** gombra.
    
    ![Hitelesítés](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)

6. Kérheti az Azure hitelesítő adatokkal ismét hitelesítést végezni. Az irányítópult hitelesítését követően jön létre. Az irányítópult létrehozása után jelenik meg a hasonló szerkezettel jelentés az alábbi képen látható módon:

    ![A Power BI-irányítópult](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)


> [AZURE.NOTE] A jelentés frissítés helyébe naponta van ütemezve. Abban az esetben, ha a frissítés a hibát tapasztal, olvassa el a [Power BI Azure biztonsági központ frissítése potenciális problémák](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/)elhárítása További információt.

Itt láthatja a biztonsági figyelmeztetések és javaslatok számát, illetve VMs Azure SQL-adatbázisok és Azure biztonság otthon felügyeli hálózati erőforrások számát.

Azure biztonság otthon mutató hivatkozás átirányítja az Azure-portálra. A diagramok könnyítse ábrázolása biztonsági javaslatok és a riasztásokat, beleértve a információt:

- Erőforrás biztonsági állam
- Függőben lévő javaslatok
- Virtuális javaslatok
- Adott idő alatt értesítések
- Megtámadott erőforrások
- Megtámadott IP-címei

Minden diagram mögött van további az összefüggéseket. Jelölje ki a megfelelő csempéjére kattintva talál további információt. Például az **Erőforrás biztonsági állapot** csempére megtekintheti további részletek kapcsolatos javaslatok függőben lévő erőforrások által az alábbi képen látható módon:

![Javaslatok](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Ha bármelyik sorban a diagram gombra kattint, a mások tekinthessék meg a sötétszürke és összpontosíthat csak a kijelölt. Az irányítópult visszaállításához kattintson erre a lapra a bal oldali ablaktáblában a **irányítópultok** megadására **Azure biztonság otthon** .

> [AZURE.NOTE] Ha szeretné, hogy a jelentések testreszabása további mezők hozzáadásával vagy módosítani szeretné a meglévő vizuális, szerkesztheti a jelentést. Olvassa el [a Power bi Szerkesztőnézetben jelentés beavatkozás](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) további információt.

A **riasztások időbeli elleni erőforrások** és a **Támadó IP -címei** csempék lesz a hasonló eredményt ad, minden egyikére kattintva. Ennek oka az, hogy a jelentés e három változóval vonatkozó információk összesíti, és meghívja az **erőforrások homonimaszerű webcímmel csoportban** az alábbi képen látható módon:

![Erőforrások homonimaszerű webcímmel alapján](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

Ezen a ponton, is mentheti a jelentés egy példányát, nyomtassa ki vagy közzététele a weben elérhető a **fájl** menü beállítások használatával.

![Fájl menü](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>A Power BI-szolgáltatásokat Azure biztonság otthon adatokat felfedezése

A Power BI a [Power BI tartalom csomag szolgáltatások](https://msit.powerbi.com/groups/me/getdata/services) csatlakozzon, és hajtsa végre az alábbi lépéseket:

1. A **Power BI tartalom csomagjához** ablakban két lehetőség közül választhat alább látható módon jelenik meg.

    ![A Power BI tartalom csomag](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

    >[AZURE.NOTE] Ha ez a cikk első részét már végrehajtása az Azure biztonsági központ házirend-szolgáltatás csak az egyik beállítás jelenik meg.

2. Ebben a példában céljából **első** kattintson az **Azure biztonsági központ házirendkezelő** csempére.

3. A **Csatlakozás az Azure biztonsági központ házirendkezelő** ablakban győződjön meg arról, jelölje be az **oauth2 hitelesítési mód** **Hitelesítési módszer** legördülő listája, ahogy az alábbi, és kattintson a **Bejelentkezés** gombra.

    ![Házirend-kezelés ablaka](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)

4. Ha kézzel kell beírnia a hitelesítő adatok Azure biztonság otthon csatlakozhat használni kívánt hitelesítési lapra mutató irányítja. A hitelesítési folyamat befejezése után, a Power BI elkezdenek importálja az adatokat a jelentések készítése. Ez idő alatt a böngésző jobb felső sarkában kattintson a következő üzenet jelenhetnek meg:

    ![Csatlakozás használata a Power BI Azure biztonság otthon](./media/security-center-powerbi/security-center-powerbi-fig4.png)

    >[AZURE.NOTE] Ha először készül az irányítópulton tarthat szokottnál, főleg az esetek, amikor több előfizetéssel rendelkezik. 

5. Miután a folyamat befejeződik, az Azure biztonsági központ Power BI-irányítópult tölti be a **Csoportházirend kezelése** hasonlít az alább látható módon egy jelentéssel:

    ![Adatkezelési házirend az irányítópult](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Lásd még:
A jelen dokumentum használatáról a Power BI Azure biztonság otthon megtanulta azt. Többet szeretne tudni az Azure biztonság otthon, olvassa el az alábbiakat:

- [Azure biztonsági központ tervezése és a műveletek útmutató](security-center-planning-and-operations-guide.md) – megtudhatja, hogy miként Azure biztonság otthon elfogadása megtervezése.
- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – megtudhatja, hogy miként Azure biztonság otthon biztonsági beállításainak konfigurálása
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések megválaszolása
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés a szolgáltatással kapcsolatos gyakori kérdések
- [Azure biztonsági Blog](http://blogs.msdn.com/b/azuresecurity/) – Azure biztonság és megfelelőség kapcsolatos blogbejegyzéseket megkeresése
