# Installazione PHP per il corso

## 1. Installazione del web platform installer 5.0
Scaricare il web platform installer di Microsoft dal [sito](https://www.microsoft.com/en-us/download/details.aspx?id=6164)

## 2. Installazione di IIS Express
Installare IIS Express (v10.0) cercandolo nel web platform installer

## 3. Installazione di PHP
Installare, sempre tramite il web platform installer, PHP versione 5.6 for ISS Express (x86)

## 4. Opzionale
Il web platform dovrebbe aggiornare autonomamente il file di configurazione di IIS con PHP, ma a volte non succede.  

Nel caso seguire le seguenti istruzioni:  

__Nota__: Controllare tramite l'editor se ogni tag già esiste

__Nota__: __ATTENZIONE ALLE PATH, SE IL SISTEMA E' A 32BIT O AVETE INSTALALTO IIS EXPRESS CON PHP A 64BIT, DOVETE RIMUOVERE L'X86__

### A. Aggiungere le pagine di default al server
- Aprire il file applicationhost.config presente in __Documenti\IIS Express\config__
- Aggiungere sotto il tag __defaultDocument__ il valore:
```xml
<add value="index.php" />
```

### B. Aggiungere la configurazione del FastCGI
- Aprire il file applicationhost.config presente in __Documenti\IIS Express\config__
- Aggiungere sotto il tag __system.webServer__ il valore:
```xml
<fastCgi>
    <application fullPath="C:\Program Files (x86)\iis express\PHP\v5.6\php-cgi.exe" monitorChangesTo="php.ini" activityTimeout="600" requestTimeout="600" instanceMaxRequests="10000">
        <environmentVariables>
        <environmentVariable name="PHP_FCGI_MAX_REQUESTS" value="10000" />
        <environmentVariable name="PHPRC" value="C:\Program Files (x86)\iis express\PHP\v5.6" />
        </environmentVariables>
    </application>
</fastCgi>
```
- Ovviamente per i percorsi adeguarsi al sistema in esecuzione

### C. Aggiungere l'icona modulo
- Aprire il file applicationhost.config presente in __Documenti\IIS Express\config__
- Aggiungere sotto il tag __globalModules__
```xml
<add name="FastCgiModule" image="%IIS_BIN%\iisfcgi.dll" />
```

### D. Aggiungere il tracing
- Aprire il file applicationhost.config presente in __Documenti\IIS Express\config__
- Aggiungere sotto il tag __traceProviderDefinitions__ -> __add__ -> __areas__
```xml
<add name="FastCGI" value="4096" />
```

### E. Aggiungere il modulo al server
- Aprire il file applicationhost.config presente in __Documenti\IIS Express\config__
- Aggiungere sotto il tag __location__ -> __system.webServer__ -> __modules__
```xml
<add name="FastCgiModule" lockItem="true" />
```

### G. Aggiungere l'handler
- Aprire il file applicationhost.config presente in __Documenti\IIS Express\config__
- Aggiungere sotto il tag __location__ -> __system.webServer__ -> __handlers__
```xml
<add name="PHP55_via_FastCGI" path="*.php" verb="GET,HEAD,POST,DELETE,PUT" modules="FastCgiModule" scriptProcessor="C:\Program Files (x86)\iis express\PHP\v5.6\php-cgi.exe" resourceType="Either" />
```

### G. Aggiungere il modulo alla configurazione
- Aprire il file applicationhost.config presente in __Documenti\IIS Express\config__
- Aggiungere sotto il tag __configSections__ -> __sectionGroup name="system.webServer"__
```xml
<section name="fastCgi" allowDefinition="AppHostOnly" overrideModeDefault="Deny" />
```

## Installazione di XDEBUG
Ora installeremo l'ambiente di debug:

1. Andare sul wizard del [sito ufficiale](https://xdebug.org/wizard.php)
2. Creare ed eseguire uno script php con il comando __phpinfo()__
3. Aprire la pagina nel browser e copiare l'output html nativo (solitamente premendo f12 si può visualizzare)
4. Incollare l'output nella pagina del punto 1 e premere su "analyse"
5. Seguire le istruzioni

Come ultima cosa, inserire nel proprio __php.ini__ i seguenti comandi:
```xml
xdebug.remote_autostart=1
xdebug.remote_enable=1
```

# Editor
Come editor si consiglia __Visual Studio Code__, installarlo da [qui](https://code.visualstudio.com/)

Per sfruttare al meglio VSCODE installare node.js da [qui](https://nodejs.org/)

Una volta fatto scaricare l'estensione __IIS Express Executer__ da Visual Studio Code
