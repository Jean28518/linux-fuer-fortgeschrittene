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

**