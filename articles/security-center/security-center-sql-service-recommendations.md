<properties
   pageTitle="Az SQL Azure szolgáltatás az Azure biztonság otthon védelme |} Microsoft Azure"
   description="A dokumentum címek javaslatok az Azure biztonság otthon, amelyek segítségével Azure SQL szolgáltatás védelme és biztonsági házirendek megfelelően maradjon."
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
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-azure-sql-service-in-azure-security-center"></a>Az SQL Azure szolgáltatás az Azure biztonság otthon védelme

Azure biztonság otthon elemzi a biztonsági állapot Azure erőforrásait. Biztonság otthon azonosítja a potenciális biztonsági rés, amikor javaslatok, amely végigvezeti Önt a folyamaton, konfigurálása a szükséges vezérlőelemeket hoz létre.  Javaslatok Azure erőforrástípus alkalmazása: virtuális gépeken futó VMs hálózatot, és SQL-alkalmazásokat.

Ez a cikk az Azure SQL szolgáltatás vonatkozó javaslatok foglalkozik.  Azure SQL javaslatok Szolgáltatásközpontban körül engedélyezése a naplózás az Azure SQL-kiszolgálók és adatbázisoknál és engedélyezése az SQL-adatbázisok titkosítása.  A hivatkozások segít megérteni a rendelkezésre álló SQL-javaslatok, és mindegyik fog mire alkalmazhat, ha az alábbi táblázatban használják.

## <a name="available-sql-service-recommendations"></a>SQL-szolgáltatás elérhető javaslatok

|Javaslat|Leírás|
|-----|-----|
|[Naplózás az SQL server engedélyezése](security-center-enable-auditing-on-sql-servers.md)|Javasolja, kapcsolja be a naplózás az Azure SQL-kiszolgálók (Azure SQL szolgáltatás csak; nem tartalmazza a virtuális gépeken futó SQL).|
|[Adatbázis SQL a naplózás engedélyezése](security-center-enable-auditing-on-sql-databases.md)|Javasolja, kapcsolja be a naplózás az Azure SQL-adatbázisok (Azure SQL szolgáltatás csak; nem tartalmazza a virtuális gépeken futó SQL).|
|[Áttetsző titkosítás engedélyezéséhez a SQL-adatbázis](security-center-enable-transparent-data-encryption.md)|(Csak az Azure SQL szolgáltatás) SQL-adatbázisok titkosítása engedélyezését javasolja.|

## <a name="see-also"></a>Lásd még:

Szeretne többet megtudni más Azure erőforrástípus vonatkozó javaslatok, olvassa el az alábbiakat:

- [A virtuális gépeken futó az Azure biztonság otthon védelme](security-center-virtual-machine-recommendations.md)
- [Az alkalmazások az Azure biztonság otthon védelme](security-center-application-recommendations.md)
- [A hálózat az Azure biztonság otthon védelme](security-center-network-recommendations.md)

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
