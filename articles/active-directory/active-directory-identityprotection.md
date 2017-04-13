<properties
    pageTitle="Azure Active Directory-identitás védelem |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure Active Directory-identitás védelem lehetővé teszi, hogy korlátozza a támadó kihasználni egy biztonságos identitás vagy eszközön, és biztonságos identitás vagy korábban gyanús vagy kell sérül ismert eszközt."
    services="active-directory"
    keywords="Azure active directory identitás védelmét, cloud app feltárás, alkalmazásokat, biztonsági, kockázat, kockázat szint, rés, biztonsági házirendek kezelése"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection"></a>Azure Active Directory identitás-védelem 

Azure Active Directory identitás védelem lehetővé teszi az Azure Active Directory prémium P2 kiadásának, amely a kockázatok események és a szervezet identitások érintő potenciális biztonsági összevont betekintést nyújt. Microsoft van már biztonságossá tétele a felhőalapú Identitások fölé egy évtizede és Azure Active Directory-identitás védelemmel, a Microsoft van e azonos védelem rendszerek elérhetővé tétele a nagyvállalati ügyfelek. Identitás védelem használja a meglévő Azure AD rendellenességet észlelés (Azure AD-Anomalous tevékenység jelentések keresztül érhető el), és bemutatja az új kockázati esemény adattípusa rendellenességeinek képes észlelni a lapra.



##<a name="getting-started"></a>Első lépések

A biztonsági szabályok megsértésével többsége érvénybe támadók hozzáférni környezetben úgy, hogy egy felhasználó személyazonosságára ellopásának megakadályozására. A támadók egyre hatékony, a harmadik féltől származó szabályok megsértésével vizsgál, és használja az összetett adathalászati célú támadásokkal váltak. A támadó hozzáfér még egy kis jogosultságokkal rendelkező felhasználó fiókkal, ha meglehetősen egyszerű fontos vállalati erőforrások keresztül oldalsó mozgását eléréséhez szükséges hozzájuk. Ezért fontos összes védő, és identitás sérül meg, amikor ezzel kapcsolatban beérkező megakadályozhatja, hogy a biztonságos identitás visszaélt alatt. 

Nincs egyszerű feladat biztonságos identitások felfedezése. Identitás védelem segítséget Szerencsére: identitás védelme és használja az adaptív gépi tanulási algoritmusok heurisztikus rendellenességeinek észleli és kockázati esemény, amelyek jelezheti, hogy az identitás napvilágra kerülhetett.
 
Ezeket az adatokat használ, identitás védelem hoz létre, jelentések és figyelmeztetések, amelyek lehetővé teszi, hogy vizsgálja meg ezeket a kockázat események és készíthet remediation megfelelő vagy a kezelési műveletet.
 
De Azure Active Directory identitás védelem több, mint a nyomon követési és jelentési eszközben. A kockázat események alapján, a identitás védelem minden olyan felhasználóhoz, úgy automatikusan védő a a szervezet kockázat-alapú házirendek beállítása egy felhasználó kockázat szint számítja ki.  A kockázat-alapú házirendek kívül más feltételes hozzáférésének Azure Active Directory és EMS, által biztosított automatikusan letiltani vagy ajánlja fel a jelszó alaphelyzetbe állítása és többtényezős hitelesítés kényszerítési adaptív remediation műveletek.  

####<a name="explore-identity-protections-capabilities"></a>Identitás védelem funkciók feltárása 

**Annak ellenőrzése, kockázat események és a kockázatos fiókok:**  

- Annak ellenőrzése, 6 kockázati esemény típusa gépi tanulási és heurisztikus szabályok használatával 

- Felhasználói kockázat szintek kiszámítása

- Egyéni javaslatok biztonsági kiemelés által általános biztonsági testtartását növelésének kezeléséről



**Vizsgálat alatt kockázati esemény:**

- A kockázat eseményekkel kapcsolatos értesítések küldése

- Vizsgálat alatt a kockázat események releváns és környezetfüggő információk alapján

- Alapvető munkafolyamatok vizsgálatok nyomon követéséhez kezeléséről

- Egyszerű hozzáférést biztosít remediation műveletek, például a jelszó alaphelyzetbe állítása



**A kockázat alapuló feltételes hozzáférés házirendek:**

- Kockázatos bejelentkezési bővítmények csökkentésében bejelentkezések blokkolás vagy kihívásokkal többtényezős hitelesítést igénylő házirend.

- Továbbfejlesztett fájlblokkolás vagy biztonságos kockázatos felhasználói fiókok házirend

- Házirend többtényezős hitelesítéshez regisztrálhatja a felhasználóknak


## <a name="detection-and-risk"></a>Észlelése és kockázatok

### <a name="risk-events"></a>A kockázat események

Kockázati esemény lettek megjelölve gyanús identitás védelem eseményeket, és jelzi, hogy identitás előfordulhat, hogy rendelkezik kerülhetett. Kockázati esemény listáját olvassa el a [kockázat események észleli Azure Active Directory identitás védelem típusú](active-directory-identityprotection-risk-events-types.md)című témakört. 


### <a name="risk-level"></a>A kockázat szint

Kockázati esemény kockázat szintje feltüntetése (magas, közepes vagy alacsony) a kockázati esemény súlyosságát. A kockázat szint segítik identitás védelem prioritást rendelhet a műveleteket azok kell a kockázatok csökkentésére a szervezet számára A kockázati esemény súlyosságát a egy előrejelző identitás biztonságos, kombinálva a zajforrásoktól, amely általában bemutatja az összeget, mint jel erősségének jelöli. 

- **Magas**: magas megbízhatóság és magas súlyosságát kockázati esemény. Az események, amelyek a felhasználói azonosító napvilágra kerülhetett, és az összes olyan felhasználói fiók hatással azonnal remediated kell erős jelölők.

- **Közepes**: magas súlyosságát, de az alsó megbízhatóság kockázati esemény, vagy fordítva. Az események esetleg kockázatos, és olyan felhasználói fiók hatással kell lennie remediated.

- **Alacsony**: alacsony megbízhatóság és az alacsony súlyosságát kockázati esemény. Ebben az esetben nem megkövetelheti azonnali művelet, de más kockázati esemény kombinálva biztosíthatják jelzi, hogy az identitás sérül. 


![A kockázat szint] (./media/active-directory-identityprotection/01.png "A kockázat szint")

 

Kockázat események vagy a megadott **valós idejű**, vagy utáni feldolgozás után a kockázati esemény már hozott helyezze (offline). Legtöbb kockázati esemény identitás-védelem jelenleg offline számítja ki, és megjelennek a identitás védelem 2-4-es órán belül. Miközben kiértékelt a valós idejű, a valós idejű kockázati esemény jelennek meg a identitás védelem konzolban 5-10 percen belül.

Több örökölt ügyfelek jelenleg nem támogatja valós idejű kockázati esemény észlelési és megelőzése. Emiatt az ügyfelektől bejelentkezések nem észlelhető vagy megakadályozza a lapra.


## <a name="investigation"></a>Vizsgálat
Az identitás-védelem keresztül utazás általában az identitás-védelem irányítópult kezdődik. 

![Remediation] (./media/active-directory-identityprotection/1000.png "Remediation")

Az irányítópult való hozzáférést biztosít:
 
- Jelentések, például a **felhasználók megjelöli a kockázat**, **kockázat események** és **biztonsági**
- Beállítások, mint például a a **Biztonsági házirendeket**, **értesítések** , és a **regisztrációs többtényezős hitelesítés** konfigurálása
 

A szokásos a kiindulási pont vizsgálat, amely a tevékenységeket, naplók és más vonatkozó információk döntse el, hogy szükség-e remediation vagy kezelési lépéseket kockázati esemény véleményezési eljárás, és hogyan az identitás jutott, és a biztonságos identitás használatának ismertetése.

Azure Active Directory-védelem küld egy e-mailben [értesítést](active-directory-identityprotection-notifications.md) a vizsgálat tevékenységek is kötik.

A következő szakaszokban további információra kíváncsi, és a lépések, amelyek való nyomozásban.  



## <a name="what-is-a-user-risk-level"></a>Mi az, hogy egy felhasználó kockázat szint?

Egy felhasználó kockázat szintje annak valószínűsége, hogy a felhasználói azonosító napvilágra kerülhetett megjelölése (magas, közepes vagy alacsony). Azt számolja ki alapján a felhasználói kockázat eseményeket, amelyek a felhasználói azonosító társítva. 

Kockázati esemény állapota **aktív** vagy **Lezárva**. Csak **aktív** kockázati esemény hozzájárulhatnak a felhasználó kockázati számításon. 

A kockázat felhasználói szintű számítja ki a következő bemeneti adatok alapján:

- A felhasználó érintő aktív kockázat események
- Az alábbi események kockázat szint 
- Hogy bármilyen remediation műveletek nyílt 

![Felhasználói kockázatok] (./media/active-directory-identityprotection/1001.png "Felhasználói kockázatok")



A bejelentkezést kockázatos felhasználók letiltása az access feltételes házirendek létrehozása a felhasználó kockázat szintek használatával, vagy hogy biztonságosan módosítani a jelszavát. 


## <a name="closing-risk-events-manually"></a>Kockázati esemény manuálisan bezárása

A legtöbb esetben lép, hogy remediation műveletek, többek között a biztonságos jelszó alaphelyzetbe állítása kockázati esemény automatikusan bezárásához. Jó helyen jár ez lehet, hogy mindig nem lehetséges.  
Ez, például a helyzet, ha:

- Aktív kockázat események rendelkező felhasználó törlése után
- Vizsgálat során kiderül, hogy az jelentett kockázati esemény a jogos felhasználó által végrehajtása-e

**Aktív** kockázat eseményeket a felhasználó kockázat számítás hozzájárul, mivel előfordulhat manuálisan csökkentse a kockázat szint kockázati esemény manuálisan bezárásával.  
Vizsgálat során választania kockázati esemény állapotának módosítása a következő műveletek bármelyikének végrehajtása:

![Műveletek] (./media/active-directory-identityprotection/34.png "Műveletek")

- **Feloldása** – Ha kockázati esemény vizsgálja meg, miután kívüli identitás védelem megfelelő remediation művelet megtette, és úgy gondolja, hogy az kockázati esemény kell figyelembe venni megjelölés megnyitva, feloldva az eseményt. A rendszer események a kockázati esemény állapotát Lezárva értékre állítja, és a kockázati esemény már nem hozzájárul ahhoz, hogy a felhasználó a kockázat.

- **Megjelölés hamis pozitív** - egyes esetekben előfordulhat, hogy vizsgálja meg a kockázati esemény és felfedezése helytelenül lett megjelölve, a kockázatos. Csökkentheti az ilyen előfordulásainak száma az kockázati esemény hamis pozitív megjelölésével. Ez segít a gépi tanulási algoritmusok hasonló események besorolása a jövőben javítása érdekében. A hamis pozitív események állapota **Lezárva** és azok már nem hozzájárul ahhoz, hogy a felhasználó a kockázat.

- **Figyelmen kívül hagyása** – Ha bármelyik remediation művelet nem léptek, de szeretné a kockázati esemény, el kell távolítani az aktív listából, akkor úgy jelölheti kockázati esemény figyelmen kívül hagyása, és az esemény állapot bezárul. Figyelmen kívül hagyott események nem szerkeszthetik azokat a felhasználó a kockázat. Ez a beállítás csak szokatlan körülmények között használható. 

- **Aktiválja újra** - (az **megoldani**, **vakriasztás**, vagy **figyelmen kívül hagyása**) manuálisan lezárt kockázat események aktiválhatók, az esemény állapot állítsa vissza az **aktív**. A felhasználói szintű számítás kockázat bővíthessék újraaktivált kockázati esemény. A kockázat események remediation keresztül zárva (mint például a biztonságos jelszó alaphelyzetbe állítása) nem aktiválhatók. 




**A kapcsolódó konfigurációs párbeszédpanel megnyitása**:

1. Kattintson az **Azure Active Directory-identitás védelem** lap **vizsgálat**, **kockázati esemény**.

    ![Kézi jelszó alaphelyzetbe állítása] (./media/active-directory-identityprotection/1002.png "Kézi jelszó alaphelyzetbe állítása")

2. A **kockázat események** listában kattintson a kockázat.

    ![Kézi jelszó alaphelyzetbe állítása] (./media/active-directory-identityprotection/1003.png "Kézi jelszó alaphelyzetbe állítása")

2. A kockázat lap kattintson a felhasználó gombbal.

    ![Kézi jelszó alaphelyzetbe állítása] (./media/active-directory-identityprotection/1004.png "Kézi jelszó alaphelyzetbe állítása")



### <a name="closing-all-risk-events-for-a-user-manually"></a>A felhasználó összes kockázati esemény manuálisan bezárása

Helyett a saját kezűleg bezárása egyenként kockázat eseményeket egy felhasználóhoz, Azure Active Directory identitás védelem is biztosít zárja be a felhasználó egyetlen kattintással összes eseményének módszert.


![Műveletek] (./media/active-directory-identityprotection/2222.png "Műveletek")

Gombra kattintva **Zárja be az összes eseményének**összes eseményének be van zárva, és az érintett felhasználó már nem érinti.



## <a name="remediating-user-risk-events"></a>Felmérésével felhasználói kockázati esemény

Egy remediation egy olyan művelet, biztonságos identitás vagy korábban gyanús vagy kell sérül ismert eszközt. Egy remediation művelet visszaállítása a identitás vagy eszközön biztonságos állapotba, és úgy oldja fel az azonosító vagy más eszközhöz tartozó korábbi kockázat események.

A felhasználó kockázati esemény ismételt, a következőkre van lehetősége:

- A biztonságos jelszó visszaállítása a felhasználó kockázati esemény manuálisan ismételt végrehajtása 

- Egy felhasználó kockázat biztonsági házirendet, amely csökkentésében vagy felhasználói kockázati esemény automatikusan ismételt konfigurálása

- Újra kép fertőzött eszköz  


### <a name="manual-secure-password-reset"></a>Kézi a biztonságos jelszó-visszaállítás

A biztonságos jelszó alaphelyzetbe állítása egy hatékony remediation sok kockázati esemény, és amikor végzett, automatikusan bezárja az kockázati esemény újraszámítja a felhasználó kockázati szintjét. Az identitás-védelem irányítópult jelszó alaphelyzetbe állítása kockázatos a felhasználó kezdeményezésére is használhatja. 

A kapcsolódó párbeszédpanel jelszó alaphelyzetbe állítása két másik módszert kínál:

Többtényezős hitelesítés **jelszó** - választó **felhasználó jelszavának alaphelyzetbe** engedélyezése a felhasználó saját helyreállítása, ha a felhasználó regisztrálta. A felhasználó következő bejelentkezés közben, a felhasználó lesz szükséges többtényezős hitelesítés bonyolulttá sikerült megoldani, és majd, ki kell módosítani a jelszót. Ez a beállítás nem érhető el, ha a felhasználói fiók még nem regisztrált többtényezős hitelesítés.

**Ideiglenes jelszó** - választó **ideiglenes jelszó létrehozása** azonnal érvényteleníti a jelenlegi jelszót, és hozzon létre egy új, ideiglenes jelszót a felhasználó számára. A felhasználó egy másodlagos e-mail címet vagy a felhasználó felettese, küldje el az új, ideiglenes jelszót. Mivel ideiglenes jelszavát, a felhasználónak bejelentkezés után a jelszó módosítása kérni fogja.


![Házirend] (./media/active-directory-identityprotection/1005.png "Házirend")


**A kapcsolódó konfigurációs párbeszédpanel megnyitása**:

1. Kattintson az **Azure Active Directory-identitás védelem** a lap, a **felhasználók kockázati menügombon**.

    ![Kézi jelszó alaphelyzetbe állítása] (./media/active-directory-identityprotection/1006.png "Kézi jelszó alaphelyzetbe állítása")


2. A felhasználók listáját jelölje ki a felhasználót legalább egy kockázat eseményekkel.

    ![Kézi jelszó alaphelyzetbe állítása] (./media/active-directory-identityprotection/1007.png "Kézi jelszó alaphelyzetbe állítása")


2. Kattintson a felhasználó a lap, a **jelszó alaphelyzetbe állítása**.

    ![Kézi jelszó alaphelyzetbe állítása] (./media/active-directory-identityprotection/1008.png "Kézi jelszó alaphelyzetbe állítása")





## <a name="user-risk-security-policy"></a>Felhasználói kockázat biztonsági házirendek:

Egy felhasználó kockázat biztonsági házirend egy feltételes hozzáférési házirendet, amely az adott felhasználó kockázat értéket ad, és alkalmazza a szabályok és a előre meghatározott feltételek alapján remediation és kezelési műveletek.


![Felhasználói ridk házirend] (./media/active-directory-identityprotection/1009.png "Felhasználói ridk házirend")


Azure Active Directory-identitás Protection segítségével a kezelési és remediation meg van jelölve a kockázat, mivel az, hogy a felhasználók kezelése:

- Állítsa be a felhasználók és csoportok a házirend vonatkozik: 

    ![Felhasználói ridk házirend] (./media/active-directory-identityprotection/1010.png "Felhasználói ridk házirend")


- A felhasználó kockázati szintű küszöbértékét (a Min, a közepes vagy a max), amely elindítja a házirend beállítása: 

    ![Felhasználói ridk házirend] (./media/active-directory-identityprotection/1011.png "Felhasználói ridk házirend")


- A vezérlők kikényszerítését, amikor elindítja a házirend beállítása:

    ![Felhasználói ridk házirend] (./media/active-directory-identityprotection/1012.png "Felhasználói ridk házirend")


- Váltás a házirend állapotát:

    ![Felhasználói ridk házirend] (./media/active-directory-identityprotection/403.png "MFA regisztráció")


- Tekintse át, és milyen következményekkel járnak módosítás előtt aktiválás felmérése:

    ![Felhasználói ridk házirend] (./media/active-directory-identityprotection/1013.png "Felhasználói ridk házirend")


Válassza a **felső** küszöbértéket csökkenti a számú alkalommal a házirend induljanak, és a kis méretűre állítása a felhasználókra gyakorolt hatás.
Azonban azt nem tartalmazza a házirendet, amely nem biztonságos, identitások vagy korábban gyanús vagy kell sérül ismert eszközök veszélyt menügombon **alacsony** és **Közepes** felhasználók.

A szabály beállítása során

- Kizárja a felhasználók, akik várhatóan sok hamis-pozitív (fejlesztők, biztonsági elemzők) létrehozása

- A területi beállításokat, ahol a házirend engedélyezése nem célszerű felhasználók kizárása (például nincs hozzáférés segélyszolgálat)

- Használatára, egy kezdeti házirend során **felső** küszöbértéket bevezetésére, vagy ha kell kisméretűvé alakítása a végfelhasználók számára láthatóak problémáit.

- Egy **alacsony** küszöbérték használja, ha a szervezete megköveteli a nagyobb biztonság. További felhasználó bejelentkezési problémáit, de nagyobb biztonságot kijelölése egy **alacsony** küszöbértéke vezet be.

A legtöbb szervezet ajánlott alapértelmezés szerint egy szabályt, a **Közepes** küszöb optimális megoldást találni közötti használhatósági és biztonság beállítása.

A kapcsolódó felhasználói felület áttekintést talál:

- [Áldozatul esett számítógépek fiók helyreállítási folyamat](active-directory-identityprotection-flows.md#compromised-account-recovery).  

- [Áldozatul esett számítógépek fiók letiltott továbbításához](active-directory-identityprotection-flows.md#compromised-account-blocked).  


**A kapcsolódó konfigurációs párbeszédpanel megnyitása**:

1. Kattintson az **Azure Active Directory-identitás védelem** lap **beállítása** csoportban a **felhasználói kockázat házirend**.

    ![Felhasználói ridk házirend] (./media/active-directory-identityprotection/1009.png "Felhasználói ridk házirend")






## <a name="mitigating-user-risk-events"></a>Enyhítő felhasználói kockázati esemény
Rendszergazdák felhasználói kockázat biztonsági házirendek adhatja meg a felhasználók letiltása után bejelentkezés kockázat szinttől függően. 

A bejelentkezés letiltása:
 
- Megakadályozza, hogy az új felhasználói kockázat események a szóban forgó felhasználó létrehozása

- Lehetővé teszi a rendszergazdák kézi ismételt a kockázat események érintő a felhasználói azonosító, és visszaállítja biztonságos állapotát



## <a name="what-is-a-sign-in-risk-level"></a>Mi az a bejelentkezési kockázat szint?

A bejelentkezési kockázati szint annak valószínűsége, hogy az egy adott bejelentkezés, valaki megpróbálja hitelesíteni a felhasználói azonosító megjelölése (magas, közepes vagy alacsony). A bejelentkezési kockázat szint kiértékeli a bejelentkezés során, és úgy véli, kockázat események és jelölők található-e adott bejelentkezés valós időben. 

## <a name="mitigating-sign-in-risk-events"></a>Enyhítő bejelentkezési kockázati esemény 
A kezelési egy olyan művelet, támadó lehetőségét, hogy egy biztonságos identitás vagy eszköz kihasználhatja a identitás vagy eszközön biztonságos állapotba visszaállítással korlátozhatja. A kezelési nem oldja meg a társított identitás vagy eszköz előző bejelentkezési kockázati esemény.

Azure Active Directory-identitás védelem feltételes access segítségével automatikusan a események bejelentkezés kockázatok csökkentésében. Ezek a házirendek használ, érdemes megfontolni a kockázat szintet a felhasználó vagy a bejelentkezés kockázatos bejelentkezési bővítmények engedélyezése és a felhasználó többtényezős hitelesítés végrehajtásához. Az alábbi műveletek megakadályozhatja, hogy a támadók egy ellopott személyazonosságát kárt kihasználása, és előfordulhat, hogy ad egy kis időt, az Identitáskezelés biztonságossá tétele. 


## <a name="sign-in-risk-security-policy"></a>Bejelentkezés a kockázat eszközbiztonsági házirend beállítása

A bejelentkezési kockázat házirend egy feltételes hozzáférési házirendet, amely kiértékeli a kockázatot egy konkrét bejelentkezés, és alkalmazza a szabályok és a előre meghatározott feltételek alapján megoldásokkal kapcsolatban.

![Bejelentkezés a kockázat házirend] (./media/active-directory-identityprotection/1014.png "Bejelentkezés a kockázat házirend")


Azure Active Directory-identitás Protection segítségével kezelheti a kezelési, a kockázatos bejelentkezések úgy:

- Állítsa be a felhasználók és csoportok a házirend vonatkozik: 

    ![Bejelentkezés a kockázat házirend] (./media/active-directory-identityprotection/1015.png "Bejelentkezés a kockázat házirend")


- A bejelentkezési kockázati szintű küszöbértékét (a Min, a közepes vagy a max), amely elindítja a házirend beállítása: 

    ![Bejelentkezés a kockázat házirend] (./media/active-directory-identityprotection/1016.png "Bejelentkezés a kockázat házirend")


- A vezérlők kikényszerítését, amikor elindítja a házirend beállítása:  

    ![Bejelentkezés a kockázat házirend] (./media/active-directory-identityprotection/1017.png "Bejelentkezés a kockázat házirend")
    

- Váltás a házirend állapotát:

    ![MFA regisztráció] (./media/active-directory-identityprotection/403.png "MFA regisztráció")

- Tekintse át, és milyen következményekkel járnak módosítás előtt aktiválás felmérése: 

    ![Bejelentkezés a kockázat házirend] (./media/active-directory-identityprotection/1018.png "Bejelentkezés a kockázat házirend")


### <a name="what-you-need-to-know"></a>Mit kell tudnia

Többtényezős hitelesítést igényel bejelentkezési kockázat biztonsági házirendek adhatja meg:

![Bejelentkezés a kockázat házirend] (./media/active-directory-identityprotection/1017.png "Bejelentkezés a kockázat házirend")

Azonban biztonsági okokból ez a beállítás csak akkor működik, már van regisztrálva többtényezős hitelesítéshez felhasználónál. Ha a feltétel többtényezős hitelesítést igényel egy felhasználóhoz, aki még nem regisztrált többtényezős hitelesítéshez teljesül, a felhasználó le van tiltva. 

Legjobb módszer Ha azt szeretné, hogy többtényezős hitelesítést igényel kockázatos bejelentkezési bővítmények, tegye a következőt:

1. Engedélyezze az érintett felhasználónál a [többtényezős hitelesítés regisztrációs házirend](#multi-factor-authentication-registration-policy) .
2. Az érintett felhasználókat, hogy a bejelentkezés szükséges MFA regisztráció végrehajtásához nem kockázatos munkamenetben

Az alábbi lépésekkel biztosítja, hogy többtényezős hitelesítés szükséges kockázatos bejelentkezés. 


### <a name="best-practices"></a>Ajánlott eljárások

 
Válassza a **felső** küszöbértéket csökkenti a számú alkalommal a házirend induljanak, és a kis méretűre állítása a felhasználókra gyakorolt hatás.  
 
Azonban azt nem tartalmazza a **kis** - és **Közepes** bejelentkezési bővítmények a házirendet, amely nem lehet letiltani a kihasználhassa egy biztonságos személyazonosságára támadók veszélyt menügombon. 

A szabály beállítása során 

- A felhasználók, akik nem / nem lehet többtényezős hitelesítés kizárása

- A területi beállításokat, ahol a házirend engedélyezése nem célszerű felhasználók kizárása (például nincs hozzáférés segélyszolgálat)

- Kizárja a felhasználók, akik várhatóan sok hamis-pozitív (fejlesztők, biztonsági elemzők) létrehozása

- Használatára, egy kezdeti házirend során **felső** küszöbértéket bevezetésére, vagy ha kell kisméretűvé alakítása a végfelhasználók számára láthatóak problémáit.

- Egy **alacsony** küszöbérték használja, ha a szervezete megköveteli a nagyobb biztonság. További felhasználó bejelentkezési problémáit, de nagyobb biztonságot kijelölése egy **alacsony** küszöbértéke vezet be.

A legtöbb szervezet ajánlott alapértelmezés szerint egy szabályt, a **Közepes** küszöb optimális megoldást találni közötti használhatósági és biztonság beállítása.

 
A bejelentkezési kockázat házirend van:

- Érvényes böngésző forgalmat és a bejelentkezési bővítmények modern hitelesítés használatával.
- Alkalmazások régebbi biztonsági protokollok használatával az WS – adatvédelmi végpontot, a szövetséges IDP, például az ADFS letiltásával nem érvényes.

A **Kockázat események** lapot, az Identitáskezelés védelem konzolban összes eseményének sorolja fel:

- Ezzel a házirenddel alkalmazott
- Tekintse át a tevékenységet, és megtudhatja, hogy a művelet megfelelő volt-e vagy sem 

A kapcsolódó felhasználói felület áttekintést talál:

- [Kockázatos bejelentkezési helyreállítás](active-directory-identityprotection-flows.md#risky-sign-in-recovery) 

- [Kockázatos bejelentkezés blokkolt](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  

- [Többtényezős hitelesítés regisztrációs kockázatos bejelentkezés során](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)  





**A kapcsolódó konfigurációs párbeszédpanel megnyitása**:

1. Kattintson az **Azure Active Directory-identitás védelem** lap **beállítása** csoportban a **bejelentkezési kockázat házirend**.

    ![Felhasználói ridk házirend] (./media/active-directory-identityprotection/1014.png "Felhasználói ridk házirend")





## <a name="multi-factor-authentication-registration-policy"></a>Többtényezős hitelesítés regisztrációs házirend

Azure többtényezős hitelesítés a ellenőrzésére, akik nem egyszerűen csak felhasználónevet és jelszót használatához módszer. Felhasználói bejelentkezések, és a tranzakciók biztonsági második réteg biztosít.  
Azt javasoljuk, hogy elő Azure többtényezős hitelesítés felhasználói bejelentkezések mivel azt:

- Egyszerű ellenőrzési beállítások tartományba erős hitelesítés biztosítja.

- Játszik szerepet védelme és a fiók kompromisszumok helyreállítása a szervezet felkészítése az

![Felhasználói ridk házirend] (./media/active-directory-identityprotection/1019.png "Felhasználói ridk házirend")



További részletekért lásd: [Mi az Azure többtényezős hitelesítést?](../multi-factor-authentication/multi-factor-authentication.md)


Többtényezős hitelesítés regisztrációs bevezetési kezelése egy házirendet, amely lehetővé teszi, hogy konfigurálásával Azure Active Directory-identitás védelem segítségével: 



- Állítsa be a felhasználók és csoportok a házirend vonatkozik: 

    ![MFA házirend] (./media/active-directory-identityprotection/1020.png "MFA házirend")



- A vezérlők kikényszerítését, amikor elindítja a házirend beállítása:  

    ![MFA házirend] (./media/active-directory-identityprotection/1021.png "MFA házirend")


- Váltás a házirend állapotát:

    ![MFA házirend] (./media/active-directory-identityprotection/403.png "MFA házirend")

- Regisztráció jelenlegi állapotának megtekintéséhez: 

    ![MFA házirend] (./media/active-directory-identityprotection/1022.png "MFA házirend")


A kapcsolódó felhasználói felület áttekintést talál:

- [Többtényezős hitelesítés regisztrációs folyamat](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  

- [Többtényezős hitelesítés regisztrációs kockázatos bejelentkezés során](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in).  





**A kapcsolódó konfigurációs párbeszédpanel megnyitása**:

1. Kattintson az **Azure Active Directory-identitás védelem** lap **konfigurálása** csoportban **többtényezős hitelesítés regisztrációs**.

    ![MFA házirend] (./media/active-directory-identityprotection/1019.png "MFA házirend")





## <a name="next-steps"></a>Következő lépések

 - [Csatorna 9: Azure Active Directory és a Megjelenítés identitás: identitás védelem előzetes verzió](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [Kockázat események észleli Azure Active Directory identitás védelem típusai](active-directory-identityprotection-risk-events-types.md)
 - [Azure Active Directory identitás védelem észlelni biztonsági](active-directory-identityprotection-vulnerabilities.md)
 - [Azure Active Directory identitás védelem értesítések](active-directory-identityprotection-notifications.md)
 - [Azure Active Directory identitás védelem playbook](active-directory-identityprotection-playbook.md)
 - [Azure Active Directory identitás védelem szószedet](active-directory-identityprotection-glossary.md)

 - [Bejelentkezési Azure Active Directory-identitás védelem szolgáltatások](active-directory-identityprotection-flows.md)

 - [Azure Active Directory-identitás védelem bekapcsolása](active-directory-identityprotection-enable.md)
 - [Azure Active Directory identitás védelem – letiltásának feloldása a felhasználóknak](active-directory-identityprotection-unblock-howto.md)

 - [Első lépések a Azure Active Directory identitás védelme és a Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)


