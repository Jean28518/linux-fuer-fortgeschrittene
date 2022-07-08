Grundlagen
==========


4 Freiheiten von Freier Software
--------------------------------
- **Verwenden:** Freiheit, die Software uneingeschränkt und für jeden Zweck einzusetzen.
- **Verstehen:** Freiheit, die Funktionsweise der Software untersuchen und verstehen zu können.
- **Verbreiten:** Freiheit, Kopien der Software zu verbreiten, um damit seinen Mitmenschen zu helfen.
- **Verändern:** Freiheit, die Software zu verändern und die Änderungen an die Öffentlichkeit weiterzugeben, sodass die gesamte Gesellschaft davon profitieren kann. 


      | > „Freie Software ist frei im Sinne von Redefreiheit, nicht im Sinne von Freibier!“ 
      | > **Richard M. Stallman**


Unterschied zwischen Open Source und Freier Software:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Open Source bedeutet lediglich, dass man den Code einsehen darf.
Dass man den Code automatisch anpassen, als bspw. große Firma nutzen, oder die Software weiterverbreiten darf, ist nicht unbedingt gegeben.

      -> Das Open Source Prinzip ist also ein Teil des Prinzips Feier Software. Freie Software ist viel mehr als "nur" Open Source Software.



Freie Sotware-Lizenzen
----------------------
Im folgenden werden die wichtigsten Software-Lizenzen vorgestellt, die einem im Linux-Alltag begegnen werden.

GNU GPL
^^^^^^^
Die GNU General Public License wurde maßgeblich von Richard Stallman im Jahr 1989 verfasst. Aktuell gibt es drei große Versionen der Software.
Sie stellt die 4 Freiheiten der Freien Software sicher und schließt die Garantie der Software aus.
Man darf die Software auch kommerziell nutzen, man kann auch damit unfreie Software, Bilder, etc. damit erstellen.

| **Aber einen "Haken" gibt es dann doch:**
| Die Lizenz definiert ebenfalls die Copyleft-Bedingung: 
| Bei der Weitergabe der (geänderten) Software müssen die 4 Freiheiten weitergegeben werden. 
| (Intern oder privat muss man den (/geänderten) Code nicht öffentlich teilen.)

Viele große Software-Projekte sind unter der GPL veröffentlicht:

- Linux-Kernel
- Weite Teile von Linux Distributionen, wie Linux Mint, Ubuntu, Debian, ..
- F-Droid
- Signal
- VLC
- Audacity
- Gedit
- Git
- MySQL
- WordPress
- Nextcloud
- ...

**Varianten**:

- "Strenger": GNU Affero General Public License (AGPL): *Der Quelltext muss für Nutzer verfügbar sein, auch wenn die Software nur auf einem Server läuft*
- "Schwächer": GNU Lesser General Public License (LGPL): *Erlaubt das Verwenden und Einbinden in eigene proprietäre Software, ohne den Code der gesamten Software offenlegen zu müssen. Das Ändern der LGPL-Software-Teile muss aber Endnutzern ermöglicht werden -> DLL/.so (Beispiel: glibc)*

MIT, Apache und BSD Lizenz
^^^^^^^^^^^^^^^^^^^^^^^^^^
Diese Lizenzen beinhalten ebenfalls die 4 Freiheiten, erlauben aber quasi "alles" mit der Software zu machen.
Man kann sie also problemlos auch in proprietäre Software-Projekte einbinden, weiterverkaufen, etc.

Die drei Lizenzen unterscheiden sich untereinander nur in Nouncen (Trademark, Patentbestimmungen).

Beispielsweise Android, Chromium, Apache (Webserver), oder Jitsi sind unter einer dieser Lizenzen veröffenticht.

Welche Lizenz sollte man nun wählen/bevorzugen?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Das liegt natürlich ganz an einem selber. Für den reinen End-Nutzer ist das egal. 

Entwickelt man die Software weiter, oder programmiert man gerade neue Software, sieht die Sache schon anders aus:
Möchte man den "Freie Software"-Gedanken für alle Ewigkeit festsetzen, so ist die GPL-Lizenz wahrscheinlich die bessere Wahl.
Soll man aber alles mit der Software machen können, dann ist einer der Apache, MIT, oder BSD Lizenzen zu wählen. 
Solche Projekte sind auch sehr erfolgreich. Man muss sich aber damit abfinden, 
dass ein Unternehmen Deine Software weiterentwickelt, den Quellcode schließt und für viel Geld verkauft.

MIT, Apache, BSD sind für Programm-Bibliotheken in der Regel besser geeignet. 
Sobald man nämlich Source-Code von einer unter der GPL lizenzierten Software verwenden möchte, muss man den gesamten Code auch unter einer der GPL ähnlichen Lizenz veröffentlichen. 
Das ist für viele Programmierer unpraktisch/unbrauchbar. (Abhilfe kann hier die vorher vorgestellte LGPL schaffen, die in der "Mitte" der beiden Fronten steht.)


Terminal-Grundwissen
--------------------
Diese Befehle sollte man alle aus dem FF können. 
Sie helfen einem in so ziemlich allen Lebenslagen auf Linux.
Sofern die Programme installiert sind, funktionieren die Befehle/Tastenkombinationen auf ALLEN Linux-Distributionen und UNIX-artigen Systemen.

========================== ================================================================================================================
   **Befehl**                 **Beschreibung**
-------------------------- ----------------------------------------------------------------------------------------------------------------
cd                         wechsle in das aktuelle Home-Verzeichnis (persönlicher Ordner)
cd <PFAD>                  wechsle ein Verzeichnis
ls (<PFAD>)                zeige Dateien an
ls -al #oder ll            zeige alle Dateien mit allen Infos an
cp <PFAD> <PFAD>           kopiere einen Pfad an einen anderen (neuen Pfad)  (mit -r kann man auch ganze Ordner kopieren)
mv <PFAD> <PFAD>           verschiebe eine Datei an einen anderen Pfad oder bennenne sie um
rm <PFAD>                  lösche eine Datei (oder mit -r einen ganzen Ordner)
mkdir <PFAD>               erstelle einen neuen Ordner
touch <PFAD>               erstelle eine leere Datei
ln -s <PFAD> <PFAD>        erstelle eine Verknüpfung
cat <PFAD>                 zeige den gesamten Inhalt der Datei
less <PFAD>                zeige einen Ausschnitt der Datei, scrolle mit den pfeiltasten, beende mit q
find -L -iname \*<Text>\*  suche nach einer bestimmten Datei oder einem Ordner.
 -                          -
clear                      leere das Terminal (Alternative)
pwd                        zeige aktuelles Arbeitsverzeichnis an
exit                       bash-Sitzung beenden (="abmelden")/Terminal beenden
man <BEFEHL> (<SEITE>)     lese Dokumentation zu dem jew. Befehl oder der Bibliothek
history                    zeige letzte Befehle
echo '<TEXT>' > <PFAD>     schreibe Text in eine (neue) Datei (!überschreiben!)
echo '<TEXT>' >> <PFAD>    schreibe Text in eine (neue) Datei (hinten anfügen)
ip a                       zeige aktuelle IP-Adressen
ping <Server>              zeige Erreichbarkeit zu einem Server (bspw. linuxguides.de, um den Internetzugang zu überprüfen)
wget <ADRESSE>             lade Datei aus dem Internet herunter
ncdu                       zeige detaillierten Speicherverbrauch an 
htop                       starte taskmanager
nano <PFAD>                bearbeite/erstelle eine Datei im Terminal-Texteditor
vim <PFAD>                 wie nano, nur krasser und schwerer, aber sehr lohnenswert
gedit <PFAD>               öffne/erstelle Textdatei und bearbeite sie mit dem grafischen Texteditor von Gnome
kate <PFAD>                öffne/erstelle Textdatei und bearbeite sie mit dem grafischen Texteditor von KDE
xed <PFAD>                 öffne/erstelle Textdatei und bearbeite sie mit dem grafischen Texteditor von Linux Mint
<BEFEHL> | grep '<TEXT>'   durchsuche die Ausgabe eines Befehls nach einem bestimmten Text-Ausschnitt
<BEFEHL> && <BEFEHL>       führe zwei Befehle nacheinander aus
firefox                    starte den Firefox im Terminal und lese den aktuellen log mit
firefox &                  starte den Firefox aus dem Terminal, und "schiebe" ihn in einen neuen Prozess (man kann normal weiter arbeiten)
<BEFEHL> &                 dies geht auch bei jedem anderen Programm/Befehl
 -                          -
<Strg> + <L>               leere das Terminal
<Strg> + <C>               breche eine laufende Aktion ab
<Strg> + <D>               sende ein End Of File (EOF) zeichen (manchmal nützlich zum Beenden von Eingaben)
<Pfeiltaste hoch>          wähle vorher ausgeführten Befehl aus
<q>                        beende einen Ansichts-Modus/ein Terminal-Programm (fuktioniert nicht bei allen, ist aber üblich)
<Strg> + <Shift> + <C>     Kopieren
<Strg> + <Shift> + <V>     Einfügen
========================== ================================================================================================================

.. note:: 
   Die meisten oben vorgestellten Befehle bieten noch eine Menge an weitreichenden Optionen an, 
   die entweder in der Man-Page (Dokumentation) oder mit der Option --help nachgelesen werden können.

Terminal-Statusanzeige
^^^^^^^^^^^^^^^^^^^^^^
Wenn man das Terminal öffnet, steht direkt immer Text, hinter diesen man einen Befehl eintippen kann:

.. code-block:: console

   jean@rechner:~/Downloads$

- ``jean`` zeigt den aktuellen angemeldeten Benutzer an, als wessen man die Befehle ausführt.
- ``rechner`` zeigt den Rechnernamen des Rechners an, auf dem man momentan angemeldet ist
- ``~`` zeigt den aktuellen Pfad an. ``~`` ist eine Besonderheit und steht immer für den persönlichen Ordner (in diesem Fall ``/home/jean``)
- ``$`` zeigt an, dass wir mit normalen Nutzerrechten arbeiten. Ein ``#`` würde Administrator-Rechte bedeuten

Relative und Absolute Pfade
^^^^^^^^^^^^^^^^^^^^^^^^^^^
Ein relativer Pfad ist immer "der Weg" vom aktuellen Verzeichnis aus zu einem anderen Verzeichnis oder zu einer Datei irgendwo auf dem Rechner.
**Je nach dem, in welchem Verzeichnis man sich befindet, ändert sich der relative Pfad zu einer Datei.**
Sie sind immer mit einem ``.`` vor dem Pfad zu erkennen oder beginnen direkt mit dem Namen eines Ordners/einer Datei.

Valide Beispiele für relative Pfade sind:

- ``./Downloads``
- ``Downloads``
- ``Downloads/``
- ``Dokumente/Dokument.odt``
- ``../user2/Downloads``
- ``./``
- ``../../etc/fstab``

.. tip:: 
   ``./`` referenziert das aktuelle Verzeichnis, ``../`` das Verzeichnis "oben drüber"

Absolute Pfade sind eindeutige Adressen auf einem Rechner, die immer vom Wurzelverzeichnis aus refernziert werden. Sie beginnen immer mit einem ``/``.

Valide Beispiele für absolute Pfade sind:

- ``/home/jean/Downloads``
- ``/home/jean/Downloads/``
- ``/home/jean/Dokumente/Dokument.odt``
- ``/home/user2/Downloads/``
- ``/home/jean/``
- ``/etc/fstab``


Administrator-Rechte
^^^^^^^^^^^^^^^^^^^^
Administrator-Rechte kann man mit ``sudo`` vor einem Befehl bekommen.
Möchte man sich im Terminal als Administrator "anmelden" und alle weiteren Befehle mit dem Terminal ausführen, 
kann man das abhängig von der Distribution mit ``sudo -i``, ``su -`` oder ``su root`` erreichen.

Text-Editor vim
^^^^^^^^^^^^^^^
Es kann sehr hilfreich sein, Dateien direkt im Terminal zu bearbeiten. 
Alleine für vim kann man einen kompletten Kurs anbieten.
Die nachfolgende Tabelle soll die wichtigsten Befehle zeigen.

============================= ======================================================
 **Befehl**                   **Beschreibung**
----------------------------- ------------------------------------------------------
i                             Einfügen-Modus
Esc                           Kommando-Modus
:w (<DATEINAME>)              Speichern (unter)
:q                            Beenden
:wq                           Speichern & Beenden
:q!                           Beenden ohne zu Speichern
:set <ZEILEN-NUMMER>          Gehe zu Zeile
u                             Zurück/Rückgängig
<STRG> + <R>                    Vorwärts
/<SUCHWORT>                   Starte Suche
  n                           Springe zu nächstem Suchergebns
  N                           Springe zum letzten Suchergbnis
:%s/<SUCHE>/<ERSETZEN>        Suche und Ersetze alle Vorkommnisse
:%s/<SUCHE>/<ERSETZEN>/c      Suche und Ersetze (mit Fragen)
(<ZAHL>) yy                   Kopiere (eine gewisse Anzahl an) Zeilen
(<ZAHL>) dd                   Lösche (eine gewisse Anzahl an) Zeilen
p                             Einfügen
============================= ======================================================

.. tip:: 
   Für Dich zu kompliziert? Nutze nano, der ist viel einfacher.
   Mit <Strg> + <O> speichert man ab, mit <Strg> + <X> beendet man den Text-Editor.
