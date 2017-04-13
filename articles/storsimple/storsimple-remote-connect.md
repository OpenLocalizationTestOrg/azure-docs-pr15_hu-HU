<properties 
   pageTitle="Csatlakozás távoli az StorSimple eszköz |} Microsoft Azure"
   description="Megtudhatja, hogy miként konfigurálható az eszközt a távfelügyelet és a Windows PowerShell keresztüli StorSimple HTTP-és HTTPS."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/21/2016"
   ms.author="alkohli" />

# <a name="connect-remotely-to-your-storsimple-device"></a>Csatlakozás távoli StorSimple eszköze

## <a name="overview"></a>– Áttekintés

A Windows PowerShell távelérési StorSimple eszköze csatlakozhat is használhatja. Amikor ilyen módon csatlakozni, menüt nem jelenik meg. (Megjelenik egy menü csak akkor, ha a soros konzol az eszközön való csatlakozáshoz használ.) A Windows PowerShell távelérési kapcsolódni lehet egy adott runspace. Megadhatja, hogy a megjelenítési nyelvet. 

További információt a Windows PowerShell távelérési látni eszköze kezelését, nyissa meg [Az StorSimple StorSimple eszköze felügyeletéhez a Windows PowerShell](storsimple-windows-powershell-administration.md)használatával.

Ebben az oktatóanyagban beállításáról az eszközt a távfelügyelet, majd a csatlakozás a Windows PowerShell-StorSimple ismerteti. Http- vagy HTTPS segítségével csatlakoztatása a Windows PowerShell távelérési keresztül. Jó helyen jár hogyan csatlakozhat a Windows PowerShell-StorSimple kiválasztásakor is vegye figyelembe a következőket: 

- Csatlakozás közvetlenül a soros eszköz-konzol biztonságos, de a soros konzol csatlakozás hálózati kapcsolók keresztül nem. Legyen a biztonsági kockázatot jelentenek a óvatos, csatlakozáskor a soros eszköz konzolhoz hálózati kapcsolók fölé. 

- A HTTP-munkamenet keresztül csatlakozik kínálhatnak nagyobb biztonság, mint a hálózaton keresztül a soros konzol csatlakozik. Bár ez nem a legbiztonságosabb módszer, is elfogadható megbízható hálózatokon. 

- Egy HTTPS-munkamenetet a önaláírt tanúsítvány keresztül csatlakozik, a legbiztonságosabb, és a a javasolt lehetőséget.

A Windows PowerShell-felület távolról csatlakoztathatja. A Windows PowerShell felületén keresztül StorSimple eszközére távelérési alapértelmezés szerint azonban nincs engedélyezve. Az eszközön Távfelügyelet először engedélyeznie kell, és kattintson az ügyfél használt eléréséhez az eszközön.

A jelen cikkben ismertetett lépések végeztek host operációs rendszert futtató Windows Server 2012 R2 rendszeren.

## <a name="connect-through-http"></a>HTTP protokollal csatlakozzon

Nagyobb biztonság, mint a soros konzol a StorSimple eszköz keresztül kapcsolódik egy HTTP-munkamenet keresztül csatlakozik a Windows PowerShell StorSimple kínál. Bár ez nem a legbiztonságosabb módszer, is elfogadható megbízható hálózatokon.

Használhatja az Azure klasszikus portálon vagy a soros konzol távfelügyelet beállítása. Az alábbi eljárások közül választhat:

- [Az Azure klasszikus portal segítségével HTTP engedélyezése a távoli kezelése](#use-the-azure-classic-portal-to-enable-remote-management-over-http)

- [A soros konzol használatával lehetővé teszi a Távfelügyelet HTTP keresztül](#use-the-serial-console-to-enable-remote-management-over-http)

Miután engedélyezte a távfelügyelet, az alábbi eljárással az ügyfél Felkészülés távoli kapcsolatot.

- [Az ügyfél előkészítése távkapcsolat](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-http"></a>Az Azure klasszikus portal segítségével HTTP engedélyezése a távoli kezelése 

Hajtsa végre az alábbi lépéseket a Távfelügyelet ahhoz, hogy HTTP Azure klasszikus portálon.

#### <a name="to-enable-remote-management-through-the-azure-classic-portal"></a>Ahhoz, hogy az Azure klasszikus portálon keresztül Távfelügyelet

1. **Eszközök**elérése > **konfigurálása** az eszközön.

2. Görgessen le a **Távoli kezelése** csoportban.

3. **Engedélyezése a távoli kezelése** állítsa **Igen**értékre.

4. Most már megadhatja HTTP segítségével szeretne csatlakozni. (Az alapértelmezett érték https csatlakozni.) Győződjön meg arról, hogy HTTP van-e jelölve.

    >[AZURE.NOTE] Kapcsolódás HTTP keresztül is elfogadható csak megbízható hálózatokon.

6. A lap alján a **Mentés** gombra.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>A soros konzol használatával lehetővé teszi a Távfelügyelet HTTP keresztül

Hajtsa végre az alábbi lépéseket a soros konzolon eszköz engedélyezése a távoli kezelése.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Ahhoz, hogy a soros eszköz-konzolon keresztül Távfelügyelet

1. A soros konzol menüben jelölje ki az 1 beállítást. További információt a soros konzol használata az eszközön nyissa meg [a Windows PowerShell-eszköz a soros konzol keresztül StorSimple csatlakozás](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. A parancssorba írja be:`Enable-HcsRemoteManagement –AllowHttp`

3. Meg fog értesítést kapni a biztonsági rés HTTP használatának csatlakoztatni az eszközt. Amikor a rendszer kéri, írja be az **Y**erősítse meg.

4. Győződjön meg arról, hogy HTTP engedélyezve van-e beírásával:`Get-HcsSystem`

5. Győződjön meg arról, hogy a **RemoteManagementMode** **HttpsAndHttpEnabled**mutatja. Az alábbi ábrán látható gitt ezeket a beállításokat.

     ![Egymás utáni HTTPS és a HTTP engedélyezve](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>Az ügyfél előkészítése távkapcsolat

A következő lépésekkel engedélyezése a távoli kezelése az ügyfélgépen.

#### <a name="to-prepare-the-client-for-remote-connection"></a>Az ügyfél Felkészülés távkapcsolat

1. Indítsa el a Windows PowerShell-munkamenetet rendszergazdaként.

2. Írja be a StorSimple eszköz IP-címének felvétele az ügyfél megbízható hosts listára a következő parancsot: 

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

     <*Device_ip*> cserélje ki az eszköz; IP-címe Példa: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Adja meg az eszköz hitelesítő adatok mentését típusú változóban a következő parancsot: 

     *$cred = get-hitelesítő adatok*

4. A megjelenő párbeszédpanelen:

    1. Írja be a felhasználó nevét a következő formátumban: *device_ip\SSAdmin*.
    2. Írja be a beállítási varázsló konfigurálták, az eszköz beállítása óta eszközt rendszergazdai jelszót. Az alapértelmezett jelszava *Jelszo1*.

7. Ez a parancs megadásával először az eszközön a Windows PowerShell-munkamenetet:

     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`

     >[AZURE.NOTE] Virtuális StorSimple eszközzel a Windows PowerShell-munkamenetet használatra létrehozásához hozzáfűzése a `–Port` paraméter, és adja meg a nyilvános olyan portot StorSimple virtuális készülék a távelérési konfigurált.

     Az aktív távoli Windows PowerShell-munkamenetet az eszközhöz ezen a ponton kell rendelkeznie.

    ![A PowerShell távelérési HTTP használatával](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>HTTPS protokollal csatlakozzon

Keresztül HTTPS-KAPCSOLATON keresztül csatlakozik a Windows PowerShell StorSimple a legbiztonságosabb és ajánlott mód az távolról Csatlakozás Microsoft Azure StorSimple eszközére. Az alábbi eljárások bemutatják, hogy miként állíthatja be a soros konzol és az ügyfél számítógépeket, így csatlakoztatása a Windows PowerShell-StorSimple HTTPS is használhatja.

Használhatja az Azure klasszikus portálon vagy a soros konzol távfelügyelet beállítása. Az alábbi eljárások közül választhat:

- [Az Azure klasszikus portal segítségével HTTPS engedélyezése a távoli kezelése](#use-the-azure-classic-portal-to-enable-remote-management-over-https)

- [Használja a soros konzolt HTTPS engedélyezése a távoli kezelése](#use-the-serial-console-to-enable-remote-management-over-https)

Miután engedélyezte a távfelügyelet, használatával az alábbi eljárások az állomás előkészítése a távfelügyelet, és csatlakozzon az eszközön, a távoli számítógépről.

- [A host előkészítése Távfelügyelet](#prepare-the-host-for-remote-management)

- [Az eszköz csatlakoztatása a távoli állomáswebhelyén](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-classic-portal-to-enable-remote-management-over-https"></a>Az Azure klasszikus portal segítségével HTTPS engedélyezése a távoli kezelése

A következő lépésekkel HTTPS engedélyezése a távoli kezelése az Azure klasszikus portálon.

#### <a name="to-enable-remote-management-over-https-from-the-azure-classic-portal"></a>Ahhoz, hogy Távfelügyelet HTTPS az Azure klasszikus portálról

1. **Eszközök**elérése > **konfigurálása** az eszközön.

2. Görgessen le a **Távoli kezelése** csoportban.

3. **Engedélyezése a távoli kezelése** állítsa **Igen**értékre.

4. Most már megadhatja a HTTPS csatlakozni. (Az alapértelmezett érték https csatlakozni.) Győződjön meg arról, hogy a HTTPS van-e jelölve. 

5. Kattintson a **Letöltés Távfelügyelet tanúsítvány**. Adjon meg egy helyet a fájl mentéséhez. Szüksége lesz a tanúsítványának telepítése az ügyfél vagy host rendszerű számítógépen használt csatlakoztatni az eszközt.

6. A lap alján a **Mentés** gombra.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>Használja a soros konzolt HTTPS engedélyezése a távoli kezelése

Hajtsa végre az alábbi lépéseket a soros konzolon eszköz engedélyezése a távoli kezelése.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Ahhoz, hogy a soros eszköz-konzolon keresztül Távfelügyelet

1. A soros konzol menüben jelölje ki az 1 beállítást. További információt a soros konzol használata az eszközön nyissa meg [a Windows PowerShell-eszköz a soros konzol keresztül StorSimple csatlakozás](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console).

2. A parancssorba írja be: 

     `Enable-HcsRemoteManagement`

    Ez engedélyezze a HTTPS az eszközön.

3. Győződjön meg arról, hogy engedélyezte-e a HTTPS beírásával: 

     `Get-HcsSystem`

    Győződjön meg arról, hogy a **RemoteManagementMode** **HttpsEnabled**mutatja. Az alábbi ábrán látható gitt ezeket a beállításokat.

     ![Engedélyezve van, a soros HTTPS](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)

4. A kimenő `Get-HcsSystem`, másolja a vágólapra az eszköz dátumértékét és a későbbi felhasználás céljából menteni.

    >[AZURE.NOTE] A sorszám rendeli hozzá a CN a tanúsítvány neve.

5. Távfelügyelet tanúsítvány beszerzése beírásával: 
 
     `Get-HcsRemoteManagementCert`

    Egy tanúsítványt, az alábbihoz hasonlóan fog megjelenni.

    ![Távfelügyelet tanúsítvány beszerzése](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)

5. Másolja az adatokat a tanúsítványban **---KEZDŐ tanúsítvány---** **---** befejezheti a tanúsítvány---.cer fájl szövegszerkesztőben például a Jegyzettömbben, és mentse azt. (, Másolja a fájlt a távoli szolgáltató Ha arra készül, hogy a host.)

    >[AZURE.NOTE] Új tanúsítvány szeretne létrehozni, használja a `Set-HcsRemoteManagementCert` parancsmag.

### <a name="prepare-the-host-for-remote-management"></a>A host előkészítése Távfelügyelet

Felkészülés a számítógép egy távoli kapcsolat által használt HTTPS-KAPCSOLATON keresztül, végezze el az alábbi eljárások valamelyikét:

- [Importálás a .cer fájl az ügyfél vagy a távoli kiszolgáló a legfelső szintű tárolóba](#to-import-the-certificate-on-the-remote-host).

- [Adja hozzá a eszköz dátumértékeket a hosts fájlt a távoli állomáson](#to-add-device-serial-numbers-to-the-remote-host).

Ezeket a műveleteket leírását alatt.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>A távoli állomáson a tanúsítvány importálása

1. Kattintson a jobb gombbal a .cer fájl, és válassza a **tanúsítvány telepítése**. Ez a tanúsítvány importálása varázsló elindul.

    ![Tanúsítvány importálása varázsló 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)

2. **Tár helyét**válassza a **Helyi számítógép zónában**, és kattintson a **Tovább gombra**.

3. Jelölje be **a tárolóban található összes tanúsítvány tárolása**, és kattintson a **Tallózás**gombra. Nyissa meg a távoli gazdaszámítógép a legfelső szintű áruházból, és kattintson a **Tovább gombra**.

    ![Tanúsítvány importálása varázsló 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)

4. Kattintson a **Befejezés gombra**. Megjelenik egy üzenet, amely arról tájékoztat, hogy az importálás sikeres volt.

    ![Tanúsítvány importálása varázsló 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>A távoli állomás eszköz dátumértékeket hozzáadása

1. Indítsa el a Jegyzettömb rendszergazdaként, és nyissa meg a hosts fájlt \Windows\System32\Drivers\etc helyen található.

2. Az alábbi három tételek hozzáadása a hosts fájlhoz: **adatok 0 IP-cím**, a **vezérlő 0 rögzített IP-cím**és a **vezérlő 1 rögzített IP-cím**.

3. Adja meg, amely a korábban mentett eszköz időértékét. Feleltesse meg az IP-cím, az alábbi képen látható módon. A vezérlő 0 és 1 vezérlő fűzze hozzá **Controller0** és **Controller1** végén található a soros számot (CN neve).

    ![Hosts fájl CN nevének hozzáadása](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)

4. Mentse a hosts fájlt.

### <a name="connect-to-the-device-from-the-remote-host"></a>Az eszköz csatlakoztatása a távoli állomáswebhelyén

A Windows PowerShell és SSL-SSAdmin munkamenet eszközön adja meg az ügyfél vagy egy távoli állomástól. A SSAdmin munkamenetre választógombot 1 a [soros konzol](storsimple-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-device-serial-console) menü az eszköz rendeli hozzá.

Az alábbi eljárással azon a számítógépen, amelyhez képest a távoli a Windows PowerShell-kapcsolat létrehozásához.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Egy SSAdmin munkamenet beírása az eszközön a Windows PowerShell és az SSL használatával

1. Indítsa el a Windows PowerShell-munkamenetet rendszergazdaként.

2. Az ügyfél megbízható hosts az eszköz IP-cím hozzáadásához írjon be:

     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`

    Hol található a <*device_ip*> az eszköze; IP-címe Példa: 

     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`

3. Hozzon létre egy új hitelesítő adatok beírásával: 

     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`

    Hol található a < a*célként megadott eszköz IP*> az eszköze; 0 adatok IP-címe Ha például **10.126.173.90** a hosts fájlt a fenti képen látható módon. Is adja meg az eszköz rendszergazdai jelszavát.

4. Munkamenet létrehozása beírásával:

     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`

    A parancsmaggal - számítógépnév paraméterhez <*cél eszköz dátumértékét*> adja. Ez a soros szám adat a hosts fájlban a távoli állomáson; 0 IP-címét is hozzá van rendelve. Ha például **SHX0991003G44MT** az alábbi képen látható módon.

5. Írja be: 

     `Enter-PSSession $session`

6. Meg kell Várjon néhány percet, és ezután fog is kapcsolatban kell az eszköz keresztül HTTPS SSL. Ekkor megjelenik egy üzenet, amely jelzi, hogy az eszköz csatlakozik.

    ![A PowerShell távelérési HTTPS és az SSL használatával](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Következő lépések

- További tudnivalók [felügyelheti StorSimple eszköze a Windows PowerShell használatával](storsimple-windows-powershell-administration.md).

- További tudnivalók [felügyelheti StorSimple eszköze az StorSimple kezelő szolgáltatás használatával](storsimple-manager-service-administration.md).
