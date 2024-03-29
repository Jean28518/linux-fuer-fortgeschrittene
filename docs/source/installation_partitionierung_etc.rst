Betriebssysteminstallation - Erweitert
======================================

In diesem Kapitel geht es um die Installation, Aktualisierung von Linux (Debian),
die Partitionierung, die Kernel und Bootloaderverwaltung.

Neu-Installation
----------------
Um ein robustes Linux aufzusetzen, 
ist es wichtig mit der Installation die richtigen Grundlagen zu setzen.
Geht die Installation schief, ist es in der Regel lohnenswerter, 
die Insstallation nochmal direkt von Anfang an auszuführen,
anstatt am System herumzudoktoren.
In den folgenden Zeilen behandeln wir die Installation von Debian.
Die Installation ist an allen Punkten selbsterklärend.
Genaues Lesen ist hier hilfreich!

Partitionierung
^^^^^^^^^^^^^^^
Die Partitionierung wird mittlerweile auch vom Debian-Installer automatisch 
übernommen, wenn man möchte.
In Ausnahmefällen kann die manuelle Partitionierung dennoch nützlich sein,
weswegen hier verschiedene Szenarien behandelt werden.

.. note:: Swap/Auslagerungsspeicher
    Der Auslagerungsspeicher wird verwendet, wenn der reguläre Arbeitsspeicher voll ist. 
    Somit werden dann dieser auf die Festplatte ausgelagert. 
    Dies geschieht traditionell in einer Swap-Partition. 
    Bei neuen Ubuntu basierten Systemen wird diese in der Regel nicht mehr benötigt.
    Stattdessen wird ein Swapfile verwendet, welcher nicht in der Partitionierung berücksichtigt werden muss.


**BIOS/Legacy Mode**:
Hierfür ist lediglich das Minimum notwendig: 

- Eine einfache Partition (ext4/btrfs) mit dem Einhängepunkt ``/``
- (Swap-Partition)

Der Bootloader muss am Ende "auf die Festplatte" installiert werden (in den MBR).

.. note:: GPT-Partitionstabelle:
    Wenn das System auf einer GPT-Partitionstabelle installiert ist, existiert der Master Boot Record (MBR)
    so in dieser Form nicht. 
    Dafür muss eine Partition mit dem Typ "Reservierter BIOS-Boot-Bereich" erstellt werden und der Bootloader
    auf diese Partition installiert werden. \ 
    (Der Installer warnt hier leider nicht, wenn man hier was falsch macht.) 



**EFI Mode**:

- FAT32 / EFI / ESP Partition, welche als bootfähig markiert wurde (500 MB reichen locker aus).
- Eine einfache Partition (ext4/btrfs) mit dem Einhängepunkt ``/``
- (Swap-Partition)

Der Bootloader muss am Ende auf die EFI Partition installiert werden.

.. note:: 
    Wenn bereits eine EFI-Partition existiert, muss keine neue zusätzlich erstellt werden.
    Mehrere Systeme teilen sich auch ohne Probleme die EFI-Partition.

Zusätzliche Partitionen
^^^^^^^^^^^^^^^^^^^^^^^
Zusätzlich zur ``/`` (Root) Partition, können auch einzelne Systemordner ausgelagert werden.
Typisch wärden: ``/home``, ``/boot`` (bei verschlüsselten Systemen), ``/tmp``, ...
Man kann aber auch seine ganz eigene zusätzliche Partition definieren.
Bspw. ist es nicht unüblich, eine zusätzliche ``/data`` Partition zu definieren.
Dort werden dann alle wichtigen Dateien auf dem Rechner gespeichert, welche später über Verknüpfungen
(Sym-Links) im Persönlichen Ordner verfügbar gemacht werden.


Logical Volume Manager
^^^^^^^^^^^^^^^^^^^^^^
Zusätzlich kann ein Logical Volume Manager konfiguriert werden, mit dem im Nachheinein die Partitionierung
einfacher werden soll.
Allerdings hat sich die Technologie wegen Mehraufwand bei der Konfiguration und die fehlende Intuitivität bis jetzt 
nur in wenigen Bereichen durchgesetzt.

Bei Interesse bietet Red Hat eine sehr ausführliche und leicht verständliche Anleitung an: 
https://access.redhat.com/documentation/de-de/red_hat_enterprise_linux/6/html/logical_volume_manager_administration/index

Verschlüsseltes System
^^^^^^^^^^^^^^^^^^^^^^
Um zu die ``/`` Partition zu verschlüsseln, muss eine weitere (ext4) Partition für den Boot-Sektor erstellt werden.
Der Einhängepunkt für diese Partition muss ``/boot``sein.
Über die normale ``/`` Partition muss eine dm-crypt Partition angelegt werden.
Die manuelle Vorgehensweise ist dort von Installer zu Installer unterschiedlich.
Meist bieten aber die Installer auch eine automatische Methode zur Verschlüsselung an, die empfehlenswert ist.

Kurs-Beispiel
^^^^^^^^^^^^^
- Wir erstellen eine Virtuelle Maschine und aktivieren EFI.
- Daraufhin starten wir die aktuellste Net-Install .iso Datei.
- In der Partitionierung erstellen wir folgende Partitionen:
  - EFI-Partition
  - Root-Partition (``/``)
  - Zusätzliche verschlüsselte ``/data`` Partition
- Als Desktop wählen wir später Xfce aus.

Upgrade auf große, neue Version
-------------------------------

.. warning::
    Upgrades bergen ein großes Risiko und sollten nur dann getätigt werden, wenn ausreichend Backups gemacht wurden.

Bei Ubuntu-Server gibt es einen eigenen Upgrade-Prozess, welcher über ``do-release-upgrade`` gestartet werden kann.

Um bei Debian auf eine neue Version zu aktualisieren, 
werden einfach die Software-Quellen in ``/etc/apt/sources.list`` auf den Codenamen der neuen Version gesetzt 
und dann ein ``apt dist-upgrade`` durchgeführt, was ein etwas erweitertes apt upgrade darstellt.
Der Unterschied ist hier, dass nicht mehr benötigte Pakete direkt automatisch mit entfernt werden.

.. hint::
    Sollten PHP-Seiten wie bspw. Nextcloud mit aktualisiert werden, muss geprüft werden, 
    welche PHP-Version die neue Betriebssysteminstallation mit sich bringt
    und ob die vorhandene Nextcloud-Version diese unterstützt.

Häufigkeit
^^^^^^^^^^

Es wird empfohlen, die Systeme wenige Monate (bei Server-Systemen: ein Jahr) nach Erscheinungsdatum einer großen Version jeweils zu aktualisieren.
Lässt man ein Linux einige Jahre auf einer Version weiter laufen, lohnt sich dann meist eine Neuinstallation mehr, 
welche vor allem bei großen Server-Systemen sehr aufwändig werden kann.


Kernel wechseln
---------------
Manchmal kann es hilfreich sein, das System mit einem neueren Linux-Kernel auszustatten,
besonders, wenn neue Hardware verwendet wird.
Dies kann man bspw. auf Linux Mint über die Aktualisierungsverwaltung erledigen,
auf debian kann man dies relativ einfach über APT erledigen:

::

    apt-cache search linux-image
    sudo apt install linux-image-<flavour>

Die gerade ausgeführte Version findet man mit dem Befehl: ``uname -a`` 

.. note::
    Je nach dem, ob man auf dem stable, testing, oder unstable branch ist werden andere Kernel-Versionen 
    verfügbar sein. Standardmäßig ist der stable branch aktiv. Wechseln kann man dies in ``/etc/apt/sources.list*``

Bootloader: GRUB
----------------
GRUB ist der gängigste Bootloader im Linux-Umfeld, welcher von fast jedem Linux verwendet wird.

Die Konfigurationsdatei ist ``/etc/default/grub`` zu finden. Nach dem Editieren ist der Befehl ``update-grub`` nötig.
Eine ausführliche Anleitung dazu ist hier zu finden: https://www.gnu.org/software/grub/manual/grub/grub.html

.. tip:: 
    Stattdessen sich durch die ``/etc/default/grub`` zu schlagen, gibt es eine einfachere, grafische Methode: 
    Grub-Customizer ist selbstverständlich und in den offiziellen Paketquellen bereits vorhanden: ``sudo apt install grub-customizer``.

GRUB erneut installlieren
^^^^^^^^^^^^^^^^^^^^^^^^^
Jede auf dem Rechner installierte Distribution bringt ihr eigenes GRUB mit. 
In den MBR einer Festplatte passt jedoch nur ein Verweis auf ein GRUB. 
Möchte man das GRUB des momentanen System erneut in den MBR installieren, und die anderen somit "überschreiben",
führt man bspw. den Befehl: ``sudo grub-install /dev/sda`` aus. 
Die zu verwendente Festplatte/Partition kann man bspw. mit dem Befehl ``sudo lsblk`` herausfinden.

.. note:: 
    Nutzt man anstattdessen EFI, muss man GRUB auf die jeweilige EFI Partition installieren.

GRUB reparieren
^^^^^^^^^^^^^^^
Entweder kann man dies über ``chroot`` auf einem Livesystem 
mit ``update-grub`` und ``grub-install`` manuell erledigen
oder man verwendet das Tool grafische, sehr einfache Tool ``Boot-Repair``, 
welches beispielsweise direkt über das Live-System von Linux Mint verfügbar ist.

Partitionen einhängen
---------------------
Man kann Partitionen auf Linux mit dem Befehl ``sudo mount /dev/PARTITION /ORDNERPFAD`` einhängen,
mit dem Befehl ``sudo umount /ORDNERPFAD`` aushängen:

::

    jean@debian:~$ sudo lsblk
    NAME        MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
    nvme0n1     259:0    0 476,9G  0 disk  
    ├─nvme0n1p1 259:1    0   511M  0 part  /boot/efi
    ├─nvme0n1p2 259:2    0  56,8G  0 part  
    ├─nvme0n1p3 259:3    0   977M  0 part  
    ├─nvme0n1p4 259:4    0 232,8G  0 part  
    │ └─secret  253:0    0 232,8G  0 crypt /data
    └─nvme0n1p6 259:5    0   186G  0 part  /
    jean@debian:~$ sudo mount /dev/nvme0n1p2 /mnt
    jean@debian:~$ sudo umount /mnt

Verknüpfungen
-------------
Bevor wir zu Verknüpfungen kommen, müssen wir den Unterschied zwischen Symbolischen (Softlink) und harten Links klären:

Softlink vs Hardlink
^^^^^^^^^^^^^^^^^^^^
Ein symbolischer oder weicher Link ist ein tatsächlicher Link zur Originaldatei, 
während ein harter Link eine Spiegelung der Originaldatei ist. 
Wenn Sie die Originaldatei löschen, geht der Softlink "kaputt", 
da er auf eine nicht mehr vorhandene Datei verweist.

Bei einem Hardlink ist es genau umgekehrt:
Selbst wenn Sie die Originaldatei löschen, enthält der Hardlink immer noch die Daten der Originaldatei. 
Denn der Hardlink fungiert als Spiegelung der Originaldatei.

In der Regel wollen Sie einen Softlink erstellen:

::

    ln -s RICHTIGE_DATEI VERKNÜPFUNG

Hardlink erstellen:

:: 

    ln RICHTIGE_DATEI VERKNÜPFUNG

/etc/fstab
----------
In der ``/etc/fstab`` Datei werden Partitionen definiert, 
welche zum Start des Systems eingehängt werden sollen.

Ein Beispiel ist:

::

    # <file system>                             <mount point>   <type>  <options>           <dump>  <pass>
    # Normale ext4 Partition für das Root File System:
    UUID=54d677fd-a528-4711-aee4-b0ba4a2532ee   /               ext4    errors=remount-ro   0       1
    # Verweis auf die EFI-Partition:
    UUID=889E-E45A                              /boot/efi       vfat    umask=0077          0       1
    # Verweis auf einen Swapfile. (Keine Swappartition!)
    /swapfile                                   none            swap    sw                  0       0
    # Verweis auf eine verschlüsselte Partition, welche am Ende als /data eingehangen wird:
    /dev/mapper/secret                          /data           ext4    defaults            0       0


Das manuelle Bearbeiten dieser Datei mit einem Text-Editor ist Gang und Gebe.
Pro Zeile wird eine Partition "beschrieben/referenziert".
Eine normale Partition wird mit der UUID referenziert, darauf folgt der Einhängepunkt (wie bei mount), 
der Typ ist selbstverständlich. Danach kann man verschiedene Optionen einfügen. In der Regel reicht hier ``defaults``.
Alle Optionen kann man unter ``man fstab`` nachschlagen.
Die ``dump`` Spalte gibt nur an, ob bei dem Backup-Programm dump dieser Eintrag mit berücksichtigt werden soll 
(Kann für uns ignoriert werden). 
Unter ``pass`` wird die Reihenfolge beim Einhängen während des Systemstarts angegeben. 
``0`` steht hier für "Ignorieren".

.. tip:: 
    Generell kann man auch sehr gut die Einhängeoption von Partitionen mit dem Programm ``gnome-disks`` 
    (Laufwerke) bearbeiten.