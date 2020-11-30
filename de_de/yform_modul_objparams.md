# YForm-Modul: Objparams

> ## Inhalt
> - [Zweck der Objparams](#zweck-der-objparams)
> - [Allgemeine Objparams des Formular](#allgemeine-objparams)
>	- [Formular anzeigen](#formular-anzeigen)
>	- [Eindeutige Namen für Felder](#eindeutige-namen)
>	- [CSS-ID für Formular](#css-id-formular)
>	- [CSS-Klasse für Formular](#css-klasse-formular)
>	- [CSS-ID für den HTML-Wrapper](#css-id-wrapper)
>	- [CSS-Klasse für den HTML-Wrapper](#css-klasse-wrapper)
>	- [Ausgabe der Label](#ausgabe-label)
>	- [CSRF-Schutz](#csrf-schutz)
> - [Objparams zur Formular-Optik](#objparams-optik)
>	- [Themes](#themes)
>	- [Submit-Button benennen](#submit-benennen)
>	- [Submit-Button anzeigen](#submit-anzeigen)
>	- [CSS-Klasse für Fehler](#css-klasse-fehler)
>	- ["Echte" Feldnamen](#echte-feldnamen)
> - [Objparams zum Formularversand](#objparams-formularversand)
>	- [Versandmethode des Formulars](#versandmethode)
>	- [Zieladresse des Formulars](#zieladresse)
>	- [Sprunganker](#sprunganker)
>	- [Formular anzeigen nach Abschicken](#formular-anzeigen-nach-abschicken)
>   - [Formular debuggen](#formular-debug)
>   - [Fehlermeldungen ausschalten/verstecken](#formular-hide-top-warning-messages)
>   - [Feldwerte aus Datenbank laden](#formular-get-data)
>   - [Weiterleitung forcieren](#form_exit)

## Zweck der Objparams

<a name="zweck-der-objparams"></a>
**Objektparameter** fungieren vor allem als Einstellungen, die das ganze Formular betreffen. Diese Paramenter können - ähnlich wie die Values oder Validates – als einzeilige Anweisung gesetzt werden.

Zusätzlich kann man bestimmen, ob der Objektparameter an genau der Stelle des Formulars verändert wird, an der er im Formular gesetzt wird (`runtime`) oder den Wert initial setzt (`init`, das ist die Standardeinstellung).

Die **allgemeine Syntax** für das Setzen eines objparams lautet so:

	// Im YForm-Formbuilder
	objparams|key|newvalue|[init/runtime]

```php
// In PHP
$yform->setObjectparams('key', 'newvalue', '[init/runtime]');
```

- key = die Bezeichnung des Wertes
- newvalue = der Neue Wert, der gesetzt werden soll
- Der letzte Parameter ist optional und lautet `init` (default) oder `runtime`.
- **Im Folgenden werden alle objparams mit Beispiel aufgelistet.**

<a name="allgemeine-objparams"></a>
## Allgemeine Objparams des Formular

<a name="formular-anzeigen"></a>
### Formular anzeigen

	// Im YForm-Formbuilder
	objparams|form_show|0

```php
// In PHP
$yform->setObjectparams('form_show','1');
```

Mit dem Wert `0` wird das Formular nach dem Abschicken nicht angezeigt. Dieses Ausblenden benötigt man, wenn man eine Formulaktion auslösen will, aber kein sichtbares Formular haben möchte. **Beispiel:** Ein User wird durch den Aufruf einer bestimmten URL freigeschaltet.  
Der Defaultwert ist `1` (anzeigen).

<a name="eindeutige namen"></a>
### Eindeutige Namen Für Felder

	// Im YForm-Formbuilder
	objparams|form_name|formular

```php
// In PHP
$yform->setObjectparams('form_name','zweites_formular');
```

Wenn man mehrere Formulare auf einer Seite verwenden möchte, muss der `form_name` für jedes Formular verschieden sein. Der hier gewählte Name wird bei jedem Feld eines Formulars dem Namen und der ID hinzugefügt, so erhält man eine Eindeutigkeit.  
Der Defaultwert ist `formular`.

<a name="css-klasse-formular"></a>
### CSS-Klasse für Formular

	// Im YForm-Formbuilder
	objparams|form_class|contact_form

```php
// In PHP
$yform->setObjectparams('form_class','contact_form');
```

Damit kann dem Formular eine individuelle CSS-Klasse vergeben werden.  
Default-Ausgabe:
`<form class="rex-yform">`

<a name="css-id-wrapper"></a>
### CSS-ID für den HTML-Wrapper

	// Im YForm-Formbuilder
	objparams|form_wrap_id|contact_form

```php
// In PHP
$yform->setObjectparams('form_wrap_id','contact_form');
```
Damit kann dem das Formular umgebenden Container eine individuelle CSS-ID vergeben werden.  
Default-Ausgabe:
`<form id="yform">`

<a name="css-klasse-wrapper"></a>
### CSS-Klasse für den HTML-Wrapper

	// Im YForm-Formbuilder
	objparams|form_wrap_class|contact_form

```php
// In PHP
$yform->setObjectparams('form_wrap_class','contact_form');
```

Damit kann dem das Formular umgebenden Container eine individuelle CSS-Klasse vergeben werden.  
Default-Ausgabe:
`<form class="yform">`

<a name="ausgabe-label"></a>
### Ausgabe der Label

	// Im YForm-Formbuilder
	objparams|form_label_type|html

```php
// In PHP
$yform->setObjectparams('form_label_type','html');
```

Wenn man den Wert hier auf `plain` setzt, werden die Feld-Label nicht als HTML interpretiert, sondern mit `htmlspecialchars` und `nl2br` maskiert.  
Default ist `html`.

<a name="csrf-schutz"></a>
### CSRF-Schutz

	// Im YForm-Formbuilder
	objparams|csrf_protection|0

```php
// In PHP
$yform->setObjectparams('csrf_protection', false);
```

Der Parameter zum CSRF-Schutz (Cross-Site-Request-Forgery, auch XSRF) verhindert, dass speziell präparierte Anfragen von YForm ausgeführt werden. Angriffsszenario auf ein YForm-Formular wäre bspw. ein Nutzer, der einen präparierten Link erhält und durch einen Klick dann Daten seines REDAXO-Besuchs preisgibt oder unbemerkte/ungewollte Aktionen durch das YForm-Formular ausführt.

Vereinfacht gesagt sorgt der CSRF-Schutz dafür, dass Formulare nur dann erfolgreich abgesendet werden, wenn der Nutzer sich zum Zeitpunkt des Formular-Absendens auf der Seite befunden hat.

Der CSRF-Schutz sollte daher immer aktiviert bleiben, außer, wenn der direkte Aufruf und Versand eines Formulars explizit durch einen präparierten Link erfolgen muss - bspw. beim Account-Aktivieren-Link des Addons YCom.

---

<a name="objparams-optik"></a>
## Objparams zur Formular-Optik

<a name="themes"></a>
### Themes

	// Im YForm-Formbuilder
	objparams|form_ytemplate|bootstrap

```php
// In PHP
$yform->setObjectparams('form_ytemplate','bootstrap');
```

YForm verfügt über `Templates`, in denen das HTML-Markup definiert ist, das die Felder umgibt. Im Ordner `ytemplates` gibt es Unterordner für jedes Theme, in denen dann die Templates für die einzelnen Felder zu finden sind. Auf diese Weise kann man schnell eigene Themes definieren, die auf dem Basis-Theme aufbauen.
Themes können in eigenen Addons, im project Addon oder im theme Addon abgelegt werden. Der Defaultwert lautet `bootstrap`, d.h. als Basis-Theme ist das HTML-Schema des CSS-Frameworks "Bootstrap" hinterlegt.
 
Möchte man sein eigenes Template mit Feldtypanpassungen verwenden, sollten nur die veränderten Felder im eigenen Templateordner abgelegt werden.
Wenn es für einen Feldtyp ein eigenes Template gibt, wird dieses verwendet, anonsten das des Basis-Themes. Der Fallback `bootstrap` muss immer mit angegeben werden.

  // Im YForm-Formbuilder
  objparams|form_ytemplate|custom,bootstrap

```php
// In PHP
$yform->setObjectparams('form_ytemplate','custom,bootstrap');
```


<a name="submit-benennen"></a>
### Submit-Button benennen

	// Im YForm-Formbuilder
	objparams|submit_btn_label|Formular senden

```php
// In PHP
$yform->setObjectparams('submit_btn_label','Formular senden');
```

Damit kann die Standard-Button-Beschriftung `Abschicken` verändert werden.

<a name="submit-anzeigen"></a>
### Submit-Button anzeigen

	// Im YForm-Formbuilder
	objparams|submit_btn_show|0

```php
// In PHP
$yform->setObjectparams('submit_btn_show',0);
```

Mit dem Wert `0` wird der Standard-Submit-Button versteckt. Dies ist zum Beispiel sinnvoll, wenn man eigene Buttons definiert hat.  
Default ist `1` (Anzeigen).

<a name="css-klasse-fehler"></a>
### CSS-Klasse für Fehler

	// Im YForm-Formbuilder
	objparams|error_class|my_form_error

```php
// In PHP
$yform->setObjectparams('error_class','my_form_error');
```

Diese individuelle CSS-Klasse kommt an zwei Stellen zum Tragen:  
1. im Container mit den Fehlerhinweisen zu Beginn des Formulars:  
`<div class="alert alert-danger my_form_error">`  
2. im Container aller Felder, die bei einer Validierung fehlschlagen:  
`<div class="form-group my_form_error">`.  
So kann man sowohl Label als auch Feld als fehlerhaft formatieren.

Die Default-CSS-Klasse ist `form_warning`.

<a name="echte-feldnamen"></a>
### "Echte" Feldnamen

	// Im YForm-Formbuilder
	objparams|real_field_names|1

```php
// In PHP
$yform->setObjectparams('real_field_names',1);
```

Mit dem auf `1` gesetzten Wert werden exakt die Feldnamen im Formular genommen, die auch in der Formulardefinition gesetzt wurden. Der Feldname lautet dann z.B. nicht mehr `name="FORM[formular][2]"`, sondern `name=vorname`.  
Der Default-Wert ist `0`.

---

<a name="objparams-formularversand"></a>
## Objparams zum Formularversand

<a name="versandmethode"></a>
### Versandmethode des Formulars

	// Im YForm-Formbuilder

```php
// In PHP
$yform->setObjectparams('form_method','get');
```

Mit dem Wert `get` wir die Versandmethode auf get geändert, d.h. alle Feldwerte sind als get-Paramater in der URL enthalten.  
Der Defaultwert ist `post`.

<a name="zieladresse"></a>
### Zieladresse des Formulars

	// Im YForm-Formbuilder
	objparams|form_action|zielseite.html

```php
// In PHP mit rex_getUrl() auf die Artikel-ID 5
$yform->setObjectparams('form_action',rex_getUrl(5));
```

Als Ziel nach dem Abschicken kann eine andere Adresse definiert werden, z.B. für eine ausführliche Danke-Seite. Es könnte auch die aktuelle Artikel-ID gesetzt weden, ergänzt um weitere Parameter.  
Der Defaultwert ist `index.php`, bzw. die URL der Formularseite.

<a name="sprunganker"></a>
### Sprunganker

	// Im YForm-Formbuilder
	objparams|form_anchor|my_form

```php
// In PHP
$yform->setObjectparams('form_anchor','my_form');
```

Wenn sich ein Formular weiter unten auf der Seite befindet, sieht man nach dem Abschicken zunächst keine Erfolgs- oder Fehlermeldung. Über den `form_anchor`lässt sich ein Sprunganker definieren, der in der  URL nach dem Abschicken angehängt wird, so dass die Seite zum Anker springt. Im Normalfall wird man als Anker die ID des Formulars nutzen.  
Der Defaultwert ist leer.

<a name="formular-anzeigen-nach-abschicken"></a>
### Formular anzeigen nach Abschicken

	// Im YForm-Formbuilder
	objparams|form_showformafterupdate|1

```php
// In PHP
$yform->setObjectparams('form_showformafterupdate',1);
```

Mit dem Wert `1` kann man das Formular nach dem Versand nochmal anzeigen, um zum Beispiel direkt eine neue Eingabe zu ermöglichen oder die eingegebenen Werte erneut zum Verändern anzubieten.  
Default ist `0` (nicht anzeigen).


<a name="formular-debug"></a>
### Formular debuggen

	// Im YForm-Formbuilder
	objparams|debug|1

```php
// In PHP
$yform->setObjectparams('debug',1);
```

Mit dem Wert `1` kann man zb Aktionen Formular debuggen und Aktion prüfen.


<a name="formular-hide-top-warning-messages"></a>
### Fehlermeldungen ausschalten/verstecken

	// Im YForm-Formbuilder
	objparams|hide_top_warning_messages|1

```php
// In PHP
$yform->setObjectparams('hide_top_warning_messages',1);
```

Mit dem Wert `1` können die Fehlermeldung die über eine Validierung ausgegeben werden versteckt werden.


<a name="formular-get-data"></a>
### Feldwerte aus Datenbank laden

	// Im YForm-Formbuilder
	objparams|getdata|1
	objparams|main_where|id=1
	objparams|main_table|rex_table

```php
// In PHP
$yform->setObjectparams('getdata',1);
$yform->setObjectparams('main_where','id=1');
$yform->setObjectparams('main_table','rex_table');
```

Mit dem Wert `1` bei `getdata` in Verbindung mit `main_where` (hier die id auf den Datensatz) und `main_table` (hier der Tabellename) können Felder mit Werten aus eine Datenbanktabelle vorbelegt/geladen werden.

<a name="form_exit"></a>
### Weiterleitung forcieren

Mit `form_exit` wird gesteuert, ob die Abarbeitung des weiteren Codes, bspw. in Modulen und Templates, nach erfolgreichem Versand beendet wird oder nicht. Ein vorzeitiges Beenden des Codes ist bspw. dann nötig, wenn man eine redirect-Action (Weiterleitung) verwenden möchte und diese direkt ausgeführt werden soll, oder, um in bestimmten Fällen eine doppelte Ausführung des Formulars zu verhindern.

	// Im YForm-Formbuilder
	objparams|form_exit|1

	// In PHP
	$yform->setObjectparams('form_exit',1);