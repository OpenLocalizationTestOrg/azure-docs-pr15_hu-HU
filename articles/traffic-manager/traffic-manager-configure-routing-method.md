<properties
    pageTitle="Adatforgalom Manager útválasztási módszerek beállítása |} Microsoft Azure"
    description="Ez a cikk ismerteti, hogyan útválasztási különböző módszerek az adatforgalom-kezelőben konfigurálása"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-traffic-manager-routing-methods"></a>Adatforgalom Manager útválasztási módszerek beállítása

Azure forgalom Manager bemutat három útválasztási, amelyek meghatározzák, hogy hogyan továbbítás elérhető szolgáltatások végpontokhoz forgalmat. A forgalom útválasztás módszer minden DNS-lekérdezés megállapíthatja, hogy melyik végpont vissza kell a DNS-válasz érkezett érvényes.

Háromféleképpen forgalom útválasztási érhető el az adatforgalom-kezelőben:

- **Prioritás:** Jelölje ki a "Prioritása", ha meg szeretné használni egy elsődleges szolgáltatás végpontjának, és adja meg a biztonsági másolatok, abban az esetben, ha az elsődleges nem érhető el.
- **Súlyozott:** Select "súlyozott" Ha forgalom elosztása végpontok halmazának szeretne, vagy egyenletesen vagy súlyok, amely szerint adja meg.
- **Teljesítmény:** Jelölje be a "Teljesítmény", amikor végpontjait a különböző földrajzi helyek és azt szeretné, hogy a végfelhasználók számára, hogy a "legközelebbi" végpont értelmez a legmagasabb hálózati késés használatára.

## <a name="configure-priority-routing-method"></a>Állítsa be a prioritás útválasztási módszer

A webhely mód függetlenül Azure webhelyekre már funkcionalitással feladatátvevő a webhelyeket belül egy adatközponthoz (más néven régió). Adatforgalom-kezelővel feladatátvevő a különböző adatközpont esetén webhelyeket.

Szolgáltatás feladatátvevő közös mintájának forgalmat küldeni egy elsődleges szolgáltatást, és adja meg a feladatátvételi azonos biztonsági szolgáltatások is. A következő lépések bemutatják, hogy miként állítható be a rangsorolt feladatátvételi az Azure felhőszolgáltatások és a webhelyek:

1. Az Azure klasszikus portálon, a bal oldali ablaktáblában kattintson a **Forgalom Manager** ikonra kattintva nyissa meg a forgalmat a kezelő ablaktáblában.
2. Az Azure klasszikus portálon a forgalom kezelő ablakban keresse meg a módosítani kívánt beállításokat tartalmazó forgalom Manager profil. A profil-beállítások lap megnyitásához kattintson a profil nevétől jobbra található nyílra.
3. A profil lapon kattintson a lap tetején a **Végpontok** . Győződjön meg arról, hogy a felhőszolgáltatások és a webhelyek, a konfigurációban felvenni kívánt megtalálhatók.
4. Kattintson a **Konfigurálás** gombra kattintva nyissa meg a konfigurációs lap tetején.
5. A **forgalom útválasztási módszer beállításait**ellenőrizze, hogy a forgalmat útválasztási módszer **feladatátvevő**. Ha nem, kattintson a **feladatátvevő** a legördülő listából.
6. **Feladatátvevő prioritás listában**állítsa át a feladatátvevő sorrendben, a végpontok. Ha bejelöli a **feladatátvevő** forgalom útválasztási módszer, a kijelölt végpontok sorrendjének ügyekben. Az elsődleges végpontot van az előtérben. A felfelé és lefelé mutató nyilakkal sorrendjének módosítása, szükség szerint. Feladatátvevő prioritásának beállítása a Windows PowerShell használatával kapcsolatos további tudnivalókért olvassa el a [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880)című témakört.
7. Győződjön meg arról, hogy a **Beállítások ellenőrzése** megfelelően vannak-e beállítva. Figyelés biztosítja, hogy az offline végpontokat nem kapnak forgalmat. Lync-végpontok, meg kell adnia egy elérési utat és a fájlnevet. Perjel "/" relatív elérési útja az érvényes bejegyzés és azt jelenti, hogy a fájlt a legfelső szintű címtárban (alapértelmezett).
8. A konfiguráció módosítások elvégzése után kattintson a **Mentés** elemre a lap alján.
9. Tesztelje a módosításokat a konfigurációban.
10. A forgalom Manager profil működik, ha a vállalat tartománynevét a forgalom Manager tartománynevére mutasson a mérvadó DNS-kiszolgáló a DNS-rekord szerkesztése

## <a name="configure-weighted-routing-method"></a>Súlyozott útválasztási mód konfigurálása

Közös forgalom útválasztási módszer mintát, hogy egy sor olyan azonos végpontok, amely tartalmazza a felhőszolgáltatások és a webhelyek, adja meg, és küldjön minden ciklikus módon. Az alábbi lépésekkel szerkezeti forgalom útválasztási módszer a következő típusú konfigurálásáról.

>[AZURE.NOTE] Azure webhelyek már ciklikus terheléselosztási funkciók a webhelyeket belül egy adatközponthoz (más néven régió) adja meg. Adatforgalom Manager különböző adatközpont esetén ciklikus forgalom útválasztási módszer a webhelyeket megadását teszi lehetővé.

1. Az Azure klasszikus portálon, a bal oldali ablaktáblában kattintson a **Forgalom Manager** ikonra kattintva nyissa meg a forgalmat a kezelő ablaktáblában.
2. A forgalom kezelő ablakban keresse meg a módosítani kívánt beállításokat tartalmazó forgalom Manager profil. A profil-beállítások lap megnyitásához kattintson a profil nevétől jobbra található nyílra.
3. A profil lapon kattintson a lap tetején a **Végpontok** , és győződjön meg róla, hogy az Ön konfigurációjának szerepeltetni kívánt szolgáltatás végpontokat bemutató.
4. A profil lapon kattintson a **beállítás** gombra kattintva nyissa meg a konfigurációs lap tetején.
5. **Adatforgalom útválasztási mód beállításai**ellenőrizze, hogy a forgalmat útválasztási módszer **ciklikus**. Ha nem, kattintson a legördülő lista **ciklikus** .
6. Győződjön meg arról, hogy a **Beállítások ellenőrzése** megfelelően vannak-e beállítva. Figyelés biztosítja, hogy az offline végpontokat nem kapnak forgalmat. Lync-végpontok, meg kell adnia egy elérési utat és a fájlnevet. Perjel "/" relatív elérési útja az érvényes bejegyzés és azt jelenti, hogy a fájlt a legfelső szintű címtárban (alapértelmezett).
7. A konfiguráció módosítások elvégzése után kattintson a **Mentés** elemre a lap alján.
8. Tesztelje a módosításokat a konfigurációban.
9. A forgalom Manager profil működik, ha a vállalat tartománynevét a forgalom Manager tartománynevére mutasson a mérvadó DNS-kiszolgáló a DNS-rekord szerkesztése

## <a name="configure-performance-traffic-routing-method"></a>Teljesítmény forgalom útválasztási mód konfigurálása

A teljesítmény forgalom útválasztási módszer közvetlen forgalmat a végpontra a legalacsonyabb válaszidejű az ügyfél hálózatról teszi lehetővé. A legalacsonyabb válaszidejű a adatközponthoz jellemzően a legközelebbi földrajzi távolság. A forgalom útválasztási módszer nem valós idejű módosítások esetén a hálózati konfigurálásról fiók, és töltse be.

1. Az Azure klasszikus portálon, a bal oldali ablaktáblában kattintson a **Forgalom Manager** ikonra kattintva nyissa meg a forgalmat a kezelő ablaktáblában.
2. A forgalom kezelő ablakban keresse meg a módosítani kívánt beállításokat tartalmazó forgalom Manager profil. A profil-beállítások lap megnyitásához kattintson a profil nevétől jobbra található nyílra.
3. A profil lapon kattintson a lap tetején a **Végpontok** , és győződjön meg róla, hogy az Ön konfigurációjának szerepeltetni kívánt szolgáltatás végpontokat bemutató.
4. A profil lapon kattintson a **beállítás** gombra kattintva nyissa meg a konfigurációs lap tetején.
5. A **forgalom útválasztási módszer beállításait**ellenőrizze, hogy a forgalom útválasztási módszer * *teljesítményét*. Ha nem, kattintson a * *Teljesítmény** a legördülő listában.
6. Győződjön meg arról, hogy a **Beállítások ellenőrzése** megfelelően vannak-e beállítva. Figyelés biztosítja, hogy az offline végpontokat nem kapnak forgalmat. Lync-végpontok, meg kell adnia egy elérési utat és a fájlnevet. Perjel "/" relatív elérési útja az érvényes bejegyzés és azt jelenti, hogy a fájlt a legfelső szintű címtárban (alapértelmezett).
7. A konfiguráció módosítások elvégzése után kattintson a **Mentés** elemre a lap alján.
8. Tesztelje a módosításokat a konfigurációban.
9. A forgalom Manager profil működik, ha a vállalat tartománynevét a forgalom Manager tartománynevére mutasson a mérvadó DNS-kiszolgáló a DNS-rekord szerkesztése

## <a name="next-steps"></a>Következő lépések

* [Adatforgalom Manager profilok kezelése](traffic-manager-manage-profiles.md)
* [Adatforgalom Manager útválasztási módszerek](traffic-manager-routing-methods.md)
* [Adatforgalom kezelő beállításainak tesztelése](traffic-manager-testing-settings.md)
* [Internetes vállalati tartomány mutasson a forgalom Manager tartomány](traffic-manager-point-internet-domain.md)
* [Adatforgalom Manager végpontok kezelése](traffic-manager-manage-endpoints.md)
* [Hibaelhárítási forgalom Manager csökkent állam](traffic-manager-troubleshooting-degraded.md)
