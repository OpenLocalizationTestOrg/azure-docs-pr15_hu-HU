<properties
    pageTitle="Házirend- és Eszközkezelésről csoportbeállítások |} Microsoft Azure"
    description="Csoportházirend és mobileszköz információt tartalmaz a vállalati tulajdonú eszközön használandó kezelés (MDM) beállításait. Ezek a házirendek alkalmazza a program a felhasználó teljes eszközre."
    services="active-directory"
    keywords="Mik azok a házirend csoport és a vállalati állam barangoló, vállalati állam barangoló, windows felhő MDM beállításait"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="group-policy-and-mdm-settings"></a>A csoportházirend- és MDM beállításai

Használja ezeket a csoportházirend- és mobileszköz-kezelés (MDM) beállításai csak vállalati tulajdonú eszközön, mert ezek a házirendek alkalmazva vannak a felhasználó teljes eszköze. Személyes beállítások szinkronizálása letiltása az MDM házirend alkalmazása, felhasználó által birtokolt eszköz negatív hatással lesz, hogy az eszköz használata. Továbbá más felhasználói fiókokat az eszközön is érinti a házirend.

Személyes (nem felügyelt) eszközök barangoló kezelni kívánt nagyvállalatoknak az Azure portal segítségével engedélyezése vagy letiltása a központi helyett csoportházirend vagy-ben.
Az alábbi táblázatban a házirend-beállítások ismertetik.

## <a name="mdm-settings"></a>MDM beállításai
A Mobileszköz házirend-beállítások a Windows 10-es és a Windows 10 Mobile vonatkozik.  Windows 10 Mobile támogatási létezik, csak a Microsoft-fiók alapú barangoló felhasználó OneDrive-fiók használatával.  A "Eszközök és végpontok" rész milyen eszközök támogatják alapú Azure AD-szinkronizálás kapcsolatos részletekért olvassa el.

| név                               | Leírás                                                          |
|------------------------------------|----------------------------------------------------------------------|
| Microsoft-fiókkal kapcsolat engedélyezése | Lehetővé teszi a felhasználóknak hitelesíteni a Microsoft-fiók használata az eszközön |
| Szinkronizálás engedélyezése a saját beállítások             | Lehetővé teszi, hogy a felhasználók számára a Windows és az alkalmazás adatfájl; Ezzel a házirenddel letiltása letiltja a szinkronizálási, valamint biztonsági másolatok mobileszközökön                  |

## <a name="group-policy-settings"></a>Csoportházirend-beállításai
A csoportházirend-beállításai a Windows 10-eszközök, amelyek az Active Directory-tartományhoz vonatkozik. A tábla is régebbi tűnik, hogy a szinkronizálási beállítások kezelése, de, amely nem működnek a vállalati állam barangoló a Windows 10, amelyek beállítások vannak jegyezni, a "Do not use" a Leírás mezőben.

| név                                | Leírás |
|-------------------------------------|-------------|
| Partnerek: Tiltása a Microsoft-fiókok  |Ezt a beállítást megakadályozza, hogy a felhasználók új Microsoft-fiók felvétele az ezen a számítógépen|
| Nem szinkronizálása                         |Megakadályozza, hogy a felhasználók számára a Windows és az alkalmazás adatfájl|
| Nem szinkronizálhatja személyre szabása             |A témák csoport szinkronizálásának letiltása|
| Nem szinkronizálhatja a böngésző beállításai        |Az Internet Explorer csoport szinkronizálásának letiltása|
| Nem szinkronizálhatja a jelszavak               |Jelszavak csoport szinkronizálásának letiltása|
| Nem szinkronizálhatja más Windows beállításai  |A Windows egyéb beállítások csoport szinkronizálásának letiltása|
| Nem szinkronizálhatja a asztali személyre szabása |Ne használjon; nem befolyásolja|
| A forgalmi díjas kapcsolatokon nem szinkronizálása  |A központi letiltja forgalmi kapcsolatok, például a mobil 3 G|
| Alkalmazások nem szinkronizálása                    |Ne használjon; nem befolyásolja|
|Nem szinkronizálása az alkalmazás beállításai             |Az alkalmazás adatok barangoló letiltása|
|Nem szinkronizálhatja a kezdő beállításai           |Ne használjon; nem befolyásolja|


## <a name="related-topics"></a>Kapcsolódó témakörök
- [Vállalati állam barangoló – áttekintés](active-directory-windows-enterprise-state-roaming-overview.md)
- [Engedélyezze az Azure Active Directory barangoló vállalati állam](active-directory-windows-enterprise-state-roaming-enable.md)
- [És az adatfájl barangoló – gyakori kérdések](active-directory-windows-enterprise-state-roaming-faqs.md)
- [A Windows 10 központi beállítások hivatkozás](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
