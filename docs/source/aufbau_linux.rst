Aufbau von Linux
================

Boot-Prozess (Komponenten)
--------------------------

1. BIOS
^^^^^^^
Das BIOS ist gar keine Komponente von Linux. 
Allerdings besitzt jeder Rechner ein solches BIOS, welches die Aufgabe besitzt, 
alle Hardware-Komponenten funktionsfähig zu machen und im Anschluss das Betriebssystem zu starten.

2. Master Boot Record (MBR)
^^^^^^^^^^^^^^^^^^^^^^^^^^^
Starteinträge liegen im MBR, welcher immer Am Anfang einer Festplatte liegt.

Alternativ werden in modernen Rechnern UEFI-Einträge unterstützt, welche eine etwas andere Startprozedur des Betriebssystem erfordern.
Die Einträge liegen in der GUID-Partitionstabelle, welche auf der Festplatte so existieren muss (spezielle Formatierung erforderlich).

3. Boot Loader (GRUB)
^^^^^^^^^^^^^^^^^^^^^
Sobald der jeweilige Eintrag gestartet wurde, wird bei den meisten Distributionen der GRand Unified Bootloader gestartet.
Er liegt schon auf der Linux-Partition und weiß ganz genau, welches Betriebssystem und welchen Kernel mit welchen Optionen er starten muss/kann.
Dies kann der Nutzer meist durch wählen mit den Pfeiltasten auch selber bestimmen.

4. Kernel (Linux)
^^^^^^^^^^^^^^^^^
Nachdem nun ein Eintrag im GRUB ausgewählt wurde, startet dieser den Linux-Kernel.
Er ist der zentrale Bestandteil eines Betriebssystems und kann als "Herz" gesehen werden.
Er hat direkten Zugriff auf die Hardware und verwaltet unter anderem den Arbeitsspeicher, 
Prozesse (wann soll welches Programm berechnet werden), Festplatten, etc...
In ihm sind auch die Treiber für Hardware-Teile enthalten.
Geräte, wie Maus und Tastatur werden hier direkt initalisiert und im Betriebssystem lauffähig gemacht.

5. Initial RAM Disk (initramfs image)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Nachdem der Kernel gestartet wurde, lädt dieser sofort das initramfs in den Arbeitsspeicher, welches alle weiteren wichtigen Dateien für den Systemstart enthält.
Manche ganz kleine Rechner hören hier schon auf und sind einsatzfähig (embedded Systems). 
Diese sind dann aber meist nur für eine ganz konkrete, kleine Aufgabe gebaut.

6. /sbin/init (Eltern-Prozess)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Auf dem initramfs liegt das Programm init, welches den Mutter-Prozess darstellt.
Dieser startet alle weiteren Prozesse (Programme).

7. Service-Manager (systemd)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Eines der wichtigsten Prozesse bei den meisten Linux-Distributionen ist systemd, welcher dafür verantwortlich ist, dass alle weiteren Prozesse (meist Services) gestartet werden.
Dies wäre der Netzwerkmanager, der Fenstermanger, der Druck-Server oder beispielsweise Webserver.
Irgendwann startet er auch den Anmeldebildschirm.
Ab hier können wir auch später selber gut eingreifen und Services aktivieren, hinzufügen, löschen, deaktivieren, neu starten etc..

8. Displaymanager
^^^^^^^^^^^^^^^^^
Nach einer Zeit wird auch dann schließlich der Anmeldebildschirm gestartet. Dahinter verbirgt sich meist ein Display Manager,
welcher die einzelnen Bildschirmauflösungen und die Anordnungen letztendlich konfiguriert.

9. X-Fenstermanager/Wayland Implementierung
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Meldet man sich als Benutzer an, so wird ein Fenstermanager gestartet, der einzelne Fenster wie beispielsweise das Browser-Fenster
oder die Taskleiste oder auch häufig Bildschirmaufnahmen verwaltet. 
Traditionell ist der der X-Server dafür zuständig. 
Dieser wird aber nach und nach auf allen Linux-Distributionen durch Wayland-Implementierungen ersetzt, 
da X-Server sehr sehr alt ist und große Sicherheitslücken mit sich bringt, was beispielsweise Bildschirmaufnahmen angeht.

10. Desktop
^^^^^^^^^^^
Nun, nachdem der Fenstermanager gestartet wurde wird direkt (meist damit sehr eng verbunden) 
die eigentliche Oberfläche mit der Taskleiste und dem Menü gestartet.
Jetzt kann man seinen Rechner wie gewohnt nutzen.



Ordnerstruktur
--------------
Linux ist komplett in Ordnern und Dateien organisiert. Man kann ALLES einsehen.

- **/bin/** Alle Programme (zeigt auf /usr/bin)
- **/boot/** Startinformationen für den Bootloader
- **/dev/** Einzelne Geräte-Dateien
- **/etc/** Spezifische Rechner-Konfigration für Fenstermanger, Netzwerkmanager, einzubindende Festplatten, Webserver, etc..
- **/home/** Persönliche Ordner aller normalen Nutzer
- **/lib/** Essentielle dynamischen Bibliotheken und Kernel Module (".dll" Verzeichnis)
- **/media/** Einhängepunkt für externe Laufwerke
- **/mnt/** Einhängepunkt für temporär eingehängte Partitionen
- **/opt/** Addon Dateien für manche Software 
- **/sbin/** Essentielle Programme (zeigt auf /usr/sbin)
- **/srv/** Daten für Services (meist ungenutzt)
- **/tmp/** Temporäre Dateien, werden beim Herunterfahren/Neustarten gelöscht.
- **/usr/** User System Resources: Alle Benutzer Resourcen und Programme
- **/var/** verschiedenes, (beispielsweise Ort von Websites, oder log-Dateien)
- **/root/** persönlicher Ordner vom Root-Benutzer
- **/proc/** virtuelles Dateisystem (existiert nicht wirklich) für die Kernel-Verwaltung von Prozessen, etc. in Text-Dateien