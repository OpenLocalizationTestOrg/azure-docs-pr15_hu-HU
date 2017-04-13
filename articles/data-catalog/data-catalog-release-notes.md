<properties
   pageTitle="Azure adatkatalógus kibocsátási megjegyzések |} Microsoft Azure"
   description="Kibocsátási megjegyzések az Azure adatkatalógus."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-release-notes"></a>Azure adatkatalógus kibocsátási megjegyzések

## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Jegyzetek 2015 a November 20, engedje fel az Azure-adatkatalógus

### <a name="opening-data-sources-in-power-bi-desktop"></a>Adatforrások a Power BI Desktop megnyitása

Amikor a "Nyissa meg a Power BI Desktop" lehetőséget az **Azure adatkatalógus** portálról, felhasználók előfordulhatnak a Power BI Desktop alkalmazásával kapcsolatos problémák két egyikét:

- Megjelenik egy párbeszédpanel, "Nem sikerült a megnyitott dokumentum" címmel
- A Power BI Desktop alkalmazás megjelenik, de a fájlt úgy tűnik, hogy üres

Az egyes esetében a probléma le, és a Power BI Desktop legújabb verziójának telepítése a [PowerBI.com](https://powerbi.com)oldható.

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Jegyzetek 2015 a November 13, engedje fel az Azure-adatkatalógus

### <a name="registering-and-connecting-to-teradata"></a>Nyilvántartására és csatlakozás Teradata

Csatlakozás Teradata adatforrásokhoz felhasználók telepítette a megfelelő Teradata ODBC-illesztőprogram, amelyek megegyeznek a szoftver használatban bitszámértékének (32 bites vagy 64 bites).

LÉPETT megjelenés dátumot, kompatibilis a legutóbbi [Teradata ODBC-illesztőprogram Windows (15.10 verzió)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) Office 2013, de nem az Office 2016-ban.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Jegyzetek 2015 a július 13, engedje fel az Azure-adatkatalógus

### <a name="registering-and-connecting-to-oracle-database"></a>Regisztrálás és csatlakozás Oracle-adatbázishoz

Ha az Oracle-adatbázis adatforrásokhoz felhasználók telepítette a megfelelő Oracle-illesztőprogramok, amelyek megegyeznek a szoftver használatban bitszámértékének (32 bites vagy 64 bites).

-   Amikor regisztrál az Oracle-adatforrások 32 bites Windows rendszerű számítógépen, a 32 bites Oracle-illesztőprogramok lesz.
-   Amikor regisztrál az Oracle-adatforrások 64 bites Windows rendszerű számítógépen, a 64 bites Oracle-illesztőprogramok lesz.
-   A Microsoft Office 32 bites verzióját futtató számítógépen használja az Excel Oracle adatforrásokhoz csatlakozáskor a 64 bites Windowson, beleértve a 32 bites Oracle-illesztőprogramok lesz
-   Amikor csatlakozik az Oracle adatforrásokat a Microsoft Office 64 bites verzióját futtató számítógépen használja az Excel, a 64 bites Oracle-illesztőprogramok lesz

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Regisztrálása, és Csatlakozás SQL Server Reporting Services

Az SQL Server Reporting Services (SSRS) adatforrások támogatása az éppen natív mód kiszolgálókon csak korlátozott. SharePoint-üzemmódban SSRS támogatása belekerül az későbbi kiadásokban.

### <a name="opening-data-assets-in-excel"></a>Az Excel adatok eszközök megnyitása

Megnyitásakor adatok eszközök a Microsoft Excel az **Azure adatkatalógus** portálról, kéri, a **Microsoft Excel biztonsági értesítés** párbeszédpanelen. Ez a normál, várható működését és a felhasználók is jelölje be a **engedélyezése** továbbra is.

További tudnivalókért lásd: [a gyanús webhelyekre mutató hivatkozásokkal kapcsolatos biztonsági riasztások engedélyezése és letiltása](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>A forrás regisztrációs adatok és a proxy és házirend beállítása

Felhasználók előfordulhatnak olyan helyzet, ha azokat is jelentkezzen be az adatkatalógusban Azure-portálra, de amikor megpróbálnak jelentkezzen be az adatok forrása regisztrációs eszköz találkoznak hibaüzenet jelenik meg, hogy megakadályozza, hogy a bejelentkezés.

Létezik a probléma megoldásához két lehetséges okai:

**OK 1: az Active Directory összevonási szolgáltatások konfigurációs** Az adatok forrása regisztrációs eszköz űrlapok hitelesítést használ, felhasználói bejelentkezések, az Active Directory ellen érvényesítéséhez. A sikeres bejelentkezési űrlap-hitelesítés engedélyezve kell lennie az a globális hitelesítési házirend az Active Directory-rendszergazda által.

Bizonyos esetekben a hiba akkor fordulhat csak akkor, amikor a felhasználó be van kapcsolva a vállalati hálózaton, vagy csak akkor, amikor a felhasználó a csatlakozás a vállalati hálózaton kívüli. A globális hitelesítési házirenddel intranetes és extranetes kapcsolatok külön-külön engedélyezésük hitelesítési módszereket. Bejelentkezési hibák akkor fordulhat elő, ha az űrlap-hitelesítés engedélyezve van a hálózaton, amelyből a felhasználó csatlakozik.

További tudnivalókért olvassa el a [Hitelesítési házirendek beállítása](https://technet.microsoft.com/library/dn486781.aspx)című témakört.

**2 okai lehetnek: hálózati proxy konfiguráció** Ha a vállalati hálózathoz proxykiszolgálót használ, a regisztrációs eszköz nem lehet csatlakozni Azure Active Directory proxyn keresztül. Felhasználók biztosítható, hogy a regisztrációs eszköz szerkesztésével, hogy az eszköz konfigurációs fájl, ez a szakasz hozzáadása a fájlhoz:


      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Keresse meg a RegistrationTool.exe.config fájlt, indítsa el a regisztrációs eszközt, és nyissa meg a Windows Feladatkezelő segédprogramot. A Feladatkezelő részletei lapon kattintson a jobb gombbal a RegistrationTool.exe, és a legördülő menüből válassza a fájl helyének megnyitása.
