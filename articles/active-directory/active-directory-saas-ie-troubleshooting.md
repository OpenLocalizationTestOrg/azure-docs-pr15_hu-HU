<properties
    pageTitle="Az Access Vezérlőpult-kiterjesztés hibaelhárítás az Internet Explorer |} Microsoft Azure"
    description="Hogyan lehet csoportházirend segítségével az Office-alkalmazások portált a az Internet Explorer bővítmény telepítése."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Az Access Vezérlőpult-kiterjesztés hibaelhárítás az Internet Explorer

Ez a cikk segít a következő problémák elhárítása:

- Ön nem fér hozzá az alkalmazások az Office-alkalmazások portálon keresztül az Internet Explorer használata közben.
- A "Szoftver telepítése" üzenet akkor látható, akkor is, ha már telepítette a szoftvert.

Ha Ön rendszergazda, lásd még: [az Internet Explorer csoportházirend segítségével az Access Vezérlőpult-kiterjesztés telepítéséről](active-directory-saas-ie-group-policy.md)

##<a name="run-the-diagnostic-tool"></a>A diagnosztikai eszköz futtatása

Az Access Vezérlőpult-kiterjesztés telepítési problémáinak felderítheti le, és az Access Panel diagnosztikai eszköz futtatása:

1. [Ide kattintva töltse le a diagnosztikai eszközt.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. Nyissa meg a fájlt, és nyomja le **az összes kibontása** gombra.

    ![Nyomja meg az összes kibontása](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. Nyomja le a **kibontása** gombra.

    ![Nyomja le a kivonat](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. Az eszköz futtatása, kattintson a jobb gombbal a **AccessPanelExtensionDiagnosticTool**nevű fájlt, és válassza a **Megnyitás > a Microsoft Windows-alapú Script Host**.

    ![A Megnyitás > a Microsoft Windows Script Host alapján](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. Ezután a következő diagnosztikai ablakot, amelyen leírja, hogy mi történt a telepítés lehet jelenik meg.

    ![Minta: a diagnosztikai ablaka](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. Kattintson az "**Igen**", hogy a program talált elhárításához.

7. A módosítások mentéséhez, zárja be az összes Internet Explorer-ablak, és majd nyissa meg újra az Internet Explorer.<br />Ha még mindig nem tud hozzáférni az alkalmazásokat, próbálkozzon az alábbi lépéseket.

##<a name="check-that-the-access-panel-extension-is-enabled"></a>Ellenőrizze, hogy engedélyezve van-e az Access Vezérlőpult-kiterjesztés

Ellenőrizze, hogy az Access Vezérlőpult-kiterjesztés engedélyezve van-e az Internet Explorerben:

1. Az Internet Explorerben kattintson az ablak jobb felső sarokban lévő **fogaskerék ikonra** . Válassza az **Internetbeállítások parancsra**.<br />(Az Internet Explorer régebbi verzióiban ez csoportban található **Eszközök > Internetbeállítások**.

    ![Kattintson az eszközök > Internetbeállítások](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. Kattintson a **programok** fülre, majd kattintson a **Bővítmények kezelése** gombra.

    ![Kattintson a bővítmények kezelése](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. Ezen a párbeszédpanelen jelölje be a **Vezérlőpult-kiterjesztés Access** , és válassza az **Engedélyezés** gombra.

    ![Kattintson az Engedélyezés gombra](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. A módosítások mentéséhez minden az Internet Explorer-ablak bezárása és nyissa meg az Internet Explorer újra.

##<a name="enable-extensions-for-inprivate-browsing"></a>InPrivate-böngészési bővítmények engedélyezése

Ha az InPrivate-böngészési módban használja:

1. Az Internet Explorerben kattintson az ablak jobb felső sarokban lévő **fogaskerék ikonra** . Válassza az **Internetbeállítások parancsra**.<br />(Az Internet Explorer régebbi verzióiban ez csoportban található **Eszközök > Internetbeállítások**.

    ![Minta: a diagnosztikai ablaka](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. Nyissa meg az **Adatvédelem** fülre, majd **törölje a jelet** az **Eszköztárak és bővítmények, amikor elindítja az InPrivate-böngészési letiltása** jelölőnégyzetet</p>

    ![Törölje a jelet letiltása eszköztárak és bővítmények InPrivate-böngészési indításakor](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. A módosítások mentéséhez minden az Internet Explorer-ablak bezárása és nyissa meg az Internet Explorer újra.

##<a name="uninstall-the-access-panel-extension"></a>Távolítsa el az Access Vezérlőpult-kiterjesztés

Az Access Vezérlőpult-kiterjesztés eltávolítása a számítógépről:

1. A billentyűzeten nyomja le a **Windows-billentyűt** a Start menü megnyitásához. Ha meg nyitva a menüben, bármi való kereséshez írhat. Írja be a "Vezérlőpult", és nyissa meg a **Vezérlőpultot** , amikor megjelenik a keresési eredmények között.

    ![Vezérlőpult keresése](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. A jobb felső sarkában a Vezérlőpultot a **nagy ikonok**módosítsa a **szerinti nézet** lehetőséget. Ezután keresse meg, és kattintson a **Programok és szolgáltatások** gombra.

    ![A nézet cseréli a nagy ikonok megjelenítéséhez.](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. A listából jelölje ki a **Hozzáférési Vezérlőpult-kiterjesztés**, majd kattintson az **Eltávolítás** gombra.

    ![Kattintson az Eltávolítás parancsra](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. Ezután újra szeretné látni, ha a probléma megoldódott a bővítmény telepítése megpróbálhatja.

Ha a bővítmény eltávolítása problémák, azt is távolíthatja el a [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) eszköz használatával.

## <a name="related-articles"></a>Kapcsolódó cikkek

- [Az alkalmazások kezelése az Azure Active Directory cikk indexben](active-directory-apps-index.md)
- [Access alkalmazás és az egyszeri bejelentkezés az Azure Active Directory címtárral](active-directory-appssoaccess-whatis.md)
- [Az Access Vezérlőpult-kiterjesztés telepítéséről az Internet Explorer csoportházirend használatával](active-directory-saas-ie-group-policy.md)
