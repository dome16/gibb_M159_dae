# M159Thema2-AB01.pdf:
## Fragen:
### Lösen Sie ein Ticket für den User administrator:
```
kinit administrator
klist
```
### Verbindung testen mit smbclient -L vmls1 -k
#### Was bewirkt der Parameter -k?
```
smbclient --help

---------------------------------------------------------------------------------------

-k, --kerberos     |    DEPRECATED: Migrate to --use-kerberos
```
### Wie sieht der Credential Cache aus?
```
klist -A

---------------------------------------------------------------------------------------

Ticket cache: FILE:/tmp/krb5cc_1000
Default principal: administrator@SAM159.IET-GIBB.CH

Valid starting       Expires              Service principal
11/03/2022 20:51:09  11/04/2022 06:51:09  krbtgt/SAM159.IET-GIBB.CH@SAM159.IET-GIBB.CH
	renew until 11/04/2022 20:51:04

```

### Warum funktioniert der Verbindungsaufbau mit localhost nicht?
#### Versuchen Sie es mit: smbclient -L localhost -k localhost entspricht doch vmls1? warum hat es mit localhost bei 7(a)-Testen der Verbindung geklappt bzw. warum klappt es hier nicht mit -k?


### Passwort-Komplexität deaktivieren mit samba-tool:
#### 1. Password complexity = deactivated:
```
 sudo samba-tool domain passwordsettings set --complexity=off
```

#### 2. Password history length = 0
```
sudo samba-tool domain passwordsettings set --history-length=0
```

#### 3. Minimum password age = 0
```
sudo samba-tool domain passwordsettings set --min-pwd-age=0
```

#### 4. Maximum password age = 0
```
sudo samba-tool domain passwordsettings set --max-pwd-age=0
```

#### 5. Expiration Time für den User administrator ausschalten
```
sudo samba-tool user setexpiry administrator --noexpiry
```

### Anlegen eines DNS A Records mit samba-tool
#### Legen sie vmls2 und vmls3 als A-Record im DNS an. Verwenden Sie dazu den Befehl samba-tool. Für Hilfe verwenden sie samba-tool -h oder finden sie Beispiele im Internet.
```
Ip Addresse vmls2: 192.168.220.12	vmls3: 192.168.220.13

---------------------------------------------------------------------------------------

samba-tool dns add 192.168.220.10 sam159.iet-gibb.ch vmls2 A 192.168.220.12 -Uadministrator
samba-tool dns add 192.168.220.10 sam159.iet-gibb.ch vmls3 A 192.168.220.13 -Uadministrator
```

### Anlegen der PTR Records für vmls2 und vmls3
#### Legen Sie für beide Rechner die PTR Record an mit dem Befehl samba-tool.

```
Ip Addresse vmls2: 192.168.220.12	vmls3: 192.168.220.13

---------------------------------------------------------------------------------------

samba-tool dns add 192.168.220.10 220.168.192.in-addr.arpa 12 PTR vmls2.sam159.iet-gibb.ch -Uadministrator
samba-tool dns add 192.168.220.10 220.168.192.in-addr.arpa 13 PTR vmls3.sam159.iet-gibb.ch -Uadministrator
```

### Was zeigt samba-tool fsmo show
#### Erklären Sie die Bedeutung von jedem Eintrag und notieren Sie das im Arbeitsjournal.
```
Output vom Command:
---------------------------------------------------------------------------------------
SchemaMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
InfrastructureMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
RidAllocationMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
PdcEmulationMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
DomainNamingMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
DomainDnsZonesMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
ForestDnsZonesMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch

Interpretation:
---------------------------------------------------------------------------------------
Das sind die fsmo Rollen die hier angezeigt werden. fsmo steht für (  Flexible Single Master Operations ). Dieser Command zeigt die Owner dieser fsmo Rollen an.
```








# M159Thema2-AB02.pdf:
## Installieren Sie dazu zuerst die ldap-tools und den LAM (LDAP-Account-Manager):
```
sudo apt-get install smbldap-tools
sudo apt install ldap-account-manager
```

## Firefox auf der vmlp1 starten und folgende Seite aufrufen:
```
http://192.168.220.10/lam/
```

## 3.2 User, Gruppen und DNS mit RSAT Tools verwalten
### Fügen Sie vmWP1 in die Domain sam159.iet-gibb.ch ein. Was müssen Sie beim Client netzwerkseitig
unbedingt beachten, damit das klappt?
```
1. Vm Netwerkkonfiguration anpassen und adapter neu starten.
2. VMnet8 verschieben über VMwarePlayer einstellungen
3. Domäne Joinen
```

### Installieren Sie die RSAT-Tools. Unter Windows 11 können diese Tools unter Optional Features
ausgewählt werden.
```
Ist bei Windows 10 geleich auch über die Optional Features Möglich.
```

### Welche Domain-Admin Tools stehen ihnen jetzt auf vmWP1 zur Verfügung?
```
https://www.google.com/url?sa=i&url=https%3A%2F%2F4sysops.com%2Farchives%2Finstall-rsat-for-windows-10%2F&psig=AOvVaw0MIj7KBH5yHPEzZb3ZdFdk&ust=1669368202934000&source=images&cd=vfe&ved=0CA0QjRxqFwoTCJCsm6i_xvsCFQAAAAAdAAAAABAS
```

## Active Directory mit ldapsearch abfragen
```
sudo apt -y install ldap-utils
ldapsearch -x -LLL -H ldap://vmls1.sam159.iet-gibb.ch -b dc=sam159,dc=iet-gibb,dc=ch -D CN=administrator,CN=Users,DC=sam159,DC=iet-gibb,DC=ch -w SmL12345** cn=dominic
```
