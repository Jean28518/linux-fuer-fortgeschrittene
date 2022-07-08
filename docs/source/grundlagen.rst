Grundlagen
==========


4 Freiheiten von Freier Software
--------------------------------
- **Verwenden**
   Die Freiheit, die Software uneingeschränkt und für jeden Zweck einzusetzen.
- **Verstehen**
   Die Freiheit, die Funktionsweise der Software untersuchen und verstehen zu können.
- **Verbreiten**
   Die Freiheit, Kopien der Software zu verbreiten, um damit seinen Mitmenschen zu helfen.
- **Verändern**
   Die Freiheit, die Software zu verändern und die Änderungen an die Öffentlichkeit weiterzugeben, sodass die gesamte Gesellschaft davon profitieren kann. 


   „Freie Software ist frei im Sinne von Redefreiheit, nicht im Sinne von Freibier!“
   **Richard M. Stallman**

Unterschied zwischen Open Source und Freier Sofware:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Open Source bedeutet lediglich, dass man den Code einsehen darf.
Dass man den Code automatisch anpassen, als bspw. große Firma nutzen, oder die Software weiterverbreiten darf, ist nicht unbedingt gegeben.

   -> Das Open Source Prinzip ist also ein Teil des Prinzips Feier Software. Freie Software ist viel mehr als "nur" Open Source Software.



Freie Sotware-Lizenzen
----------------------


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

