# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
- Федорова Елизавета Евгеньевна
- РИ210932

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

## Цель работы
Познакомиться с работой перцептрона на практике при помощи движка Unity. Реализовать перцептрон который умеет решать логические операции.

## Задание 1
### В проекте Unity реализовать перцептрон, который умеет производить вычисления: OR, AND, NAND, XOR, и дать комментарии о корректности работы.

- Для начала создадим новый проект в Unity, затем в проекте создадим пустой объект. К этому объекту прикрепим скрипт с кодом из методических указаний:

```py
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(8);
		Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));		
	}
	
	void Update () {
		
	}
}
```

![image_1](https://user-images.githubusercontent.com/103308669/204554810-21d0b53e-5cd4-4443-b7c7-2c7a7eacdd39.png)
![image_2](https://user-images.githubusercontent.com/103308669/204554856-081cfa02-2c10-4f21-8e93-808061141570.png)

- Реализация операции OR | ИЛИ:

Дизъюнкция - логическая операция, по своему применению максимально приближённая к союзу «или» в смысле «или то, или это, или оба сразу».

![image](https://user-images.githubusercontent.com/103308669/204555558-48604af2-42ae-4f9f-817f-efaaa1378e5f.png)

Установим следующие значения для скрипта:
![image_3](https://user-images.githubusercontent.com/103308669/204555637-8cbfcf37-d938-48ca-b5df-bbc8ee7a901a.png)

Для начала зададим 1 обучающую эпоху и посмотрим на результаты тестов:
![image_4](https://user-images.githubusercontent.com/103308669/204555928-454ae457-5ea7-4100-bac7-f36d14a5c162.png)

Значение Total Error отвечает за обучение перцептрона: если оно отлично от нуля, то модель не обучилась, если же нуль - тогда модель успешно обучилась. При первой эпохе обучения значение Total Error = 2, и из результатов тестов видно, что значения отличны от истины: в первом случае, ответ 1, хотя должен быть 0.

Далее зададим 4 эпохи обучения:
![image_5](https://user-images.githubusercontent.com/103308669/204556134-85ab6cd5-54cb-4b13-b706-41829c163d5a.png)
![image_6](https://user-images.githubusercontent.com/103308669/204556172-8776cd6e-ecf9-46de-a503-4ba94dbab189.png)

Можно наблюдать, что даже с 4-мя эпохами обучения значение Total Error не равно нулю.

Повторим запуск программы.
![image_7](https://user-images.githubusercontent.com/103308669/204556438-e7e6722a-d9b1-45cc-8eee-93f905525a76.png)
![image_8](https://user-images.githubusercontent.com/103308669/204556466-4830269c-ccdf-4801-929b-45e3819c57e1.png)

В данном случае модель обучилась за 3 эпохи и значение Total Error стало равно 0. При подстановке данных получены правильные значения, отражающие логику логического "или".
Можем сделать вывод, что не всегда 4 эпохи обучения достаточно, бывает, что и на 4 итерации Total Error отлично от нуля и программа работает некорректно.

- Реализация операции AND | И:

Конъюнкция - логическая операция, по смыслу максимально приближенная к союзу «и».

![image](https://user-images.githubusercontent.com/103308669/204556654-e7d5c0c0-cc27-422e-9aee-909ffc99b300.png)

Установим новые значения для скрипта:
![image_9](https://user-images.githubusercontent.com/103308669/204556820-5fb19315-9fd2-4e2a-b95b-0477cee43e5b.png)

Для начала зададим 1 обучающую эпоху и посмотрим на результаты тестов:
![image_10](https://user-images.githubusercontent.com/103308669/204556882-8fae6faf-a9ef-4ca8-add5-a752e648d61f.png)

Можно наблюдать, что 1 эпохи обучения мало для правильного обучения модели.

Повторим запуск программы, только уже с 4 эпохами:
![image_11](https://user-images.githubusercontent.com/103308669/204557039-e96f67fd-4a93-436c-9498-b61e433975df.png)
![image_12](https://user-images.githubusercontent.com/103308669/204557070-bf5a72cc-1b02-493f-b966-2b964c9c77af.png)

Такого количества эпох тоже мало для успешного обучения модели.

Попробуем увеличить количество эпох до 6:

![image_13](https://user-images.githubusercontent.com/103308669/204557163-0334a8e3-2b38-4442-9e15-a91be545e0fb.png)
![image_14](https://user-images.githubusercontent.com/103308669/204557187-ace0c82d-46b8-47b4-ae4a-5f7aa71e2138.png)
![image_15](https://user-images.githubusercontent.com/103308669/204557206-e567af22-6138-4a06-99b2-6db5f8e6c14a.png)

В этот раз модель смогла обучиться за 3 эпохи, получен правильный вывод, отражающий логику логического "и".

- Реализация операции NAND | инвертированный элемент И:

NAND — это такой логический элемент NOT-AND, то есть НЕ-И. Фактически, это перевернутый вентиль И. По таблице истинности на выходе вентиля И мы получаем единицу только в случае если на оба входа тоже приходит единица. В NAND всё наоборот.

![image](https://user-images.githubusercontent.com/103308669/204557723-65e59778-1db9-4254-a29b-fbc7c861765d.png)

Установим новые значения для скрипта:
![image_16](https://user-images.githubusercontent.com/103308669/204557891-ef755a0a-3840-4089-953b-f0aa3a83821d.png)

Для начала зададим 1 обучающую эпоху и посмотрим на результаты тестов:
![image_17](https://user-images.githubusercontent.com/103308669/204557975-9a5759dc-7a35-4a40-9b48-86dcfb48e843.png)

Как и в прошлые 2 раза одной эпохи для обучения мало.
Установим 4 эпохи обучения:
![image_18](https://user-images.githubusercontent.com/103308669/204558049-6bfa3275-1b6c-4ad5-8150-3cc4e7506a64.png)
![image_19](https://user-images.githubusercontent.com/103308669/204558064-48b5479b-4419-4a85-a205-b74e49bee2a6.png)

В данной попытке модель обучилась за 4 эпохи и значение Total Error было равно нулю, получен правильный вывод, отражающий логику "не и".

- Реализация операции XOR | исключающее ИЛИ:

XOR — это логический оператор, работающий с битами. Если два получаемых им на входе бита одинаковы, то результат будет равен 0, в противном случае 1. При этом применяется операция исключающего или — чтобы результат был равен 1, только один аргумент должен быть равен 1.

![image](https://user-images.githubusercontent.com/103308669/204558254-7bfdfc5c-4795-49f6-b500-cbcd79ccbef3.png)

Установим новые значения для скрипта:
![image_20](https://user-images.githubusercontent.com/103308669/204558393-a1249312-2813-48d3-91f0-53ac2503912d.png)

Сразу применим 4 эпохи обучения, т.к. по предыдущим попыткам понятно, что с 1 эпохи модель не обучится:
![image_21](https://user-images.githubusercontent.com/103308669/204558472-de1e7cf5-1547-401f-9b97-2545be5daacd.png)
![image_22](https://user-images.githubusercontent.com/103308669/204558485-331684ac-c98c-4987-bbc5-22b0cc7fc4a4.png)

При нескольких попытках четырех эпох было мало для обучения. В ходе работы с разными значениями эпох обучения было выяснено, что начиная с третьей-четвертой эпохи значение Total Error всегда было равно 4. Программа работает некорректно.

Блок обработки однослойной сети персептронов способен классифицировать набор шаблонов на два класса, поскольку линейная пороговая функция определяет их линейную разделимость. И наоборот, два класса должны быть линейно разделяемыми, чтобы сеть персептрона функционировала правильно. Действительно, это основное ограничение однослойной сети персептронов.

1-слойный персептрон ≡ 1-мерный разделитель, 2-слойный персептрон ≡ 2-мерный разделитель, 3-слойный персептрон ≡ 3-мерный разделитель, n слой персептрона ≡ n разделитель измерений.

Следовательно, однослойный перцептрон не может обучиться этой операции, поскольку XOR не линейно-разделяемый.

Можем сделать вывод, что один слой перцептрона может решать только линейные задачи, такие как AND, OR, NAND. Для решения нелинейной задачи XOR необходимо добавлять еще слои перцептронов.

## Задание 2
### Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения.
Для каждой операции было запущено 4 попытки с 8-ю эпохами обучения. На графиках указано среднее значение Total Error за 4 попытки.

- Операция OR | ИЛИ:

![image_23](https://user-images.githubusercontent.com/103308669/204562619-3612191f-986e-45e9-ba8b-3d39ec2f3921.png)

- Операция AND | И:

![image_24](https://user-images.githubusercontent.com/103308669/204562679-f4ce6bf7-7f10-40ff-bdbb-764422e2ce10.png)

- Операция NAND | инвертированный элемент И:

![image_25](https://user-images.githubusercontent.com/103308669/204562808-1c94c230-1d0e-42c7-850f-68bc02576d3b.png)


## Задание 3
### Построить визуальную модель работы перцептрона на сцене Unity.

- Для начала создадим сцену с тремя вариантами: черный куб - 1, белый куб - 0; три варианта: (0 0), (0 1) и (1 1).

![image_26](https://user-images.githubusercontent.com/103308669/204604519-15250ea0-86c5-4f87-a3c3-dc5be19f14c7.png)

При столкновении кубиков, они окрасятся в цвет результата той или иной операции (черный - 1, белый - 0).

- Чтобы менялись цвета, создадим новый скрипт:

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ColorChanger : MonoBehaviour
{
    private void OnTriggerEnter(Collider other)
    {
        if (gameObject.GetComponent<Renderer>().material.color == Color.white && other.gameObject.GetComponent<Renderer>().material.color == Color.white)
        {
            gameObject.GetComponent<Renderer>().material.color = Color.white;
            other.gameObject.GetComponent<Renderer>().material.color = Color.white;
            
        }
        else
        {
            gameObject.GetComponent<Renderer>().material.color = Color.black;
            other.gameObject.GetComponent<Renderer>().material.color = Color.black;
        }
    }
}
```
- Также у верхнего куба нужно поставить Rigidbody, а у нижнего - Is trigger. 
- Запустим программу и посмотрим на результат работы операции OR:

https://user-images.githubusercontent.com/103308669/204605399-d0cb65b9-9872-40ab-a704-d7392d5346d1.mp4

Цвета кубиков сменились корректно, в соответствии с функцией.

## Выводы

В ходе выполнения данной лабораторной работы, я познакомилась с моделью нейроной сети - перцептроном. С помощью однослойного перцептрона смогла реализовать такие логические операции как OR, AND, NAND и XOR. Обнаружила, что с помощью одного перцептрона нельзя найти данные через XOR, поскольку XOR не линейно-разделяемый. Также построила графики для оценки обучаемости модели той или иной операции, легче всего перцептрону далась операция OR. Применив функционал Unity, построила наглядную модель работы перцептрона.
