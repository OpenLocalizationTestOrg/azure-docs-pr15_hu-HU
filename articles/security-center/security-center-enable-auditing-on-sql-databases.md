<properties
   pageTitle="SQL-adatbázisait Azure biztonság otthon a naplózás engedélyezése |} Microsoft Azure"
   description="A dokumentum bemutatja, hogyan tudnak megvalósítani az Azure biztonság otthon ajánlást **SQL-adatbázisait naplózás engedélyezése**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-auditing-on-sql-databases-in-azure-security-center"></a>SQL-adatbázisait Azure biztonság otthon a naplózás engedélyezése

Azure biztonság otthon javasol, kapcsolja be a naplózás az SQL-adatbázisait, ha Képletvizsgálat még nincs engedélyezve. A naplózás segítséget nyújtanak a szabályozási megfelelőségről karbantartása, adatbázis tevékenység megértését és eltérések és üzleti kérdéseket jelezheti, vagy biztonsági megsértése gyanús rendellenességeinek bepillantást.

Miután bekapcsolta a naplózás veszélyt észlelési beállítások és az e-mailek biztonsági üzeneteket kapjanak beállíthatja. Veszélyt észlelési észleli az adatbázis potenciális biztonsági veszélyek jelző anomalous adatbázis-műveletek. Lehetővé teszi, hogy észleli és megválaszolása a potenciális veszélyek megjelenésükkor.

Az Azure SQL szolgáltatás csak; vonatkozik a javaslat a virtuális gépeken futó SQL nem tartalmazza.

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást.  Ez a cikk részletesen ismerteti nem.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. A **javaslatok** a lap válassza a **Naplózás engedélyezése SQL-adatbázisait**.  Ekkor megnyílik a **Naplózás engedélyezése SQL-adatbázisait** lap.
![SQL-adatbázisait naplózás engedélyezése][1]

2. Jelölje be a naplózás engedélyezése SQL-adatbázishoz. Ekkor megnyílik a **naplózás & veszélyt észlelése** lap.
![Naplózás és veszélyt kimutatására][2]
3. A **naplózás & veszélyt észlelése** lap jelölje be **a** **naplózás**csoportban.
![Naplózás és veszélyt szűrésének bekapcsolása][3]


5. Kövesse a az [első lépések az SQL-adatbázis veszélyt észlelési](../sql-database/sql-database-threat-detection-get-started.md) és veszélyt észlelési konfigurálása és konfigurálhatja az e-mailek, amely kapja a biztonsági figyelmeztetések észlelése anomalous tevékenységek listáját.

## <a name="see-also"></a>Lásd még:

Ez a cikk mutatott megvalósításáról a biztonság otthon ajánlási "SQL-adatbázisait naplózás engedélyezése". Többet szeretne tudni az SQL-adatbázis biztonságossá tétele, olvassa el az alábbiakat:

- [Az SQL-adatbázis biztonságossá tétele](../sql-database/sql-database-security.md)

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Kezelése biztonsági ajánlások az Azure biztonság otthon](security-center-recommendations.md) névre hallgat – ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Biztonsági állapot ellenőrzése Azure biztonság otthon](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Figyelés partnermegoldások Azure biztonsági központ](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – legújabb Azure biztonsági híreket és információkat.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]:./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection.png
[3]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
