## История одного проекта

#### Проект
Головная организация и ее дилеры. Дилерам назначается планы реализации товаров, услуг и товарные кредиты (лимиты). Периодически производятся взаиморасчеты. Проглядываются и где-то даже описаны какие-то бизнес процессы. Сотрудники заказчика держат в голове или ищут где-то в excel и т.п. что они должны сделать в конкретной ситуациии с конкретным партнером. Частично описаны временные рамки. Обмен между участниками никак не регламентирован (телеграм, а в основном почта). 

#### Текущая архитектура
Karaf в микросервисе kubrenetes. Жирное, хоть и модульное(плагинное) приложение в микросервисе кубера. Сотрудник самомоятельно обращается к тому или другому REST исходя из собственного опыта.  

Для разработчиков, код плагинов находится в разных maven-репозитриях.<br/>

Часть функций реализована во внешних системах. 

#### Команда
1 тимлид<br/>
2 аналитика<br/>
3 backend азработчика (java)<br/>
3 frontend разработчика (react)<br/>
3 тестировщика<br/>
2 devops инженера<br/> 
Итого: 14 чел<br/>
<br/>
Вполне нормальная команда, не перенаселена, не 30 человек. Backend разработчики и один из devops инженеров работали на двух проектах в параллель. Параллельные проекты никак не связаны с текущим проектом, ни заказчиком, ни командой и т.п.<br/>

#### Что было сделано
Ушли от karaf. Полностью перешли на типичные микросервисы.<br/>При определении функциональности микросервисов и их связей активно использовали доску miro.<br/> Для обмена между микросервисами использовали не только REST, но и очереди. 

Выделили в отдельную библиотеку(DTO) форматы обмена с системой. Настроили оповещение фронт разработчиков (да и всей команды) при изменении в этом слое. Внутри библиотеки DTO сделали 2 слоя "dto-short" для краткого описания бизнес сущностей (id:integer, name:string ...) и dto-full с полным описанием.

Для обмена между микросервисами внутри kubernetes  использовали отдельную библиотеку DTO.

Внутри kubernetes микросервисы разделили на несколько групп:
- для доступа извне(rest)
- для доступа к собственным базам данных 
- для доступа к внутренним сервисам (camunda, хэш системы)
- для общения с внешними системами (почта, авторизация и пр. внешние бизнес системы)
- прочие служебные микросервисы
