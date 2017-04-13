### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Hozzon létre egy egyetlen olyan rekordot be AAAA rekordot

    $rs = New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-a-cname-record-set-with-a-single-record"></a>Az egyetlen olyan rekordot CNAME rekord létrehozása

    $rs = New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-an-mx-record-set-with-a-single-record"></a>Az egyetlen olyan rekordot az MX-rekord létrehozása

Ebben a példában a rekordkészletben neve használjuk "@" az MX-rekord létrehozása a a zóna csúcs (ebben az esetben "contoso.com"). Ez a közös MX-rekord esetében.

    $rs = New-AzureRmDnsRecordSet -Name "@" -RecordType MX -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-an-ns-record-set-with-a-single-record"></a>Egy egyetlen olyan rekordot az NS rekord létrehozása

    $rs = New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-a-ptr-record-set-with-a-single-record"></a>Az egyetlen olyan rekordot PTR-rekord létrehozása
Ebben az esetben "saját-arpa-zónában (Zone.com)" a ARPA zónában, amely az IP-tartomány jelöli.  IP-címet a IP-tartományon belül minden PTR-rekordja, a zóna felel meg.  

    $rs = New-AzureRmDnsRecordSet -Name "10" -RecordType PTR -Ttl 3600 -ZoneName my-arpa-zone.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ptrdname "myservice.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-an-srv-record-set-with-a-single-record"></a>Az egyetlen olyan rekordot a rekord létrehozása

A rekord a legfelső szintű zóna hoz létre, ha elég megadni *_service* és *_protocol* a rekord neve. Nem szükséges, amelyet fel szeretne venni "@" a rekord nevét.

    $rs = New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-a-txt-record-set-with-a-single-record"></a>Az egyetlen olyan rekordot TXT rekord létrehozása

    $rs = New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs
