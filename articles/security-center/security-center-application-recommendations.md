<properties
   pageTitle="Az alkalmazások az Azure biztonság otthon védelme |} Microsoft Azure"
   description="A dokumentum címek javaslatok az Azure biztonság otthon, amelyek segítségével az Azure alkalmazások védelme és biztonsági házirendek megfelelően maradjon."
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

# <a name="protecting-your-applications-in-azure-security-center"></a>Az alkalmazások az Azure biztonság otthon védelme

Azure biztonság otthon elemzi a biztonsági állapot Azure erőforrásait. Biztonság otthon azonosítja a potenciális biztonsági rés, amikor javaslatok, amely végigvezeti Önt a folyamaton, konfigurálása a szükséges vezérlőelemeket hoz létre.  Javaslatok Azure erőforrástípus alkalmazása: virtuális gépeken futó VMs hálózatot, és SQL-alkalmazásokat.

Ez a cikk alkalmazások vonatkozó javaslatok foglalkozik.  Alkalmazás javaslatok központ a webes alkalmazás tűzfalat telepítésének körül.  A hivatkozások segít megérteni a rendelkezésre álló javaslatok, és mindegyik fog mire alkalmazhat, ha az alábbi táblázatban használják.

## <a name="available-application-recommendations"></a>Rendelkezésre álló javaslatok

|Javaslat|Leírás|
|-----|-----|
|[A webes alkalmazás tűzfal hozzáadása](security-center-add-web-application-firewall.md)|Javasolja, hogy a webes alkalmazás tűzfalat (WAF) webes végpontok rendszerbe. A biztonság otthon több webalkalmazások megvédheti ezeket az alkalmazásokat, a meglévő WAF környezetekben való hozzáadásával. (Az erőforrás-kezelő telepítési modell használatával létrehozott) WAF készülékek kell telepíthetők külön virtuális hálózathoz. (A klasszikus telepítési modell használatával létrehozott) WAF készülékek korlátozva hálózati biztonsági csoport használata. Ez a támogatás a jövőben kiterjesztik egy teljesen testre szabott példányhoz egy WAF készülék (klasszikus).|
|[Alkalmazás védelmének véglegesítése](security-center-add-web-application-firewall.md#finalize-application-protection)|Egy WAF konfigurációja befejezéséhez forgalom WAF készülékre kell átirányítva. Ennek ellenére követő befejezi a szükséges az oldalbeállítások módosításához.|

## <a name="see-also"></a>Lásd még:

Szeretne többet megtudni más Azure erőforrástípus vonatkozó javaslatok, olvassa el az alábbiakat:

- [A virtuális gépeken futó az Azure biztonság otthon védelme](security-center-virtual-machine-recommendations.md)
- [A hálózat az Azure biztonság otthon védelme](security-center-network-recommendations.md)
- [Az Azure SQL Azure-biztonság otthon szolgáltatásba védelme](security-center-sql-service-recommendations.md)

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
