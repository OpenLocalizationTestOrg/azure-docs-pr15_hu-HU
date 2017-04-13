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


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Külön hálózatok és az összekötő csoportokkal helyek alkalmazások közzététele

> [AZURE.SELECTOR]
- [Azure portál](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure klasszikus portál](active-directory-application-proxy-connectors.md)


Összekötő csoportok különböző forgatókönyvekben, beleértve a hasznosak:

- Webhelyek több egymáshoz kapcsolódó adatközpontokkal. Ebben az esetben meg szeretné őrizni minél nagyobb a forgalmat a adatközponthoz belül a lehető mivel határokon-adatközponthoz hivatkozások általában drága és lassú. Az egyes adatközponthoz csak a belül az Adatközpont telepített alkalmazások szolgáló összekötők telepítheti. Ezt a megközelítést kis méretűre állítása a határokon-adatközponthoz hivatkozások, és a felhasználók egy teljesen átlátszóvá felületet nyújt.
- Elszigetelt hálózatok, amelyek nem a fő vállalati hálózat részét a telepített alkalmazások kezelése. Összekötő csoportok segítségével dedikált összekötők telepítése elszigetelt hálózatok is elkülönítése az alkalmazásokat a hálózathoz.
- Összekötő csoportok alkalmazások felhőelérés IaaS van telepítve, az access az összes alkalmazás biztonságos egy közös szolgáltatásnak a biztosítása. Összekötő csoportok nem további függőség létrehozása a vállalati hálózaton, vagy az alkalmazás élmény részlet. Összekötők minden felhő adatközponthoz telepíthető, és csak a hálózat tárolnak alkalmazások szolgálnak. Több összekötők magas rendelkezésre állás érdekében telepítheti.
- Több elem erdőkörnyezetben, amelyben bizonyos összekötők erdőnként rendszerbe és kiszolgálására egyes alkalmazások beállítása támogatása.
- Összekötő csoportok használhatók vagy feltárása feladatátvevő vészhelyreállítás webhelyeken, vagy biztonsági másolatot a fő webhely.
- Összekötő csoportok is használható több cégek szolgálnak az egyetlen bérlői webhelyre.

## <a name="prerequisite-create-your-connectors"></a>Előfeltétel: Az összekötők létrehozása
Annak érdekében, hogy az összekötők csoportosítani, hogy győződjön meg arról, hogy [több összekötőkhöz telepítve](active-directory-application-proxy-enable.md), és Ön a név, majd csoportosíthatja őket. Végül ha adott alkalmazások rendelni.

## <a name="step-1-create-connector-groups"></a>Lépés: 1: Összekötő csoportok létrehozása
Összekötő csoportot hozhat létre. Összekötő csoport létrehozása az Azure klasszikus portálon hajtható végre.

1. Jelölje ki a címtárában, és kattintson a **Konfigurálás**gombra.  
    ![Szolgáltatásalkalmazás-proxy beállítása kép - kattintson összekötő csoportok kezelése](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)

2. Szolgáltatásalkalmazás-Proxy csoportban **Összekötő csoportok kezelése** gombra, és hozzon létre új összekötő csoport azzal, hogy a csoport nevét.  
    ![Alkalmazás proxy összekötő csoportok képernyőképe – új csoport neve](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Lépés: 2: A csoportokhoz rendelése összekötők
Az összekötő csoportok létrehozása után az összekötők áthelyezése a kívánt csoportra.

1. A **Szolgáltatásalkalmazás-Proxy**kattintson a **Összekötők kezelése**elemre.
2. A **csoport**jelölje be a az egyes összekötő kívánt csoportra. Az összekötők szeretne alakítani, kattintson az új csoport active legfeljebb 10 percig is eltarthat.  
    ![Alkalmazás proxy összekötőkhöz képernyőképe - legördülő menüben láthatnak](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>3 lépés: Az összekötő csoportok alkalmazások hozzárendelése
Az utolsó lépésként az összekötő csoport fog szolgálni minden alkalmazás beállítása.

1. Az Azure klasszikus portálon címtárában jelölje be az alkalmazás rendelje hozzá a csoporthoz, és kattintson a **Konfigurálás**gombra.
2. A **összekötő csoport**jelölje ki a csoportot, amelyből a használni kívánt alkalmazást. Ez a változtatás a program azonnal alkalmazza.  
    ![Alkalmazás proxy összekötő csoport képernyőképe - legördülő menüben láthatnak](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)


## <a name="see-also"></a>Lásd még:

- [Szolgáltatásalkalmazás-Proxy engedélyezése](active-directory-application-proxy-enable.md)
- [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md)
- [Feltételes elérésének engedélyezése](active-directory-application-proxy-conditional-access.md)
- [Szolgáltatásalkalmazás-Proxy tapasztal kapcsolatos problémák megoldása](active-directory-application-proxy-troubleshoot.md)

A legújabb hírek és frissítések tanulmányozza a [blog szolgáltatásalkalmazás-Proxy](http://blogs.technet.com/b/applicationproxyblog/)
