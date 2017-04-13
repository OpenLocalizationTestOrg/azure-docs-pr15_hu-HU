<properties
    pageTitle="A helyreállítási terv létrehozása |} Microsoft Azure" 
    description="Azure webhely helyreállítási átveszi és csoportok virtuális gépeken futó és fizikai kiszolgálók helyreállítása helyreállítási tervet készíthet." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="create-recovery-plans"></a>A helyreállítási terv létrehozása

A webhely-helyreállítás Azure szolgáltatás hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, replikációs, feladatátvétel és helyreállítási virtuális gépeken futó és fizikai kiszolgálók orchestrating. Gépek replikálhatók Azure, illetve egy másodlagos helyszíni adatközpont. Gyors áttekintéséhez olvassa el a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md).


## <a name="overview"></a>– Áttekintés

Ez a cikk létrehozásáról és testreszabásáról a helyreállítási terv információt tartalmaz. 

Egy vagy több rendezett tartalmazó csoportok virtuális gépeken futó vagy a fizikai kiszolgálók közös átadni, amelyet helyreállítási csomagok állnak. A helyreállítási terv tegye a következőket:

- Gépek, amely fölé sikertelen, és kezdje közös csoportok megadása parancsra.
- A modellben függőségek csoportosításával őket a helyreállítási terv csoportban gépek között. Példa: Ha nem sikerül fölé, és jelenítse meg az egy adott alkalmazás, a helyreállítási terv csoporton belül az alkalmazás számára a virtuális gépeken futó volna csoportnak.
- Automatizálható és feladatátvevő bővítése. A próba, a tervezett, vagy nem tervezett feladatátvételre helyreállítási csomag futtathatók. Testre szabhatja a helyreállítási terv parancsfájlok, Azure automatizálási és manuális műveletek.

A **Helyreállítási tervek** a webhely helyreállítási portálon a helyreállítási csomagok jelennek meg.


Ez a cikk, illetve a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)alsó megjegyzések vagy kérdések tartalmakat tehet közzé.

## <a name="before-you-start"></a>Előzetes teendők

Vegye figyelembe az alábbiakat:

- A helyreállítási terv VMs ne keverje egy vagy több hálózati adaptereken. Ennek az oka az Azure felhőszolgáltatásában ezek VMs keverése nem engedélyezett.
- A parancsfájlok és a kézi műveletek helyreállítási csomagok bővítése Vegye figyelembe az alábbiakat:
    - A Windows PowerShell használatá parancsfájlokat.
    - Biztosíthatja, hogy a parancsfájlok próbálja-tényleges megakadályozza, hogy a kivételek biztonságosan kezeli. Ha a parancsfájl kivételt akkor leáll, és a tevékenység jeleníti meg, ahogy sikertelen volt.  Hiba fordul elő, nem futtassa a parancsfájl bármelyik hátralévő részét. Ha az ez történik, amikor feladatátvételhez futtat, a helyreállítási terv továbbra is. Ez akkor fordul elő, ha futtatja a tervezett feladatátvevő, ha a helyreállítási terv leáll. Ez akkor fordulhat elő, ha javítása a parancsfájlt, ellenőrizze, hogy az elvárásoknak megfelelően fut, és futtassa újra a helyreállítási csomagot.
    - Az írás-Host parancs nem érhető el a helyreállítási terv parancsfájl, és a parancsfájl sikertelen lesz. Ha azt szeretné, kimeneti létrehozása, hozzon létre egy a fő parancsfájl viszont futtató proxy parancsfájlt, és biztosítani, hogy az összes kimenő adatcsatornán-e használni a >> parancsot.
    - A parancsprogram időtúllépés történik, ha nem ad vissza 600 másodpercek.
    - Bármi írott ezzel, ha a parancsfájl fog sorolható, ahogy sikertelen volt. Az információk jelennek meg a parancsprogram-végrehajtási részletei.
    - Ha VMM használ a környezetben vegye figyelembe, hogy:

        - A helyreállítási terv parancsfájlok futtassa a VMM szolgáltatásfiók környezetében. Ellenőrizze, hogy a fiók olvasási engedéllyel rendelkezik a távoli megosztott, amelyen a parancsfájl található, és tesztelje a parancsfájlt közötti a VMM szolgáltatási fiók jogosultsági szintet.
        - VMM parancsmagok érkeznek a Windows PowerShell-modult. A VMM Windows PowerShell-modult telepítve van, ha telepíti a VMM konzolt. A VMM modul töltődnek be a parancsfájl, használja a következő parancsot a parancsfájl: a modul importálása-virtualmachinemanager nevét. [Ha további részleteket](hhttps://technet.microsoft.com/library/hh875013.aspx).
        - Gondoskodjon arról, hogy rendelkezik legalább egy tárat a VMM környezetben. Alapértelmezés szerint a tár megosztás elérési útját VMM kiszolgáló található helyileg a mappa nevét MSCVMMLibrary VMM kiszolgálón.
        - Ha a tár megosztás elérési út távoli (vagy a helyi, de nincs megosztva MSCVMMLibrary, a következőképpen konfigurálása a megosztás (használatával \\libserver2.contoso.com\share\ példaként):
            - Nyissa meg a Beállításszerkesztőt, és kattintson a webhely-Recovery\Registration HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure.
            -  Az érték ScriptLibraryPath szerkesztése, és helyezze az értéket, mint \\libserver2.contoso.com\share\. adja meg a teljes teljesen minősített tartománynév. Adja meg azt a helyet, megosztási engedélyeket.
            -  Győződjön meg arról, hogy tesztelje a parancsfájl egy felhasználói fiókhoz, amelynek a VMM szolgáltatásfiók annak érdekében, hogy a különálló tesztelt parancsfájlok futtatása a helyreállítási tervekben lesz hasonló módon megegyező engedélyeket. A VMM kiszolgálón házirendjének az adatvégrehajtás megkerülje az alábbiak szerint:
                -  Nyissa meg a 64 bites Windows PowerShell-konzol magasabb szintű jogosultságokkal használatával.
                -  Típus: a **Set-végrehajtási házirend figyelmen kívül hagyása**. [Ha további részleteket](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="create-a-recovery-plan"></a>Helyreállítási terv létrehozása

A helyreállítási terv hoz létre módja attól függ, hogy a webhely helyreállítási üzembe.

- **Replikálása VMware VMs és fizikai kiszolgálók**– terv létrehozása és a virtuális gépeken futó és helyreállítási csomagra fizikai kiszolgálók tartalmazó replikációs csoportok formájában.
- **A Hyper-V VMs replikálása (a VMM felhőket)**– terv létrehozása és a felhő VMM védett a Hyper-V virtuális gépeken futó hozzáadása a helyreállítási terv.
- **A Hyper-V VMs replikálása (nem a VMM felhőket)**– terv létrehozása és hozzáadása védett a Hyper-V virtuális gépeken futó védelem csoportból a helyreállítási terv.
- **SAN replikációs**– terv létrehozása és hozzáadása replikációs virtuális gépeken futó a helyreállítási terv tartalmazó csoport. Kijelölhet replikációs csoporthoz, hanem adott virtuális gépeken futó mivel minden virtuális gépeken futó replikációs csoport együtt kell átveszi (feladatátadás a tárhely rétegben először).


Helyreállítási terv létrehozása az alábbi képlettel történik:

1. Kattintson a **Helyreállítás csomagok** lap > **Helyreállítási terv létrehozása**.
Adja meg a helyreállítási tervet, és a forrás- és célwebhelyek nevét. A forráskiszolgálóval virtuális gépeken futó feladatátvétel és helyreállítási engedélyezett kell rendelkeznie.

    - Ha Ön éppen esetében, amelyek a VMM VMM jelölje be az **Adatforrás típusa** > **VMM**, és a forrás- és célwebhelyek VMM kiszolgálókhoz. Kattintson **A Hyper-V** lásd: felhő, amely a Hyper-V replika használatára van beállítva. 
    - Ha, esetén esetében, amelyek a VMM SAN választó **Forrástípus**használó VMM > **VMM**, és a forrás- és célwebhelyek VMM kiszolgálókhoz. Kattintson a **SAN** SAN replikációs konfigurált felhőket megjelenítéséhez.
    - Ha Ön éppen esetében, amelyek a VMM Azure jelölje be a **Forrás típusa** > **VMM**.  Jelölje ki a forráskiszolgálóval VMM és **Azure** a célként.
    - Ha, használja a Hyper-V webhelyről replikálása jelölje be az **Adatforrás típusa** > **a Hyper-V webhelyet**. Jelölje be a forrás- és **Azure **a célként a webhelyet.
    - Ha, hogy esetében, amelyek VMware vagy a fizikai helyszíni kiszolgálót az Azure, jelölje be a fiókkonfigurációs kiszolgáló a forrás- és **Azure** és a cél megjelölése

2. **Jelölje ki a virtuális gépeken futó** jelölje ki az virtuális gépeken futó (vagy replikációs csoport), amelyet a helyreállítási tervben vegye fel az alapértelmezett csoportba (1-es csoport).

## <a name="customize-recovery-plans"></a>A helyreállítási terv testreszabása

Miután felvette a védett virtuális gépeken futó vagy az alapértelmezett helyreállítási terv csoport replikációs csoportokat, és testre szabhatja a terv létrehozása:

- **Hozzáadás új csoportokat**– további helyreállítási terv csoportjait felveheti. Csoportok felvétele számozásának abban a sorrendben, amelyben hozzáadása őket. Legfeljebb hét csoportjait felveheti. További gépek vagy replikációs csoportok vehet ezekbe a csoportokba. Figyelje meg, hogy virtuális gép vagy replikációs csoport csak beépíthetők egy helyreállítási terv csoport.
- **Parancsfájl hozzáadása **– parancsprogramok előtti vagy utáni a helyreállítás csoport megtervezése is hozzáadhat. Parancsfájlok hozzáadása hozzáadja a csoport új műveletsorozat. Például az 1-es csoport előtti lépései halmazának nevű létrejön: 1 csoport: előtti lépéseket. Minden előtti lépést belül ez a beállítás látható. Megjegyzés: csak vehet fel egy parancsprogramot a elsődleges webhelyen, ha rendelkezik egy VMM rendszerbe.
- **Kézi művelet hozzáadása**– futtatása előtt vagy után helyreállítási terv csoport kézi műveleteket is hozzáadhat. A helyreállítási terv futtatásakor megáll a pont, ahol az éppen beszúrt a kézi műveletet, és egy párbeszédpanel kéri, hogy a megadhatja, hogy a kézi művelet befejeződött.

## <a name="extend-recovery-plans-with-scripts"></a>Kijelölés méretének növelése a helyreállítási tervek parancsfájlok

A helyreállítási terv vehet egy parancsprogramot:

- Ha egy VMM forráswebhely parancsfájl az VMM kiszolgálón hozza létre, és adja hozzá a helyreállítási terv.
- Ha Ön éppen esetében, amelyek Azure Azure automatizálási runbooks integrálhatja a helyreállítási tervbe

### <a name="create-a-vmm-script"></a>Hozzon létre egy VMM parancsfájl


Parancsfájl létrehozása az alábbi képlettel történik:

1. Hozzon létre egy új mappát tár megosztása \<VMMServerName > \MSSCVMMLibrary\RPScripts. Helyezése a forrás és cél VMM kiszolgálók.
2. Hozzon létre a parancsprogram (például RPScript), és ellenőrizze a várt módon működik.
3. Helyezze a parancsfájl helyre \<VMMServerName > forrás- és célwebhelyek VMM kiszolgálókon \MSSCVMMLibrary.

### <a name="create-an-azure-automation-runbook"></a>Hozzon létre egy Azure automatizálási runbook

A helyreállítási terv bővítheti a terv részeként az Azure automatizálási runbook futtatásával. [További információ:](site-recovery-runbook-automation.md).


### <a name="add-custom-settings-to-a-recovery-plan"></a>Helyreállítási csomagra egyéni beállítások hozzáadása

1. Nyissa meg a testre szabni kívánt helyreállítási csomagot.
2. A virtuális gépeken futó beírásához kattintson ide vagy az új csoportot.
3. Kézi vagy parancsprogram hozzáadása a művelet a kattintson a **lépés** minden elemet, és válassza a **parancsfájlt** vagy **Kézi művelet**. Adja meg, hogy a parancsfájlt vagy a művelet előtt vagy után a kijelölt elem kíván hozzáadni. A **Fel** és **Le** gomb gombok segítségével felfelé vagy lefelé mozgatása a parancsfájl pozícióját.
4. Ha ad VMM parancsfájl, jelölje be a **VMM parancsfájl áttérni**, és **Parancsfájl** elérési útja írja be a megosztás relatív elérési útját. Igen, ahol a megosztás található a példában szereplőhöz \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, adja meg, az elérési út: \RPScripts\RPScript.PS1.
5. Ha ad az Azure automatizálási könyv futtatni, adja meg a runbook található **Automatizálás Azure-fiókjába** , és jelölje ki a megfelelő **Azure Runbook parancsfájl**.
5. Végezze el annak biztosítására, hogy a parancsprogram a várt módon működik a helyreállítási terv feladatátvevő.


## <a name="next-steps"></a>Következő lépések

Különböző típusú feladatátadás futtatását is lehetővé teszi a helyreállítási tervet, többek között a próba áttérni jelölje be a környezetet és a tervezett vagy nem tervezett feladatátadás. [Tudjon meg többet](site-recovery-failover.md).


 
