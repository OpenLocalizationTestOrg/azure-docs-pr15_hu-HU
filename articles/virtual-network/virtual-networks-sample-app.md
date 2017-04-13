<properties
   pageTitle="Példa a biztonsági oszlopazonosító környezetekben való használatra alkalmazás |} Microsoft Azure"
   description="Egyszerű webalkalmazás létrehozása a DMZ után tesztelje a forgalom folyamat esetek terjesztése"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor"/>

# <a name="sample-application-for-use-with-security-boundary-environments"></a>Minta alkalmazás biztonsági oszlopazonosító környezetekben való használatra

[Lépjen vissza a biztonsági oszlopazonosító ajánlott eljárások a lapra][HOME]

Ezeket a PowerShell-parancsfájlokat helyileg futtatását is lehetővé teszi a kiszolgálókon IIS01 és AppVM01 telepítése és beállítása rendkívül egyszerű webalkalmazás HTML-lap az előtér-IIS01 kiszolgáló-tartalom a kódmentes AppVM01 kiszolgálóról jeleníti meg.

Ez lesz az alkalmazás egy egyszerű tesztelési környezet biztosít a DMZ példák számos, és hogyan módosításokat a végpontok, NSGs, UDR és tűzfal szabályok a forgalom is hatással.

## <a name="firewall-rule-to-allow-icmp"></a>Tűzfal szabály ICMP engedélyezése
Az egyszerű PowerShell nyilatkozat futtatását is lehetővé teszi a bármely Windows virtuális ICMP (Ping) forgalmának engedélyezésére. Ezzel lehetővé teszi a könnyebb tesztelési és hibaelhárítási azáltal, hogy a ping protokoll át a windows tűzfal (a legtöbb Linux distros ICMP alapértelmezés szerint be van kapcsolva).

    # Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
        -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

**Megjegyzés:** Ha használja az alábbi parancsfájlok, a tűzfal szabály hozzáadása az első utasítás.

## <a name="iis01---web-application-installation-script"></a>IIS01 – webes alkalmazás telepítése parancsfájl
Ez a parancsfájl lesz:

1.  Nyissa meg a IMCPv4 (Ping), a helyi kiszolgáló a windows tűzfalon könnyebben teszteléshez
2.  Telepítse az IIS és a .net keretrendszer v4.5
3.  ASP.NET-weblap és egy fájlt létrehozása
4.  Az alapértelmezett alkalmazáskészlet megkönnyítheti az access fájl módosítása
5.  A rendszergazdai fiókjával, és a jelszavát a névtelen felhasználók beállítása

A PowerShell-parancsprogramot helyileg kell futtatni RDP volna be IIS01 közben.

    # IIS Server Post Build Config Script
    # Get Admin Account and Password
        Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
        $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
        $thePassword = Read-Host -Prompt "The Admin Password"
        
    # Turn On ICMPv4
        Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
        
    # Install IIS
        Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
        add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console
        
    # Create Web App Pages
        Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
        $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
        <%@ Import Namespace="System.IO" %>
        <script language="vb" runat="server">
            Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
                Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
                Dim objStreamReader As StreamReader
                objStreamReader = File.OpenText(FILENAME)
                Dim contents As String = objStreamReader.ReadToEnd()
                lblOutput.Text = contents
                objStreamReader.Close()
                lblTime.Text = Now()
            End Sub
        </script>
            
        <!DOCTYPE html>
        <html xmlns="http://www.w3.org/1999/xhtml">
        <head runat="server">
            <title>DMZ Example App</title>
        </head>
        <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
          <form id="frmMain" runat="server">
            <div>
              <h1>Looks like you made it!</h1>
              This is a page from the inside (a web server on a private network),<br />
              and it is making its way to the outside! (If you are viewing this from the internet)<br />
              <br />
              The following sections show:
              <ul style="margin-top: 0px;">
                <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
                <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
                <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
              </ul>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Image File Linked from the Internet</b>:<br />
                <br />
                <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
            </div>
          </form>
        </body>
        </html>'
        
        $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <system.web>
            <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
            <httpRuntime targetFramework="4.5" />
            <identity impersonate="true" />
            <customErrors mode="Off"/>
          </system.web>
          <system.webServer>
            <defaultDocument>
              <files>
                <add value="Home.aspx" />
              </files>
            </defaultDocument>
          </system.webServer>
        </configuration>'
            
        $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
        $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii
    
    # Set App Pool to Clasic Pipeline to remote file access will work easier
        Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
        c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
        c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost
        
    # Make sure the IIS settings take
        Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
        Restart-Service -Name W3SVC
        
        Write-Host
        Write-Host "Web App Creation Successfull!" -ForegroundColor Green
        Write-Host


## <a name="appvm01---file-server-installation-script"></a>AppVM01 - fájl Server telepítési parancsfájl
Ez a parancsfájl beállítja a háttér az egyszerű alkalmazáshoz. Ez a parancsfájl lesz:

1.  Nyissa meg a IMCPv4 (Ping) a tűzfalon könnyebben teszteléshez
2.  Hozzon létre egy új könyvtárat
3.  Hozzon létre egy szövegfájlt, hogy távolról a fenti lap az access
4.  A könyvtár és a hozzáférés engedélyezése a névtelen hozzáférés fájl engedélyeinek beállítása
5.  Internet Explorer fokozott biztonság engedélyezni a kiszolgáló könnyebben böngészési kikapcsolása 

>[AZURE.IMPORTANT] **A legjobb**: Soha ne kapcsolja ki a IE fokozott biztonság egy gyártási kiszolgálón, valamint érdemes általában egy hibás egy gyártási kiszolgálótól böngészik. A névtelen hozzáférés fájlmegosztások nyitása is egy hibás arról is, de kész itt az egyszerűség.

A PowerShell-parancsprogramot helyileg kell futtatni RDP volna be AppVM01 közben. A PowerShell szükség futtatásához rendszergazdaként sikeres végrehajtás biztosítása érdekében.
    
    # AppVM01 Server Post Build Config Script
    # PowerShell must be run as Administrator for Net Share commands to work
    
    # Turn On ICMPv4
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
    
    # Create Directory
        New-Item "C:\WebShare" -ItemType Directory
    
    # Write out Rand.txt
        $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
        $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii
    
    # Set Permissions on share
        $Acl = Get-Acl "C:\WebShare"
        $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
        $Acl.SetAccessRule($AccessRule)
        Set-Acl "C:\WebShare" $Acl
    
    # Create network share
        Net Share WebShare=C:\WebShare "/grant:Everyone,READ"
    
    # Turn Off IE Enhanced Security Configuration for Admins
        Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0
    
        Write-Host
        Write-Host "File Server Setup Successfull!" -ForegroundColor Green
        Write-Host
    

## <a name="dns01---dns-server-installation-script"></a>DNS01 - DNS Server telepítési parancsfájl
Nincs parancsfájl, benne a minta alkalmazás beállítása a DNS-kiszolgáló nem. Ha teszteli a tűzfal szabályok, NSG vagy UDR tartalmaznia kell a DNS-forgalom, a DNS01 kiszolgáló telepíthető manuálisan kell. A hálózati konfigurálásról XML-fájl mindkét példák DNS01 az elsődleges DNS-kiszolgáló és a nyilvános DNS-kiszolgáló szint 3, mint a biztonsági másolat DNS-kiszolgáló által üzemeltetett is tartalmaz. A szint 3 DNS-kiszolgáló volna tényleges DNS-kiszolgáló nem helyi forgalmához, és a DNS01 vagy nincs beállítva, nem helyi DNS alakulhat ki.

<!--Link References-->
[HOME]: ../best-practices-network-security.md
