# Классификация по MongoDB, MSSQL, Cassandra по CAP теореме
## MongoDB
    Монго дб относится к классу CP- во время сбоев разделения сети она остаётся консистентной

     Монго распределенная система с одним лидером, в которой используется асинхронная репликация для распространения нескольких копий данных для обеспечения высокой доступности. На наборе реплик хранятся одни и теже данные.
     Лидер : вляется главным узлом и получает все операции записи. 
     фоловеры : получают реплицированные данные от основного для поддержания идентичного набора данных. 

     По умолчанию Лидер обрабатывает все операции чтения и записи. При желании клиент MongoDB может направлять некоторые или все операции чтения фоловерам. Записи должны быть отправлены на Лидера. 

     В случае сбоя Лидера а все операции записи приостанавливаются до тех пор, пока из одного из фоловеров не будет выбран новый Лидер. Согласно документации MongoDB, для завершения этого процесса требуется до 12 секунд. Во время выборов монго является частично-жоступной
     Чтобы повысить доступность, кластер можно распределить по географически разным центрам обработки данных. Максимально можно сделать 50 реплик. 
     Монго можно перекласифицировать с помощью настраиваемому уровню согласованности read concern/write concern.
     По PACELC Монго может жертовать солгасованностью в угоду скорости чтения.



## MSSQL
MS SQL - классическая реляцилнна СУБД, которые  по CAP можно отнести к AC системе
  Реляционные данные сильно структурированы и требуют ссылочной целостности. 
  Для обеспечания высокой досупности сиквел поддерживат репликации. 

  Операции записи выполняются в основной экземпляр и реплицируются в каждый из вторичных. В случае сбоя первичный экземпляр может переключиться на вторичный, чтобы обеспечить высокую доступность. Вторичные серверы также могут использоваться для распределения операций чтения. В то время как операции записи всегда выполняются на первичной реплике, операции чтения могут быть перенаправлены на любую из вторичных реплик, чтобы снизить нагрузку на систему.

  С помощью настройки можно переклассифицировть сиквел с помощью сегментирования и сдеалть частично устойчивой к разделению из за чего резго снижается производительность.
  
## Cassandra
   Кассандра является АР - системой - во время сбоя разделения сети она остаётся быть доступной.
   Кластер кассандры состояит из одноранговых распределённх реплик, данные на которых хранятся согласно хешу-ключу партиционирования.
   Лидера нет - и каждый узел может выполнять все операции с базой данных, и каждый может обслуживать запросы клиентов. 

   Данные имеют replication factor, который определяет количество реплик, которые необходимо сделать. Реплики автоматически сохраняются на разных узлах. 

   Узел, который первым получает запрос от клиента, является координатором, который направляет запросы узлам, содержащим данные для этого запроса, и отправить результаты обратно координатору. Любой узел в кластере может выступать в роли координатора. 
   Кассандру можно перекласифицировать с помощью настроек и сделать строго-согласованной.

   По PACELC Кассандра может жертовать доступностю в случае настроек согласованности, в случает отказа одной репики база потеряет доступность или увеличится задерж~~ка