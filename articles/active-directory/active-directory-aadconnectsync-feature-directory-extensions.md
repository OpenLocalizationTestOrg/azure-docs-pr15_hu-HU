<properties
   pageTitle="Azure AD Connect szinkronizálása: címtár-bővítmények |} Microsoft Azure"
   description="Ez a témakör ismerteti a címtár bővítmények funkció az Azure AD Connect."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/19/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect szinkronizálása: címtár-bővítmények
A címtár-bővítmények lehetővé teszi a séma kiterjesztésére Azure AD a saját attribútumokkal rendelkező a helyszíni Active Directoryból. Ez a szolgáltatás lehetővé teszi, hogy továbbra is kezelheti a helyszíni attribútumok használata más üzleti alkalmazások készítéséhez. Következő attribútumok igénybe vehető [Azure Active Directory Graph címtár bővítmények](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) vagy [Microsoft Graph](https://graph.microsoft.io/)keresztül. Megjelenik a rendelkezésre álló [Azure Active Directory Graph explorer](https://graphexplorer.cloudapp.net) és a [Microsoft Graph Intéző](https://graphexplorer2.azurewebsites.net/) használatával rendre tulajdonságait.

Jelenleg nem az Office 365 terhelést a következő attribútumok fogyaszt.

Beállíthatja az egyéni beállítások elérési út az telepítővarázslóban szinkronizálni kívánt mely további attribútumokkal.
![Séma bővítmény varázsló](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png) a telepítést a következő attribútumok, amelyek érvényes jelöltek jeleníti meg:

- Felhasználók és csoportok objektumtípusok
- Egyetlen értékű attribútumok: karakterláncot, logikai érték, egész szám, binárissá
- Többértékű attribútum: karakterlánc, binárissá

Az Azure AD Connect telepítése során létre gyorsítótárból olvasható attribútum listáját. Az Active Directory-séma további attribútumokkal rendelkező van bővített, ha a [séma frissíteni kell](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) előtt új attribútumait jelennek meg.

Objektum van legfeljebb 100 könyvtár bővítmények attribútumainak. A maximális hossza 250 karaktert. Ha egy attribútumérték hosszabb, egésszé lesz csonkítva a szinkronizálási motor által.

Azure AD Connect telepítésekor az alkalmazások van regisztrálva, ha a következő attribútumok állnak rendelkezésre. Megjelenik az alkalmazás az Azure-portálon.  
![Séma bővítmény alkalmazás](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3.png)

Következő attribútumok Graph keresztül elérhetők:  
![Diagram](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Az attribútumok elé kiterjesztésű\_{AppClientId}\_. A AppClientId az Azure Active directory ugyanazt az összes attribútum értéket tartalmaz.

## <a name="next-steps"></a>Következő lépések
További tudnivalók az [Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md) konfigurációt.

További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
