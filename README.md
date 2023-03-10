# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]

Отчет по лабораторной работе #4 выполнил:
- Упаев Кирилл Анатольевич 
- РИ-210943
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

## Цель работы
Изучить работу перцептрона на реализации команд OR, AND, XOR, NAND.

## Задание 1
### Реализовать перцептрон, который умеет производить вычисления OR, AND, XOR, NAND.
Ход работы:
Был создан пустой 3D проект на Unity, к нему был подключен скрипт newPerceptron.cs.
Затем код был модифицирован и вынесен в класс ```PerceptronClass```

```csharp
private class PerceptronClass
    {
        public TrainingSet[] ts;
        double[] weights = { 0, 0 };
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
            double dp = DotProductBias(weights, ts[i].input);
            if (dp > 0) return (1);
            return (0);
        }

        void InitialiseWeights()
        {
            for (int i = 0; i < weights.Length; i++)
            {
                weights[i] = Random.Range(-1.0f, 1.0f);
            }
            bias = Random.Range(-1.0f, 1.0f);
        }

        void UpdateWeights(int j)
        {
            double error = ts[j].output - CalcOutput(j);
            totalError += Mathf.Abs((float)error);
            for (int i = 0; i < weights.Length; i++)
            {
                weights[i] = weights[i] + error * ts[j].input[i];
            }
            bias += error;
        }

        public double CalcOutput(double i1, double i2)
        {
            double[] inp = new double[] { i1, i2 };
            double dp = DotProductBias(weights, inp);
            if (dp > 0) return (1);
            return (0);
        }

        public void Train(int epochs, TrainingSet[] ts)
        {
            this.ts = ts;
            InitialiseWeights();

            for (int e = 0; e < epochs; e++)
            {
                totalError = 0;
                for (int t = 0; t < ts.Length; t++)
                {
                    UpdateWeights(t);
                    Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
                }
                Debug.Log("TOTAL ERROR: " + totalError);
            }
        }
    }
```


Затем были заполненны Training set'ы, каждый для своей операции
```OR, AND, XOR, NAND```
![image](https://user-images.githubusercontent.com/104893843/209110130-94d2cacc-de63-4d7d-b525-d9b840be5e4d.png)
![image](https://user-images.githubusercontent.com/104893843/209110198-45af3522-57fe-4fe5-b214-b7003b460b32.png)
![image](https://user-images.githubusercontent.com/104893843/209110262-8219d0a9-bff4-4297-9d51-d0ab600ac302.png)
![image](https://user-images.githubusercontent.com/104893843/209110301-5d296a0a-38b9-4897-a19f-1c288e91ce02.png)
![image](https://user-images.githubusercontent.com/104893843/209110349-88a54647-1572-4d24-9312-d56ef13dc603.png)



Мы инициализируем 4 обьекта класса ```PerceptronClass``` и обучаем их, каждого своей операции.
А затем выводим резальтаты обучения

```csharp
        int iterations = 8;
        var orPerceptron = new PerceptronClass();
        orPerceptron.Train(iterations, OrTs);
        var andPerceptron = new PerceptronClass();
        andPerceptron.Train(iterations, AndTs);
        var xorPerceptron = new PerceptronClass();
        xorPerceptron.Train(iterations, XorTs);
        var nandPerceptron = new PerceptronClass();
        nandPerceptron.Train(iterations, NandTs);



        Debug.Log("Test 0 or 0: " + orPerceptron.CalcOutput(0, 0));
        Debug.Log("Test 0 or 1: " + orPerceptron.CalcOutput(0, 1));
        Debug.Log("Test 1 or 0: " + orPerceptron.CalcOutput(1, 0));
        Debug.Log("Test 1 or 1: " + orPerceptron.CalcOutput(1, 1));

        Debug.Log("Test 0 and 0: " + andPerceptron.CalcOutput(0, 0));
        Debug.Log("Test 0 and 1: " + andPerceptron.CalcOutput(0, 1));
        Debug.Log("Test 1 and 0: " + andPerceptron.CalcOutput(1, 0));
        Debug.Log("Test 1 and 1: " + andPerceptron.CalcOutput(1, 1));

        Debug.Log("Test 0 xor 0: " + xorPerceptron.CalcOutput(0, 0));
        Debug.Log("Test 0 xor 1: " + xorPerceptron.CalcOutput(0, 1));
        Debug.Log("Test 1 xor 0: " + xorPerceptron.CalcOutput(1, 0));
        Debug.Log("Test 1 xor 1: " + xorPerceptron.CalcOutput(1, 1));

        Debug.Log("Test 0 nand 0: " + nandPerceptron.CalcOutput(0, 0));
        Debug.Log("Test 0 nand 1: " + nandPerceptron.CalcOutput(0, 1));
        Debug.Log("Test 1 nand 0: " + nandPerceptron.CalcOutput(1, 0));
        Debug.Log("Test 1 nand 1: " + nandPerceptron.CalcOutput(1, 1));
```
В консоли мы видим результаты. 
![image](https://user-images.githubusercontent.com/104893843/209110575-14e9af7c-cfb1-4a01-ab5e-ef0017ccdde5.png)

Однако как мы можем заметить, только у операции ```XOR``` не верные результаты, из-за того что линейная модель перцептрона не может правильно разделить одной линией подобного рода плоскость, а примерно так и выглядит операция ```XOR```.
![9af085a4c6ff4f83bcb92b6ea974fa01](https://user-images.githubusercontent.com/49115035/204156563-172dd9d6-c94b-4476-bc8e-29ad88230723.png)


Остальные же операции способны быть явно выполненны и перцептрон их выполняет без ошибок после обучения.




## Задание 2
### Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения.
![image](https://user-images.githubusercontent.com/104893843/209120543-1d929303-4dd1-4b8c-ac5c-fe31b4bff108.png)

необходимое количество эпох обучения зависит от bias (смещение) и weights (вес) =>
```csharp
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
```


## Задание 3
### Построить визуальную модель работы перцептрона

Ход работы:

Создадим модель для работы фунции OR. Черные кубы - единицы, белые - нули. Результат работы = результат логического сложения.
![image](https://user-images.githubusercontent.com/104893843/209204665-344acc73-95ea-4ddd-a814-194a23224176.png)

Создадим скрипт для изменения цвета при столкновении.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ChangeColor : MonoBehaviour
{
    private void OnTriggerEnter(Collider other)
    {
        if (other.gameObject.GetComponent<Renderer>().material.color == Color.white && this.gameObject.GetComponent<Renderer>().material.color == Color.white)
        {
            other.gameObject.GetComponent<Renderer>().material.color = Color.white;
            this.gameObject.GetComponent<Renderer>().material.color = Color.white;
        }
        else
        {
            other.gameObject.GetComponent<Renderer>().material.color = Color.black;
            this.gameObject.GetComponent<Renderer>().material.color = Color.black;
        }
    }
}
```

При запуске программы видим, что все работает корректно (цвета кубов правильно меняются в зависимости от выполнения логического сложения):
![lab4-SampleScene-Windows_-Mac_-Linux-Unity-2021 3 10f1-Personal-_DX11_-2022-12-23-00-01-10](https://user-images.githubusercontent.com/104893843/209209742-aecad483-ba8a-49a8-85fc-276298df5f16.gif)


## Выводы
В ходе лабораторной работы я научился использовать перцептрон и полноценно научил его 3 базовым операциям а так же XOR операции. Я понял как работает линейная модель перцептрона и узнал про XOR problem.



