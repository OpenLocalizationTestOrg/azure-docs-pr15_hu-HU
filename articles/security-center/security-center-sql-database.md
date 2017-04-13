<properties
   pageTitle="Azure biztonság otthon és Azure SQL-adatbázis szolgáltatás |} Microsoft Azure"
   description="Ebből a cikkből megtudhatja, hogyan biztonság otthon segíthetnek Azure SQL-adatbázisban az adatbázisok biztonságossá."
   services="sql-database"
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
   ms.date="10/18/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure biztonság otthon és Azure SQL-adatbázis szolgáltatás

[Azure biztonság otthon](https://azure.microsoft.com/documentation/services/security-center/) segítségével megakadályozhatja, hogy észleli és veszélyek válaszolni. Integrált adatvédelem figyelése és a Csoportházirend kezelése az Azure-előfizetésekben biztosít, segít észleli, egyéb esetben lépjen észrevétlen veszélyek és dolgozik egy széles ökológiai biztonsági megoldásokkal együtt.

Ebből a cikkből megtudhatja, hogyan biztonság otthon segíthetnek Azure SQL-adatbázisban az adatbázisok biztonságossá.

## <a name="why-use-security-center"></a>Miért érdemes használni a biztonság otthon?

Biztonság otthon segít SQL-adatbázisban tárolt adatok védelme betekintést kap abba, hogy a kiszolgáló és a adatbázisok biztonsága megadásával. A biztonság otthon a következőkre van lehetősége:

- SQL-adatbázis titkosítást és naplózási házirendek megadása
- SQL-adatbázis-erőforrások biztonsága figyelheti a előfizetésekben.
- Egyszerűen azonosíthatók, és ismételt a biztonsági problémákról.
- Integrálása az [Azure SQL-adatbázis veszélyt észlelési](../sql-database/sql-database-threat-detection-get-started.md)érkező riasztások.

Az SQL-adatbázis-erőforrások védelmének elősegítése, nemcsak a biztonság otthon emellett biztonsági figyelő és felügyeleti Azure virtuális gépeken futó, a Cloud Services, a alkalmazás szolgáltatások, a virtuális hálózatok és sok másra. További tudnivalók a biztonság otthon [Itt](security-center-intro.md).

## <a name="prerequisites"></a>Előfeltételek

Első lépések a biztonság otthon, a Microsoft Azure-előfizetést kell rendelkeznie. Biztonság otthon: a szabad réteg engedélyezve van-előfizetéséhez. Biztonság otthon szabad és normál rétegek további tudnivalókért olvassa el a [Biztonsági központ árak](https://azure.microsoft.com/pricing/details/security-center/)című témakört.

Biztonság otthon szerepköralapú hozzáférés-támogatja. Szerepköralapú hozzáférés-szerepalapú Azure-ban kapcsolatos további információért olvassa el az [Azure Active Directory szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)című témakört. A biztonsági központja – gyakori kérdések a [Biztonság otthon az engedélyek kezelésének](security-center-faq.md#how-are-permissions-handled-in-azure-security-center)adatait találja.

## <a name="access-security-center"></a>Az Access biztonsági központ

Biztonság otthon az [Azure portál](https://azure.microsoft.com/features/azure-portal/)érheti el. [Jelentkezzen be a portálra](https://portal.azure.com/) , és válassza a **Biztonság otthon lehetőséget**.

![Biztonság otthon beállítás][1]

A **Biztonság otthon** lap megnyitása
![Biztonság otthon lap][2]

## <a name="set-security-policy"></a>Biztonsági szabály beállítása

Biztonsági házirendek meghatároz a vezérlők, amelyek a megadott előfizetés vagy erőforráscsoport erőforrások ajánlott. Biztonság otthon, az előfizetések vagy az erőforrás-csoportok szerint a vállalat igényeinek, és milyen típusú alkalmazásokat vagy védelmi szintjének az adatokat az egyes előfizetések házirendek definiálhatók.

Beállíthatja, hogy egy házirendet, amely SQL Képletvizsgálat és az SQL átlátszó adatok (TDE) javaslatok megjelenítése.

- **SQL-Képletvizsgálat és veszélyt észlelési**bekapcsolásakor a biztonság otthon megfelelőségi, speciális észlelési és a vizsgálat érdekében, hogy az Azure-adatbázis eléréséhez naplózás engedélyezve javasolja.
- Ha bekapcsolja **SQL-átlátszó adatok titkosítást**, biztonság otthon ajánlja, hogy a többi titkosítási az Azure SQL-adatbázissal, tartozó másolatok és tranzakció naplófájlok engedélyezni.

Biztonsági házirendek, válassza a **házirend** csempe a biztonság otthon lap. Jelölje be az előfizetést, amelyen a biztonsági házirend engedélyezni szeretné a **biztonsági házirendje** lap. Jelölje be **a házirend-megelőzés** , és kapcsolja **be** az előfizetés a használni kívánt biztonsági beállítására vonatkozó javaslatok.
![Eszközbiztonsági házirend beállítása][3]

További tudnivalókért olvassa el a [biztonsági házirendek beállítása](security-center-policies.md)című témakört.

## <a name="manage-security-recommendation"></a>Biztonsági ajánlási kezelése

Biztonság otthon rendszeres elemzi a biztonsági állapot Azure erőforrásait. Ha a biztonság otthon azonosítja a potenciális biztonsági rés, javaslatok hoz létre. A javaslatok végigvezeti Önt a folyamaton, konfigurálása a szükséges vezérlőelemeket.

Biztonsági házirendek beállítása után a biztonság otthon elemzi a biztonsági állapotát az erőforrások azonosítása potenciális biztonsági. Táblázatos formátumban, ahol minden sora egy adott ajánlási felel meg a javaslatok jelennek meg. Referenciaként az alábbi táblázat segítségével megismerheti az Azure SQL-adatbázissal, és az egyes ajánlási leírása alkalmazhat, ha a rendelkezésre álló javaslatok. Kijelölés ajánlást megnyitja egy cikk, amely az ajánlási megvalósítását a biztonság otthon ismerteti.

| Javaslat | Leírás |
| ----- | ----- |
| [Naplózás és veszélyt észlelési SQL-kiszolgálókon engedélyezése](security-center-enable-auditing-on-sql-servers.md) | Javasolja, kapcsolja be a naplózás és veszélyt észlelése az SQL-adatbázis-kiszolgálók. (Csak az SQL-adatbázis szolgáltatás. Nem tartalmazza a virtuális gépeken futó Microsoft SQL Server.) |
| [Naplózás és veszélyt észlelése a SQL-adatbázisait engedélyezése](security-center-enable-auditing-on-sql-databases.md) | Azt az SQL-adatbázis adatbázisok naplózási és veszélyt szűrésének bekapcsolása javasolja. (Csak az SQL-adatbázis szolgáltatás. Nem tartalmazza a virtuális gépeken futó Microsoft SQL Server.) |
| [Áttetsző titkosítás engedélyezéséhez](security-center-enable-transparent-data-encryption.md) | SQL-adatbázisok titkosítása engedélyezését javasolja. (SQL-adatbázis-szolgáltatás csak.) |

Az Azure erőforrások javaslatok megjelenítéséhez jelölje be a **javaslatok** csempe a biztonság otthon lap. Válassza a **javaslatok** a lap, részletek ajánlást. Ebben a példában jelöljük ki **a naplózás engedélyezése és veszélyt észlelési SQL-kiszolgálókon**.

![Javaslatok][4]

Amint alább látható, a biztonság otthon megtudhatja, ahol naplózási és veszélyt észlelése nem engedélyezett SQL-kiszolgálók. A naplózás bekapcsolása után veszélyt észlelési és beállítások e-mailek biztonsági üzeneteket kapjanak beállíthatja. Veszélyt észlelési figyelmezteti, ha azt észleli, hogy az adatbázis potenciális biztonsági veszélyek jelző anomalous adatbázis-műveletek. Figyelmeztetések a biztonsági központ irányítópult jelenik meg.
![Naplózás és veszélyt kimutatására][5]

Kövesse a lépéseket az [első lépések az SQL-adatbázis veszélyt észlelési](../sql-database/sql-database-threat-detection-get-started.md) és veszélyt észlelési konfigurálása és konfigurálhatja az e-mailek, amely kapja a biztonsági figyelmeztetések észlelése anomalous tevékenységek listáját.

Javaslatok kapcsolatos további információért olvassa el a [kezelése biztonsági javaslatok](security-center-recommendations.md)című témakört.

## <a name="monitor-security-health"></a>Monitor biztonsági állapota

Miután engedélyezte a [biztonsági házirendek](security-center-policies.md) egy előfizetés erőforrások, a biztonság otthon fog elemezheti az erőforrások azonosítása potenciális biztonsági biztonsága.  Az erőforrások biztonsági állapotának megtekintése az **erőforrás biztonsági állapot** csempére. Ha **adatok** **erőforrás biztonsági állapot** csempéjén gombra kattint, az **Adatok erőforrások** lap megnyílik, és problémák, például a nem engedélyezett naplózási és áttetsző adatok titkosítási javaslatok SQL-e. Az adatbázis általános állapotának javaslatok is rendelkezik.
![Erőforrás biztonsági állapota][6]

További tudnivalókért olvassa el a [biztonsági állapot ellenőrzése](security-center-monitoring.md)című témakört.

## <a name="manage-and-respond-to-security-alerts"></a>Kezelése, és a biztonsági figyelmeztetések megválaszolása

Biztonság otthon automatikusan összegyűjti, elemzi és egyesíti a naplóadatokat származó [Azure SQL-veszély észlelése](../sql-database/sql-database-threat-detection-get-started.md), valamint egyéb Azure erőforrások valós veszélyek észleli és téves csökkentése. Együtt az adatokat kell gyorsan kivizsgáljuk a probléma és javaslatok, hogy hogyan támadások ismételt biztonság otthon elsőbbségi biztonsági figyelmeztetések listája látható.

Értesítések megjelenítéséhez jelölje be a **biztonsági figyelmeztetések** csempe a biztonság otthon lap. Jelölje be a **biztonsági figyelmeztetések** lap jelzést, ha többet szeretne megtudni az események, amelyen a értesítés és, ha bármelyik támadások ismételt még szükséges lépéseket. Ebben a példában jelöljük ki **potenciális SQL-beszúrás**.
![Biztonsági figyelmeztetések][7]

Amint alább látható, a biztonság otthon további részleteket tartalmazó mi indított az értesítésre, a cél erőforrás, ha alkalmazható betekintést nyújt a forrás IP-címét, és arról, hogy miként ismételt javaslatok.
![A lehetséges SQL-beszúrás][8]

További tudnivalókért olvassa el a [kezelése és a biztonsági figyelmeztetések megválaszolása](security-center-managing-and-responding-alerts.md)című témakört.

## <a name="next-steps"></a>Következő lépések

- [Biztonsági központja – gyakori kérdések](security-center-faq.md) – gyakori kérdések a szolgáltatással a keresés.
- [Biztonság otthon tervezése és a műveletek útmutató](security-center-planning-and-operations-guide.md) - kövesse a lépéseket, és a feladatok optimalizálja a biztonság otthon a szervezete biztonsági követelményeknek, és a felhőbeli adatkezelési modell alapján.
- [Központ adatok biztonsági](security-center-data-security.md) – ismerje meg, hogy összegyűjti a biztonság otthon, és az Azure erőforrások, köztük konfigurációs adatok, metaadatok, eseménynaplók, összeomlik kiírása fájlok és egyéb adatokat dolgoz fel.
- [Védelmi események kezelését](security-center-incident.md) – a biztonsági értesítés képességek használata a biztonság otthon, amely segít a biztonsági események kezelésében.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
