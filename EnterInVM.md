
# Домашнее задание к занятию 1. «Введение в виртуализацию»
        
      ## Задача 1
Выберите один из вариантов использования организации физических серверов, в зависимости от условий использования.

Организация серверов:

  физические сервера,
  паравиртуализация,
  виртуализация уровня ОС.
  
Условия использования:
  1. Высоконагруженная база данных, чувствительная к отказу.
  2. Различные web-приложения.
  3. Windows системы для использования бухгалтерским отделом.
  4. Системы, выполняющие высокопроизводительные расчеты на GPU.
Опишите, почему вы выбрали к каждому целевому использованию такую организацию.
```
УСЛОВИЯ: Высоконагруженная база данных, чувствительная к отказу	
ОРГАНИЗАЦИЯ: физические сервера
ПРИЧИНА ВЫБОРА: Виртуализация даёт оверхед и увеличивает латентность. В высоконагруженной системе это нежелательно,
а достичь отказоустойчивости можно резервированием. Постоянно нагруженной системе потребуется максимум ресурсов хоста,
присутствие соседей которые могут их отобрать тоже нежелательно.
```
```
УСЛОВИЯ:Различные web-приложения	
ОРГАНИЗАЦИЯ: виртуализация уровня ОС	
ПРИЧИНА ВЫБОРА: Современная практика - разворачивать часто и много веб-приложений на одном хосте, при этом нужно обеспечить
их изоляцию друг от друга. Виртуализация ОС подходит лучше всего, так свернуть приложение в контейнер и развернуть из него
быстрей, чем делать это с виртуальными машинами с полноценной ОС и отдельным ядром.
```
```
УСЛОВИЯ: Windows системы для использования бухгалтерским отделом	
ОРГАНИЗАЦИЯ: паравиртуализация	
ПРИЧИНА ВЫБОРА: Виртуализация поможет системе быть более отказоустойчивой, из предложенных вариантов
для Windows возможна только паравиртуализация.
```
```
УСЛОВИЯ: Системы, выполняющие высокопроизводительные расчеты на GPU
ОРГАНИЗАЦИЯ: виртуализация уровня ОС	
ПРИЧИНА ВЫБОРА: виртуализация GPU может потребоваться в проектах с машинным обучением, из предложенных видов
виртуализации для GPU возможна только виртуализация средствами ОС.
```
```
        ДОПОЛНЕНИЕ: Использование контейнеризации уместно использовать для запуска софта в облаке и дистрибуции 
        серверных программ. Также наверное ее можно использовать при работе над исследованием поведения вирусных и 
        вредоносных программ. Контейнер тогда представляет из себя обособленную виртуальную машину и ее поведение 
        никак не влияет на хозяйскую ОС.
```

          ## Задача 2
Выберите подходящую систему управления виртуализацией для предложенного сценария. Детально опишите ваш выбор.

Сценарии:
```
1. 100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based инфраструктура,
   требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
  ВЫБОР: Подойдут Hyper-V, vSphere. Хорошо поддерживают виртуальные машины с Windows и Linux, имеют встроенные требуемые возможности (балансировка,
репликация, бэкапы) и могут работать в кластере гипервизоров, что необходимо для работы 100 виртуальных машин.
```
```
2. Требуется наиболее производительное бесплатное open source решение для виртуализации небольшой (20-30 серверов) инфраструктуры на базе
   Linux и Windows виртуальных машин.
  ВЫБОР: Подойдёт Proxmox в режиме KVM: open source решение, хорошо поддерживает Linux и Windows гостевые ОС, по управлению сравним
с платными гипервизорами.
```
```
3. Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows инфраструктуры.
  ВЫБОР: Hyper-V Server, максимально совместим c Windows гостевыми ОС, бесплатен.
```
```
4. Необходимо рабочее окружение для тестирования программного продукта на нескольких дистрибутивах Linux.
  ВЫБОР:Оптимально использовать LXD, т.к. содержит огромную библиотеку с разными дистрибутивами в большом количестве конфигураций контейнеров.
Версия 4.XX позволяет запускать ещё и виртуальные машины, что позволит тестить даже ПО, требующее собственное полноценное ядро.
```
        ДОПОЛНЕНИЕ: Использование Docker уместно использовать во 2 и 4 сценарии.
        
        
        ## Задача 3
  Опишите возможные проблемы и недостатки гетерогенной среды виртуализации (использования нескольких систем управления виртуализацией одновременно) и что необходимо сделать для минимизации этих рисков и проблем. Если бы у вас был выбор, то создавали бы вы гетерогенную среду или нет? Мотивируйте ваш ответ примерами.
```
Возможные проблемы и недостатки гетерогенной среды виртуализации:
    - сложность администрирования;
    - необходимое наличие высококвалифицированных специалистов;
    - повышенный риск отказа и недоступности;
    - завышенная стоимость обслуживания;
```
```
Действия для минимизации рисков и проблем:
       - если гетерогенность не оправдана, то рассмотреть возможность отказа от нее;
       - если она оправдана, то часть инфраструктуры можно перенести на IaaS (AWS), а саму инфраструктуру вывести в IaC (Terraform);
       - максимальное автоматизировать развертывание и тестирование инфраструктуры, чтобы она была единая.
```
Лучше всего работать в единой среде. Малые выгоды в цене и производительности при использовании разных систем управления виртуализацией ведут
к большим затратам и не всегда верно мотивированы.
