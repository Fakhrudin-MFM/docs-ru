.. role:: raw-html-m2r(raw)
   :format: html


Подсистема задания по расписанию
================================
:doc:`Оглавление <../../index>`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Назад: :doc:`Функциональность <functionality>`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Запуск подсистемы заданий по расписанию выполняется двумя способами:


#. в отдельном процессе посредством скрипта ``bin/schedule.js``
#. внутри процесса веб-приложения ION (bin/www) путем указания в ini-файле опции ``jobs.enabled=true``

Во втором случае управление заданиями возможно реализовать в веб-приложении.
Задания по расписанию настраиваются в deploy.json приложений в разделе глобальных настроек как параметр jobs.

Пример:
^^^^^^^

.. code-block:: json

       "jobs": {
         "dummy": {
           "launch": { // Периодичность запуска задания
             "month": [2,5], // в феврале и мае
             "week": 3, // каждую третью неделю (month и week  - взаимоисключающие настройки),
             "weekday": [1, 3, 5], // по понедельникам, средам и пятницам
             "dayOfYear": 5, // раз в 5 дней в течение года,
             "day": 10, // раз в 10 дней в течение месяца
             "hour": 3, // раз в 3 часа 
             "minute": [10, 15, 35], // на 10-ой, 15-ой и 35-ой минуте
             "sec": 10 // раз в 10 секунд
           },
           "di": { // скоуп задания
             "dummy": {
               "module": "applications/develop-and-test/jobs/dummy",
               "options": {
               }
             }
           },
           "worker": "dummy", // имя компонента из скоупа задания, который будет исполняться
         }
       }

В качестве запускаемого задания может быть указан компонент, в этом случае он должен иметь метод ``run``. В качестве запускаемого задания может быть указана и функция. Тогда в **di** она описывается аналогично компоненту, но с использованием настройки ``executable``\ :

.. code-block:: json

           "di": {
             "dummy": {
               "executable": "applications/develop-and-test/jobs/dummy",
               "options": {}
             }
           }

Пример
^^^^^^

*Раздел jobs в глобальных настройках.*

.. code-block:: json

   ...
   "jobs": {
         "dummy": {
           "launch": {
             "sec": 30
           },
           "worker": "dummy",
           "di": {
             "dummy": {
               "executable": "applications/develop-and-test/jobs/dummy"
             }
           }
         }
   ...

----

`License <https://github.com/iondv/framework/blob/master/LICENSE>`_                                        `Contact us <https://iondv.com/portal/contacts>`_                                         `English <https://iondv.readthedocs.io/en/latest/index.html>`_
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


.. raw:: html

   <div><img src="https://mc.iondv.com/watch/local/docs/framework" style="position:absolute; left:-9999px;" height=1 width=1 alt="iondv metrics"></div>


----

Copyright (c) 2018 **LLC "ION DV"**.\ :raw-html-m2r:`<br>`
All rights reserved. 
