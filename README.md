# Сar-number-detection
Проект реализуется в рамках курса по глубокому обучению.



# Задача
Задача стояла в следующем:
Существует предприятие, директора интересуют автомобильные номера. Есть камера, она шлет поток, мы из него выделяем фото. Распознаем:
<ul>
<li>Российские номера</li>
<li>Тип автомобиля</li>
<li>Цвет автомобиля</li>
<li>Кто из работников уезжает домой пораньше</li>
</ul>
Важно запоминать в какое время приехала машина, не обязательно сохранять сами данные о машине и сотруднике и ее номера.

# Quick Start

Чтобы запустить проект локально, необходимо:
1. Создать виртуальное окружение
2. Установить зависимости из requirements.txt:
```
pip install -r requirements.txt
```
3. Проверить пути до весов/видео
4. ``` python main.py ```

# Решение
Для распознавания автомобиля по типам и номерного знака на изображении была дообучена YOLOv5.
Пайплайн выглядит следующим образом:
1. Сначала определяется, что бокс номера лежит внутри бокса машины, тогда номерной знак присваивается этой машине
2. Записывается тип машины и координаты номера и бокса автомобиля
3. Затем происходит распознавание номера машины и проверка распознанного номера по регулярному выражению
4. Производится распознование цвета автомобиля
5. Все данные записываются в базу данных со временем, когда проиходила детекция

## Пример распознавания:

<br>
<img width="1080" alt="Снимок экрана 2022-10-09 в 13 36 12" src="https://user-images.githubusercontent.com/27068383/194752096-7be94ab1-7f43-4a9b-9314-9ff58d9016a6.png">

<img width="1080" alt="Снимок экрана 2022-10-09 в 13 35 48" src="https://user-images.githubusercontent.com/27068383/194752086-e0f2957b-a509-46d9-a11d-eebafa9f8725.png">


## Распознавание номера
Для распознавания номера на номерных знаках сначала использовался tesseract, но точность получилась низкая:
<br>
![2022-10-05 16 55 17](https://user-images.githubusercontent.com/27068383/194079080-494d75e1-ec2c-44c9-9404-e665737329ff.jpg)
<br>
<p>После этого было принято решение обучить нейросеть архитектуры LPRnet. Точность распознавания текста 89,6%.</p>

## Детекция цвета

## Дерево проекта

```bash
├── ColourDetection
│   ├── __pycache__
│   │   ├── detect_color.cpython-36.pyc
│   │   └── detect_color.cpython-39.pyc
│   ├── detect_color.py
│   ├── test.data
│   ├── training.data
│   └── training_dataset
│       ├── black
│       │   ├── black1.png
│       │   ├── black10.png
│       │   ├── black11.png
│       │   ├── ...
│       ├── blue
│       │   ├── blue.jpg
│       │   ├── blue1.jpg
│       │   ├── ...
│       ├── green
│       │   ├── green1.jpg
│       │   ├── green1.png
│       │   ├── ...
│       ├── orange
│       │   ├── orange1.png
│       │   ├── orange10.png
│       │   ├── ...
│       ├── red
│       │   ├── red1.jpg
│       │   ├── red2.jpg
│       │   ├── red3.png
│       │   ├── ...
│       ├── violet
│       │   ├── violet1.png
│       │   ├── violet10.png
│       │   ├── ...
│       ├── white
│       │   ├── white1.png
│       │   ├── white10.jpg
│       │   ├── white2.png
│       │   ├── ...
│       └── yellow
│           ├── yellow1.jpg
│           ├── yellow10.png
│           ├── ...
├── DataBase
│   ├── DBlogic.py
│   └── cars_numbers_check.db
├── LPRnet
│   ├── __pycache__
│   │   ├── __init__.cpython-310.pyc
│   │   ├── rec_plate.cpython-310.pyc
│   │   ├── rec_plate.cpython-36.pyc
│   │   └── rec_plate.cpython-39.pyc
│   ├── data
│   │   ├── NotoSansCJK-Regular.ttc
│   │   ├── __init__.py
│   │   ├── __pycache__
│   │   │   ├── __init__.cpython-310.pyc
│   │   │   ├── __init__.cpython-36.pyc
│   │   │   ├── __init__.cpython-39.pyc
│   │   │   ├── load_data.cpython-310.pyc
│   │   │   ├── load_data.cpython-36.pyc
│   │   │   └── load_data.cpython-39.pyc
│   │   └── load_data.py
│   ├── model
│   │   ├── LPRNet.py
│   │   ├── __init__.py
│   │   ├── __pycache__
│   │   │   ├── LPRNet.cpython-310.pyc
│   │   │   ├── LPRNet.cpython-36.pyc
│   │   │   ├── LPRNet.cpython-39.pyc
│   │   │   ├── __init__.cpython-310.pyc
│   │   │   ├── __init__.cpython-36.pyc
│   │   │   └── __init__.cpython-39.pyc
│   │   └── weights
│   │       ├── Final_LPRNet_model.pth
│   │       └── LPRNet__iteration_2000_28.09.pth
│   ├── rec_plate.py
│   └── weights
│       ├── Final_LPRNet_model.pth
│       └── LPRNet__iteration_2000_28.09.pth
├── ObjectDetection
│   ├── YOLOS_cars.pt
│   ├── __pycache__
│   │   ├── detect_car_YOLO.cpython-36.pyc
│   │   └── detect_car_YOLO.cpython-39.pyc
│   └── detect_car_YOLO.py
├── README.md
├── __pycache__
│   ├── detect_car_YOLO.cpython-310.pyc
│   ├── detect_car_YOLO.cpython-39.pyc
│   ├── general_utils.cpython-310.pyc
│   ├── settings.cpython-36.pyc
│   ├── settings.cpython-39.pyc
│   ├── template.cpython-310.pyc
│   ├── template.cpython-36.pyc
│   ├── template.cpython-39.pyc
│   ├── track_logic.cpython-310.pyc
│   ├── track_logic.cpython-36.pyc
│   └── track_logic.cpython-39.pyc
├── main.py
├── requirements.txt
├── settings.py
├── template.py
├── test
│   ├── db_check.py
│   └── videos
│       ├── test.mp4
│       └── test2.mp4
└── track_logic.py
```

# Датасет

Датасет состоит из 4 классов:

- 0 - номера автомобилей
- 1 - легковые автомобили
- 2 - грузовые автомобили
- 3 - общественный транспорт

### Train/test split

**В обучающей выборке:** 1200 изображений (92%)

**В тестовой выборке:** 103 изображения (8%)


# Описание обученных моделей
В задаче распознавания объектов на изображении были проведены эксперименты с обучением моделей на сетях с разной архитектурой. Все гиперпараметры в YOLO изначально подобраны оптимальным образом, мы меняли только архитектуру, чтобы получить наиболее точную.

## Модель YOLOv5n


|  Класс | mAP50 | mAP50-95 |
| --- | --- | --- |
| Для всех | 0.855 | 0.708 |
| 0 (Номерные знаки) | 0.907 | 0.641 |
| 1 (Легковые автомобили) | 0.851 | 0.76 |
| 2 (Грузовые автомобили) | 0.87 | 0.75 |
| 3 (Общественный транспорт) | 0.792 | 0.682 |


## Модель YOLOv5s

|  Класс | mAP50 | mAP50-95 |
| --- | --- | --- |
| Для всех | 0.807 | 0.709 |
| 0 (Номерные знаки) | 0.91 | 0.691 |
| 1 (Легковые автомобили) | 0.827 | 0.786 |
| 2 (Грузовые автомобили) | 0.777 | 0.723 |
| 3 (Общественный транспорт) | 0.712 | 0.635 |

## Модель YOLOv5m
Модель была обучена на 19 эпохах, так как размер датасета был слишком маленьким для сети такого размера.


|  Класс | mAP50 | mAP50-95 |
| --- | --- | --- |
| Для всех | 0.848 | 0.726 |
| 0 (Номерные знаки) | 0.932 | 0.69 |
| 1 (Легковые автомобили) | 0.859 | 0.795 |
| 2 (Грузовые автомобили) | 0.839 | 0.762 |
| 3 (Общественный транспорт) | 0.761 | 0.658 |


# Reference

<p>[YOLOv5](https://github.com/ultralytics/yolov5?ysclid=l9187ounp212699888)</p>
<p>[COLOR RECOGNITION](https://github.com/ahmetozlu/color_recognition)</p>
<p>[]</p>
