<properties
   pageTitle="Erőforrások figyelése a műveletek Management csomagja biztonsági és naplózási megoldás |} Microsoft Azure"
   description="A dokumentum segít a MOBILE biztonsági és naplózási funkciók figyelemmel az erőforrások és kiválogathatja azokat a biztonsági problémákról."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Műveletek Management csomagja biztonsági és naplózási megoldás erőforrásokkal figyelése

Ehhez a dokumentumhoz, mellyel MOBILE biztonsági és elemzési funkciók figyelemmel az erőforrások és kiválogathatja azokat a biztonsági problémákról.

## <a name="what-is-oms"></a>Mi az MOBILE?

A Microsoft műveletek Management csomagja (MOBILE), a Microsoft cloud-alapú, amelyek segítségével kezelése és védelme a helyszíni és felhőbeli infrastruktúra management levelezőprogramból. MOBILE kapcsolatos további tudnivalókért olvassa el a cikk [Műveletek Management csomagja](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Erőforrások figyelése

Amikor csak lehetséges, érdemes biztonsági események megakadályozása az elsőként történik. Azt azonban nem lehetséges, hogy az összes védelmi eseményt. Védelmi esemény fordulhat elő, amikor szüksége lesz győződjön meg arról, hogy hatásának kis méretre van állítva.  Vannak három kritikus javaslatok a szám és a biztonsági események hatásait minimalizálásához használható:

- A környezet biztonsági rendszeresen értékeli.
- Rendszeresen ellenőrzi a számítógép rendszerek és a hálózati eszközök gondoskodhat arról, hogy azok minden legújabb javításokat.
- Rendszeresen ellenőrzi naplókról és a naplózás mechanizmusok, beleértve a operációs rendszer eseménynaplók, az adott naplók alkalmazás és a behatolási észlelési rendszer naplók.

MOBILE biztonsági és naplózási megoldás lehetővé teszi, hogy informatikai aktívan figyelheti az összes erőforrás, amely segít a biztonsági események hatásának minimalizálása érdekében. MOBILE és a naplózási rendelkezik biztonsági tartományok erőforrások figyelemmel kísérésére használható. A biztonsági tartományok figyelemmel kísérésére biztonsági a következő tartományokat vonatkozik a további részleteket az olyan beállítások, gyors hozzáférést biztosít:

- Kártevőt értékelése
- Felmérés módosítása
- Identitás- és hozzáférés

> [AZURE.NOTE] Ezek a beállítások áttekintése olvassa el a [tevékenységek kezelése csomagja biztonsági és naplózási megoldás az első lépések](oms-security-getting-started.md).

### <a name="monitoring-system-protection"></a>Rendszer védelem figyelése

A z megközelítés védelmi minden réteg védelem fontos általános biztonsági állapotát az eszköz. Az észlelt veszélyek és a nem megfelelő védelmet számítógépek jelennek meg a biztonsági tartományok kártevőt értékelési csempéjén. A kártevőt felmérés információk segítségével megállapítható egy tervet védelmet alkalmazhat a kiszolgálókat, szüksége lenne rá. Ez a beállítás eléréséhez kövesse az alábbi lépéseket:

1. A **Microsoft műveletek Management Suite** fő irányítópulton kattintson a **Biztonság és naplózási** csempére.

    ![Biztonság és naplózás](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. A **Biztonság és naplózási** irányítópulton kattintson **Antimalware értékelési** **Biztonsági tartományok**területen. A **Felmérés Antimalware** irányítópult jelenik meg az alább látható módon:

![Kártevőt értékelése](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

A **Kártevőt értékelési** irányítópult is használhatja az alábbi biztonsági kapcsolatos problémák azonosítása:

- **Aktív veszélyek**: számítógépek, melyek azok sérül és aktív veszélyek a rendszer.
- **Remediated veszélyek**: azok sérül számítógépekre, de a kockázatok remediated.
- **Aláírás elavult**: engedélyezve kártevők elleni védelem számítógépekre, de az aláírás elavult.
- **Valós idejű tiltása**: számítógépeken, amelyeken nincs telepítve a antimalware telepítve.

### <a name="monitoring-updates"></a>Frissítések ellenőrzése 

A legutóbbi biztonsági frissítések alkalmazása biztonsági okokból, és azt a frissítés management vonatkozó stratégia kell foglalni. Microsoft figyelése Agent szolgáltatás (HealthService.exe) adatainak frissítése beolvassa a felügyelt számítógépekről, és kattintson a frissített adatokat küld a feldolgozás a MOBILE szolgáltatást a felhőben. A Microsoft figyelése Agent szolgáltatás van beállítva az automatikus szolgáltatás, és hogy mindig futnia kell a tároló számítógép.

![frissítések ellenőrzése](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

A frissítés adatokon alkalmazott logika, és a felhőbeli szolgáltatástól rekordok az adatokat. Ha hiányzó frissítések találhatók, azok jelennek meg a **frissítések** irányítópulton. A **frissítések** irányítópult használata hiányzik a frissítések és alkalmazni azokat a kiszolgálókat, szüksége van rájuk terv kidolgozása is használhatja. A **frissítések** irányítópult eléréséhez az alábbi lépésekkel:

1. A **Microsoft műveletek Management Suite** fő irányítópulton kattintson a **Biztonság és naplózási** csempére.
2. A **Biztonság és naplózási** irányítópulton kattintson a **Frissítés értékelési** **Biztonsági**a tartományok. A frissítés irányítópult megjelenik az alább látható módon:

![felmérés módosítása](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

Az irányítópult frissítése annak kiértékelése, a számítógépek aktuális állapotát és a legfontosabb veszélyek cím végezhet. A **kritikus vagy biztonsági frissítések** csempére használatával az informatikai rendszergazdák fog fér hozzá részletes információt a frissítések hiányzó alább látható módon:

![keresési eredmény](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Ez a jelentés tartalmazza használható azonosítják az veszélyt jelent, a rendszer kitéve, mely tartalmazza a Microsoft KB cikkek a frissítés és az MS Bulletin, amelynek a biztonsági kapcsolatos részletekért kapcsolódó fontos információkat.

### <a name="monitoring-identity-and-access"></a>Identitás- és az access figyelése

Dolgozó felhasználókkal bárhonnan, különböző eszközt használ, és elérése a felhőben, és a helyszíni alkalmazások roppant mennyiségű elengedhetetlen, hogy a hitelesítő adatait-e védelemmel ellátva. Hitelesítő adatok eltulajdonításának támadások azok, amelyben a támadó kezdetben hozzáfér egy normál felhasználói hitelesítő adatok eléréséhez a rendszert a hálózaton belül. Sokszor a kezdeti támadásokat módja a csak a hálózathoz való hozzáférés, a végső cél felderítésére jogosultsággal rendelkező fiókot. 

A támadók lesz látható az a hálózaton, ingyenesen elérhető, hitelesítő adatok kinyerése a munkamenetek más bejelentkezett fiókok szerszámok használatával. Attól függően, hogy a rendszer konfigurációja a hitelesítő adatokat is kibontott tiltva, jegyek vagy akár egyszerű szöveges jelszavak formájában.  

> [AZURE.NOTE] közvetlenül az interneten vannak kitéve gépek jelenik meg, próbálja meg az összes típusú jól ismert felhasználónevét (például rendszergazda) használatával számos sikertelen kísérletek. A legtöbb esetben célszerű az OK gombra, ha nem használják a jól ismert felhasználónevét, és a jelszó nem elég erős.

E támadóknak azonosítása, mielőtt azok behatolhatnak egy jogosultság fiók lehetőség. **Biztonsági MOBILE és a naplózási megoldás** figyelheti a identitás- és az access is élvezheti. Az **identitás- és az Access** irányítópult eléréséhez az alábbi lépésekkel:

1. A **Microsoft műveletek Management Suite** fő irányítópulton, kattintson a biztonság és naplózási csempe.
2. A **Biztonság és naplózási** irányítópulton kattintson a **identitás- és az Access** **Biztonsági tartományok**területen. Az **identitás- és az Access** -irányítópultot az alább látható módon jelenik meg:

![identitás- és hozzáférés](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

A normál felügyeleti stratégia részeként meg kell adnia személyazonosságának ellenőrzésére. Informatikai rendszergazda, ha egy adott érvényes felhasználónév, amelynek sok kísérlet kell kinéznie. Ez lehet, hogy bármelyik olyan szerezte be valós felhasználónév támadó jelezheti, és próbálja meg ellen vagy egy automatikus eszközzel, hogy használja csomagolásukkor a jelszó lejárt.

Ez az irányítópult engedélyezése IT-identitás és a vállalat erőforrásokhoz kapcsolatos potenciális veszélyek gyors azonosítását. Adott fontos is a potenciális trendek azonosítása, például bejelentkezések során idő csempéjén megjelenik egy sikertelen bejelentkezési kísérlet történt meg, hogy hány alkalommal időn keresztül. Ebben az esetben a számítógép **fájlkiszolgáló** kapott 35 bejelentkezési kísérleteket. Többet szeretne tudni az ezen a számítógépen található gombra kattintva Ismerkedjen meg. 

![További részletek](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

A jelentést hoz létre ezen a számítógépen felvette a minta értékes olvashat. Láthatta, hogy a **fiók** oszlopban a próbálja ki a rendszer eléréséhez használt felhasználói fiók lehetővé teszi, a **TIMEGENERATED** oszlopot ad az időszakot, amelyben a kísérlet történt, és az **LOGONTYPENAME** oszlopot ad a helyet, ahová a kísérlet történt. A következő kísérletek volt helyileg a rendszer által elvégzett egy programot, ha szeretné a **feldolgozás** oszlopban megjelenítő a folyamat nevét. Már rendelkezik a rendelkezésre álló folyamat neve alkalmazási eseteit, ahol elérhető lesz a bejelentkezési kísérletet a programból, és végezhetők el további vizsgálat target rendszerben.

## <a name="see-also"></a>Lásd még:

A jelen dokumentum szüksége a tanultakhoz MOBILE biztonsági használatáról és naplózási figyelemmel az erőforrások megoldás. MOBILE biztonsággal kapcsolatos további információért olvassa el a következő cikkeket:

- [Tevékenységek kezelése csomagja (MOBILE) – áttekintés](operations-management-suite-overview.md)
- [Tevékenységek kezelése csomagja biztonsági és naplózási megoldás első lépések](oms-security-getting-started.md)
- [Figyelés és a biztonsági figyelmeztetések válaszol a műveletek Management csomagja biztonsági és naplózási megoldás](oms-security-responding-alerts.md)