# MORO-D20
2020.02.21
 ESP32 + M0

### 		Особенности аппаратной реализации проекта
![](https://github.com/Molegvas/MORO-D20E/blob/master/Resources/D20v03top.jpg)
***
   Отличие проекта от предыдущих в основном связано с применением драйвера силового преобразователя. Обработка сигналов датчиков тока (их два), напряжения, перегрузки по току возложено на контроллер вместо аналогового блока ранее. Выбор контроллера скорее гипотетический. Исследовался принцип. Оптимальный выбор представляется таким: тактовая частота 48МГц, 12-разрядный АЦП с не менее чем 6-8 каналами, аналоговый компаратор с задаваемым уровнем срабатывания, SPI-порт для связи с центральным процессором. Цена должна быть сопоставима с аналоговой схемой на rail-to-rail операционниках. Цена инструментария может быть достаточно высокой. Выигрыш может оказаться совсем с необычной стороны - на проблемную часть - открытое ПО, а прошивка драйвера авторская, защищенная от копирования.
Главный секрет фирмы - там!
  ***
  ![](https://github.com/Molegvas/MORO-D20E/blob/master/Resources/D20v03bot.jpg)
   Предварительный подсчет показывает, что высвобождается место примерно 4-5 десятков комплектующих с соответствущим сокращением поверхности платы для лучшего теплоотвода.
***
   Подсчитана примерная стоимость комплектации платы за исключением трансформаторов и радиаторов - около 1950 рублей, включая дисплей за 450р (1.8" TFT полноцветный, с подсветкой. Цены брались с этого ресурса: https://teta.online/ Подсчет приведен в файле plist.txt.
***
   Протокол связи между ESP32 и драйвером источника предполагается заимствовать у УРК-2Т - пакетный с контролем контрольных сумм и подтверждениями. Как? Вы не знали, что там "всё по-взрослому"? За почти полтора десятка лет, что выпускается и работает в режиме 7/24 (сколько тыс?) нарекания были?   
***
   За основу периферии была взята модель кулона 912. Силовые транзисторы и диод в корпусе ТО-247. Параметры могут быть оптимизированы в части уменьшения разброса, который в конечном виде ведет к повышенному тепловыделению в части устройств.
***
   Подключение к датчикам тока и напряжения произведено по дифференциальной схеме с целью исключения сдвига показаний из-за падения напряжения на силовых проводах. Топология платы допускает использование 4-х проводной схемы подключения заряжаемой батареи, что уже реализовано у серьезных конкурентов. При этом устраняется сама природа погрешности измерения напряжения на батарее за счет падения напряжения на проводах.
***
   Подключение выхода драйвера для управления силовым преобразователем выполнено по парафазной системе с возможностью определиться с оптимальным управлением в момент подачи питания (рестарта).
***
   Топология платы позволяет устанавливать дисплеи TFT 1.8" 128*160 и LCD 128*128 SPI-3. Последний, правда пока без подсветки (размеры неизвестны).
***
   Вместо кнопочной клавиатуры возможна установка энкодера (как у кулона 720).
***
   RGB светодиод не выводится на лицевую панель. В полупрозрачном корпусе вполне достаточно таинственной цветомузыки, исходящей из недр. К тому же не слепит.
***
   Карточки памяти нет, но есть USB порт типа "B", выведенный на заднюю панель, там же держатель предохранителя, сетевой выключатель и кнопка сброса. WiFi и BT никуда не делись.
***
   Предполагается забыть о подключении батареи через клеммы. Многожильные провода в силиконовой изоляции сечением AWG-12 достаточно мягкие, с низким сопротивлением и хорошо паяются. Да и больший простор для 4-х проводного подключения. Показаниям сотых долей вольта можно будет в большей степени доверять.
***
   О продуваемости объема прибора - радиаторы развернуты на 180 градусов по сравнению с 912-м и перестали перекрывать свободный проход воздуха от вентилятора. Плата нигде не прикмыкает к боковым стенкам прибора, оставляя канал продува. Термочувствительные компоненты, и в первую очередь модуль ESP и драйвер (он же измеритель) находятся в холодной зоне прибора - у задней стенки. Осталось выяснить каково будет дисплею.
В папке Resources есть 30-секундное видео D2v03.mp4.
   https://github.com/Molegvas/MORO-D20E/blob/master/Resources/D2v03.mp4
 *** 
   Предполагается, что двусторонняя плата с толщиной по меди 35мкм с каждой стороны обеспечит возможность повысить диапазон тока заряда и разряда. Ширина дорожек и длина оптимизированы. Чего не удалось избежать, так это открытых проводников под низковольтным радиатором - потребуется прокладка. Под высоковольтным - всё в порядке.
***
   Прибор оснащен датчиком тока на эффекте Холла. Измерение малых токов таким датчиком затруднительно, но выше нескольких сотен миллиампер возможно любой полярности. Измерение тока разряда снимет массу вопросов пользователей. При добавлении внешнего токового разъема можно реализовать регулируемый пользователем ток разряда в пределах порядка 10 ампер без выделения тепла в самом приборе, но с его измерением, отображением и записью прибором.
***
   Инструментарий. Да, хороший инструмент стоит дорого. По крайней мере для меня выложить 9000 рублей за китайский клон ATMEGA-ICE для программирования и отладки ПО микроконтроллера Cortex M0 - без комментариев. Всё остальное - для нас традиционно. Обе среды разработки - бесплатны.
***
В Documents найдете схему на 4-х листах и несколько изображений отмоделированной платы. 
   ... и это ещё не всё  ...
 
