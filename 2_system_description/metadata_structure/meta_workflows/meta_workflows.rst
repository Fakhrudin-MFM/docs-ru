.. role:: raw-html-m2r(raw)
   :format: html

Мета бизнес-процесса
====================
`Оглавление </docs/ru/index.md>`_
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Предыдущая страница: `Мета навигации - Условия выборки </docs/ru/2_system_description/metadata_structure/meta_navigation/conditions.md>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Описание
--------

**Бизнес-процесс** - это четкая последовательность действий, которую выполняют для получения заданного результата. Как правило, процесс многократно повторяется. Применение бизнес-процесса позволяет отображать стадии выполняемого процесса и задавать условия для его выполнения.

JSON
^^^^

.. code-block::

   {
     "name": "basic",
     "caption": "Поручения",
     "wfClass": "basic",
     "startState": "new",
     "states": [],
     "transitions": [],
     "metaVersion": "2.0.61"
   }

Описание полей
--------------

.. list-table::
   :header-rows: 1

   * - Поле
     - Наименование
     - Описание
   * - ``"name"``
     - Системное имя
     - Системное имя бизнес-процесса
   * - ``"caption"``
     - Логическое имя
     - логическое имя бизнес-процесса
   * - ``"wfClass"``
     - Класс БП
     - Класс, к которому применяется бизнес-процесс
   * - ``"startState"``
     - Статус
     - Статус, присвоенный началу бизнес-процесса
   * - ``"states"``
     - `Список статусов <status_wf.md>`_
     - Список статусов бизнес-процесса.
   * - ``"transitions"``
     - `Переходы <transitions_wf.md>`_
     - Переходы между статусами для бизнес-процесса.
   * - ``"metaVersion"``
     - Версионирование
     - Версия метаданных.


Условие "contains" ("содержит")
-------------------------------

*Внимание!* Чтобы в БП работало поле "contains", в коллекции должна быть настроена жадная загрузка. Иначе в коллекции будет пусто, и результат будет всегда false. Условия применяются к объекту извлеченному из БД, дополнительных запросов не делается.

Пример
^^^^^^

.. code-block::

             {
                 "property": "charge",
                 "operation": 10,
                 "value": null,
                 "nestedConditions": [
                   {
                     "property": "state",
                     "operation": 1,
                     "value": [
                       "close"
                     ],
                     "nestedConditions": []
                   }
                 ]
               }

Настройка подсказок
-------------------

Настройка подсказок при переходе по статусу БП - представляет собой вывод инструкции в отдельном модальном окне с кнопками - "продолжить" или "отменить". При наведении на кнопку появляется всплывающий хинт, для более удобного использования бизнес-процессов.

Пример
^^^^^^

.. code-block:: json

   "transitions": [
       {
         ...
         "confirm": true,
         "confirmMessage": null
       }


* 
  ``"confirm"`` - подтверждение действия на переходе (+ стандартный вывод текста - вы действительно хотите выполнить действие "name").

* 
  ``"confirmMessage"`` - уникальный текст для вывода в подтверждении взамен стандартного.

Утилита формирования массива объектов
-------------------------------------

Утилита позволяет создавать объект в коллекцию при переходе основного объекта в заданный статус. Поля созданного объекта автоматически заполняются в соответствии с настройками, заданными для свойства ``"values"``.

Теперь чтобы прикрепить утилиту создания значений показателя к этапу БП, надо в ``di`` в свойстве ``options`` прописать опцию 

.. code-block::

   state - имя этапа БП

при переходе на который, должны создаваться объекты в коллекцию. 

Есть возможность использовать утилиту как "action". При переделке нужно просто убрать команду из модели представления.
Параметры задаются в файле deploy.json проекта. Синтаксис настройки:

.. code-block::

   "map": {
       "workflow@namespace.stage": {
          "className@namespace": { // для объекта какого класса создается объект в коллекцию
              "collection": { // наименование атрибута коллекции, в которой создается объект
                  "elementClass": "className2@namespace", // класс, объекты которого создаются утилитой
                  "patterns": [
                     {
                         "values": {
                             "attr1": "string", // строка
                             "attr2": 123, // число
                             "attr3": true,
                             "attr4": "$containerProperty1", // свойство контейнера
                             "attr5": {"add": ["$containerProperty2", 300]} // формула
                         },
                         "push": [
                            "workflow2@namespace.stage1", // присвоение БП созданных объектов статуса
                         ]
                     },
                     ...
                  ]
              },
              ...
          },
          ...
       },
       ....
   }

Следующая страница: `Статусы бизнес-процесса <status_wf.md>`_
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

----

`Licence </LICENSE>`_ &ensp;  `Contact us <https://iondv.com/portal/contacts>`_ &ensp;  `English </docs/en/2_system_description/metadata_structure/meta_workflows/meta_workflows.md>`_   &ensp;
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


.. raw:: html

   <div><img src="https://mc.iondv.com/watch/local/docs/framework" style="position:absolute; left:-9999px;" height=1 width=1 alt="iondv metrics"></div>


----

Copyright (c) 2018 **LLC "ION DV"**.\ :raw-html-m2r:`<br>`
All rights reserved. 
