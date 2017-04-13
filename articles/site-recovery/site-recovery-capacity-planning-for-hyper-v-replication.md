<properties
    pageTitle="A Hyper-V kapacitás Csapattervező eszköz futtatása a webhely-helyreállítás |} Microsoft Azure"
    description="Ez a cikk tartalmazza a Hyper-V kapacitás Csapattervező eszközzel Azure webhely helyreállítás"
    services="site-recovery"
    documentationCenter="na"
    authors="rayne-wiselman"
    manager="jwhit"
    editor="" />
<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/12/2016"
    ms.author="raynew" />

# <a name="run-the-hyper-v-capacity-planner-tool-for-site-recovery"></a>A Hyper-V kapacitás Csapattervező eszköz futtatása a webhely-helyreállítás

A webhely-helyreállítás Azure üzembe részeként kell tudni a replikáció és sávszélességre vonatkozó követelmények. A Hyper-V kapacitásának Csapattervező eszköz webhely helyreállításhoz segít a Hyper-V virtuális gép replikációs replikációs és sávszélesség követelményei megállapítása.


Ez a cikk ismerteti, hogyan a Hyper-V kapacitás Csapattervező eszköz futtatása. Ez az eszköz tervezési eszközök kapacitása és [kapacitás tervezés webhely-helyreállítás](site-recovery-capacity-planner.md)leírt információk együtt kell használni.


## <a name="before-you-start"></a>Előzetes teendők

Az eszköz futtatása a Hyper-V kiszolgáló vagy fürt csomópontra elsődleges webhelyéhez. Az eszköz futtatása a Hyper-V kiszolgálók van szüksége:

- Operációs rendszer: a Windows Server® 2012 vagy Windows Server® 2012 R2
- Memória: 20 MB (legalább)
- Processzor: 5 százalék terhelést (legalább)
- Szabad lemezterület: 5 MB (legalább)

Az eszköz futtatása előtt kell előkészítése az elsődleges webhely. Ha meg van replikálása között két helyszíni webhelyek és meg szeretné tekinteni a sávszélesség,, valamint replika kiszolgáló előkészítése kell.


## <a name="step-1-prepare-the-primary-site"></a>Lépés: 1: Az elsődleges webhely előkészítése
1. Az elsődleges webhely ellenőrizze a Hyper-V virtuális gépeken futó való replikáció kívánt és a Hyper-V hosts/fürt amelyen legyenek található teljes listáját. Az eszköz futtatását is lehetővé teszi minden alkalommal, amikor több önálló hosts, illetve egyetlen fürt, de nem mindkettőt együtt. Azt is futtatásához szükséges külön-külön az egyes operációs rendszerek, érdemes gyűjtse össze, és vegye figyelembe az alábbi képlettel történik a Hyper-V kiszolgálók:

  - A Windows Server® 2012 önálló kiszolgálók
  - A Windows Server® 2012 fürt
  - A Windows Server® 2012 R2 önálló kiszolgálók
  - A Windows Server® 2012 R2 fürt

3. A Hyper-V hosts és a fürt WMI távoli hozzáférés engedélyezése. Ez a parancs futtatása a minden server/fürthöz győződjön meg arról, hogy tűzfalszabályokat és felhasználói engedélyek vannak beállítva:

        netsh firewall set service RemoteAdmin enable

5. Engedélyezze a teljesítményét figyelve kiszolgálókon és fürt, az alábbi képlettel történik:

  - Nyissa meg a Windows tűzfal a **Speciális biztonsági** beépülő modult, és engedélyezheti a következő bejövő szabályok: **COM + hálózati hozzáférés (DCOM-IN)** és a **Távoli Eseménynapló kezelése csoport**összes szabály.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Lépés: 2: Felkészülés a replika server (helyszíni való replikáció helyszíni)

Nem kell tennie, ha Ön éppen esetében, amelyek Azure.

Azt javasoljuk, hogy beállítása a Hyper-V állomáshoz a helyreállítási kiszolgálójával, hogy egy üres virtuális replikálhatók sávszélesség ellenőrzéséhez.  Kihagyhatja ezt azonban nem tudja sávszélesség felmérni, hacsak nem hajtja végre.

1. Ha szeretné-e a replika, állítsa be a Hyper-V replika ügynök csomópont használja:

    - Nyissa meg **A Kiszolgálókezelő** **Feladatátvevőfürt-kezelő**.
    - Csatlakozzon a fürthöz, jelölje ki a csoport nevét, és kattintson a **Műveletek** > **Konfigurálása szerepkör** nyissa meg a magas elérhetősége varázslót.
    - Kattintson az **Szerepkör jelölje be** **A Hyper-V replika ügynök**. A varázslóban adja meg a csatlakozási pontot a fürthöz (ügyfél-hozzáférési pont neve) helyőrzőként **NetBIOS nevét** és **IP-címet** . A **Hyper-V replika ügynök** lesz beállítva, így egy ügyfél-hozzáférési pont nevét, vegye figyelembe.
    - Ellenőrizze, hogy a Hyper-V replika ügynök szerepkör sikeresen online állapotba kerül a fürt csomópontjait között történhet fölé. Ehhez kattintson a jobb gombbal a szerepkör **áthelyezése**mutasson, kattintson a **Csomópontra jelölje ki**. Jelölje ki a csomópontot > **OK gombra**.
    - Ha a tanúsítvány-alapú hitelesítés használata esetén ellenőrizze, hogy minden fürt csomópontot, és az ügyfél-hozzáférési ponthoz összes van telepítve a tanúsítvány.
2.  Replika kiszolgáló engedélyezése:

    - Fürt nyissa meg a hiba felülete, csatlakozzon a fürthöz, és kattintson a **szerepkörök** elemre > select szerepkör > **Replikációs**beállításai > **engedélyezése a fürt replika kiszolgáló**. Figyelje meg, hogy használata fürt, a kópia kell a fürt a elsődleges hely a Hyper-V replika ügynök szerepkör bemutató tartalmaz.
    - Nyissa meg a Hyper-V Manager önálló kiszolgáló. Kattintson a **Műveletek** ablaktáblában a kiszolgálón is engedélyezni szeretné **A Hyper-V beállításai** gombra kattintva, majd a **Replikáció konfigurációs** **ezen a számítógépen, mint replika kiszolgáló engedélyezése**.
3. Hitelesítés beállítása:

    - **Hitelesítés** és portokat hitelesítést végezni az elsődleges-kiszolgáló és a hitelesítési portokat módjának kiválasztása. Tanúsítvány esetén kattintson a **Tanúsítvány kiválasztása** jelöljön ki egy parancsra. Ha az elsődleges és helyreállítási a Hyper-V hosts, akkor az ugyanabban a tartományban vagy a megbízható tartományok, használja a Kerberos. Tanúsítványok használata másik tartomány vagy munkacsoport környezetet alakítana ki.
    - **Hitelesítés és tároló** szakaszban engedélyezése **bármelyik** hitelesített (elsődleges) kiszolgáló replikációs adatokat küld a replika kiszolgáló. Kattintson **az OK gombra** , vagy **alkalmazni**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

    - Futtassa a **netsh http megjelenítése servicestate** ellenőrizze, hogy a figyelő fut-e a megadott protokoll/port:  
4. Állítsa be a tűzfalak. A Hyper-V a telepítés során tűzfalszabályokat létrejönnek alapértelmezett forgalmának engedélyezésére a portokat (HTTPS a 443-as, a 80 Kerberos). Engedélyezze a szabályok az alábbi képlettel történik:

        - Certificate authentication on cluster (443): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}**
        - Kerberos authentication on cluster (80): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}**
        - Certificate authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"**
        - Kerberos authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"**

## <a name="step-3-run-the-capacity-planner-tool"></a>3 lépés: A kapacitás Csapattervező eszköz futtatása

Az elsődleges hely készített és helyreállítási kiszolgáló beállítása után futtatását is lehetővé teszi az eszközt.

1. [Töltse le](https://www.microsoft.com/download/details.aspx?id=39057) a a Microsoft Download Center eszközt.
2. Futtassa az eszközt egy elsődleges kiszolgálója (például egy, az elsődleges fürt a csomópontok). Kattintson a jobb gombbal az .exe fájl, és válassza a **Futtatás rendszergazdaként**.
3. **Első lépések** adja meg, hogy mennyi ideig adatgyűjtés szeretné. Azt javasoljuk, hogy az eszközt futtatva munkakörnyezeti órán belül annak érdekében, hogy adatait képviselő. Ha csak a hálózati kapcsolat érvényesítéséhez próbálja, csak egy percig összegyűjtheti.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. **Elsődleges hely adatai** adja meg a kiszolgáló nevét, vagy teljesen minősített tartománynév egy különálló szolgáltatója vagy a fürt adja meg az ügyfél teljes Tartománynevét fogadja el a pont, fürt neve vagy a fürt bármely csomópontjának, és kattintson a **Tovább**gombra. Az eszköz automatikusan észleli azt a szolgáltatást futtató kiszolgáló nevét. Az eszköz a megadott kiszolgálók figyelhető VMs felveszi.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. **Replika webhely** részletei Ha Ön éppen esetében, amelyek Azure vagy éppen esetében, amelyek egy másodlagos adatközponthoz, és még nem állította be a replika kiszolgálón, jelölje ki **replika webhely érintő vizsgálatok ugorja át**. Ha meg vannak esetében, amelyek egy másodlagos adatközponthoz, és be van állítva egy replika típusú a teljesen minősített tartománynév az önálló kiszolgáló vagy a **kiszolgáló nevét (**vagy) a Hyper-V replika ügynök Vonalvég fürt ügyfél-hozzáférési pont.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. A **bővített replika** részletei **ugorja át a bővített replika webhely érintő vizsgálatok**engedélyezése. Azok a webhely helyreállítási által nem támogatott.
7. **Válassza a VMs való replikáció** az eszközök csatlakozik a kiszolgáló vagy fürt vagy VMs jeleníti meg, és a lemezt a elsődleges kiszolgálón futó, a beállításoknak megfelelően megadott **Elsődleges hely a részletek** lapon. Nem látható, hogy nem fut a replikáció, vagy már engedélyezett VMs jelenik meg. Jelölje ki a mértékek összegyűjtéséhez kívánt VMs. Automatikusan kijelöli a VHD adatokat gyűjt a VMs számára is.
9. Ha beállított egy replika kiszolgálón vagy fürthöz, **hálózati adatok** adja meg a közelítő WAN sávszélesség úgy gondolja, hogy lesz az elsődleges és replika webhelyek között, és jelölje be a tanúsítványok, ha beállított hitelesítés.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

10. Az **összefoglaló** beállításainak ellenőrzése, és kattintson a **következő** mértékek gyűjtése a kezdéshez. Eszköz folyamat és állapot **Kiszámítása kapacitás** lapon megjelenik. Ha az operációs rendszert futtató eszköz befejeződik a **Jelentés megtekintése** a kimenet fölé Ugrás gombra. Alapértelmezés szerint jelentések és a naplókat tárolja **a Csapattervező %systemdrive%\Users\Public\Documents\Capacity**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)


## <a name="step-4-interpret-the-results"></a>Lépés: 4: Az eredmények értelmezése
Az alábbiakban a fontos mérési módja miatt. Mértékek, amely itt nem felsorolt figyelmen kívül hagyható. Azok nem megfelelő webhely-helyreállítás.

### <a name="on-premises-to-on-premises-replication"></a>A helyszíni való replikáció helyszíni
  - Az elsődleges állomás számítási, memória a replikáció hatását
  - Az elsődleges, helyreállítási hosts tároló lemezterületet IOPS a replikáció hatását
  - A sávszélesség-szükséges delta replikációs (MB)
  - Az elsődleges állomás és a helyreállítási host (MB) között megfigyelt hálózati sávszélesség
  - A két hosts fürt közötti aktív párhuzamos pénzátutalásokhoz ideális számú javaslatok

### <a name="on-premises-to-azure-replication"></a>A helyszíni való replikáció Azure
  - Az elsődleges állomás számítási, memória a replikáció hatását
  - Az elsődleges állomás szabad tárterület, IOPS a replikáció hatását
  - A sávszélesség-szükséges delta replikációs (MB)

## <a name="more-resources"></a>További források

- Az eszköz, olvassa el a dokumentumot, az eszköz letöltési olvashatja el részletes információt.
- Megtekintés az eszköz a [TechNet-blog](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx)Kiss Mayer útmutató.
- [Az eredmény érhető el](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) a teljesítmény a helyszíni a Hyper-V replikációs helyszíni vizsgálata



## <a name="next-steps"></a>Következő lépések

Amikor befejezte a kapacitás tervezés indítható el webhely helyreállítási üzembe helyezése:

- [A Hyper-V VMs VMM felhőket a replikáció az Azure](site-recovery-vmm-to-azure.md)
- [Az Azure bizonyos a Hyper-V VMs (nélkül VMM)](site-recovery-hyper-v-site-to-azure.md)
- [A Hyper-V VMs bizonyos VMM webhelyek között](site-recovery-vmm-to-vmm.md)
- [A Hyper-V VMs bizonyos SAN VMM webhelyek között](site-recovery-vmm-san.md)
- [Bizonyos a hyper-V VMs egyetlen VMM kiszolgálón](site-recovery-single-vmm.md)