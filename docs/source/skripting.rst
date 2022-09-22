Skripting
=========

Sie dachten, der Kurs neigt sich langsam dem Ende?
Absolut nicht, jetzt geht's erst richtig los!
Wer am Ende Skripting in Linux vollumfänglich versteht braucht sich quasi um nichts mehr Sorgen zu machen.
Denn so kann man sein Linux "kinderleicht" anpassen modifizieren und bequem nutzen.
Zugegeben ist bis zu diesem Level im Bash-Scripting ein weiter Weg.
Aber gehen wir in unserem Kurs den ersten Schritt!

BASH
----
Bevor wir uns dem eigentlichen Skripten zuneigen, gehen wir ein Schritt zurück und betrachten die Schwarz-Weiße Schrift, 
die uns die gesamte Zeit begleitet etwas genauer.
Genaugenommen "Skripten" wir mit jedem Befehl schon. 
Alle Befehle und alle Zeilen, die wir in der Konsole eingeben, können nahezu 1 zu 1 im Skript abgebildet werden.

Doch was ist jetzt eigentlich die Bash?
Sie ist eine Shell. Also eine Software die den Benutzer mit dem Betriebssystem interagieren lässt.
Der Clou: Nahezu alle Linux-Komponenten sind über diese Shell ansteuerbar. 
Am Ende kann man durch die BASH mit dem gesamten Linux-Betriebssystem kommunizieren.

Dafür kann man aber durch die BASH schlecht Endnutzerprogramme bedienen. Schonmal versucht Firefox oder Kdenlive mit der Bash zu bedienen?
Das geht quasi nicht. Aber tatsächlich gibt es auch über die BASH ansteuerbare Prgramme, womit man Tastatur und Mauseingaben simulieren kann.
Verrückt.

BASH-Scripts
------------
Einen kompletten Kurs in BASH zu geben würde Tage dauern. 
Deswegen werden wir nur die wichtigsten Konzepte behandeln, in dem verschiedene BASH Skripte gezeigt werden.
Man kann sich den Code jeweils in eine Datei mit der Endung .sh packen und dann ausführen. 
Ein Workflow wäre:

::

    nano hello-world.sh
    # Jetzt hier den Code einfügen, und abspeichern
    chmod +x hello-world.sh 
    ./hello-world.sh


Hello World
^^^^^^^^^^^

:: 
    
    #!/bin/bash

    # Die erste Zeile zeigt dem Aufrufer den Interpreter an. 
    # Wir wollen die Datei mit dem Interpreter an der Stelle von /bin/bash verarbeiten.
    # Möchte man bspw. ein Python Skript schreiben, heißt es: #!/bin/python3

    # Achso, so schreibt man übrigens Kommentare. Einfach ein '#' vor die Zeile packen

    # Jetzt zu unserem eigentlichen Programm:
    echo "Hello World"

Einschub: Umleiten der Ausgabe
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Mit ``>`` leitet man eine Ausgabe um. Man kann beispielsweise die ganze Ausgabe direkt in eine Datei leiten.
Ruft man ``./hello-world.sh > log.txt`` auf, wird die Ausgabe in eine Datei geleitet.
Existiert die Datei bereits, wird diese dann überschrieben. Will man lediglich Zeilen anhängen, 
ist statt ``>`` ein ``>>`` zu verwenden.

Möchte man nur die normale Ausgabe umleiten, schreibt man ``1>``.
Möchte man nur Fehlermeldungen umleiten, schreibt man ``2>``.

Möchte man einen Ausgabe-Stream weder in einer Datei haben noch "im Terminal", 
kann man den Stream zu ``/dev/null`` umleiten. 
Dann wird dieser Stream quasi gelöscht. 
Beispielsweise mit ``./hello-world.sh 1> /dev/null`` landet das "Hello World" nirgendswo.

Normale Befehle starten
^^^^^^^^^^^^^^^^^^^^^^^

::

    #!/bin/bash

    # Startet den Firefox und man bekommt die Ausgabe mit.
    # Der BASH-Interpreter geht erst eine Zeile weiter, wenn Firefox wieder beendet wurde.
    firefox 

    # Möchte man allerdings, dass Befehle nur angestoßen werden und der Interpreter direkt zur nächsten Zeile geht, hängt man ein '&' dran:
    firefox &

    # Man kann so auch mehr Programme nacheinander starten:
    libreoffice &
    thunar &

Administrative Befehle wie apt
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

:: 

    #!/bin/bash
    
    # Für apt-Befehle sollte man apt "sagen", dass es gerade durch ein Skript ansgesteuert wird.
    # Dies macht man hierrüber. Klären wir später, was das genau bedeutet:
    export DEBIAN_FRONTEND=noninteractive

    sudo apt update

    # Das -y bedeutet, dass allem zugestimmt werden soll.
    sudo apt dist-upgrade -y

    sudo apt install cowsay

    # Ist ähnlich wie echo. Aber cooler ;)
    cowsay "Skripten ist cool!"


Variablen
^^^^^^^^^

::

    #!/bin/bash

    # Variable definiert man relativ einfach:
    VARIABLE_1="Hallo Welt!"
    PROGRAMM="echo"

    # Auf eine Variable wieder zugreifen:
    $PROGRAMM "$VARIABLE_1"

    # Man schreibt Variablen in BASH groß.
    # Um auf eine Variable zuzugreifen, setzt man einfach ein '$' davor.
    # Am Ende wird die Variable wieder "in Text verwandelt".

    ANZAHL=30

    # So gibt man Variablen aus:  
    echo $ANZAHL
    
    # Oder mit Beschreibung:
    echo "Alter: $ANZAHL Jahre"
    
    ANZAHL=40

    # Variablen wieder löschen/zurücksetzen:
    unset ANZAHL

    echo $ANZAHL

    # -> Die 40 wird niemals ausgegeben


Eingabe
^^^^^^^

::

    #!/bin/bash

    read NAME

    echo "Hello $NAME!"

 
Konditionen (if)
^^^^^^^^^^^^^^^^

::

    #!/bin/bash

    # Variable definiert man relativ einfach:
    ERSTE_ZAHL=20
    ZWEITE_ZAHL=30

    # Die if-Struktur ist sehr gewühnungsbedürftig
    if [[ $ERSTE_ZAHL == $ZWEITE_ZAHL ]]; 
    then
        echo "Die Nummern sind gleich"
    fi

    # Das ganze geht auch mit "-eq" für "equal"
    if [[ $ERSTE_ZAHL -eq $ZWEITE_ZAHL ]]; 
    then
        echo "Die Nummern sind gleich"
    fi

    # Mit -n bei echo wird am Ende kein '\n' geschrieben (Neue Zeile-Zeichen)
    echo -n "Bitte geben Sie eine Zahl ein: "
    read INPUT

    # -gt: greater than, -lt: lower than
    if [[ $INPUT -gt $ZWEITE_ZAHL ]]; 
    then
        echo "Die eingegeben Nummer ist größer als $ZWEITE_ZAHL"
    else
        echo "Die eingegeben Nummer ist kleiner oder gleich $ZWEITE_ZAHL"
    fi

Fazit über BASH-Skripting
^^^^^^^^^^^^^^^^^^^^^^^^^

Wie Sie sehen, wird BASH-Skripting meiner Ansicht nach sehr schnell sehr hässlich und unübersichtlich,
sodass ich mich persönlich nicht weiter eingearbeitet habe.
Für einfache Skripts reicht es, sowohl zum Verstehen und korrigieren.
Suche ich etwas spezielles, muss ich das sowieso nachschlagen.

**Möchten Sie größere System-Skripts schreiben?**
Ich empfehle anstattdessen Python. 
Python ist in der Regel auf jeder Linux-Distribution vorinstalliert und (für mich) viel intuitiver (ja im Gegensatz zu BASH ist alles intuitiver) 
als auch übersichtlicher.
Ich habe mir ein dafür eine eigene kleine Bibliothek geschrieben, die ich stetig ausbaue.
Verwenden Sie diese gerne, lernen Sie daraus, Kopieren Sie sie, oder machen Sie was auch immer mit ihr.
Sie gehört ihnen!: https://github.com/Jean28518/jtools-unix-python

**Sie möchten Bash weiterlernen?**
Ein ausführlichen Guide gibt es hier:
https://wiki.ubuntuusers.de/Shell/Bash-Skripting-Guide_f%C3%BCr_Anf%C3%A4nger/


Umgebungs-Variablen
-------------------

Erinnern Sie sich vorhin an ``export DEBIAN_FRONTEND=noninteractive`` ?
Dies ist eine sogenannte Umgebungs-Variable.
Mit dem Befehl ``export`` kann man in der aktuellen BASH-Sitzung eine Variable stetzen,
die dann ein anderes Programm später (wenn es auch in dieser BASH-Sitzung ausgeführt wird),
auslesen kann.
Schließt und öffnet man das Terminal neu, so ist die Variable weg.

Möchte man anstattdessen immer, wenn man das Terminal öffnet eine solche Variable setzen?
Dann kann man das in der Datei ``~/.bashrc`` definieren.
Dies ist am Ende auch nur ein BASH-Skript, welches vor dem Start eines Terminals oder eine Sitzung gestartet wird.

alias
^^^^^
Hiermit kann man Befehle verkürzen.
Schreibt man beispielsweise 

::

    alias update='sudo apt update && sudo apt dist-upgrade -y'

in die ``~/.bashrc``, kann man nach einem Neustart des Terminals einfach ``update`` eingegeben, und es wird anstattdessen diese Befehlskette ausgeführt.

Systemctl
---------
.. note:: 
    In den folgenden Zeilen werden wir ``mintetest-server`` als Beispiel verwenden.
    Anstattdessen eigenen sich aber alle anderen Spiele-Server, Webserver, Datenbankserver oder andere Dienste wie bspw. docker.

Nahezu alle Dienste auf dem Linux-Rechner werden in der Regel über Systemd verwaltet. (Siehe Aufbau von Linux).
Folgende Befehle sind hilreich:

::

    # Mit 'q' kann man den Befehl wieder beenden.
    systemctl status minetest-server.service

    systemctl stop minetest-server.service

    systemctl start minetest-server.service

    systemctl restart minetest-server.service

    # Danach wird bei jedem Start von Linux dieser Service ebenfalls mitgestartet.
    systemctl enable minetest-server.service

    # Danach wird bei jedem Start von Linux dieser Service nicht (mehr) mitgestartet.
    systemctl disable minetest-server.service

Eigenen Service definieren
^^^^^^^^^^^^^^^^^^^^^^^^^^
Der oben verwendete Service wurde durch ``sudo apt install minetest-server``
automatisch hinzugefügt, aktiviert und gestartet.

Die Service-Datei kann man mit folgendem Befehl einsehen:

::

    cat /etc/systemd/system/multi-user.target.wants/minetest-server.service 

    # Ausgabe: 
    [Unit]
    Description=Minetest multiplayer server minetest.conf server config
    Documentation=man:minetestserver(6)
    After=network.target
    RequiresMountsFor=/var/games/minetest-server

    [Service]
    Restart=on-failure
    User=Debian-minetest
    Group=games
    ExecStart=/usr/lib/minetest/minetestserver --config /etc/minetest/minetest.conf --logfile /var/log/minetest/minetest.log
    StandardOutput=null

    [Install]
    WantedBy=multi-user.target

Im Grunde genommen kann man diese Datei einfach kopieren, umbenennen und anpassen.
Wie bei so vielem in Linux gibt es hier auch etliche Konfigurationsmöglichkeiten, welche wir nicht im Detail durchsprechen.
Eine vollständige Dokumentation findet man hier: https://www.freedesktop.org/software/systemd/man/systemd.service.html

Im Laufe meiner Linux-Jahre habe ich mir eine .service-Datei Vorlage erstellt, die ich gerne hier teilen möchte:

::

    [Unit]
	Description=Name der Anwendung              # Optional

	[Service]
	Type=simple
	RemainAfterExit=yes                         # Optional
	WorkingDirectory=/arbeits/verzeichnis       # Optional
	ExecStart=/pfad/zur/ausführbaren/datei      
	Restart=on-failure                          # Optional
	RestartSec=5                                # Optional

	[Install]
	WantedBy=multi-user.target                  # Optional
	
Neue ``.service``-Dateien speichere ich in ``/usr/lib/systemd/system/``. (Eventuell muss der ``system`` Ordner erst erstellt werden)

Log/Ausgabe von Services einsehen:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Verwenden Sie dazu den Befehl ``journalctl``. Mit der Taste 'Ende' kommen Sie an das Ende sowie an die aktuellsten Einträge.

Mit dem bspw. Befehl ``journalctl -u minetest-server.service`` sehen Sie nur die Ausgabe des minetest-server.

Sie können die Ansicht mit der Taste 'q' beenden.


Wiederkehrende, automatische Ausführung von Skripts
---------------------------------------------------
Soll beispielsweise jeden Tag um 2 Uhr morgens ein BackUp gemacht werden? Nichts einfacher als das.

::

    # Erstellen Sie dafür beispielsweise ein einfaches Bash-Skript im ``/root/`` Verzeichnis. 
    # (Home-Verzeichnis des imaginären Administrators)
    sudo nano /root/backup-minetest.sh



    # Schreiben Sie diesen Text in die Datei:
    #!/bin/bash
    systemctl stop minetest-server.service
    # Wird im nächsten Kapitel genauer behandelt:
    rsync -rptgo /var/games/minetest-server /backups/minetest-server/
    systemctl start minetest-server.service



    # Machen Sie die Datei ausführbar
    sudo chmod +x /root/backup-minetest.sh

    # Testen Sie immer Skripts vor der Automatisierung!
    sudo /root/backup-minetest.sh

    # (Sie können Strg+C drücken, wenn keine Fehlermeldung kommt. 
    # Allerdings sollte das in diesem Beispiel nicht all zu lange dauern.)

    # Nun müssen wir dem System nur noch sagen, 
    # dass wir das Skript jeden Tag automatisch ausgeführt haben wollen:
    sudo crontab -e

    # Wählen Sie hier Ihren Lieblings-Editor

    # Nun fügen Sie ganz am Ende eine neue Zeile hinzu:
    0 2 * * * /root/backup-minetest.sh

    # Erklärung (von links nach rechts):
    # 0: Wenn die Minuten Zahl = 0 ist
    # 2: Wenn die Stunden Zahl = 2 ist (immer im 24 Stunden Format)
    # *: Tag des Monats egal (Mögliche Eingaben: *, 1 - 31)
    # *: Monat egal (Mögliche Eingaben: *, 1 - 12)
    # *: Tag der Woche egal (Mögliche Eingaben: 0 - 7) (wobei 0=Sonntag, 1=Montag, ..., 6=Samstag, 7=Sonntag)
    # /root/backup-minetest.sh: Offensichtlich der Pfad zum auszuführenden Skript

    # Nach dem Abspeichern der Datei wird das Skript nun jeden Tag um 2 Uhr morgens 
    # automatisch ausgeführt, sofern der Rechner zu dieser Zeit läuft.







