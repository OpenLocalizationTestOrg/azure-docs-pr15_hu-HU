<properties
   pageTitle="Azure biztonság otthon riasztások integrálása az Azure napló integrációs (előzetes verzió) |} Microsoft Azure"
   description="Ez a cikk segít az első lépések a biztonság otthon riasztások integrálása az Azure napló integráció."
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
   ms.date="08/08/2016"
   ms.author="terrylan"/>

# <a name="integrating-security-center-alerts-with-azure-log-integration-preview"></a>Biztonság otthon riasztások integrálása az Azure napló integrációs (előzetes verzió)

Számos biztonsági műveletek és az esemény válasz csapatok támaszkodhat biztonsági információk és esemény-kezelés (SIEM) megoldást triaging és vizsgálja meg a biztonsági figyelmeztetések kezdőpontjának. Azure napló-integrációval ügyfelek szinkronizálhat Azure biztonság otthon riasztásokat, együtt virtuális gép biztonsági események gyűjti össze a Azure diagnosztikai és a naplókat Azure, a naplófájl analytics vagy SIEM metaadattárához a valós időben követheti.

Azure napló integrációs működik-e a HP ArcSight, Splunk, IBM QRadar és másokkal.

## <a name="what-logs-can-i-integrate"></a>Milyen naplók is integrálni?

Azure minden kiterjedt naplózás eredményez. Ezek a naplók kategóriába sorolt:

- A **vezérlő/adatkezelési naplók** amelyek betekintést kap abba, hogy az Azure erőforrás-kezelő létrehozása, frissítése és törlése műveletek.
- **Adatok sík naplók** ad betekintést kap abba, az események következik be, amikor egy Azure erőforrás használatával. Példa a Windows eseménynaplójának - biztonsági és alkalmazás naplózza virtuális gépen.

Azure napló integrációs jelenleg támogatott integrációja:

- Azure virtuális naplók
- Azure naplókat
- Azure biztonság otthon értesítések

## <a name="install-azure-log-integration"></a>Telepítse az Azure napló-integráció

Töltse le a [Azure jelentkezzen integráció](https://www.microsoft.com/download/details.aspx?id=53324).

Az Azure naplózási integrációs szolgáltatás telemetriai adatokat gyűjt a a számítógépről, amelyen telepítve van.  Összegyűjtött telemetriai adatokat a következő:

- Azure végrehajtása során fellépő kivételek jelentkezzen integrációja
- Tudnivalók a lekérdezések és feldolgozása események száma mérőszámok
- Melyik Azlog.exe kapcsolatos parancssori kapcsolói használják statisztika

> [AZURE.NOTE] Adatgyűjtés telemetriai kikapcsolhatja ezt a beállítást jelölésének törlése.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integrálása az Azure naplókat és biztonság otthon értesítések

1. Nyissa meg a parancssor és **CD-re** **c:\Program Files\Microsoft Azure napló integrációs**be.

2. Futtassa az [Azure Active Directory szolgáltatás egyszerű](../active-directory/active-directory-application-objects.md) létrehozása az Azure Active Directory (AD) bérlők, amely az Azure előfizetések tárolni a **azlog createazureid** parancsot.

    Az Azure bejelentkezés kéri.

    > [AZURE.NOTE] Az előfizetés tulajdonosa vagy az előfizetés a közös rendszergazdájának kell lennie.

    Azure-hitelesítés és Azure Active Directory történik.  Azure napló integráció a szolgáltatás rendszerbiztonsági tag létrehozása az Azure Active Directory identitás Azure előfizetésekről olvasási hozzáférést biztosítandó hozzon létre.

3. Futtassa a **azlog engedélyezheti <SubscriptionID> ** az előfizetés olvasó elérésének hozzárendelése a szolgáltatás egyszerű lépés: 2 létrehozott parancsot. Ha egy **SubscriptionID**nem adja meg, majd a szolgáltatás egyszerű jogokat kap az Olvasó szerepkör összes előfizetést, amelyhez hozzáférése van.

    > [AZURE.NOTE] Figyelmeztetések a **engedélyezheti** parancs futtatása közvetlenül azután, hogy a **createazureid** parancs, mert bizonyos késés az Azure Active Directory-fiók létrehozásakor, és a fiók esetén használható között látható. Ha a **createazureid** parancs futtatása a **engedélyezheti** parancs futtatása után körülbelül 10 másodperc várakozás, majd nem kellene látnia ilyen figyelmeztetések.

4. Jelölje be a következő mappák győződjön meg arról, hogy a JSON naplófájlok van:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Jelölje be az alábbi mappák kattintva erősítse meg, hogy biztonság otthon riasztások megtalálható őket:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. A szokásos SIEM továbbító összekötő mutasson az adatokat az SIEM példány pipe a megfelelő mappát. A SIEM konfigurálásról hivatkozzon [SIEM konfigurációk](https://azsiempublicdrops.blob.core.windows.net/drops/ALL.htm) .

Ha Azure Log integrációs kérdéseket, küldjön e-mailben [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Következő lépések

Azure naplókat és tulajdonság-definíciók kapcsolatos további tudnivalókért lásd:

- [Erőforrás-kezelő műveleteinek naplózása](../resource-group-audit.md)
- [Az előfizetés management események lista](https://msdn.microsoft.com/library/azure/dn931934.aspx) - naplóbeli események beolvasásához.

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – gyakori kérdések a szolgáltatással a keresés.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – legújabb Azure biztonsági híreket és információkat.
