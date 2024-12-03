# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Иванова Ивана Варкравтовна
- РИ000024
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Ознакомиться с основными операторами зыка Python на примере реализации линейной регрессии.

## Задание 1
### Выберите одну из игровых переменных в игре СПАСТИ РТФ: Выживание (HP, SP, игровая валюта, здоровье и т.д.), опишите её роль в игре, условия изменения / появления и диапазон допустимых значений. 
###Постройте схему экономической модели в игре и укажите место выбранного ресурса в ней.
Ход работы:
Для работы над данным заданием была выбрана величина Здоровье(HP). Она является шкалой и сочетает в себе показатель прошедшего по игроку урона и его статуса т.е. она уменьшается в случае получения урона и если она становится <0, то игрок считается погибшим, игра завершается.
Здоровье пополняется от способности Vampyrism и восстанавливается со временем, потерять его можно при получении урона от зомби. Здоровье как и любая шкала имеет max и min значения. Диапазон варируется от 0 до 30 на начальном уровне, так же возможна прокачка по ходу игры. 
Экономическая модель игры  СПАСТИ РТФ: Выживание
Она может быть описана цикличная система, где заработок, траты и прогресс завязаны на игровой активности.
1. Ресурсы:
Монеты: Основная внутриигровая валюта, получаемая за уничтожение зомби.
Патроны: Расходуемый ресурс для стрельбы из пистолета. Покупаются за монеты.
Здоровье: Жизнеспособность игрока.

2. Доход
Убийство зомби: Каждый убитый зомби приносит игроку доход.

3. Траты
Оружие и апгрейды:
Пистолет: Можно улучшать скорострельность, урон.
Патроны: Ограниченный ресурс, который нужно периодически докупать.
Здоровье: Возможна покупка способности vampirism

5. Баланс ресурсов
Монеты игрок должен зарабатывать быстрее, чем тратить их на базовые ресурсы (патроны и здоровье), чтобы оставались средства на апгрейды.
Чрезмерный дефицит здоровья или патронов сделает игру слишком сложной и демотивирующей.
Для поддержания интереса к игре, стоимость улучшений должна увеличиваться экспоненциально, а награды постепенно расти.

Роль здоровья в экономической модели:
Стимул к тратам: Потеря здоровья вынуждает игрока покупать аптечки или другие способы восстановления. Это создаёт постоянный спрос на монеты.
Тактический ресурс: Здоровье определяет, сколько ошибок игрок может совершить, прежде чем проиграет, что влияет на его стратегию: агрессивное нападение или экономия ресурсов.
Стимул прогресса: Улучшения здоровья (например, увеличение максимального HP или регенерация) могут быть важной частью апгрейдов, мотивируя игрока зарабатывать больше монет.


![image](https://github.com/user-attachments/assets/ec4293cd-50fd-4861-89ca-e077d399be76)



## Задание 2
### Должна ли величина loss стремиться к нулю при изменении исходных данных? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ.

```py

import gspread
import numpy as np

gc = gspread.service_account(filename='unitydatascience-442516-75e88216de80.json')
sh = gc.open("AD_GameDev")

hp = np.random.randint(0, 30, 10)
time_hp = list(range(0, 10))
i = 1 

damage = np.random.randint(1, 15, 10)

while i <= len(time_hp) + 1:
    current_hp = hp[i - 2] - damage[i - 2]
    status = "Жив" if current_hp > 0 else "Мертв"
    current_hp = str(current_hp).replace('.', ',')

    # Обновление значений в Google Sheets
    sh.sheet1.update(range_name=f'A{i}', values=[[i]])
    sh.sheet1.update(range_name=f'B{i}', values=[[int(hp[i - 2])]]) # Исходное здоровье
    sh.sheet1.update(range_name=f'C{i}', values=[[int(current_hp)]])# Изменение hp
    sh.sheet1.update(range_name=f'D{i}', values=[[int(damage[i - 2])]])# урон, прошедший по игроку
    sh.sheet1.update(range_name=f'E{i}', values=[[status]])# обновление статуса 

    i += 1  # Переход к следующей строке

    print(current_health, status)
```

## Задание 3
### Какова роль параметра Lr? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ. В качестве эксперимента можете изменить значение параметра.

- Перечисленные в этом туториале действия могут быть выполнены запуском на исполнение скрипт-файла, доступного [в репозитории](https://github.com/Den1sovDm1triy/hfss-scripting/blob/main/ScreatingSphereInAEDT.py).
- Для запуска скрипт-файла откройте Ansys Electronics Desktop. Перейдите во вкладку [Automation] - [Run Script] - [Выберите файл с именем ScreatingSphereInAEDT.py из репозитория].

```py

import ScriptEnv
ScriptEnv.Initialize("Ansoft.ElectronicsDesktop")
oDesktop.RestoreWindow()
oProject = oDesktop.NewProject()
oProject.Rename("C:/Users/denisov.dv/Documents/Ansoft/SphereDIffraction.aedt", True)
oProject.InsertDesign("HFSS", "HFSSDesign1", "HFSS Terminal Network", "")
oDesign = oProject.SetActiveDesign("HFSSDesign1")
oEditor = oDesign.SetActiveEditor("3D Modeler")
oEditor.CreateSphere(
	[
		"NAME:SphereParameters",
		"XCenter:="		, "0mm",
		"YCenter:="		, "0mm",
		"ZCenter:="		, "0mm",
		"Radius:="		, "1.0770329614269mm"
	], 
)

```

## Выводы

Абзац умных слов о том, что было сделано и что было узнано.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
