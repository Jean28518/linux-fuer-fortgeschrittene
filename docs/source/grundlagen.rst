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


Unterschied zwischen Open Source und Freier Sofware:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
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
- Audacity
- Gedit
- Git
- MySQL
- WordPress
- Nextcloud
- ....

**Varianten**:
- "Strenger": GNU Affero General Public License (AGPL): *Der Quelltext muss für Nutzer verfügbar sein, auch wenn die Software nur auf einem Server läuft*
- "Schwächer": GNU Lesser General Public License (LGPL): *Erlaubt das Verwenden und Einbinden in eigene proprietäre Software, ohne den Code der gesamten Software offenlegen zu müssen. Das Ändern der LGPL-Software-Teile muss aber Endnutzern ermöglicht werden -> DLL/.so (Beispiel: glibc)*

MIT und Apache 2 Lizenz
^^^^^^^^^^^^^^^^^^^^^^^




Terminal-Grundlagen
-------------------














.. code-block:: console

   (.venv) $ pip install lumache

Creating recipes
----------------

To retrieve a list of random ingredients,
you can use the ``lumache.get_random_ingredients()`` function:

.. autofunction:: lumache.get_random_ingredients

The ``kind`` parameter should be either ``"meat"``, ``"fish"``,
or ``"veggies"``. Otherwise, :py:func:`lumache.get_random_ingredients`
will raise an exception.

.. autoexception:: lumache.InvalidKindError

For example:

>>> import lumache
>>> lumache.get_random_ingredients()
['shells', 'gorgonzola', 'parsley']

