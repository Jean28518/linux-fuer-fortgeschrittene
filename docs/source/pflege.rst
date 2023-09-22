Systempflege
============

Richtig eingerichtet bedarf ein Linux-System nur sehr wenig Pflege.
Wie Sie das erreichen, wird in diesem Kapitel geklärt.
Ebenfalls werden Reparatur-Maßnahmen behandelt.

Automatische Systempflege
-------------------------
Wichtig: Ein Linux-System "verschmutzt" im Regelbetrieb kaum.

Regelmäßige Neustarts
^^^^^^^^^^^^^^^^^^^^^
Nicht mehr benötigte, temporäre Dateien sowie zu unrecht belegte Arbeitsressourcen könnten sich aber dennoch während des Betriebs nach und nach einschleichen.
Meist entstehen solche Situation in Kombination mit nicht optimal programmierter Software (was wirklich schwierig ist), die auf dem System läuft.
Um diesem vorzubeugen, wird ein täglicher Neustart des gesamten Systems empfohlen, wenn sich dies einrichten lässt.

Bei einem Neustart reinigt sich das System selber:
Alle temporären Dateien im /tmp Ordner werden gelöscht und ungenutzer, dennoch belegter Arbeitsspeicher wird wieder freigegeben.
Des weiteren wird nach einem Neustart garantiert die aktuellste, installierte Software genutzt.
Nach einem größeren Update wurde zwar die auszuführende Datei ersetzt, 
aber besondes die systemkritische Software, die sich nicht ohne weiteres im laufenden Betrieb neustarten lässt,
läuft bis zu einem Neustart in der alten Version weiter.

Zudem beugt ein regelmäßiger Neustart aller Komponenten seltenen Bugs vor, 
die sich vielleicht erst nach einigen, komplizierten, schwer reproduzierbaren Betriebsszuständen ergeben.

Ein einfacher cronjob (im Kapitel zum Skripting beschrieben) reicht hier aus. Anstatt den einzelnen Befehl ``reboot`` anzugeben, 
wird empfohlen, den Pfad zur ausführbaren Datei anzugeben, der ``/sbin/reboot`` ist.

::

    sudo crontab -e
    0 4 * * 2 /sbin/reboot

Automatische Aktualisierungen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Ab und zu werden Sicherheitslücken im Betriebssystem und in der verwendeten Software bekannt.
Um immer abgesichert zu sein, werden automatische Aktualisierungen unbedingt empfohlen.

::

    sudo apt install unattended-upgrades apt-listchanges
    sudo dpkg-reconfigure -plow unattended-upgrades
    # Hier durch die Dialoge mit den Pfeiltasten, Tabulator und der Leer- oder Eingabetaste navigieren.

Nun wird unattended-upgrades in regelmäßigen Abständen vollautomatisch Aktualisierungen von Sicherheitskritischen Anwendungen machen.

.. note::
    Möchten Sie Ihr Linux vollautomatisch komplett aktualisieren lassen (alle verfügbaren Aktualisierungen anwenden),
    können Sie dies unter ``/etc/apt/apt.conf.d/50unattended-upgrades`` einstellen.
    In vielen Konfigurationen macht dies aber auch ein cron job, welcher ein ``apt update && apt upgrade`` ausführt.


Backups
-------
Da Datenverluste nie schön sind, möchte man im Notfall möchte man immer ein Backup bereit haben.
Dabei gibt es viele verschiedene Wege, die zum Ziel führen.

rsync
^^^^^
Behandeln wir zunächst eine alte aber auch sehr weit verbreitete Konstante im Linux-Bereich.
Rsync synchronisiert/spiegelt am Ende definierte Ordner und Dateien an einen anderen Ort.
Falls Dateien an dem einen Ort kaputt gehen, kann man diese vom anderen Ort wiederherstellen.
Die Vorteile von rsync sind, dass es relativ einfach einzurichten ist, sowie die Dateien in unveränderter Form (unkomprimiert) vorliegen.
Es gibt beispielsweise ein ausführliches, funktionsreiches BASH-Skript und https://wiki.ubuntuusers.de/Skripte/Backup_mit_RSYNC/#Das-Skript

Der Nachteil an rsync ist meiner Ansicht nach die fehlende Komprimierung sowie kompliziertere Versionierung von Backups.
Ebenfalls werden sehr große Dateien sehr ineffizient gesichert.

Um ein komplettes Verzeichnis an einen anderen Ort zu spiegeln reicht folgender Befehl:

::
    sudo apt install rsync

    rsync -rptgo /ort/der/zu/sicherenden/daten /backup/ort

    # Aufschlüsselung des Befehls (von links nach rechts):
    # -r: Behandle den Ordner rekursiv (sichere alle Unterordner mit)
    # -p: Behalte die Rechte der Datei bei
    # -t: Behalte den Zeitstemepl der Datei bei
    # -g: Behalte die Gruppenrechte der Datei bei
    # -o: Behalte die Besitzrechte der Datei bei (nur wenn root der auszuführende Nutzer ist)

rsync hat weitere zahlreiche Optionen, die sich hier einsehen lassen: ``man rsync``

borg
^^^^
Borg ist extra für Backups gebaut worden: Es verschlüsselt, komprimiert und speichert verschiedene Versionsstände einer Datei sehr effizient ab.
Dafür ist borg geringfügig komplizierter in der Handhabung.

::
    sudo apt install borgbackup

    # Erstelle ein Backup Borg Repository (in dem dann die Backups später gespeichert werden)
    sudo mkdir /backup
    sudo borg init /backup --encryption=none
    sudo borg init /backup --encryption=repokey     # Alternativ für Verschlüsselung, nicht unbedingt empfohlen

    # Backup erstellen
    DATE=`date +"%Y-%m-%d"`
    sudo borg create /backup/::$DATE /ordner1 /ordner1 --exclude-caches # Beliebig viele ordner möglich
    sudo borg create /backup/::$DATE /etc /home /opt /root /usr /var/www /var/lib /var/log --exclude-caches # Mein Standard-Befehl

    # Später auf backup zugreifen
    sudo -i # Werde zum Administrator
    su - # Werde zum Administrator (Alternative, wenn sudo nicht installiert ist)

    # Borg-Repository lesbar machen und einhängen
    borg mount /backup /mnt

    # Nun kann pro erstellten Schnappschuss bequem in den Dateien herumgestöbert werden
    cd /mnt
    ls

    # Am Ende nicht vergessen, das borg repository zu schließen/aushängen:
    borg umount /mnt

    # Root-Status beenden
    exit

Borg funktioniert natürlich auch für normale Nutzer, dennoch macht dies in den Anwendungsfällen für Server wenig Sinn.

.. tip:: 
    Möchte man auf seinem privaten Computer grafisch Backups erledigen? 
    Installieren Sie das Programm ``Pika Backup`` oder ``Pika Datensicherung``,
    welches eine grafische Oberfläche für einfache Borg-Backups hat.
    Ich selber nutze das Programm für meine Rechner.

.. note:: 
    Sollten Sie keine externe Fesplatte haben empfehlen wir für Backups dringend eine.
    Sollte Ihre Festplatte im Computer kaputt gehen oder Ihr Computer beispielsweise einen Wasserschaden bekommen,
    sind Ihre Daten ohne externe Fesplatte häufig schon verloren.

.. tip:: 
    Sowohl borg als auch rsync haben ssh support. Somit können Backups automatisch über das Netzwerk verschickt werden.
    Eine Anleitung für das Borg-Backup System liegt in den Kurs-Unterlagen bei.

Fehlerbehebung
--------------
In diesem Kaptitel soll auf einige Möglichkeiten der Fehlerbehebung eingegangen werden.

Probleme in der Paketverwaltung
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Hat man den Rechner während Aktualisierungen heruntergefahren helfen folgende Befehle:

::

    sudo dpkg --configure -a
    sudo apt install -f

Angenommen, das Paket chromium wurde nicht richtig installiert oder aktualisiert und es befindet sich in einem inkonsistenten Zustand.
Dann helfen häufig nacheinander folgende Befehle, auch wenn Fehlermeldungen erscheinen:

::

    sudo apt purge chromium
    sudo apt autoremove --purge
    sudo apt install chromium
    sudo apt reinstall chromium

**Angenommen, sie haben einen Abhängigkeitsfehler (falsche Version) in den Paketen:**

- Schalten Sie PPAs, Fremdquellen und weitere zusätzliche Softwarequellen aus. 
  (``/etc/apt/sources.list`` und ``/etc/apt/sources.list.d/``, oder am besten über grafisch Programme)
- ``sudo apt update``
- ``sudo apt dist-upgrade``
- Versuchen Sie störende Pakete, die eine ungültige Version anfordern, zu entfernen
- ``sudo apt update``
- ``sudo apt dist-upgrade``
- Schalten Sie nach und nach die benötigten Fremdquellen und PPAs zu, die wirklich nötig sind und führen Sie nach jedem Schritt ``sudo apt update`` und ``sudo apt dist-upgrade`` aus.

System fährt nicht mehr hoch
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Folgende Checkliste kann helfen:

- Funktioniert Grub? -> Ansonsten Bootloader neu installieren (Siehe Kapitel Installation)
- Funktioniert ein anderer Linux-Kernel? (im GRUB wählbar) -> Wenn ja, den anderen neu installierne oder entfernen.
- Funktioniert der Recovery Modus? -> Ansonsten Timeshift-Wiederherstellung starten, wenn vorhanden
     - Wennn der Recovery-Modus funktioniert:
         - fsck ausprobieren
         - auf vollen Festplattenplatz überprüfen
         - 
- Werden alle Festplatten erkannt? Sind alle Festplatten lauffähig?
- Funktioniert das System bei der Installation aber fährt danach nicht mehr richtig hoch?
	-> Versuchen im Compatiblity Mode zu starten und dann updates zu machen, sowie Treiber zu installieren
	- Kernel Parameter nomodeset?
- Wenn alle Punkte zuvor ausgeschlossen  werden konnten, hilft es häufig nach dem auf dem Bildschirm angezeigten Fehler im Internet zu suchen.
- Kommt man nach einer Stunde nicht weiter? -> Neuinstallation häufig dann der schnellere Weg.

Debugging im laufenden System
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
- Sollte ein Service oder das Bebtriebssystem nicht mehr einwandfrei funktionieren, hilft es, mit dem Befehl ``journalctl`` auf Spurensuche zu gehen.
- Auch hilft es mit ``systemctl status`` den entsprechenden Service nachzusehen.
- Manchmal hilft eine Neuinstallation des betreffenden Paketes. Gerne mal mit ``apt purge paketname`` (löscht alle Konfigurationsdateien des Pakets) versuchen.
- Werden Geräte nicht erkannt, können Sie mit ``lspcsi`` oder ``lsusb`` weiter untersucht werden.


Notfallsystem
^^^^^^^^^^^^^
Sollte mal gar nicht's funktionieren und die Neuinstallation im Raum stehen, sollten hierfür alle wichtigen Daten gesichert werden. 
Tatsächlich hat sich dafür die Linux Mint .iso Datei in den letzten Jahren sehr bewährt. 
Gparted, Laufwerke (Gnome-Disks), Timeshift und sogar Boot-Repair sind direkt verfügbar.
Ebenfalls bietet der Linux-Mint USB-Stick auch inoffiziell schreibbaren Speicher unter ``/var/log``.

Ein ebenfalls sehr interessantes Notfallsystem ist rescatux.org oder das alt bewährte Knoppix.