# MORO-D20
2020.03.01
 ESP32 + M0

### 		Особенности аппаратной реализации проекта
![](https://github.com/Molegvas/MORO-D20E/blob/master/Resources/D20v05top.png)

   Отличие проекта от предыдущих в основном связано с применением драйвера силового преобразователя. Обработка сигналов датчиков тока (их два), напряжения, перегрузки по току возложено на контроллер вместо аналогового блока ранее. Выбор контроллера скорее гипотетический. Исследовался принцип. Оптимальный выбор представляется таким: тактовая частота 48МГц, 12-разрядный АЦП с не менее чем 6-8 каналами, аналоговый компаратор с задаваемым уровнем срабатывания, SPI-порт для связи с центральным процессором. Цена должна быть сопоставима с аналоговой схемой на rail-to-rail операционниках. Цена инструментария может быть достаточно высокой. Выигрыш может оказаться совсем с необычной стороны - на проблемную часть - открытое ПО, а прошивка драйвера авторская, защищенная от копирования.
Главный секрет фирмы - там! Вариант далеко не окончательный, это только наметки.
  ![](https://github.com/Molegvas/MORO-D20E/blob/master/Resources/D20v05bot.png)

   Предварительный подсчет показывает, что высвобождается место примерно 3-4 десятков комплектующих с соответствущим сокращением поверхности платы для лучшего теплоотвода. Слева от модуля ESP32 компактно вписался в корпусе TQFP32 ATSAMD20E18A-A коротко: 48МГц, 256+32КБ,12бит 35ksps АЦП с дифференциальными входами и усилителем от 0,5 до 16 крат и автоматической регулировкой смещения и усиления, 2 аналоговых компаратора, выходной ЦАП, таймеры ... перечислено только то, что будет наверняка задействовано - о чем ещё можно мечтать? И все это потребляет всего несколько миллиампер. К тому же более простая топология платы. 
  ![](https://github.com/Molegvas/MORO-D20E/blob/master/Resources/driver.jpg)

   За основу периферии была взята модель кулона 912. Силовые транзисторы и диод в корпусе ТО-247. Параметры могут быть оптимизированы в части уменьшения разброса, который в конечном виде ведет к повышенному тепловыделению в части устройств.

   Подключение к датчикам тока и напряжения произведено по дифференциальной схеме с целью исключения сдвига показаний из-за падения напряжения на силовых проводах. Топология платы допускает использование 4-х проводной схемы подключения заряжаемой батареи, что уже реализовано у серьезных конкурентов. При этом устраняется сама природа погрешности измерения напряжения на батарее за счет падения напряжения на проводах. Опционно.

   Управление силовым источником и разрядной цепью производится с единственного выхода DAC путем блокировки одного из двух. Для управления разрядным ключем применен единственный ОУ. Результат моделирования подтверждает как реализуемость способа, так и необходимость ОУ в цепи затвора разрядного ключа.
  ![](https://github.com/Molegvas/MORO-D20E/blob/master/Resources/Model.png)

   Топология платы позволяет устанавливать дисплеи TFT 1.8" 128*160 и LCD 128*128 SPI-3. Последний, правда пока без подсветки (нужны размеры).
***
   Органы управления. Имеются опасения, что сопротивления контактов пленочной клавиатуры весьма непостоянная величина и может со временем увеличиваться, причем существенно, что малоприемлемо для опроса аналоговыми средствами. Хотя стоит попробовать увеличить в несколько раз номиналы соответствующих резисторов и испытать при полной отдаваемой мощности, так как проводник проложен под силовым трансформатором. При дискретных (довольно "брутальных" кнопках) замечаний не было. Впрочем, вместо пленочной или кнопочной клавиатуры возможна установка энкодера (как у кулона 720). Да и сама панель 720-го без распайки, естественно, для опытного образца вполне подойдет.

   RGB светодиод не выводится на лицевую панель. В полупрозрачном корпусе вполне достаточно таинственной цветомузыки, исходящей из недр. К тому же не слепит.

   Карточки памяти нет, но есть USB порт типа "B", выведенный на заднюю панель, там же держатель предохранителя, сетевой выключатель и кнопка сброса. WiFi и BT никуда не делись. Пока реализовано лишь осциллографирование в реальном времени: тока, напряжения, отданного заряда, температуры радиатора и скорости вентилятора. На месте, отведенном для USB может быть держатель карты, но только в формате микро. USB может быть как опцион.

   Предлагается забыть о подключении батареи через клеммы - это лишние потери. Многожильные провода в силиконовой изоляции сечением AWG-12 достаточно мягкие, с низким сопротивлением и хорошо паяются. Да и больший простор для 4-х проводного подключения. Показаниям сотых долей вольта можно будет в большей степени доверять.

   О продуваемости объема прибора: радиаторы развернуты на 180 градусов по сравнению с 912-м и перестали перекрывать свободный проход воздуха от вентилятора. Плата нигде не примыкает к боковым стенкам прибора, оставляя канал продува. Термочувствительные компоненты, и в первую очередь модуль ESP и драйвер (он же измеритель) находятся в холодной зоне прибора у задней стенки. Осталось выяснить каково будет дисплею.
Для вентилятора оставлена достаточно глубокая ниша в плате - разместится экземпляр 60 х 60 х 20 мм. Подключение возможно по 3-х проводной схеме, вход тахометра подключен к контроллеру.
По ссылке ниже есть 30-секундное видео - кликните и потом Download и Open.
   https://github.com/Molegvas/MORO-D20E/blob/master/Resources/D2v03.mp4

   Предполагается, что двусторонняя плата с толщиной по меди 35мкм с каждой стороны обеспечит возможность повысить диапазон тока заряда и разряда. Ширина дорожек и длина оптимизированы - в токонесущих цепях применены сплошные полигоны максимально возможного сечения. Чего не удалось избежать, так это открытых проводников под низковольтным радиатором - потребуется изолирующая прокладка или небольшая доработка радиаторов. Под высоковольтным - всё в порядке.

   Прибор оснащен датчиком тока на эффекте Холла. Измерение малых токов таким датчиком затруднительно, но выше нескольких сотен миллиампер возможно в любой полярности. Измерение тока разряда снимет массу вопросов пользователей. При добавлении внешнего токового разъема можно реализовать регулируемый пользователем ток разряда в пределах порядка 10 ампер без выделения тепла в самом приборе, но с его измерением, отображением и записью прибором (не проверялось). Всё это может быть опционально. Как альтернатива датчику холла могут использоваться данные, снимаемые с нагрузочных резисторов. Недостаток в том, что в этом случае не будет фиксироваться ток, потребляемый контроллером от батареи при пропадании питающей сети. 

###              Цена вопроса
   Поиск типовых решений на Cortex M0 и SAMD20 привел к тому, что платы-прототипы за приемлемую стоимость есть только для SAMD21. По факту это тот же микроконтроллер, только с дополнительным функционалом, например, с встроенным интерфейсом USB. Такие платы не требуют на этапе разработки специализированного программатора. Тем более, что поддерживаются интегрированной средой разработки PlatformIO. На данный момент наиболее перспективной показалась SparkFun SAMD21 Dev Breakout. Миграция с версии v05 на v15 и вызвана в первую очередь с изменением схемы для более простого перехода в будущем от прототипа к целевому варианту прошивки для SAMD20. Стоит порядка 2500 рублей.

   Собственно программатор ATMEL-ICE - 6000-9000руб. 


###           Предлагаемое разделение функций драйвером и центральным модулем
#### драйвер:
<li> измерение напряжения на клеммах дифференциальным методом с усреднением и автоматическим выбором диапазона;
<li> то же для тока через измерительный шунт;
<li> измерение напряжения на резисторах нагрузки разрядного тока;
<li> измерение температуры радиатора;
<li> отслеживание пороговых значений по всем измеряемым величинам, отключение или масштабирование выходных    параметров источника заряда и разряда по температуре и мощности в пределах конструктивной особенности прибора данной модификации;
<li> ПИД - регулирование процессов заряда и разряда;
<li> в связке с центральным модулем работает как Slave.
</li>

#### центральный модуль:
<li> вывод на дисплей;
<li> опрос клавиатуры или энкодера;
<li> управление выходными ключами;
<li> контроль подключения батареи;
<li> управление скоростью вентилятора;
<li> вычисление параметров выбранного оператором режима работы для работы драйвера;
<li> в связке с драйвером - Master. 
</li>


***
Схемы v15:  https://github.com/Molegvas/MORO-D20E/blob/master/Documents/Schemes

### Предложение по протоколу обмена
Испытывался использованный ранее пакетный протокол обмена для VSPI. В качестве тестовой задачи передача информации о приборе из 15 байт:

  ![](https://github.com/Molegvas/MORO-D20E/blob/master/Resources/MOSI.png)

Передача на частоте 4МГц занимает около 2-х миллисекунд, что вполне удовлетворительно. Специальные меры для подготовки пакета не применялись. 

  ![](https://github.com/Molegvas/MORO-D20E/blob/master/Resources/Log.png)

Код для ведущего- в проекте SPI_Multiple_Buses (пока только на передачу).
Для ведомого - в разработке.
***
Вся последняя неделя ушла на корректировку принципиальной схемы и топологии для облегчения процесса разработки. Попутно исправлены некоторые, оставленные до лучших времен несуразности. Версия v26.  
*** 
Версия v26 - технологическая: микроконтроллер SAMD20 в 32-выводном корпусе заменен на SAMD21 (48 выводов). Задействован USB-интерфейс для отладки. Предполагается, что на начальном этапе разработки будет установлен микроконтроллер с загрузчиком, что позволит обойтись без дорогого ATMEL-ICE. Задействованы только те выводы, которые в обязательном порядке имеются в младшей, SAMD20, модели. В то же время схема подключения практически полностью повторяет серийную SAMD21 M0 Mini. Веселенькое дело: размеры корпусов TQFP32 и TQFP48 одинаковы.
***
Версия v36 - экспериментальная. Датчик тока на эффекте Холла удален. Отмоделирован (в ресурсах скриншоты) процесс измерения тока заряда и разряда одним шунтом. Правда ток разряда в таком варианте будет ограничен 5 амперами. Выбор диода пусть не смущает - это всего лишь модель. 
*** 
В разработке намечаются каникулы.  