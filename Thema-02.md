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
