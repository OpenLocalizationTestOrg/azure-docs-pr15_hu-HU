<properties 
    pageTitle="Tulajdonságok használatáról az Azure API adatkezelési házirendek" 
    description="Megtudhatja, hogy miként tulajdonságainak használata az Azure API adatkezelési házirendek." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Tulajdonságok használatáról az Azure API adatkezelési házirendek

API adatkezelési házirendek hatékony lehetőséget, a rendszer, amelyek lehetővé teszik az API keresztül konfigurációs viselkedésének módosítása a publisher. Házirendek olyan gyűjteménye, kimutatások, egymás után a kérésre végrehajtott vagy a válasz egy API. Szövegkonstans értékeket, a házirend-kifejezések és a Tulajdonságok házirend kimutatások kell kialakítani. 

Minden egyes API Management szolgáltatás példányának kulcs/érték párokká, amelyeknek a hatóköre a szolgáltatás-példányhoz tulajdonságok gyűjteménye tartozik. A Tulajdonságok használható minden API konfigurációs és házirendek kezelése a állandó megadott értékeket. Minden egyes a tulajdonságnak az alábbi attribútumok.


| Attribútum | Típus            | Leírás                                                                                             |
|-----------|-----------------|---------------------------------------------------------------------------------------------------------|
| név      | karakterlánc          | A tulajdonság neve. Ez csak betűk, számjegyek, időtartam, kötőjel, és tartalmazhat karakterek aláhúzásjelekké. |
| Érték     | karakterlánc          | A tulajdonság értékét. Nem lehet üres vagy szóköz áll.                           |
| Titkos kulcs    | logikai érték         | Határozza meg, hogy az érték egy titkos kulcs-e, és -e titkosítható.                                |
| Címkék      | a karakterlánc tömb | Választható címkék, amennyiben a tulajdonság szűrésre is használható.                               |

Tulajdonság be van állítva a publisher-portálon a **Tulajdonságok** lapon. A következő példa három tulajdonság be van állítva.

![Tulajdonságok][api-management-properties]

Tulajdonságértékeket szövegkonstansok és a [házirend kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx)is tartalmazhat. A következő táblázat mutatja az előző három példa tulajdonságokat és a attribútumaikkal. Értékének `ExpressionProperty` a házirend-kifejezés, amely a karakterláncként adja vissza az aktuális dátum és idő. A tulajdonság `ContosoHeaderValue` egy titkos kulcs van megjelölve, hogy az érték nem jelenik meg.

| név               | Érték                      | Titkos kulcs | Címkék    |
|--------------------|----------------------------|--------|---------|
| ContosoHeader      | TrackingId                 | Hamis  | A contoso |
| ContosoHeaderValue | ••••••••••••••••••••••     | Igaz   | A contoso |
| ExpressionProperty | @(DateTime.Now.ToString()) | Hamis  |         |

## <a name="to-use-a-property"></a>A tulajdonság

Házirend használni egy tulajdonság, helyezze a tulajdonság neve belül kapcsos zárójelek, például a dupla két `{{ContosoHeader}}`, az alábbi példában látható módon.

    <set-header name="{{ContosoHeader}}" exists-action="override">
      <value>{{ContosoHeaderValue}}</value>
    </set-header>

Ebben a példában `ContosoHeader` fejlécének neve lesz egy `set-header` házirendet, és `ContosoHeaderValue` lesz az érték az adott nevét. Ha ezzel a házirenddel kiértékelése során a kérelmet, vagy választ az API az adatkezelési átjáró `{{ContosoHeader}}` és `{{ContosoHeaderValue}}` a megfelelő tulajdonságértékeket helyére kerülnek.

Tulajdonságok is alkalmazható teljes attribútum vagy elem értékek, mint az előző példában, de azt is lehet szúrt vagy kombinálva olyan szövegkonstans kifejezés, az alábbi példában látható módon:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Tulajdonságok házirend kifejezések is tartalmazhatnak. A következő példában a `ExpressionProperty` használják.

    <set-header name="CustomHeader" exists-action="override">
        <value>{{ExpressionProperty}}</value>
    </set-header>

Ha ezzel a házirenddel kiértékelt, `{{ExpressionProperty}}` lecseréli az értékére: `@(DateTime.Now.ToString())`. Mivel a értéke házirend-kifejezést, a kifejezés kiértékelése, és végrehajtása során és folytatja a házirend.

A kimenő az developer Portal hívja fel egy olyan művelet, amelynek a Tulajdonságok házirend hatálya tesztelje. Az alábbi példában egy művelet a két előző példa nevű `set-header` tulajdonságok házirendeket. Figyelje meg, hogy a kérdésre adott választ állította be a Tulajdonságok házirendek használja két egyéni fejléceket tartalmazza.

![Developer Portal segítségével][api-management-send-results]

Ha, tekintse meg a [felügyelő API-nyomkövetés](api-management-howto-api-inspector.md) , amely tartalmazza a két előző minta házirendek tulajdonságok híváshoz, megjelenik a két `set-header` a beszúrt és a házirend-kifejezést tartalmazó tulajdonság házirend kifejezés kiértékelésétől tulajdonságértékeket házirendeket.

![API a felügyelő nyomon követése][api-management-api-inspector-trace]

Figyelje meg, hogy tulajdonságértékeket házirend kifejezések is tartalmazhatnak, miközben tulajdonságértékeket nem tartalmazhat egyéb tulajdonságait. Ha tulajdonság hivatkozást tartalmazó szövegre használatos tulajdonság érték, például: `Property value text {{MyProperty}}`, tulajdonság a hivatkozás nem kell cserélni, és a tulajdonság értékét része lesz.

## <a name="to-create-a-property"></a>A tulajdonság létrehozása

Hozzon létre egy tulajdonságot, kattintson a **tulajdonság hozzáadása** a **Tulajdonságok** lapon.

![A tulajdonság hozzáadása][api-management-properties-add-property-menu]

**Név** és **érték** szükséges értékeket. Ha ez a tulajdonság értékét a titkos, jelölje be a **egy titkos: az** jelölőnégyzetet. Adja meg, ami segítheti a rendezése a tulajdonságok egy vagy több választható címkék, és kattintson a **Mentés**gombra.

![A tulajdonság hozzáadása][api-management-properties-add-property]

Új tulajdonság mentésekor a **keresési tulajdonság** szövegdoboz kitölti az új tulajdonság neve, és az új tulajdonság jelenik meg. Az összes tulajdonságainak megjelenítéséhez törölje a jelet a **keresési tulajdonságok** mezőben lévő értéket, és nyomja le az enter.

![Tulajdonságok][api-management-properties-property-saved]

További információkat a REST API tulajdonság lásd: [a REST API tulajdonság létrehozása](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>A tulajdonság szerkesztése

Tulajdonság szerkesztése, kattintson a **Szerkesztés** gombra a tulajdonság szerkesztése mellett.

![A tulajdonság szerkesztése][api-management-properties-edit]

Győződjön meg a kívánt módosításokat, és kattintson a **Mentés**gombra. Ha módosítja a tulajdonság neve, bármilyen házirendek tulajdonság hivatkozó automatikusan frissülnek az új név használatára.

![A tulajdonság szerkesztése][api-management-properties-edit-property]

A REST API tulajdonság szerkesztéséről további tudnivalókért lásd [a REST API tulajdonság szerkesztése](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>A tulajdonság törlése

Ha törölni szeretne egy tulajdonság, kattintson a **Törlés** törlése a tulajdonság melletti.

![Tulajdonság törlése][api-management-properties-delete]

Kattintson a **Igen, törölheti** a megerősítéséhez.

![Törlés jóváhagyása][api-management-delete-confirm]

>[AZURE.IMPORTANT] A tulajdonság minden házirendek hivatkozik, ha sikeres törölhetők, amíg minden olyan házirendet, amely használja ezt a tulajdonságot távolít el fogja.

A REST API tulajdonság törléséről további tudnivalókért lásd [a REST API tulajdonság törlése](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Keresés és a szűrő tulajdonságai

A **Tulajdonságok** lapon a Keresés és szűrési lehetőségek hozzáadva egyszerűbben kezelheti a Tulajdonságok tartalmazza. A tulajdonság szűrésre tulajdonság neve, írjon be egy keresőkifejezést a **Keresés tulajdonság** rendelés_részletei.mennyiség. Az összes tulajdonságainak megjelenítéséhez törölje a jelet a **keresési tulajdonságok** mezőben lévő értéket, és nyomja le az enter.

![Keresés][api-management-properties-search]

A tulajdonság címke értékek szerinti szűréséhez, írja be a **címkék szerinti szűrés** mezőben lévő értéket a egy vagy több címkét. Az összes tulajdonságainak megjelenítéséhez törölje a jelet a szövegdoboz **szűrni a címkéket** , és nyomja le adja meg.

![Szűrő][api-management-properties-filter]

## <a name="next-steps"></a>Következő lépések

-   További tájékoztatás a házirendek
    -   [Az API-kezelési házirendek](api-management-howto-policies.md)
    -   [Házirend-hivatkozás](https://msdn.microsoft.com/library/azure/dn894081.aspx)
    -   [Házirend-kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Megtekintés az egy videó – áttekintés

> [AZURE.VIDEO use-properties-in-policies]

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

