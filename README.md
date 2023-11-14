# FallendPersonDetection
Проект по детекции падающих людей для Сириус.ИИ. Проект подготовили Гарифуллин Инсаф, Гарифуллин Тимур. Датасет для обучения был собран в интеренете. В качестве сервиса для разметки и обучения был использован Roboflow.

## Ссылки для просмотра
Датасет

<a href="https://universe.roboflow.com/siriusai/falling-person-detection">
    <img src="https://app.roboflow.com/images/download-dataset-badge.svg"></img>
</a>

Обученная модель

<a href="https://universe.roboflow.com/siriusai/falling-person-detection/model/">
    <img src="https://app.roboflow.com/images/try-model-badge.svg"></img>
</a>

## Датасет
Датасет находится в папке dataset. В нём содержатся изображения до разметки (not_labeled_dataset) и после (labeled_dataset).

Нейросеть натренированная в roboflow 
### Cбор датасета
Датасет собирался с интернета: использовались разные сайты, основным является kaggle.com. Всего было собрано 474 изображения. Содержатся фото 3 типов:
1. Падающие/упавшие люди.
1. Сидящие люди.
1. Стоящие люди.

Такое разбиение типов было сделано с целью устранения проблем с детекцией, так как в будущем модель могла распознать всех людей как упавших людей.
Датасет содержит в разные ракурсы, сезоны, помещения, в которых были сделаны фото.

Примеры фото из датасета:

![image 36(1)](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/9d0aee07-14c5-40ab-ab27-48ef28ffcb80)
![image 37(1)](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/a18b714f-7a4e-447a-a401-d8f27fb17b53)
![image 38(1)](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/fbb51569-79b2-487e-b318-f80567724f6a)
![image 35(1)](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/1e3ad9c0-9ee6-4e06-b8b8-aa6aafa3173e)

### Разметка датасета
Далее датасет был загружен в roboflow и размечен. Для разметки использовались все те же 3 класса.
В самой разметке они называются соответственно:
1. fallen_person
2. sitting_person
3. standing_person

Процесс разметки:
![image](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/2b61a5eb-9c37-43d7-9e4c-27c065ba1a1a)


### Генерация датасета
При генерации датасета в roboflow были указаны следующие аргументы:

![image](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/3e7284a5-6840-4391-a469-1b2c829d0659)

В аугментации отсутствует rotate, так как он наоборот мешал при обучении нейросети, потому что после поворота на 90 градусов стоящего человека он становится уже лежащим.
Все изображения были уменьшены и заполнены по краям белыми до размера 832x832, так как изображения могут быть большими или маленькими.

После аугментации и генерации в дасете было следующее распределение: 1032(88%) изображения в тренировочной группе, 102(9%) в валидационной и 30(3%) в тестовой.

![image](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/59ed2445-cabb-4386-b412-21fc8c44bcce)

## Обучение нейросети
Нейросеть была обучена на бесплатном тарифе Roboflow (Roboflow 3.0 Object Detection (Fast)) в течение 50 минут. 
Результаты:

![image](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/269ba915-ffbc-4fe4-bb4c-981199e72b60)


Графики обучения:

![image](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/8cb953cd-5b9a-42d5-bca8-788a8b870f81)

## Тесты
Графики и результаты показывают хорошие значения, но что происходит во время настоящих тестов?

Было сделано несколько фотографий, схожих с тем, что будет в настоящих условиях: плохое освещение, большая дистанция.
Нейросеть выдала такие предсказания:



![Screenshot 2023-11-14 at 19-45-21 Falling person detection Object Detection Dataset and Pre-Trained Model by SiriusAI](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/a2611def-69de-4206-b2e9-63aff9cb0dc6)
![image](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/3fbdeed0-0ff5-4925-b82a-f5a727944d13)
![image](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/c9486417-0216-4545-a394-544b485c19a3)
![Screenshot 2023-11-14 at 19-45-45 Falling person detection Object Detection Dataset and Pre-Trained Model by SiriusAI](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/98c7a24f-f5dc-4c4f-85cb-6e486ad9c2d4)
![Screenshot 2023-11-14 at 19-44-57 Falling person detection Object Detection Dataset and Pre-Trained Model by SiriusAI](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/88bdc5e4-6d60-4e25-b036-6e87ce19a38a)
![image](https://github.com/jakefish18/FallendPersonDetection/assets/74649594/48bd3f19-5549-43cf-97c8-f5a717c93c60)

Мы видим, что нейросеть распознаёт в основном только фото, которые были сделаны сблизи и с хорошим освещением, что вызвано характерностью фото из интеренета.
В случае же с близким ракурсом, нейросеть выполняет свою работу хорошо. Чтобы решить эту проблему, надо собирать данные характерные для настоящей жизни.
Чтобы собрать датасет характерный для настощей жизни, надо узнать, в каких местах будут делаться фото, с каких камер, с каким разрешением, с какого ракурса и т.д.

## Выводы
В результате нейросеть выдаёт хорошие результаты на изображениях с близким ракурсом, но появляются затруднения с другими типами фото. Чтобы решить эту проблему, надо
собрать более правдивый датасет и более подробно узнать, в каких целях и ситуациях будет использоваться нейросеть. Также стоит отметить невозможность оценивания 
реальных условий работы нейросети, требований для неё. Если ожидается, что этот проект будет использоваться для тысяч камер видеонаблюдения с высоким разрешением,
то благоразумнее будет тренировать модели нейросетей отличающихся большой скоростью(YOLOvX). 
