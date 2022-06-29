# DEMO2022-2023-linux-only
(Свободный проект альтруистов) 100% выполненное задание (публичный вариант) к демонстрационному экзамену по сетевому и системному администрированию. Полностью и только на Debian. Основано на первоисточнике: https://github.com/storm39mad/DEMO2022/tree/main/azure

В формате docx: [Very Easy Linux.docx](https://github.com/cupespresso22/DEMO2022-2023-linux-only/files/8994172/Very.Easy.Linux.docx)

![изображение](https://user-images.githubusercontent.com/28905300/175996163-e7dbdb7b-2d18-4150-906a-ac925f3a66c7.png)

## Таблица 1. Характеристики ВМ

|Name VM         |ОС                  |RAM             |CPU             |IP                    |Additionally                       |
|  ------------- | -------------      | -------------  |  ------------- |  -------------       |  -------------                    |  
|RTR-L           |Debian 11/CSR       |2 GB            |2/4             |4.4.4.100/24          |                                   |
|                |                    |                |                |192.168.100.254/24    |                                   |
|RTR-R           |Debian 11/CSR       |2 GB            |2/4             |5.5.5.100/24          |                                   |
|                |                    |                |                |172.16.100.254 /24    |                                   |
|SRV             |Debian 11/Win 2019  |2 GB /4 GB      |2/4             |192.168.100.200/24    |Доп диски 2 шт по 5 GB             |
|WEB-L           |Debian 11           |2 GB            |2               |192.168.100.100/24    |                                   |
|WEB-R           |Debian 11           |2 GB            |2               |172.16.100.100/24     |                                   |
|ISP             |Debian 11           |2 GB            |2               |4.4.4.1/24            |                                   |
|                |                    |                |                |5.5.5.1/24            |                                   |
|                |                    |                |                |3.3.3.1/24            |                                   |
|CLI             |Win 10              |4 GB            |4               |3.3.3.10/24           |                                   |

## Виртуальные машины и коммутация.
Необходимо выполнить создание и базовую конфигурацию виртуальных
машин.

1. На основе предоставленных ВМ или шаблонов ВМ создайте отсутствующие виртуальные машины в соответствии со схемой.  
   -	Характеристики ВМ установите в соответствии с Таблицей 1;
   -	Коммутацию (если таковая не выполнена) выполните в соответствии со схемой сети.	 
2.  Имена хостов в созданных ВМ должны быть установлены в соответствии со схемой.
3.  Адресация должна быть выполнена в соответствии с Таблицей 1;
4.  Обеспечьте ВМ дополнительными дисками, если таковое необходимо в соответствии с Таблицей 1;

###### #(Если изначально доступна работа из под пользователя с обычными привилегиями, то повышение привилегий, то есть работа из под root, выполняется через команду *sudo -i*).
###### #На текущем этапе, пока что выполняется настройка имён хостов и IP-адресации.
![изображение](https://user-images.githubusercontent.com/28905300/175999607-fa8103f4-050f-4a61-a7a6-735731709549.png)

## МОНТИРОВАНИЕ ОБРАЗОВ И УСТАНОВКА ПАКЕТОВ
###### #На демо-экзамене предоставляются ISO-образы с дополнительным ПО. Установка пакетов через них. Выход в реальный интернет не предоставляется.
###### #К примеру, на виртуальной машине RTR-L нужно установить необходимые по заданию пакеты. Для этого необходимо вмонтировать в VM один из ISO-образов и добавить его в source.list (необязательно все, только самый первый).
![изображение](https://user-images.githubusercontent.com/28905300/176243706-b3aeef9f-3c18-4751-9d64-8be7f0673a72.png)

![изображение](https://user-images.githubusercontent.com/28905300/176243826-8e15bde7-00d3-4bf6-8a80-8cb64cd0921c.png)

![изображение](https://user-images.githubusercontent.com/28905300/176243935-32885bf7-d4b8-4545-a043-58b7c8b9dec0.png)

![изображение](https://user-images.githubusercontent.com/28905300/176243995-4ef6b621-4f40-44a0-b17d-2b7b0985e423.png)
###### #Одновременно с добавлением образа в source.list, производится обновление списка доступных пакетов от каждого смонтированного образа.
###### #Если требуется установить ПО, связанные с которым пакеты находятся на нескольких образах, то в proxmox надо так же выбрать уже другой образ, по образцу выше. После этого в Debian, для обновления пакетов от другого образа, нужно выполнить следующие команды:
![изображение](https://user-images.githubusercontent.com/28905300/176244112-ecd85e48-fa15-4652-a48f-6f4275bfafe3.png)
###### #А далее снова *apt-cdrom add*.
###### #Добавляем автомонтирование образа с ПО при перезагрузке виртуальной машины.
![изображение](https://user-images.githubusercontent.com/28905300/176244201-f6917ac3-990a-4bd1-9b66-c66b2a4bea50.png)
###### #(в nano сохранение изменений на *CTRL+S*, выход — *CTRL+X*)
![изображение](https://user-images.githubusercontent.com/28905300/176244404-16091ebc-c042-411c-abf9-1deb5503aef4.png)
##### Очень полезные сочетания клавиш в nano: 
##### Alt + 6 — копировать одну или несколько строк.
##### Ctrl + U — вставить строку или строки.
##### Ctrl + K — вырезать (удалить) строку.
##### Alt + T — удалить ВСЁ ниже курсора.
##### Ctrl + S — сохранить изменения.
##### Ctrl + X — выход.
##### Длинные пробелы ставятся при нажатии на TAB.

###### #Далее нужно настроить статическую IP-адресацию на каждой машине. Адресация представлена в конце комплекта оценочной документации. Для сопоставления MAC-адресов с IP-адресами на каждой машине, нужно проделать следующее:
![изображение](https://user-images.githubusercontent.com/28905300/176244746-4648f67f-364e-4c7d-88aa-86664629d5a0.png)
###### #На proxmox, при выборе виртуальной машины и просмотре списка его оборудования, нужно обратить внимание на MAC-адрес и имя моста (bridge). У другой машины, присоединённой к текущей (пример ISP), имя моста будет идентичным.
![изображение](https://user-images.githubusercontent.com/28905300/176244904-2d890844-4d5f-4db3-9483-b473a16b4e4b.png)
###### #По одинаковому названию моста можно увидеть связность конкретных машин. По их MAC-адресам — связь с IP-адресами.
#### ПЕРВЫЙ ВАРИАНТ НАСТРОЙКИ
###### #Настройка IP-адресов каждой машины (кроме CLI). Каждый интерфейс настраивается через *nmcli* одной длинной командой:
#### ISP
![изображение](https://user-images.githubusercontent.com/28905300/176245056-6706c377-7406-47b8-b58b-da6be538ec09.png)
###### #Названия интерфейсов можно посмотреть и через *nmcli connection*. Если наименования не содержат *Wired connection* и они имеют красный цвет, то нужно ввести команду *systemctl restart NetworkManager* и заново ввести *nmcli connection*.
![изображение](https://user-images.githubusercontent.com/28905300/176243233-2fb935d9-5d3e-41a1-8d6c-7d8ba3d3a785.png)
###### #Элементы команд полностью не нужно вписывать (кроме имён и цифр). Достаточно вбить начало команды (пример: ipv4.ad), как в терминале Cisco, и через нажатие на TAB произойдёт автодописывание (ipv4.addresses).
###### #Пример соответствует самой первой введёной строке:
###### nmc(TAB) co(TAB) mod(TAB) Wi(TAB) 1 ipv4.a(TAB) 5.5.5.1/24 ipv4.me(TAB) m(TAB) au(TAB) y(TAB) con-n(TAB) ISP-RTRR.
#### RTR-R
![изображение](https://user-images.githubusercontent.com/28905300/176381588-691c1fa6-a173-443a-98d9-a395dd1afcc0.png)
#### RTR-L
![изображение](https://user-images.githubusercontent.com/28905300/176381793-9cb85679-d860-41ff-a184-8e01121ba158.png)
#### WEB-L
![изображение](https://user-images.githubusercontent.com/28905300/176381968-10409a3c-c436-4f75-a10f-e2ff426d656f.png)
#### SRV
![изображение](https://user-images.githubusercontent.com/28905300/176382133-444ea1e8-10a0-49cc-8937-d87cad6696fe.png)
#### WEB-R
![изображение](https://user-images.githubusercontent.com/28905300/176382245-a676d222-22e9-4c10-bc44-538660cad981.png)
#### CLI
###### #На виртуальной машине CLI, по заданию, операционная система Windows 10. Порядок изменения имени хоста и IP-адреса там другой:
![изображение](https://user-images.githubusercontent.com/28905300/176009988-c0fa9d7a-74e4-4a4b-878b-08aa0375e485.png)

![изображение](https://user-images.githubusercontent.com/28905300/176010008-f28839a2-39ce-4bb8-85be-a6c83f67fb57.png)

![изображение](https://user-images.githubusercontent.com/28905300/176010035-149e5876-2d9e-4884-b2e8-330a842ab685.png)

![изображение](https://user-images.githubusercontent.com/28905300/176010052-77bd725e-8e6c-4ef2-82a6-3a9d37e7209e.png)

![изображение](https://user-images.githubusercontent.com/28905300/176010074-29212295-b515-4b54-8521-74e93d16bd90.png)
###### #Жмём на OK, заходим в PowerShell и пингуем шлюз по умолчанию (ISP). Далее меняем имя хоста:
![изображение](https://user-images.githubusercontent.com/28905300/176010140-0332983c-106e-4a4b-8efc-50fc888bbc06.png)

![изображение](https://user-images.githubusercontent.com/28905300/176010153-90806f3e-b203-4b85-a7d7-fd4654fef1c5.png)

![изображение](https://user-images.githubusercontent.com/28905300/176010174-468b241f-463d-481c-8f46-aba1c53c740c.png)
###### #Запрос о перезагрузке подтверждаем.
#### ВТОРОЙ ВАРИАНТ НАСТРОЙКИ
###### #(короткие команды в nmcli):
#### ISP
![изображение](https://user-images.githubusercontent.com/28905300/176387802-3f73e22f-5f88-42a1-97f9-b5ebf0882296.png)

![изображение](https://user-images.githubusercontent.com/28905300/176387985-6422a16a-0319-42a9-9867-2b37763a4174.png)
#### RTR-R
![изображение](https://user-images.githubusercontent.com/28905300/176388472-d0dee906-9829-42d2-a663-25383220260c.png)
#### RTR-L
![изображение](https://user-images.githubusercontent.com/28905300/176388804-e6b337ce-539b-4ad8-a37e-5316a3c0282c.png)
#### WEB-L
![изображение](https://user-images.githubusercontent.com/28905300/176388969-f5855453-7f40-4a23-8849-baa95fec6c45.png)
#### SRV
![изображение](https://user-images.githubusercontent.com/28905300/176389146-f7792fe1-b075-49d0-8f01-13cf50cbe2ec.png)
#### WEB-R
![изображение](https://user-images.githubusercontent.com/28905300/176389663-832481a1-0da3-4e61-9199-3e532a35e74c.png)
###### #После настройки IP-адресации, проверяем доступность соседних устройств с помощью команды *ping*.
###### #Для отправки файлов с помощью *scp* (или для соединения по ssh), нужно отредактировать *sshd_config* на начальной (откуда), конечной (куда отправляем файл) и промежуточной виртуальной машине (через какую машину проходит файл). Допустим, нужно отправить файл test с RTR-L на RTR-R. После установки пакетов, включая ssh, на RTR-L, RTR-R и ISP между ними редактируется строка с разрешением на аутентификацию из под root.
![изображение](https://user-images.githubusercontent.com/28905300/176393008-670c5070-d976-44a0-a50f-b9e01bd49851.png)

![изображение](https://user-images.githubusercontent.com/28905300/176393137-5b0393ea-ab68-4953-bcbc-e95ad9b0e788.png)

![изображение](https://user-images.githubusercontent.com/28905300/176393373-af99a376-4a1c-436e-9378-319c03b99efa.png)
## Сетевая связность.
В рамках данного модуля требуется обеспечить сетевую связность между
регионами работы приложения, а также обеспечить выход ВМ в имитируемую
сеть “Интернет”

1. Сети, подключенные к ISP, считаются внешними:
   - Запрещено прямое попадание трафика из внутренних сетей во внешние и наоборот;
###### #Сперва, нужно разрешить пересылку пакетов через устройства, осуществляющих маршрутизацию (RTR-L, ISP и RTR-R).
![изображение](https://user-images.githubusercontent.com/28905300/176395129-b093c8f5-259b-411d-a916-62d44fa60676.png)

![изображение](https://user-images.githubusercontent.com/28905300/176395238-52737eea-00f5-4c06-a0ad-153250691839.png)
###### #Для выполнения условия о запрете прямого прохождения трафика, необходимо установить *firewalld* на RTR-L и RTR-R. Интерфейсы на них, смотрящие во внешние сети, определить в зону *external*. В зоне *external* разрешён не весь, а только определённый вручную тип трафика, а также выполняется трансляция трафика, идущего из внутренних сетей во внешние.
![изображение](https://user-images.githubusercontent.com/28905300/176395560-4328479c-3f44-4137-b66d-06a5e3009a58.png)

![изображение](https://user-images.githubusercontent.com/28905300/176395661-7726b0c5-527a-4177-b535-e6e324d87254.png)
###### #Выявляем интерфейсы, которые смотрят во «внешние» сети.
![изображение](https://user-images.githubusercontent.com/28905300/176395973-bbb2d5b5-3583-4dbc-bb5b-45fe29ec5c40.png)

![изображение](https://user-images.githubusercontent.com/28905300/176396149-4833c7c0-1401-45c5-b93a-148d72d9924b.png)

![изображение](https://user-images.githubusercontent.com/28905300/176396304-d10a253f-6315-4f01-9bf1-3e8871950d70.png)

![изображение](https://user-images.githubusercontent.com/28905300/176396415-b5a75ffe-fe25-4ac3-b8eb-06f6d20f6865.png)

![изображение](https://user-images.githubusercontent.com/28905300/176396503-78855e07-a419-4a63-b61a-8d60e8f21223.png)

2. Платформы контроля трафика, установленные на границах регионов, должны выполнять трансляцию трафика, идущего из соответствующих внутренних сетей во внешние сети стенда и в сеть Интернет.
   - Трансляция исходящих адресов производится в адрес платформы,расположенный во внешней сети.
###### #Условия выполнены примерами выше.
3. Между платформами должен быть установлен защищенный туннель, позволяющий осуществлять связь между регионами с применением внутренних адресов.
   - Трафик, проходящий по данному туннелю, должен быть защищен:
     - Платформа ISP не должна иметь возможности просматривать содержимое пакетов, идущих из одной внутренней сети в другую.
   - Туннель должен позволять защищенное взаимодействие между платформами управления трафиком по их внутренним адресам
     - Взаимодействие по внешним адресам должно происходит без применения туннеля и шифрования
   - Трафик, идущий по туннелю между регионами по внутренним адресам, не должен транслироваться.
###### #На RTR-L и RTR-R устанавливаем *libreswan* для работы IPsec (создание защищённого туннеля). Для его установки потребуется использовать два образа с ПО, монтируя и извлекая их поочерёдно, когда потребует этого установщик в debian.
![изображение](https://user-images.githubusercontent.com/28905300/176397254-3a369195-5a31-4aa2-ba58-a4cabd6b858f.png)
###### #Сначала выполняется создание GRE-туннеля между RTR-L и RTR-R. После — настройка файрволла на ISP. Поверх туннеля — настройка IPsec.
![изображение](https://user-images.githubusercontent.com/28905300/176400175-43c8a332-d604-4e28-9320-3f349ff8b384.png)

![изображение](https://user-images.githubusercontent.com/28905300/176400304-d06403bb-0df8-4aab-9b85-01ca7c703c95.png)

![изображение](https://user-images.githubusercontent.com/28905300/176400393-60f4e7c0-1781-4390-af4a-50a2911b4b7c.png)

![изображение](https://user-images.githubusercontent.com/28905300/176400466-113d2e78-c49c-40c3-9337-d97423d17c8c.png)

![изображение](https://user-images.githubusercontent.com/28905300/176400571-6f37bbee-d908-4c0c-8154-5a8bacc678ae.png)

![изображение](https://user-images.githubusercontent.com/28905300/176400662-42e11941-5509-434b-841a-e0f0911d8443.png)

![изображение](https://user-images.githubusercontent.com/28905300/176400746-017b9513-a8d2-4dbe-a90a-3afb576a7277.png)

![изображение](https://user-images.githubusercontent.com/28905300/176400912-de0b416d-c2df-48af-b589-9ae0f019e2a9.png)
#### IPsec
###### #Создаём файлы конфигурации и ключа.
![изображение](https://user-images.githubusercontent.com/28905300/176401785-ba074292-a2ba-4b24-88f1-1a441216019f.png)

![изображение](https://user-images.githubusercontent.com/28905300/176401885-f359c791-a588-45d5-ad71-3341ecc4ac44.png)
###### #Для большего удобства, лучше скопировать *mytunnel.conf* и *mytunnel.secrets* с RTR-L на RTR-R через scp, в аналогичную директорию.
![изображение](https://user-images.githubusercontent.com/28905300/176402205-5a06ff7d-309f-4ba7-a28d-bd6961e60171.png)

![изображение](https://user-images.githubusercontent.com/28905300/176402302-d1b1cf29-1c58-453a-b410-af655ce8d769.png)

![изображение](https://user-images.githubusercontent.com/28905300/176402401-80c074c8-5f3f-4745-8407-b319e814e672.png)

![изображение](https://user-images.githubusercontent.com/28905300/176402659-94f4885f-fde8-4ee8-ba49-00ad067d8985.png)
###### #Дополнительная проверка работы IPsec. Запуск *tcpdump* на ISP, при оправке ICMP-запросов с RTR-R на RTR-L. На скрине ниже видно, что ICMP-запросы и ответы именуются иначе.
![изображение](https://user-images.githubusercontent.com/28905300/176403111-ad521d76-25be-4402-8a20-57209e6aaa40.png)
#### EIGRP или OSPF
###### #Для организации подключения виртуальных машин, имеющих адреса во внутренних сетях, к новосозданному туннелю и для связи стороны Left со стороной Right, нужно настроить один из протоколов маршрутизации (EIGRP или OSPF) с помощью *frr*.
![изображение](https://user-images.githubusercontent.com/28905300/176403581-02e01bd4-e9b0-4920-beee-1e66ceffc689.png)
