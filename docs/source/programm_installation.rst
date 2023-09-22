Programminstallation
====================

Die Paketverwaltung APT
-----------------------
Das APT (Advanced Packaging Tool) ist ein Paketmanagementsystem, welches um Debian entstand.
Damit kann man sehr einfach Pakete/Programme installieren. 
Man kann sagen, dass das ganze Betriebssystem aus diesen Paketen zusammengebaut ist und verwaltet wird.
Ohne APT läuft bei Debian also nichts.

Es gibt grafische Software-Verwaltungen wie ``gnome-software`` oder ``synaptic``, womit grafisch Pakete
installiert werden können.

APT nutzt als Grundlage ``dpkg``, welches einzelne ``.deb`` Dateien (=Paket) installieren kann.

Mit apt in der Kommandozeile kann man sehr zügig die Software auf dem Rechner verwalten.
Folgende Befehle können helfen:

========================== ================================================================================================================
   **Befehl**                 **Beschreibung**
-------------------------- ----------------------------------------------------------------------------------------------------------------
apt update                 Aktualisiere den Zwischenspeicher über verfügbare Pakete und Updates von den Spiegelservern.
apt search BEGRIFF         Suche nach einem gewissen Paket. Tipp: Lässt sich wunderbar mit ``| grep`` erweitern
apt install PAKET          Installiere ein oder mehrere Pakete (durh Leerzeichen getrennt)
apt remove PAKET           Entferne ein oder mehrere Pakete (ohne Entfernen von System-Konfigurationsdateien)
apt purge PAKET            Entferne ein oder mehrere Pakete (mit Entfernen von System-Konfigurationsdateien)
apt upgrade                Lade und installiere Updates
apt dist-upgrade           Lade und installiere Updates, entferne nicht mehr benötigte Pakete
apt autoremove             Entferne nicht mehr benötigte Pakete
apt autoclean              Räume internen Downloadspeicher auf
apt install -f             Behebe mögliche Fehler.

========================== ================================================================================================================

Dies war nur die obere Spitze des Eisberges.
Alle Optionen können hier eingesehen werden: ``man apt``

/etc/apt/sources.list
^^^^^^^^^^^^^^^^^^^^^
Unter dieser Datei stehen alle Serveradressen mit den verwendeten Optionen (meist der Version (des Codenamen)).
Von diesen Serveradressen werden Paketinformationen für bspw. Updates bezogen, 
sowie auch neue Pakete und Updates heruntergeladen.
Außerdem können noch im /etc/apt/sources.list.d/ Verzeichnis Dateien liegen, welche auch zur sources.list dazugehören.

Eine Zeile definiert jeweils eine Quelle.

::

    deb http://site.example.com/debian distribution component1 component2 component3

Zu Beginn wird der Typ definiert. Dieser kann entweder ``deb`` oder ``deb-src`` sein. 
Letzteres ist lediglich zum Ansehen von Quellen notwendig.

Darauffolgend wird eine normale http-Adresse angegeben, dann der Distributions-/Versionsname.
Beispiele wären: ``jammy`` oder ``bookworm``.

Letztendlich kann die Quelle dann noch weiter mit Optionen (Komponenten) spezifizert werden.
Einheitliche Regeln gibt es hierfür nicht distributionsübergreifend.

Flatpak
-------
Flatpak ist eine relativ junge Softwareverwaltung.
Flatpaks haben den Vorteil, dass sie auf jedem Linux laufen sollen und 
in der Regel top aktuelle Versionen eines Programms bieten. 
Dazu laufen sie in ihrem eigenen kleinen „Linux“, auch Sandkasten genannt. 
Allerdings können so bei manchen Flatpaks Kompatibilitäts-Probleme im Zusammenspiel 
vorallem mit anderen Programmen auftreten, da diese nicht in die kleinen „Sandkästen“ herein 
oder aus ihren eigenen heraus kommen.

Dies bringt eine neue Sicherheitsschicht mit sich: 
Durch diese kann man kontrollieren, was ein Flatpak-Programm auf dem System machen darf.

.. tip:: 
    Manchmal findet man bei der Dateiauswahl nicht die gewünschte Dateien oder Ordner. 
    Dies kann darauf hinweisen, dass das Flatpak keine Berechtigungen hat, 
    auf alle Dateien zuzugreifen. 
    Durch die Anwendung Flatseal kann man unter anderem die Dateiberechtigungen 
    für einzelne Flatpak-Anwendungen einstellen und ändern.

.. warning:: 
    Auch wenn Flatpak-Anwendungen eine neue Sicherheitsschicht haben, 
    kann diese durch bösartige Anwendungen umgangen werden. 
    Hat es eine Anwendung also abgesehen, aus dieser Schicht auszubrechen, 
    kann sie dies bis zu einem gewissen Grad tun.

Flatpak lässt sich nachinstallieren mit 

::

    sudo apt install flatpak``

Danach ist es empfehlenswert, das große Flathub Repository hinzuzufügen, 
welches DER Marktplatz für Flatpaks ist:

::

    flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo


.. tip:: 
    Nutzt man die Gnome-Software-Verwaltung, kann man dafür das Paket ``gnome-software-plugin-flatpak`` installieren.


Folgende Befehle können helfen:

========================== ================================================================================================================
   **Befehl**                 **Beschreibung**
-------------------------- ----------------------------------------------------------------------------------------------------------------
flatpak list               Liste alle Flatpaks inklusive installierten Bibliotheken und Runtimes auf.
flatpak list --app         Liste alle installierten Flatpak-Apps auf.
flatpak install NAME       Installiere ein Flatpak. In neueren Versionen reicht hierzu auch ein Suchwort.
flatpak update NAME        Aktualisiere ein Flatpak.
flatpak uninstall NAME     Deinstalliere ein bestimmtes Flatpak.
flatpak uninstall --unused Deinstalliere alle unbenötigten Flatpaks.

========================== ================================================================================================================

.. note:: 
    Flatpaks aktualisieren sich automatisch mit der Zeit. Deswegen ist hier kein Update-Befehl zu sehen.

Programm bauen/kompillieren:
----------------------------
In den allerwenigsten Fällen ist das selbstständige Bauen/Kompillieren eines Programms
notwendig. Dies im Kurs zusammenzufassen ist schwierig, da je nach verwendeter Technologie
die Art und Weise sehr unterschiedlich ist.
In der Regel ist in einer ``Install.md``, ``Readme.md``, ``Build.md`` oder ähnlich eine
Anleitung vorhanden.

Wir erledigen dies beispielhaft mit Supertuxkart (https://github.com/supertuxkart/stk-code)
(Eine Flatpak Version ist aber auch schon vorhanden, ganz ohne zu bauen ;)

In der Readme.md Datei finden wir einen Hinweis, dass in der Install.md Datei die Anleitung zum Bauen steht.
Diese befolgen wir nun Schritt für Schritt.

Zuerst müssen wir per Git den Quellcode herunterladen:

::

    git clone https://github.com/supertuxkart/stk-code stk-code
    svn co https://svn.code.sf.net/p/supertuxkart/code/stk-assets stk-assets


Wenn git und/oder subversion noch nicht installiert sind:

::

    sudo apt install git subversion


.. tip:: 
    Noch neu in Git? -> https://rogerdudler.github.io/git-guide/


Ebenfalls sehen wir, dass ein paar Abhängigkeiten (Dependencies) installiert werden müssen,
damit das Programm gebaut werden kann und später funktioniert.

Manchmal muss man sich noch mit ``apt search`` herumschlagen, um das passende Paket
für die verwendete Bibliothek zu finden. Aber hier ist direkt für Debian basierte Systeme
ein einfacher Install-Befehl angegeben. Nett!

::

    sudo apt-get install build-essential cmake libbluetooth-dev libsdl2-dev \
    libcurl4-openssl-dev libenet-dev libfreetype6-dev libharfbuzz-dev \
    libjpeg-dev libogg-dev libopenal-dev libpng-dev \
    libssl-dev libvorbis-dev libmbedtls-dev pkg-config zlib1g-dev


Das aufmerksame Lesen der Anleitung erspart aus Erfahrung sehr viel Zeit.
Im Abschnitt zum 'In-game recorder' steht, dass, wenn wir Ihn nicht verwenden wollen,
wir im späteren cmake-Befehl die Option ``-DBUILD_RECORDER=off`` hinzufügen müssen.
Das merken wir uns!

Als nächstes kommen wir zum Kompillieren:

::
    
    # go into the stk-code directory
    cd stk-code

    # create and enter the cmake_build directory
    mkdir cmake_build
    cd cmake_build

    # run cmake to generate the makefile
    # Hier also die oben beschriebene Option hinzufügen!!!
    cmake .. -DBUILD_RECORDER=off

    # compile
    make -j$(nproc)


Danach können wir die Datei ``supertuxkart`` im Ordner ``bin`` sehen.

