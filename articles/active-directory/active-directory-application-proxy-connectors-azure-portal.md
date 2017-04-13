<properties
    pageTitle="Azure Active Directory alkalmazás proxy pontra összekötők használata |} Microsoft Azure"
    description="Bemutatja, hogyan hozhat létre és Azure Active Directory alkalmazás proxy pontra összekötők adatcsoport kezelésére."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups---public-preview"></a>Külön hálózatok és az összekötő csoportok - Public Preview használata helyek alkalmazások közzététele

> [AZURE.SELECTOR]
- [Azure portál](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure klasszikus portál](active-directory-application-proxy-connectors.md)


Összekötő csoportok különböző forgatókönyvekben, beleértve a hasznosak:

- Webhelyek több egymáshoz kapcsolódó adatközpontokkal. Ebben az esetben meg szeretné őrizni minél nagyobb a forgalmat a adatközponthoz belül a lehető mert határokon-adatközponthoz hivatkozások drága és lassú. Az egyes adatközponthoz csak a belül az Adatközpont telepített alkalmazások szolgáló összekötők telepítheti. Ezt a megközelítést kis méretűre állítása a határokon-adatközponthoz hivatkozások és teljesen átlátszó szolgáltatásokat nyújt a felhasználóknak.
- Elszigetelt hálózatok, amelyek nem a fő vállalati hálózat részét a telepített alkalmazások kezelése. Összekötő csoportok segítségével dedikált összekötők telepítése elkülöníteni hálózatok is elkülöníteni alkalmazások a hálózathoz.
- Összekötő csoportok alkalmazások felhőelérés IaaS van telepítve, az access az összes alkalmazás biztonságos egy közös szolgáltatásnak a biztosítása. Összekötő csoportok nem további függőség létrehozása a vállalati hálózaton, vagy az alkalmazás élmény részlet. Összekötők minden felhő adatközponthoz telepíthető, és csak a hálózat tárolnak alkalmazások szolgálnak. Több összekötők magas rendelkezésre állás érdekében telepítheti.
- Több elem erdőkörnyezetben, amelyben bizonyos összekötők erdőnként rendszerbe és kiszolgálására egyes alkalmazások beállítása támogatása.
- Összekötő csoportok használhatók vagy feltárása feladatátvevő vészhelyreállítás webhelyeken, vagy biztonsági másolatot a fő webhely.
- Összekötő csoportok is használható több cégek szolgálnak az egyetlen bérlői webhelyre.

## <a name="prerequisite-create-your-connectors"></a>Előfeltétel: Az összekötők létrehozása
A összekötőkhöz csoportosítani, ügyeljen az alábbiakra van [telepítve több összekötőkhöz](active-directory-application-proxy-enable.md). Ha egy új összekötőre, automatikusan az **alapértelmezett** összekötő csoport fűz össze.

## <a name="step-1-create-connector-groups"></a>Lépés: 1: Összekötő csoportok létrehozása
Összekötő csoportot hozhat létre. Összekötő csoport létrehozása az [Azure portál](https://portal.azure.com)hajtható végre.

1. Jelölje ki az **Azure Active Directory** nyissa meg a könyvtár az adatkezelési irányítópult. Itt jelölje be a **vállalati alkalmazások** > **szolgáltatásalkalmazás-proxy**.

2. Az **Összekötő csoportok** gombra. Az új összekötő csoport lap jelenik meg.

3. Adjon nevet az új összekötő csoportnak, majd a legördülő menü segítségével válassza ki, mely összekötők tartoznak ebben a csoportban.

4. Válassza a **Mentés** , az összekötő csoportjában befejezésekor.

## <a name="step-2-assign-applications-to-your-connector-groups"></a>Lépés: 2: Az összekötő csoportok alkalmazások hozzárendelése
Az utolsó lépésként az összekötő csoport fog szolgálni minden alkalmazás beállítása.

1. Az adatkezelési a címtár-irányítópult, jelölje be a **vállalati alkalmazások** > **minden alkalmazás** > az összekötő csoportba hozzárendelni kívánt alkalmazás > **Szolgáltatásalkalmazás-Proxy**.
2. A **csoport összekötő**a legördülő menü segítségével válassza ki a csoportot, a használni kívánt alkalmazást.
3. Válassza a **Mentés** a módosítás érvénybe léptetéséhez.


## <a name="see-also"></a>Lásd még:

- [Szolgáltatásalkalmazás-Proxy engedélyezése](active-directory-application-proxy-enable.md)
- [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
- [Feltételes elérésének engedélyezése](active-directory-application-proxy-conditional-access.md)
- [Szolgáltatásalkalmazás-Proxy tapasztal kapcsolatos problémák megoldása](active-directory-application-proxy-troubleshoot.md)

A legújabb hírek és frissítések tanulmányozza a [blog szolgáltatásalkalmazás-Proxy](http://blogs.technet.com/b/applicationproxyblog/)
