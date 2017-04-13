<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Lehetővé teszik az Azure Active Directory tartományi szolgáltatások |} Microsoft Azure"
    description="Azure Active Directory tartományi szolgáltatások – első lépések"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="enable-azure-ad-domain-services"></a>Azure Active Directory tartományi szolgáltatások engedélyezése

## <a name="task-3-enable-azure-ad-domain-services"></a>Feladat 3: Azure Active Directory tartományi szolgáltatások engedélyezése
Ebben a feladatban lehetősége van engedélyezni a címtárában az Azure Active Directory tartományi szolgáltatások. Hajtsa végre az alábbi konfigurálási lépéseket ahhoz, hogy a címtárában az Azure Active Directory tartományi szolgáltatások.

1. Nyissa meg az **Azure klasszikus portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Jelölje ki az **Active Directory** -csomópontot, a bal oldali ablaktáblában.

3. Jelölje ki az Azure Active Directory bérlői webhely (könyvtár), amelynek az Azure Active Directory tartományi szolgáltatások engedélyezése szeretné.

    ![Jelölje ki az Azure Active Directory](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Kattintson a **beállítás** fülre.

    ![Konfigurálja a címtár lapja](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Görgessen le a **tartományi szolgáltatások**című szakaszt.

    ![Tartományi szolgáltatások konfigurációs szakasz](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. Váltás a című **e Directory tartományi szolgáltatások engedélyezése** **Igen**lehetőséget. Azt tapasztalja, hogy néhány további beállítási lehetőségei Azure Active Directory tartományi szolgáltatások a lapon látható.

    ![Tartományi szolgáltatások engedélyezése](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] Ha engedélyezi az Azure Active Directory tartományi szolgáltatások, a bérlői webhelyen, a Azure Active Directory hoz létre, és a szükséges felhasználói hitelesítő Kerberos és NTLM hitelesítő adatok tiltva tárolja.

7. Adja meg a **tartományi szolgáltatások DNS-tartomány nevét**.

   - A könyvtár alapértelmezett tartománynevet (Ez azt jelenti, hogy végződésű a **. onmicrosoft.com** tartomány utótag) alapértelmezés szerint be van jelölve.

   - A lista összes van beállítva az Azure Active directory – például ellenőrzött tartományok, valamint a "a tartományok lapon konfigurálható nem ellenőrzött tartományok tartalmazza.

   - Ezenkívül is beírhatja a saját tartománynév. Ebben a példában azt írta be a saját tartománynév "contoso100.com".

     > [AZURE.WARNING] Győződjön meg arról, hogy a tartomány nevét (például a "contoso100.com" tartománynév "contoso100") megadott tartomány előtagját 15-nél kevesebb karakterből áll-e. A tartomány 15 karakternél hosszabb előtaggal nem hozható létre az Azure Active Directory tartományi szolgáltatások tartományhoz.

8. Győződjön meg arról, hogy a választott felügyelt tartományhoz tartozó DNS-tartománynév még nem létezik a virtuális hálózaton. Kifejezetten jelölje be, ha:

   - Ha már azonos nevű DNS tartomány a virtuális hálózaton tartománnyal rendelkezik.

   - a virtuális megadta a hálózatnak egy virtuális Magánhálózati kapcsolat a helyszíni hálózaton, és a tartományt az a DNS-azonos tartománynév kell a helyszíni hálózaton.

   - Ha egy meglévő felhőalapú szolgáltatást ilyen nevű a virtuális hálózaton.

9. A következő lépésként jelöljön ki egy virtuális hálózatot, amelyben meg szeretné Azure Active Directory tartományi szolgáltatások szeretné elérhetővé tenni. Jelölje ki a virtuális hálózati és hozta létre a legördülő lista **Csatlakozás tartományi szolgáltatások virtuális hálózathoz**című dedikált alhálózat.

   - Győződjön meg arról, hogy a megadott virtuális hálózat tartozik egy Azure terület Azure Active Directory tartományi szolgáltatások által támogatott. Keresse meg a [régió szerint Azure szolgáltatások](https://azure.microsoft.com/regions/#services/) lap tudnivalók a Azure régiók, amelyben Azure Active Directory tartományi szolgáltatások érhető el.

   - Egy területet, ahol nem támogatja az Azure Active Directory tartományi szolgáltatások tartozó virtuális hálózatok nem jelennek meg a legördülő listában.
   
   - Azure Active Directory tartományi szolgáltatások használata egy dedikált alhálózat a virtuális hálózaton belül. Gondoskodjon arról, hogy ne jelölje be az átjáró alhálózat. [Hálózati szempontok](active-directory-ds-networking.md)talál. 

   - Erőforrás-kezelő Azure használatával létrehozott virtuális hálózatok Hasonlóképpen, nem jelennek meg a legördülő listában. Erőforrás-kezelő alapú virtuális hálózatok jelenleg által nem támogatott Azure Active Directory tartományi szolgáltatások.

10. Ahhoz, hogy az Azure Active Directory tartományi szolgáltatások, kattintson a lap alján a munkaablakból a **Mentés** gombra.

11. A lapon megjelenik a "függőben..." az Azure Active Directory tartományi szolgáltatások engedélyezése a könyvtár közben állapota.

    ![Tartományi szolgáltatások – Függőben állapot engedélyezése](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Azure Active Directory tartományi szolgáltatások magas elérhetőségét a felügyelt tartomány biztosít. Miután beállította az Azure Active Directory tartományi szolgáltatások, figyelje meg az IP-címek, amelynél tartományi szolgáltatások érhetők el a virtuális hálózati megjelenítés egyesével be. A második IP-cím hamarosan, megjelenik, amint a szolgáltatás lehetővé teszi, hogy a tartomány magas elérhetősége. Ha magas elérhetősége beállítva, és a tartomány aktív, meg kell jelennie két IP-címek a **beállítás** lapon **tartományi szolgáltatások** szakaszában.

12. 20 – 30 perc múlva lásd: az első IP-cím, amelyre tartományi szolgáltatások érhető el a virtuális **IP-cím** mezőjében **a lap** hálózaton.

    ![Enabled - tartományi szolgáltatások első IP kiépítése](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

13. Magas elérhetősége esetén működik-e a tartomány két IP-cím jelenjen meg az oldalon megjelenik. A felügyelt tartomány érhető el a következő két IP-címek a kijelölt virtuális hálózaton. Megjegyzés: az IP-címek le, hogy ha frissíti a DNS-beállítások a virtuális hálózathoz. Ebben a lépésben lehetővé teszi, hogy virtuális gépeken futó való csatlakozáshoz a tartományt, például a tartomány join műveletek a virtuális hálózaton.

    ![Tartományi szolgáltatások engedélyezett - kiépítve mindkét IP-címei](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] A Azure AD-bérlő méretétől függően (szám, a felhasználók, csoportok stb), a felügyelt tartomány szinkronizálás is eltarthat. A szinkronizálási folyamat történik a háttérben. Az objektumok ezer tízesre nagy bérlőkhöz eltarthat egy vagy két felhasználók, csoporttagság és hitelesítő adatait, és a rendszer szinkronizálja a napon.

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Tevékenység 4 - frissítés az Azure virtuális hálózati DNS-beállításait
[A DNS-beállítások az Azure virtuális hálózat](active-directory-ds-getting-started-dns.md)frissítése, akkor a következő konfigurációs feladatot.
