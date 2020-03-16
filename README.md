# Anleitung zur Benutzung eines GEP2010IL an einem UR5

## Allgemeines
Um den Greifer nutzen zu können, muss zuerst die richtige Installationsdatei geladen werden. Diese und alle weiteren benötigten Dateien können dieser GitHub Seite heruntergeladen werden. Danach können die nötigen Anpassungen für die gewünschte Anwendung vorgenommen werden. Zu beachten ist, dass der Greifer selbst ein Gewicht von 310 g hat. Die voreingestellten Modbus-Wörter sollten unbedingt so belassen werden wie sie sind, da über diese Wörter der UR5 mit dem Greifer kommuniziert. Um die folgenden Funktionen nutzen zu können, muss die Datei, in der die Funktionen definiert sind, in jedem Programm miteingebunden werden.

## Beschreibung der Funktionen 
Für die einfache Benutzung des Greifers wurden einige Funktionen für die einfache Verwendung geschrieben. Im Nachfolgenden werden diese beschrieben und erklärt. 

### Initialisierung
`gripper_init()`

Vor dem ersten Gebrauch des Greifers nach Spannungslosigkeit muss dieser neu initialisiert werden. Dies wird mit dieser Funktion vorgenommen. Diese Funktion erwartet keine Übergabe-Parameter und hat keine Rückgabewerte. Nach dem Aufruf ist der Greifer voll funktionsfähig. Kann der Greifer nicht initialisiert werden wird ein Popup-Fenster geöffnet und eine Infotext ausgegeben. 

### Greifer öffnen
`gripper_open()`

Um den Greifer nach dem Initialisieren zu öffnen wird dieser Befehl verwendet. Auch diese Funktion erwartet keine Übergabe-Parameter und gibt keine Werte zurück. 

### Greifer schließen
`gripper_close()`

Analog zu `gripper_open()` kann dieser Befehl zum Schließen des Greifers verwendet werden. Wie beim Öffnen-Befehl werden keine Übergabe-Parameter erwartet und keine Werte werden zurückgegeben. 

### Zurücksetzen des Richtungsmerkers
`gripper_reset_direction_flag()`

Der Greifer ist intern mit einer Sicherheitsfunktion ausgestattet, die verhindert, dass man den Greifer zweimal in die gleiche Richtung verfahren kann. Um diese Funktion für bestimmte Anwendungen zu umgehen kann der Merker für die Richtung zurückgesetzt werden. Dafür wird diese Funktion verwendet. Kann der Richtungsmerker nicht zurückgesetzt werden wird eine Fehlermeldung ausgegeben. Diese Funktion erwartet keine Übergabeparameter und keine Werte werden zurückgegeben.

### Auslesen der Position 
`gripper_get_actual_position()`

Die genaue Position der Backen des Greifers kann mit dieser Funktion abgerufen werden. Auch diese Funktion erwartet keine Übergabeparameter. Der Rückgabewert stellt die Position in 0,01 mm dar. Die Werte bewegen sich zwischen 0 und 2000. Wobei die 2000 den komplett geschlossenen Zustand beschreibt. Die maximale Genauigkeit ist mit bis zu ± 0,05 mm zu erwarten. 

### Werkstückrezeptur laden (nicht stabil) 
`gripper_load_recipe(workpiece_number)`

Der Greifer kann für bis zu 32 verschiedene Werkstücke eigene Daten speichern. Dazu gehören die Backenposition der Teachposition, die Toleranz für die Bauteilerkennung, die Greifkraft sowie der Greifmodus. Um diese Daten zu laden wird diese Funktion aufgerufen. Ein Rückgabewert existiert für diese Funktion nicht. 

Übergabeparameter:
- `workpiece_number`
  - Wertebereich 1…32
  - Für bis zu 32 verschiedene Werkstücke

### Schreiben der Bauteildaten (nicht stabil) 
`gripper_write_recipe(device_mode, workpiece_number, position_tolerance, grip_force, position)`

Zum Schreiben der Bauteildaten wird diese Funktion verwendet. Einen Rückgabewert besitzt diese Funktion nicht.

Übergabeparameter:
- `device_mode`
  - Fahrmodus
  - 100 für Universalbetrieb
  - 60 Außengreifen 
  - 70 Innengreifen
- `workpiece_number`
  - Nummer der Rezeptur, die geändert werden soll 
  - Wertebereich 1…32
- `position_tolerance`
  - Positionstoleranz 
  - Wertebereich: 0…255 
  - 255 ergibt eine Toleranz von +/- 2,55mm 
- `grip_force`
  - Greifkraft 
  - Wertebereich: 1…4 (ca. 50 N…200 N)
  - 4 Stufen möglich
- `position`
  - Position des erwarteten Werkstücks 
  - Wertebereich: 0…2000

### Positionserkennung
`gripper_is_base_position()`

`gripper_is_teach_position()`

`gripper_is_work_position()`

Zusätzlich zur absoluten Positionserkennung bietet der Greifer auch eine einfache Abfrage der Position. Der Greifer unterscheidet dabei zwischen 3 verschiedenen Positionen die binär abgefragt werden können. Zum einen gibt es den komplett geöffneten Zustand, für diese Abfrage wird die Funktion `gripper_is_base_position()`. Für den komplett geschlossenen Zustand gib es die Funktion `gripper_is_work_position()`. Für die eingespeicherten Werkstückpositionen gibt es die Funktion `gripper_is_teach_position()`. Alle drei Funktionen geben `True` zurück, wenn der Greifer gerade in dieser definierten Position ist. Wenn sie nicht innerhalb der Toleranz sind dann wird `False` zurückgegeben. 

## Fehlercodes 
Kommt es zu Problemen mit dem Greifer, kann man im Reiter Modbus unter dem Input-Port „Diagnosis“ nach dem übergebenen Wert schauen. Wenn man diesen in eine Hexadezimale Zahl umrechnet kann in der Dokumentation des Greifers (Dokument 1, unter „Weitere Dokumente“) unter dem Kapitel „Fehlerdiagnose“, die Bedeutung des Fehlercodes nachgeschaut und somit der Fehler nachvollzogen werden. 

