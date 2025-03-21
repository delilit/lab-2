# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил:
- Тарасенко Алексей Романович
- РИ-230913
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * |  |
| Задание 2 | * |  |
| Задание 3 | * |  |

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
Научиться передавать в Unity данные из Google Sheets с помощью Python.

## Задание 1
### Задание 1. Выберите одну из игровых переменных в игре СПАСТИ РТФ: Выживание (HP, SP, игровая валюта, здоровье и т.д.), опишите её роль в игре, условия изменения / появления и диапазон допустимых значений. Постройте схему экономической модели в игре и укажите место выбранного ресурса в ней.

- Я выбрал переменную здоровье (hp). Её роль в игре заключаеться в том, что от неё зависит статус игрока (мёртв/жив).
- Она изменяеться при получении урона, получении хила от выбранных перков, при нанаесении урона от конкретного перка.
- Она изменяеться от 0 до 30 (или больше при выборе нужного перка).
- Схема экономической модели ресурса:
![zaebalo](https://github.com/user-attachments/assets/ac4934aa-a481-470c-bd93-6467b51171cc)


## Задание 2
### С помощью скрипта на языке Python заполните google-таблицу данными, описывающими выбранную игровую переменную в игре “СПАСТИ РТФ:Выживание”. Средствами google-sheets визуализируйте данные в google-таблице (постройте график / диаграмму и пр.) для наглядного представления выбранной игровой величины. Опишите характер изменения этой величины, опишите недостатки в реализации этой величины (например, в игре может произойти условие наступления эксплойта) и предложите до 3-х вариантов модификации условий работы с переменной, чтобы сделать игровой опыт лучше.
Ход работы:
При помощи средств google-sheets я визуализировал данные в гугл таблице:
![image](https://github.com/user-attachments/assets/c7edfd76-bf8b-4eca-8eb9-79877b1f248c)
Ссылка на таблицу: https://docs.google.com/spreadsheets/d/1PUr-R3_3y5-HK1eKMmlPOQVKZKrlMr2FeA0QbzK7wF4/edit?gid=0#gid=0

- Заполнение таблицы при помощи Python:

```Python
import gspread
import numpy as np
gc = gspread.service_account(filename = 'untiry-c709765052a1.json')
sh = gc.open('Workshop2')
current_hp = sh.sheet1.acell('B1').value
i = 1
while i < 4:
    sh.sheet1.update_acell(('A' + str(i)), str(i))
    sh.sheet1.update_acell(('B' + str(i)), str(current_hp))
    if i == 1:
        current_hp -= 10
    if i == 2:
        current_hp += 10
    i += 1
```
- О проблемах свзяанных со здоворьем:
    - В игре здоровье чаще всего меняеться таким образом, что у тебя либо нету 70% здоровья, либо оно полное. Я считаю, что оно требует перебалансировки в сторону умешьньшения его количества с 30 до 4-5 единиц. Также перебалансировать урон.
    - Также в игре при попадании в толпу ты очень часто можешь умереть мгновенное и случайно, я считаю, что получение урона долнж сопровождаться кадрами неуязвимости и небольшим отталкиванием от противника, чтобы дать игроку честный шанс остаться живым, а не просто случайно попсать в толпу и умереть в часовом ране за секунду.
    - Перк на увеличение макасимального хп, в следствии указанных проблем слишком сильный, поскольку если ты переживаешь схватку, то ты быстро лечишься до 100% при помощи вампиризма. Я считаю необходимым перебалансировку/удаление этого предмета.


## Задание 3
### Настройте на сцене Unity воспроизведение звуковых файлов, описывающих динамику изменения выбранной переменной. Например, если выбрано здоровье главного персонажа вы можете выводить сообщения, связанные с его состоянием.
Ход работы:
- Для начала я построил сцену по этой практике и взял предложенный в ней скрипт.
    - Если здоровье максимально (т.е. >= 30 с учётом перков), то воспроизводится звук "хорошо"
    - Если здоровье ниже максимального, но не на критическом уровне (т.е. < 30 ), то воспроизводится звук "средне"
    - Если здоровье находиться у критической планки ( hp < 20), то воспроизводится звук "плохо"
- Взятый скрипт был изменён и доработан. Основное изменение заключалось в добавлении команды 'return;' в случае отсутствия данных в dataset, что помогает избежать ошибок, которые возникали в оригинальном коде из-за попытки получить данные, которые ещё не были извлечены из таблицы. В моём коде эта ошибка отсутствует, и программа работает без ошибок в логе.
Также были изменены условия, что сделало скрипт более оптимизированным. Теперь он проверяет значения statusStart и dataset.Count только один раз. Кроме того, была исправлена ошибка, из-за которой не воспроизводился последний элемент, а также ошибки после завершения воспроизведения всех элементов больше не возникают.
```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;
using Unity.Loading;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string, float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;
 void Start()
{
    StartCoroutine(WaitForDataLoad());
}

IEnumerator WaitForDataLoad()
{
    yield return StartCoroutine(GoogleSheets());
    Debug.Log("Data Loaded: " + dataSet.Count);
}
    void Update()
    {

        if (dataSet.Count == 0) return;
        
        if ( i <= dataSet.Count & statusStart == false)
        {
            if (dataSet["Mon_" + i.ToString()] >= 30){
                StartCoroutine(PlaySelectAudioGood());
                Debug.Log(dataSet["Mon_" + i.ToString()]);
                Debug.Log("Проигрывается звук 'Horosho'");
            }
            if (dataSet["Mon_" + i.ToString()] < 30 & dataSet["Mon_" + i.ToString()] >= 20){
                StartCoroutine(PlaySelectAudioNormal());
                Debug.Log(dataSet["Mon_" + i.ToString()]);
                Debug.Log("Проигрывается звук 'Sredne'");
            }
            if (dataSet["Mon_" + i.ToString()] < 20 ){
                StartCoroutine(PlaySelectAudioBad());
                Debug.Log(dataSet["Mon_" + i.ToString()]);
                Debug.Log("Проигрывается звук 'Ploho'");
            }
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1PUr-R3_3y5-HK1eKMmlPOQVKZKrlMr2FeA0QbzK7wF4/values/Лист1?key=AIzaSyCqQ5r6kFPLTRbWQjD-ugaSgL-DfJRYdqw");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;

            
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[1]));
        }
    }
    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}
```
Как видно, все 3 записи проигрались, без единой ошибки в логах.

![image](https://github.com/user-attachments/assets/1033a092-5914-4fc9-8150-2476da9441d5)
## Выводы

В ходе работы я научился ананлизировать ресурсы определенной игры, визуализировать изменения и поведения этого ресурса, с помощью соотвествующих схем и ПО

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
