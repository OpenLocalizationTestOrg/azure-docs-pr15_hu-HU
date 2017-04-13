<properties
   pageTitle="Azure adatkatalógus Előfeltételek |} Microsoft Azure"
   description="Azure adatkatalógus Előfeltételek – első lépések az Azure adatkatalógus szeretne."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-prerequisites"></a>Azure adatkatalógus vonatkozó követelmények

## <a name="what-do-i-need-to-get-started-with-azure-data-catalog"></a>Mit kell Azure adatkatalógus – első lépések?

Néhány dolgot kell gondoskodik a előtt **Adatkatalógus Azure**beállíthatja. Ne aggódjon, – ezek nem ideig, mire!

## <a name="azure-subscription"></a>Azure előfizetés
Azure adatkatalógus beállítani, a tulajdonos vagy egy Azure előfizetés társtulajdonos kell lennie.

Azure előfizetések hatékonyabban rendezheti a felhőalapú szolgáltatás erőforrások, mint a Azure adatkatalógus való hozzáférést. Azok is szabályozhatja, hogy hogyan erőforrás-kihasználtság jelenteni, Súgó számlát kapni, és a kifizetett. Egyes előfizetések van különböző számlázási és fizetési beállítás, így a különböző előfizetések és különböző tervek részleg, project, regionális iroda és így tovább. Előfizetéshez tartozik minden felhőalapú szolgáltatásba, és Azure adatkatalógus beállítása előtt előfizetés van szükség. További tudnivalókért olvassa el a [fiókok kezelése, az előfizetések hivatkozásra, és a rendszergazdai szerepkörök](../active-directory/active-directory-assign-admin-roles.md)című témakört.

## <a name="azure-active-directory"></a>Azure Active Directory
Azure adatkatalógus beállításához kell bejelentkeznie az Azure Active Directory felhasználói fiókot használ.

Azure Active Directory (Azure Active Directory) egyszerűvé a vállalati identitás- és az access, mind a felhőben, és a helyszíni kezeléséhez. Felhasználók egy egyetlen munkahelyi vagy iskolai fiókba az egyszeri bejelentkezési bármely felhőbe és a helyszíni webalkalmazásban. Azure adatkatalógus Azure Active Directory használja a bejelentkezés hitelesítést végezni. További tudnivalókért olvassa el a [Mi az Azure Active Directory](../active-directory/active-directory-whatis.md)című témakört.

> [AZURE.NOTE] Az [Azure portal](http://portal.azure.com/) lehetővé teszi, hogy a felhasználóknak, hogy jelentkezzen be egy személyes Microsoft-Account vagy az Azure Active Directory munkahelyi vagy iskolai fiókjával. Azure adatkatalógus az Azure portálon vagy az [adatkatalógusban portál](http://www.azuredatacatalog.com) beállítása kell bejelentkeznie az Azure Active Directory-fiókhoz, nem egy személyes fiók használatával.

## <a name="active-directory-policy-configuration"></a>Az Active Directory-házirend beállítása

Bizonyos esetekben a felhasználók előfordulhatnak olyan helyzet, ahol bejelentkezhetne az adatkatalógusban Azure-portálra, de amikor megpróbálnak jelentkezzen be az adatok forrása regisztrációs eszköz találkoznak hibaüzenet jelenik meg, hogy megakadályozza, hogy a bejelentkezés. Ez a probléma oka lehet csak akkor, amikor a felhasználó a vállalati hálózaton, vagy csak akkor, amikor a felhasználó a csatlakozás a vállalati hálózatán kívülről.

Az adatok forrása regisztrációs eszköz űrlapok hitelesítést használ, felhasználói bejelentkezések, az Active Directory ellen érvényesítéséhez. A sikeres bejelentkezési űrlap-hitelesítés engedélyezve kell lennie az a globális hitelesítési házirend az Active Directory-rendszergazda által.

A globális hitelesítési házirend lehetővé teszi, hogy külön-külön aktívak intranetes és extranetes kapcsolatot, hitelesítési módszereket alább látható módon. Bejelentkezési hibák akkor fordulhat elő, ha az űrlap-hitelesítés engedélyezve van a hálózaton, amelyből a felhasználó csatlakozik.

 ![Az Active Directory Authentication globális házirend](./media/data-catalog-prerequisites/global-auth-policy.png)

További tudnivalókért olvassa el a [Hitelesítési házirendek beállítása](https://technet.microsoft.com/library/dn486781.aspx)című témakört.
