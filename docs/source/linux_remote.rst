Linux Remote
============

OpenSSH
-------

Um sich auf einen anderen Linux-Rechner aufzuschalten, wird übelicherweise die SSH verwendet. 
Diese stellt folgende essentiellen Eigenschaften sicher:

- Authentifizierung der Gegenstelle -> kein Ansprechen falscher Ziele
- Verschlüsselung der Datenübertragung -> kein Mithören durch Unbefugte
- Datenintegrität -> keine Manipulation der übertragenen Daten

Möchte man sich auf einen Rechner verbinden, benötigt man die IP-Adresse oder den Domain-Namen, 
den Benutzernamen und evtl. den Port, welcher standardmäßig auf 22 eingestellt ist.
Folgende Beispiele können helfen

::

    ssh benutzer@1.2.3.4
    ssh benutzer@1.2.3.4 - p 42022 # SSH Dienst über Port 42022 erreichbar
    ssh benutzer@www.example.com