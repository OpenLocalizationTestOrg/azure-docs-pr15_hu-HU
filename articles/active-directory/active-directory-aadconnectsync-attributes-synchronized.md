<properties
    pageTitle="Azure AD Connect szinkronizálási: Azure Active Directory szinkronizált attribútumok |} Microsoft Azure"
    description="Az attribútumok vannak szinkronizálva az Azure Active Directory listája."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure AD Connect szinkronizálása: Azure Active Directory szinkronizált attribútumok
Ez a témakör felsorolja az attribútumok vannak szinkronizálva az Azure AD Connect szinkronizálás.  
Az attribútumok vannak csoportosítva, a kapcsolódó Azure Active Directory-alkalmazást.

## <a name="attributes-to-synchronize"></a>A szinkronizálandó attribútumok
A gyakori kérdésekre adott, hogy *Mi az a szinkronizálandó minimális attribútum listáját*. Az alapértelmezett és ajánlott megközelítés alapértelmezett attribútumait tartani, hogy egy teljes globális Címlistában (globális címjegyzék) is alakítható ki a felhőben, és az összes-funkciók beszerzése az Office 365-munkaterhelésekből. Egyes esetekben az egyes attribútumok, amely a szervezet nem szeretné a felhővel szinkronizált mivel a következő attribútumok tartalmazhat bizalmas vagy adat (személyes azonosításra alkalmas) adatok, például az ebben a példában:  
![hibás attribútumok](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

Ebben az esetben kezdje azzal attribútumainak a jelen témakör listáját, és ezeket érzékeny vagy adat adatokat tartalmaz, és nem szinkronizálható attribútumok azonosítása. Törölje a jelölést e attribútumok az [Azure Active Directory-alkalmazás és a attribútum szűrés](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering)a telepítés során.

>[AZURE.WARNING] Ha a kijelölés megszüntetése attribútumok, legyen óvatos, és ezek nem feltétlenül lehet szinkronizálni attribútumok csak törölje. Más attribútumait unselecting szolgáltatások negatív hatással lehet.

## <a name="office-365-proplus"></a>Az Office 365 ProPlus

| Attribútumnév| Felhasználói| Megjegyzés |
| --- | :-: | --- |
| accountEnabled| X| Határozza meg, ha egy fiókjához engedélyezve van.|
| CN| X|  |
| displayName| X|  |
| objectSID| X| gépi tulajdonság. AD a felhasználói azonosító karbantarthatja szinkronizálás között Azure Active Directory és AD.|
| pwdLastSet| X| gépi tulajdonság. Mikor érdemes a már kiadott tokenek érvényteleníti szolgál. Jelszó-szinkronizálás és összevonási is használják.|
| sourceAnchor| X| gépi tulajdonság. ÖSSZEADJA és Azure Active Directory kapcsolatának fenntartani megváltoztatható azonosítója.|
| usageLocation| X| gépi tulajdonság. A felhasználó ország. Használja a licencek hozzárendelését.|
| userPrincipalName| X| Egyszerű Felhasználónévi a felhasználónak bejelentkezés Azonosítóját. Leggyakrabban a ugyanaz, mint [levelezés] értéket.|

## <a name="exchange-online"></a>Exchange Online

| Attribútumnév| Felhasználói| Névjegy| Csoport| Megjegyzés |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Határozza meg, ha egy fiókjához engedélyezve van.|
| Segéd| X| X|  |  |
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| További| X| X|  |  |
| Vállalati| X| X|  |  |
| countryCode| X| X|  |  |
| szervezeti egység| X| X|  |  |
| Leírás| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| Lakástelefon| X| X|  |  |
| információ| X| X| X| Az attribútum jelenleg nem felhasznált csoportok számára.|
| Monogram| X| X|  |  |
| l| X| X|  |  |
| legacyExchangeDN| X| X| X|  |
| mailnickname-beli| X| X| X|  |
| mangedBy|  |  | X|  |
| kezelő| X| X|  |  |
| tag|  |  | X|  |
| mobil| X| X|  |  |
| msDS-HABSeniorityIndex| X| X| X|  |
| msDS-PhoneticDisplayName| X| X| X|  |
| msExchArchiveGUID| X|  |  |  |
| msExchArchiveName| X|  |  |  |
| Arról| X| X|  |  |
| msExchAuditAdmin| X|  |  |  |
| msExchAuditDelegate| X|  |  |  |
| msExchAuditDelegateAdmin| X|  |  |  |
| msExchAuditOwner| X|  |  |  |
| msExchBlockedSendersHash| X| X|  |  |
| msExchBypassAudit| X|  |  |  |
| msExchCoManagedByLink|  |  | X|  |
| msExchDelegateListLink| X|  |  |  |
| msExchELCExpirySuspensionEnd| X|  |  |  |
| msExchELCExpirySuspensionStart| X|  |  |  |
| msExchELCMailboxFlags| X|  |  |  |
| msExchEnableModeration| X|  | X|  |
| msExchExtensionCustomAttribute1| X| X| X| Az attribútum jelenleg nem felhasznált az Exchange Online. |
| msExchExtensionCustomAttribute2| X| X| X| Az attribútum jelenleg nem felhasznált az Exchange Online. |
| msExchExtensionCustomAttribute3| X| X| X| Az attribútum jelenleg nem felhasznált az Exchange Online. |
| msExchExtensionCustomAttribute4| X| X| X| Az attribútum jelenleg nem felhasznált az Exchange Online. |
| msExchExtensionCustomAttribute5| X| X| X| Az attribútum jelenleg nem felhasznált az Exchange Online. |
| msExchHideFromAddressLists| X| X| X|  |
| msExchImmutableID| X|  |  |  |
| msExchLitigationHoldDate| X| X| X|  |
| msExchLitigationHoldOwner| X| X| X|  |
| msExchMailboxAuditEnable| X|  |  |  |
| msExchMailboxAuditLogAgeLimit| X|  |  |  |
| msExchMailboxGuid| X|  |  |  |
| msExchModeratedByLink| X| X| X|  |
| msExchModerationFlags| X| X| X|  |
| msExchRecipientDisplayType| X| X| X|  |
| msExchRecipientTypeDetails| X| X| X|  |
| msExchRemoteRecipientType| X|  |  |  |
| msExchRequireAuthToSendTo| X| X| X|  |
| msExchResourceCapacity| X|  |  |  |
| msExchResourceDisplay| X|  |  |  |
| msExchResourceMetaData| X|  |  |  |
| msExchResourceSearchProperties| X|  |  |  |
| msExchRetentionComment| X| X| X|  |
| msExchRetentionURL| X| X| X|  |
| msExchSafeRecipientsHash| X| X|  |  |
| msExchSafeSendersHash| X| X|  |  |
| msExchSenderHintTranslations| X| X| X|  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| msExchUserHoldPolicies| X|  |  |  |
| msOrg-IsOrganizational|  |  | X|  |
| objectSID| X|  | X| gépi tulajdonság. AD a felhasználói azonosító karbantarthatja szinkronizálás között Azure Active Directory és AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherTelephone| X| X|  |  |
| személyhívó| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Irányítószám| X| X|  |  |
| proxyAddresses| X| X| X|  |
| publicDelegates| X| X| X|  |
| pwdLastSet| X|  |  | gépi tulajdonság. Mikor érdemes a már kiadott tokenek érvényteleníti szolgál. Jelszó-szinkronizálás és összevonási is használják.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| GroupType származás|
| sorszám| X| X|  |  |
| sourceAnchor| X| X| X| gépi tulajdonság. ÖSSZEADJA és Azure Active Directory kapcsolatának fenntartani megváltoztatható azonosítója.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| cím| X| X|  |  |
| unauthOrig| X| X| X|  |
| usageLocation| X|  |  | gépi tulajdonság. A felhasználó ország. Használja a licencek hozzárendelését.|
| userCertificate| X| X|  |  |
| userPrincipalName| X|  |  | Egyszerű Felhasználónévi a felhasználónak bejelentkezés Azonosítóját. Leggyakrabban a ugyanaz, mint [levelezés] értéket.|
| userSMIMECertificates| X| X|  |  |
| wWWHomePage| X| X|  |  |

## <a name="sharepoint-online"></a>SharePoint online-ban

| Attribútumnév| Felhasználói| Névjegy| Csoport| Megjegyzés |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Határozza meg, ha egy fiókjához engedélyezve van.|
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| További| X| X|  |  |
| Vállalati| X| X|  |  |
| countryCode| X| X|  |  |
| szervezeti egység| X| X|  |  |
| Leírás| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| hideDLMembership|  |  | X|  |
| Lakástelefon| X| X|  |  |
| információ| X| X| X|  |
| Monogram| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| levelek| X| X| X|  |
| mailnickname-beli| X| X| X|  |
| managedBy|  |  | X|  |
| kezelő| X| X|  |  |
| tag|  |  | X|  |
| middleName| X| X|  |  |
| mobil| X| X|  |  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointLinkedBy| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| objectSID| X|  | X| gépi tulajdonság. AD a felhasználói azonosító karbantarthatja szinkronizálás között Azure Active Directory és AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherIpPhone| X| X|  |  |
| otherMobile| X| X|  |  |
| otherPager| X| X|  |  |
| otherTelephone| X| X|  |  |
| személyhívó| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Irányítószám| X| X|  |  |
| postOfficeBox| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | gépi tulajdonság. Mikor érdemes a már kiadott tokenek érvényteleníti szolgál. Jelszó-szinkronizálás és összevonási is használják.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| GroupType származás|
| sorszám| X| X|  |  |
| sourceAnchor| X| X| X| gépi tulajdonság. ÖSSZEADJA és Azure Active Directory kapcsolatának fenntartani megváltoztatható azonosítója.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| cím| X| X|  |  |
| unauthOrig| X| X| X|  |
| URL-címe| X| X|  |  |
| usageLocation| X|  |  | gépi tulajdonság. A felhasználó ország. Használja a licencek hozzárendelését.|
| userPrincipalName| X|  |  | Egyszerű Felhasználónévi a felhasználónak bejelentkezés Azonosítóját. Leggyakrabban a ugyanaz, mint [levelezés] értéket.|
| wWWHomePage| X| X|  |  |

## <a name="lync-online"></a>Lync Online

| Attribútumnév| Felhasználói| Névjegy| Csoport| Megjegyzés |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Határozza meg, ha egy fiókjához engedélyezve van.|
| c| X| X|  |  |
| CN| X|  | X|  |
| További| X| X|  |  |
| Vállalati| X| X|  |  |
| szervezeti egység| X| X|  |  |
| Leírás| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X| X|  |
| givenName| X| X|  |  |
| Lakástelefon| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| levelek| X| X| X|  |
| mailnickname-beli| X| X| X|  |
| managedBy|  |  | X|  |
| kezelő| X| X|  |  |
| tag|  |  | X|  |
| mobil| X| X|  |  |
| msExchHideFromAddressLists| X| X| X|  |
| msRTCSIP-ApplicationOptions| X|  |  |  |
| msRTCSIP-DeploymentLocator| X| X|  |  |
| msRTCSIP-sor| X| X|  |  |
| msRTCSIP-OptionFlags| X| X|  |  |
| msRTCSIP-OwnerUrn| X|  |  |  |
| msRTCSIP-PrimaryUserAddress| X| X|  |  |
| msRTCSIP-UserEnabled| X| X|  |  |
| objectSID| X|  | X| gépi tulajdonság. AD a felhasználói azonosító karbantarthatja szinkronizálás között Azure Active Directory és AD.|
| otherTelephone| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Irányítószám| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | gépi tulajdonság. Mikor érdemes a már kiadott tokenek érvényteleníti szolgál. Jelszó-szinkronizálás és összevonási is használják.|
| securityEnabled|  |  | X| GroupType származás|
| sorszám| X| X|  |  |
| sourceAnchor| X| X| X| gépi tulajdonság. ÖSSZEADJA és Azure Active Directory kapcsolatának fenntartani megváltoztatható azonosítója.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailphoto| X| X|  |  |
| cím| X| X|  |  |
| usageLocation| X|  |  | gépi tulajdonság. A felhasználó ország. Használja a licencek hozzárendelését.|
| userPrincipalName| X|  |  | Egyszerű Felhasználónévi a felhasználónak bejelentkezés Azonosítóját. Leggyakrabban a ugyanaz, mint [levelezés] értéket.|
| wWWHomePage| X| X|  |  |

## <a name="azure-rms"></a>Azure Tartalomvédelmi

| Attribútumnév| Felhasználói| Névjegy| Csoport| Megjegyzés |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Határozza meg, ha egy fiókjához engedélyezve van.|
| CN| X|  | X| Közös nevet vagy aliast. Leggyakrabban az előtag [levelezés] érték.|
| displayName| X| X| X| Egy karakterlánc, amely a gyakran jelennek meg a rövid nevet (Utónév Vezetéknév) nevet.|
| levelek| X| X| X| teljes e-mail címét.|
| tag|  |  | X|  |
| objectSID| X|  | X| gépi tulajdonság. AD a felhasználói azonosító karbantarthatja szinkronizálás között Azure Active Directory és AD.|
| proxyAddresses| X| X| X| gépi tulajdonság. Azure Active Directory által használt. A felhasználó minden másodlagos e-mail címet tartalmazza.|
| pwdLastSet| X|  |  | gépi tulajdonság. Mikor érdemes a már kiadott tokenek érvényteleníti szolgál.|
| securityEnabled|  |  | X| GroupType származik.|
| sourceAnchor| X| X| X| gépi tulajdonság. ÖSSZEADJA és Azure Active Directory kapcsolatának fenntartani megváltoztatható azonosítója.|
| usageLocation| X|  |  | gépi tulajdonság. A felhasználó ország. Használja a licencek hozzárendelését.|
| userPrincipalName| X|  |  | Ez az egyszerű Felhasználónévi a felhasználónak bejelentkezés Azonosítóját. Leggyakrabban a ugyanaz, mint [levelezés] értéket.|

## <a name="intune"></a>Intune

| Attribútumnév| Felhasználói| Névjegy| Csoport| Megjegyzés |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Határozza meg, ha egy fiókjához engedélyezve van.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Leírás| X| X| X|  |
| displayName| X| X| X|  |
| levelek| X| X| X|  |
| mailnickname-beli| X| X| X|  |
| tag|  |  | X|  |
| objectSID| X|  | X| gépi tulajdonság. AD a felhasználói azonosító karbantarthatja szinkronizálás között Azure Active Directory és AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | gépi tulajdonság. Mikor érdemes a már kiadott tokenek érvényteleníti szolgál. Jelszó-szinkronizálás és összevonási is használják.|
| securityEnabled|  |  | X| GroupType származás|
| sourceAnchor| X| X| X| gépi tulajdonság. ÖSSZEADJA és Azure Active Directory kapcsolatának fenntartani megváltoztatható azonosítója.|
| usageLocation| X|  |  | gépi tulajdonság. A felhasználó ország. Használja a licencek hozzárendelését.|
| userPrincipalName| X|  |  | Egyszerű Felhasználónévi a felhasználónak bejelentkezés Azonosítóját. Leggyakrabban a ugyanaz, mint [levelezés] értéket.|

## <a name="dynamics-crm"></a>Dynamics CRM-hez

| Attribútumnév| Felhasználói| Névjegy| Csoport| Megjegyzés |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Határozza meg, ha egy fiókjához engedélyezve van.|
| c| X| X|  |  |
| CN| X|  | X|  |
| További| X| X|  |  |
| Vállalati| X| X|  |  |
| countryCode| X| X|  |  |
| Leírás| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| l| X| X|  |  |
| managedBy|  |  | X|  |
| kezelő| X| X|  |  |
| tag|  |  | X|  |
| mobil| X| X|  |  |
| objectSID| X|  | X| gépi tulajdonság. AD a felhasználói azonosító karbantarthatja szinkronizálás között Azure Active Directory és AD.|
| physicalDeliveryOfficeName| X| X|  |  |
| Irányítószám| X| X|  |  |
| preferredLanguage| X|  |  |  |
| pwdLastSet| X|  |  | gépi tulajdonság. Mikor érdemes a már kiadott tokenek érvényteleníti szolgál. Jelszó-szinkronizálás és összevonási is használják.|
| securityEnabled|  |  | X| GroupType származás|
| sorszám| X| X|  |  |
| sourceAnchor| X| X| X| gépi tulajdonság. ÖSSZEADJA és Azure Active Directory kapcsolatának fenntartani megváltoztatható azonosítója.|
| St| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| cím| X| X|  |  |
| usageLocation| X|  |  | gépi tulajdonság. A felhasználó ország. Használja a licencek hozzárendelését.|
| userPrincipalName| X|  |  | Egyszerű Felhasználónévi a felhasználónak bejelentkezés Azonosítóját. Leggyakrabban a ugyanaz, mint [levelezés] értéket.|

## <a name="3rd-party-applications"></a>alkalmazásait 3.
Az általános terhelést vagy alkalmazás szükséges minimális attribútumok használt attribútumok. Másik szakasz nem szerepel a terhelést vagy egy-Microsoft alkalmazásokban is használhatók. Explicit módon alapul a következő:

- Yammer (csak a felhasználó elfogyasztott)
- [Hibrid üzleti üzleti (B2B) határokon-szervezeti együttműködési forgatókönyvek erőforrások, mint a SharePoint nyújtotta](http://go.microsoft.com/fwlink/?LinkId=747036)

Ebben a csoportban található attribútumok is használható, ha az Azure Active directory nem használja az Office 365-ben, a Dynamics vagy az Intune. Alapvető attribútumok kis készlete rendelkezik.

| Attribútumnév| Felhasználói| Névjegy| Csoport| Megjegyzés |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Határozza meg, ha egy fiókjához engedélyezve van.|
| CN| X|  | X|  |
| displayName| X| X| X|  |
| givenName| X| X|  |  |
| levelek| X|  | X|  |
| managedBy|  |  | X|  |
| mailnickname-beli| X| X| X|  |
| tag|  |  | X|  |
| objectSID| X|  |  | gépi tulajdonság. AD a felhasználói azonosító karbantarthatja szinkronizálás között Azure Active Directory és AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | gépi tulajdonság. Mikor érdemes a már kiadott tokenek érvényteleníti szolgál. Jelszó-szinkronizálás és összevonási is használják.|
| sorszám| X| X|  |  |
| sourceAnchor| X| X| X| gépi tulajdonság. ÖSSZEADJA és Azure Active Directory kapcsolatának fenntartani megváltoztatható azonosítója.|
| usageLocation| X|  |  | gépi tulajdonság. A felhasználó ország. Használja a licencek hozzárendelését.|
| userPrincipalName| X|  |  | Egyszerű Felhasználónévi a felhasználónak bejelentkezés Azonosítóját. Leggyakrabban a ugyanaz, mint [levelezés] értéket.|

## <a name="windows-10"></a>Windows 10-ben
A Windows 10-es tartományhoz computer(device) szinkronizálja néhány attribútumok Azure AD. Az jelenik meg a további tudnivalókért olvassa el a [Csatlakozás tartományhoz csatlakoztatott eszközök: Azure AD a Windows 10 találkozik](active-directory-azureadjoin-devices-group-policy.md)című témakört. Következő attribútumok mindig szinkronizálni, és a Windows 10-ben nem jelenik meg az app megszüntetheti. A Windows 10-es tartományhoz tartozó számítógép úgy, hogy az attribútum userCertificate töltve azonosítja.

| Attribútumnév| Eszköz| Megjegyzés |
| --- | :-: | --- |
| accountEnabled| X| |
| deviceTrustType| X| A számítógép tartományhoz szoftveresen kötött értéket. |
| displayName | X| |
| MS-DS-CreatorSID | X| RegisteredOwnerReference néven is ismert.|
| objectGUID | X| DeviceID néven is ismert.|
| objectSID | X| OnPremisesSecurityIdentifier néven is ismert.|
| Operációs | X| DeviceOSType néven is ismert.|
| operatingSystemVersion | X| DeviceOSVersion néven is ismert.|
| userCertificate | X| |

A **felhasználó** attribútumok kiválasztása után az egyéb alkalmazások mellett.  

| Attribútumnév| Felhasználói| Megjegyzés |
| --- | :-: | --- |
| domainFQDN| X| Tartománynév néven is ismert. Például: contoso.com.|
| domainNetBios| X| NetBIOSnév néven is ismert. Ha például a CONTOSO.|

## <a name="exchange-hybrid-writeback"></a>Az Exchange hibrid visszaírást
Következő attribútumok kerülnek vissza az Azure Active Directory a helyszíni Active Directory ahhoz, hogy **az Exchange hibrid**kiválasztásakor. Exchange verziójától függően előfordulhat, hogy lehet szinkronizálni a kevesebb attribútumok.

| Attribútumnév| Felhasználói| Névjegy| Csoport| Megjegyzés |
| --- | :-: | :-: | :-: | --- |
| msDS-ExternalDirectoryObjectID| X|  |  | Az Azure Active Directory cloudAnchor származik. Az attribútum Újdonságok az Exchange 2016-ban.|
| msExchArchiveStatus| X|  |  | Online archívum: Lehetővé teszi, hogy a vevők mail archiválását.|
| msExchBlockedSendersHash| X|  |  | Szűrés: Adatforrásokba helyszíni szűrés és az online biztonságos és letiltott feladó adatok ügyfelektől.|
| msExchSafeRecipientsHash| X|  |  | Szűrés: Adatforrásokba helyszíni szűrés és az online biztonságos és letiltott feladó adatok ügyfelektől.|
| msExchSafeSendersHash| X|  |  | Szűrés: Adatforrásokba helyszíni szűrés és az online biztonságos és letiltott feladó adatok ügyfelektől.|
| msExchUCVoiceMailSettings| X|  |  | Egységesített üzenetküldés (egyesített) – Online hangposta engedélyezése: Microsoft Lync Server által használt, hogy a Lync Serverhez való integráció a helyszíni, hogy a felhasználó rendelkezik-e az online services hangposta.|
| msExchUserHoldPolicies| X|  |  | Postaládánként tartás: Lehetővé teszi, hogy felhőszolgáltatások megállapíthatja, hogy mely felhasználók Postaládánként tartsa lenyomva az ujját alatt találhatók.|
| proxyAddresses| X| X| X| Csak az Exchange Online-ból cím beszúrt x500.|

## <a name="device-writeback"></a>Eszköz visszaírást
Eszköz objektumok az Active Directory jönnek létre. Az objektumok lehet eszközök csatlakozik az Azure Active Directory vagy a Windows 10-es számítógépek a tartományhoz.

| Attribútumnév| Eszköz| Megjegyzés |
| --- | :-: | --- |
| altSecurityIdentities | X| |
| displayName | X| |
| DN | X| |
| msDS-CloudAnchor | X| |
| msDS-DeviceID | X| |
| msDS-DeviceObjectVersion | X| |
| msDS-DeviceOSType | X| |
| msDS-DeviceOSVersion | X| |
| msDS-DevicePhysicalIDs | X| |
| msDS-KeyCredentialLink | X| Csak a Windows Server 2016 AD séma használata |
| msDS-IsCompliant | X| |
| msDS-IsEnabled | X| |
| msDS-IsManaged | X| |
| msDS-RegisteredOwner | X| |


## <a name="notes"></a>Jegyzetek

- Másodlagos azonosító használatakor a helyszíni attribútum userPrincipalName szinkronizálva az Azure Active Directory-attribútum onPremisesUserPrincipalName. A másodlagos azonosító attribútum, például levelek az Azure Active Directory-attribútum userPrincipalName szinkronizálva van.
- A fenti listában az objektum típusa **felhasználói** is érinti az objektum típusa **iNetOrgPerson**.

## <a name="next-steps"></a>Következő lépések
További tudnivalók az [Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md) konfigurációt.

További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
