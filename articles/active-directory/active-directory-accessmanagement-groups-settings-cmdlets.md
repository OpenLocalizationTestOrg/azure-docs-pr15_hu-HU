<properties
    pageTitle="Azure Active Directory-parancsmagjai csoportbeállítások beállítása |} Microsoft Azure"
    description="Hogyan Azure Active Directory-parancsmagok használata csoportok beállításainak kezeléséhez."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="curtand"/>


# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory-parancsmagjai csoport beállításainak konfigurálása

Az alábbi beállításokat egységesített csoportok címtárában állítható be:

1.  Osztályozás: a vesszővel tagolt listáját, hogy a felhasználók hogyan hozhatnak csoporton sorosztály. Példák a következő lesz "Classified", "Titkos" és "Felső titkos."

2.  Használati irányelvek URL-címe: egy URL-címet, amely a csoportok egyesített feltételei mutat felhasználók a szervezet által meghatározott. Ez az URL-címe megjelenik a felhasználói felület, ahol a felhasználók csoportok használatáról.

3.  Csoport létrehozása engedélyezett: e sem sikerül, vagy az összes felhasználó hozhatnak létre csoportokat az egyesített. Amikor beállításához a minden felhasználónak csoportokat hozhat létre. Ha a beállítás ki állásba, senki csoportokat hozhat létre. Ha a kikapcsolva, is megadhatja egy olyan biztonsági csoportot, amelynek a felhasználók, akik továbbra is hozhatnak létre csoportokat.

Ezekkel a beállításokkal állíthatók be a beállítások és SettingsTemplate objektumokat. Első lépésként nem jelenik meg minden beállítások objektumok a címtárában található. Ez azt jelenti, hogy a címtárában van beállítva az alapértelmezett beállításokat. Az alapértelmezett beállítások módosításához létrehoz egy új settings objektum beállítások sablon használatával. Beállítások sablonok a Microsoft határozza meg.

A parancsmagok ezeket a műveleteket a [Microsoft Connect webhelyen](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)használt tartalmazó modulja töltheti le.

## <a name="create-settings-at-the-directory-level"></a>A címtár-szintjén beállítások létrehozása

Ezeket a lépéseket létrehozása beállítások címtár szinten, amely alkalmazása az összes Office-csoportok a címtárban.

1. Ha nem tudja, hogy melyik SettingTemplate használni, az ezzel a parancsmaggal a beállítások sablonok listájának adja eredményül:

    `Get-MsolAllSettingTemplate`

    ![A beállítások sablonok listájában](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)

2. Használatát iránymutatást URL-cím hozzáadásához előbb kell beszereznie az SettingsTemplate objektumra, amely definiálja az URL-címe értéket használatát iránymutatást; Ez azt jelenti, hogy a Group.Unified sablon:

    `$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`

3. Ezután hozzon létre a sablonon alapuló új beállítások objektum:

    `$setting = $template.CreateSettingsObject()`

4. Frissítse a használatát iránymutatást értéket:

    `$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`

5. Végezetül alkalmazza a beállításokat:

    `New-MsolSettings –SettingsObject $setting`

    ![Használatát iránymutatást URL-cím hozzáadása](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

Az alábbiakban a Group.Unified SettingsTemplate a definiált beállításokat.

 **A beállítás**                          | **Leírás**                                                                                             
--------------------------------------|-----------------------------------------------
 <ul><li>ClassificationList<li>Írja be: karakterlánc<li>Alapértelmezés: ""                  | Egyesített csoportokba alkalmazott érvényes besorolás értékek vesszővel tagolt listáját.                
 <ul><li>EnableGroupCreation<li>Írja be: logikai<li>Alapértelmezett: igaz              | A jelölőre, amely azt jelzi, hogy engedélyezve van-e egységesített csoport létrehozása a címtárban.                               
 <ul><li>GroupCreationAllowedGroupId<li>Írja be: karakterlánc<li>Alapértelmezés: ""         | A biztonsági csoport, amely hozhatnak létre csoportokat az egyesített globálisan egyedi azonosítója akkor is, ha EnableGroupCreation == hamis.
 <ul><li>UsageGuidelinesUrl<li>Írja be: karakterlánc<li>Alapértelmezés: ""                  | A csoport használati irányelvek mutató hivatkozás.                                                                       

## <a name="read-settings-at-the-directory-level"></a>Olvassa el a címtár-szintjén beállításai

Ezeket a lépéseket olvassa el a címtár szintjén beállításokat, amelyek alkalmazása az összes Office-csoportok a címtárban.

1. Olvassa el az összes meglévő Webhelycímtár beállítása:

    `Get-MsolAllSettings`

2. Olvassa el az adott csoport összes beállításait:

    `Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`

3. Olvassa el az adott címtár beállításai SettingId globálisan egyedi azonosítója segítségével:

    `Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

    ![Beállítások azonosító globálisan egyedi azonosítója](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## <a name="update-settings-at-the-directory-level"></a>A címtár-szintjén beállításainak frissítése

Ezeket a lépéseket frissítése címtár szintjén beállításokat, amelyek alkalmazása az összes Office-csoportok a címtárban.

1. A meglévő beállítások objektumot kap:

    `$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

2. A frissíteni kívánt érték jelenik meg:

    `$value = $Setting.GetSettingsValue()`

3. Az érték frissítése:

    `$value["AllowToAddGuests"] = "false"`

4. A beállítás módosítása:

    `Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## <a name="remove-settings-at-the-directory-level"></a>A címtár-szintjén beállítások eltávolítása

Ebben a lépésben eltávolítja a címtár szintjén beállításokat, amelyek alkalmazása az összes Office-csoportok a címtárban.

    `Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## <a name="cmdlet-syntax-reference"></a>Parancsmag szintaxisához

Az [Azure Active Directory-parancsmagok](http://go.microsoft.com/fwlink/p/?LinkId=808260)található további Azure Active Directory PowerShell dokumentációt.

## <a name="settingstemplate-object-reference-groupunified-settingstemplate-object"></a>SettingsTemplate objektumhivatkozások (Group.Unified SettingsTemplate objektum)

- "név": "EnableGroupCreation", "típus": "Értéket-adna", "defaultValue": "igaz", "leírás": "Logikai ablakban a jelölő jelzi, ha a egységesített csoport létrehozását, hogy a."

- "név": "GroupCreationAllowedGroupId", "típus": "System.Guid", "defaultValue": "", "leírás": "A biztonsági csoport, amely egységesített csoportokat hozhat létre whitelisted GUID."

- "név": "ClassificationList", "típus": "System.String", "defaultValue": "", "leírás": "Vesszővel tagolt érvényes besorolás értékek listáját, amely egységesített csoportok alkalmazhatók."

- "név": "UsageGuidelinesUrl", "típus": "System.String", "defaultValue": "", "leírás": "A csoport használati irányelvek mutató hivatkozás."

név | típus | defaultValue | Leírás
----------  | ----------  | ---------  | ----------
"EnableGroupCreation"  | "Értéket adna"  | "igaz"  | "Logikai ablakban a jelölő jelzi, ha a egységesített csoport létrehozását, hogy a."
"GroupCreationAllowedGroupId"  | "System.Guid"  | ""  | "A biztonsági csoport, amely az egyesített csoportokat hozhat létre whitelisted GUID."
"ClassificationList"  | "System.String"  | ""  | "Vesszővel tagolt érvényes besorolás értékek listáját, amely a csoportok egyesített alkalmazhatók."
"UsageGuidelinesUrl"  | "System.String"  | ""  | "A csoport használati irányelvek mutató hivatkozás."

## <a name="next-steps"></a>Következő lépések

Az [Azure Active Directory-parancsmagok](http://go.microsoft.com/fwlink/p/?LinkId=808260)található további Azure Active Directory PowerShell dokumentációt.

További útmutatás a Microsoft program manager Papp de Jong [Papp a csoportok Blog](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad)címen érhető el.

* [Erőforrások való hozzáférés kezelése az Azure Active Directory-csoportok](active-directory-manage-groups.md)

* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
