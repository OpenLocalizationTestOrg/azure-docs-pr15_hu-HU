<properties 
    pageTitle="Első lépések az Azure Active Directory és a Visual Studio kapcsolt szolgáltatások (MVC projektek) |} Microsoft Azure" 
    description="Hogyan kezdjek hozzá az Azure Active Directory MVC projektekben való csatlakozáskor vagy létrehozása az Azure AD a Visual Studio segítségével a csatlakoztatott szolgáltatások után" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Azure Active Directory és a Visual Studio első lépések a csatlakoztatott szolgáltatások (MVC projektek)

> [AZURE.SELECTOR]
> - [Első lépések](vs-active-directory-dotnet-getting-started.md)
> - [mi történt](vs-active-directory-dotnet-what-happened.md)
 
##<a name="requiring-authentication-to-access-controllers"></a>Access-vezérlők hitelesítést igénylő 

A projektben szereplő összes vezérlő **engedélyezése** attribútumot tartalmazó voltak adorned. Az attribútum a felhasználót, hogy ezek a vezérlők megnyitása előtt hitelesíteni van szükség. Ahhoz, hogy a vezérlő névtelenül érhető el, a vezérlő az attribútum eltávolítása. Ha szeretné megállapítani, hogy az engedélyek finomabb, az attribútum alkalmazása mindegyik módszernek helyett alkalmazása, a vezérlő osztály engedély szükséges.
 
##<a name="adding-signin--signout-controls"></a>Bejelentkezés hozzáadása / SignOut típusú vezérlők 

Hozzáadása egy a bejelentkezés/SignOut vezérlők a nézetre, használhatja a **_LoginPartial.cshtml** részleges nézet hozzáadása a nézet valamelyikében a funkciókat. Az alábbi képen a hozzáadott a **_Layout.cshtml** normál nézetben a használható funkciók körét. (Tájékoztatjuk a div osztály navigációs sávja – összecsukás az utolsó elemének):

<pre>
    &lt;! DOCTYPE html&gt; 
&lt;html&gt; 
&lt;head&gt; 
&lt;meta charset="utf-8" /&gt; 
&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
&lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
@Styles.Render("~/Content/css") @Scripts.Render("~/bundles/modernizr") &lt;/head&gt; 
&lt;body&gt; 
&lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
&lt;div class="container"&gt; 
&lt;div class="navbar-header"&gt; 
&lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;/button&gt; 
@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) &lt;/div&gt; 
&lt;div class="navbar-collapse collapse"&gt; 
&lt;ul class="nav navbar-nav"&gt; 
&lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
&lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/ div&gt; 
&lt;/div&gt; 
&lt;/div&gt; 
&lt;div class="container body-content"&gt; 
@RenderBody() &lt;hr /&gt; 
&lt;footer&gt; 
&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
&lt;/footer&gt; 
&lt;/div&gt; 
@Scripts.Render("~/bundles/jquery") @Scripts.Render("~/bundles/bootstrap") @RenderSection("scripts", required: false) &lt;/body&gt; 
&lt;/html                                                                                                                                                                                                                                                                                                                                                                                                                                                           &gt;
</pre>

[További tudnivalók az Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 
