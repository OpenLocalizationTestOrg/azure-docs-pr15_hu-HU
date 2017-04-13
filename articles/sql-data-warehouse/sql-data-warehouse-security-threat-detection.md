<properties
   pageTitle="Első lépések az SQL adatok raktári veszélyt kimutatására"
   description="Hogyan veszélyt észlelése a használata"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="lodipalm;sonyama;barbkess"/>


# <a name="get-started-with-threat-detection"></a>Első lépések a kockázatot kimutatására

> [AZURE.SELECTOR]
- [A naplózás](sql-data-warehouse-auditing-overview.md)
- [Veszélyt kimutatására](sql-data-warehouse-security-threat-detection.md)

## <a name="overview"></a>– Áttekintés

Veszélyt észlelési észleli az adatbázis potenciális biztonsági veszélyek jelző anomalous adatbázis-műveletek. Veszélyt észlelési előzetes verzióban, és SQL adatraktár támogatott.

Veszélyt észlelési biztonsági, amely lehetővé teszi az ügyfelek számára észleli és megválaszolása a potenciális veszélyek, mert a biztonsági figyelmeztetések anomalous tevékenységéről megjelenésükkor új réteget biztosít. Felhasználói felfedezheti az [Azure SQL adatok raktári naplózás](sql-data-warehouse-auditing-overview.md) használata határozza meg, ha eredményeként eléréséhez, megsértése vagy kihasználhatja a adatraktár adatainak kísérlet gyanús eseményeket.
Veszélyt észlelési teszi egyszerű potenciális veszélyek címet a adatraktár szakértői értékpapír vagy rendszerek figyelése Speciális biztonságának kezelési nélkül.

Például veszélyt észlelési észleli az egyes potenciális SQL-utasítások beszúrását kísérletek jelző anomalous adatbázis tevékenységek. SQL-beszúrás egyike, a közös webes alkalmazás biztonsági problémákról az interneten, hogy adatalapú alkalmazást használja. A támadók alkalmazás biztonsági rosszindulatú SQL-utasítások mezőkbe alkalmazás bejegyzést, megsértése vagy módosítása az adatbázis adatai a beillesztendő előnyeit.


## <a name="set-up-threat-detection-for-your-database"></a>Az adatbázis-veszély észlelés beállítása

1. Indítsa el az Azure-portált a [https://portal.azure.com](https://portal.azure.com).

2. Nyissa meg a konfigurációs lap, a figyelni kívánt SQL-adatraktár. Válassza a beállítások lap **naplózás & veszélyt észlelési**.

    ![Navigációs ablak][1]

3. A **naplózás & veszélyt észlelési** konfigurációs lap kapcsolja **be** a naplózás a kockázatot észlelési beállításokat megjelenítő.

    ![Navigációs ablak][2]

4. Kapcsolja **a** kockázatot észlelési.

5. Állítsa be az e-mailek, amely kapja a biztonsági figyelmeztetések észlelése anomalous adatok raktári tevékenységek listáját.

6. A **naplózás & veszélyt észlelési** konfigurációs lap az új vagy frissített Képletvizsgálat és észlelési házirend veszélyt kattintson a **Mentés** gombra.

    ![Navigációs ablak][3]


## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Anomalous raktári a tevékenységekre vonatkozó adatok a gyanús esemény észlelése feltárása

1. Anomalous adatbázis tevékenységek észlelése e-mailben értesítést kap. <br/>
Az e-mailt a gyanús biztonsági esemény, beleértve a anomalous tevékenységek, az adatbázis neve, kiszolgáló neve és a időpontnál jellegét információt nyújt. Emellett a lehetséges okok információt nyújt, és javasolt műveleteket vizsgálja meg, és az adatbázis a potenciális veszélyt csökkentésében.<br/>

    ![Navigációs ablak][4]

2. Az e-mailben kattintson az **Azure SQL-Képletvizsgálat naplója** hivatkozásra, amely indítsa el az Azure klasszikus portál és a megfelelő naplózás rekordokat körül a gyanús esemény időtartamát.

    ![Navigációs ablak][5]

3. Kattintson a naplózás rekordok további részleteket a gyanús adatbázis-műveletek, például SQL-utasítást, a hiba okát és az ügyfél IP.

    ![Navigációs ablak][6]

4. A rekordok naplózási lap kattintson **az Excelben nyitja meg** megnyitni egy előre konfigurált az excel-sablon importálása, és futtassa a napló gyanús esemény időpontja körül mélyebb elemzéséhez.<br/>
**Megjegyzés:** Az Excel 2010-es vagy újabb a Power Query és a **Gyors összevonás** beállítás szükség

    ![Navigációs ablak][7]

5. Konfigurálja a **Gyors összevonás** beállítás – a **POWER QUERY** Menüszalaglapon, jelölje be a **beállításokat** a beállítások párbeszédpanel megjelenítéséhez. Jelölje ki a adatvédelmi szakaszt, és válassza ki a második lehetőség – figyelmen kívül hagyja a az adatvédelmi szintek és esetleg a teljesítmény javítása:

    ![Navigációs ablak][8]

6. SQL naplókat betöltéséhez, győződjön meg arról, hogy a Paraméterek lap helyesen van beállítva, és válassza a "Adatok" menüszalag, és kattintson a "Az összes frissítése" gombra a beállítások.

    ![Navigációs ablak][9]

7. Az eredmények jelennek meg, amely lehetővé teszi, hogy futtatni talált anomalous tevékenységek mélyreható elemzést **SQL naplókat** tartalmazó lapon, és az alkalmazás a biztonsági esemény hatása csökkentésében.


<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
