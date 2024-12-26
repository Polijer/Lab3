## Отчет по лабораторной работе № 3

#### № группы: `ПМ-2401`

#### Выполнил: `Никитин Иван Николаевич`

#### Вариант: `17`

### Cодержание:

- [Постановка задачи](#1-постановка-задачи)
- [Программа](#2-программа)
- [Анализ правильности решения](#3-анализ-правильности-решения)

### 1. Постановка задачи

> Разработать программу для тренировки навыков скоропечатания на нескольких языках, начиная с русского и поддерживая до пяти языков. Реализовать функционал добавления новых языков, анализа прогресса, расчёта скорости печати и её зависимости
от успехов на других языках, а также подсчёта времени и количества напечатанных
символов.

>Описание функционала
>1. Создание системы скоропечатания
   Создаёт объект системы скоропечатания. Изначально школьник знает только русский язык («ru»), печатает на нём со скоростью 100 символов в минуту.
>2. Вывод информации о системе
   Отображает список изучаемых языков, их скорости, суммарное время печати и
   общее количество символов, напечатанных на каждом языке.
>3. Печать на языке
   Выполняет печать указанного количества символов на выбранном языке. Учитывает текущую скорость печати на языке. Общее количество символов обновляется
   по завершении действия.
>4. Получение скорости печати
   Возвращает текущую скорость печати на указанном языке.
>5. Изучение нового языка
   Добавляет новый язык, если общее количество изучаемых языков меньше пяти.
   Скорость печати на новом языке рассчитывается как 100+минимальный прогресс на других языках
   2
   .
>6. Печать по времени
   Выполняет печать на указанном языке в течение переданного времени (в минутах). Рассчитывает количество символов, напечатанных за это время.
>7. Определение самого активного языка
   Возвращает язык, на котором школьник печатал дольше всего.
>8. Расчёт количества символов для сравнения скорости
   Определяет, сколько символов необходимо напечатать на самом медленном языке,
   чтобы его скорость сравнялась с самым быстрым языком. Учитывает правила
   изменения скорости (каждые 1000 символов скорость увеличивается на 5 символов
   в минуту).


### 2. Программа

```java
public class fastWriting {
    private String[] Languages = new String[5];
    private double[] Speeds = new double[5];
    private int[] Scores = new int[5];
    private double[] Times = new double[5];

    @Override
    public String toString() {
        String res = "Ln Sp Sc Tm\n";
        for (int i = 0; i < 5; i++) {
            res += Languages[i];
            res += " " + Speeds[i];
            res += " " + Scores[i];
            res += " " + Times[i] + "\n";
        }
        return res;
    }

    public fastWriting() {
        Languages[0] = "ru";
        Speeds[0] = 100;
        Scores[0] = 100;
        Times[0] = 1;
    }

    public void Test(String Language, int Score) {
        for (int i = 0; i < 5; i++) {
            if (Languages[i].equals(Language)) {
                Scores[i] += Score;
                Times[i] = Scores[i] / Speeds[i];
            }
        }
    }

    public double getSpeed(String Language) {
        for (int i = 0; i < 5; i++) {
            if (Language.equals(Languages[i])) return Speeds[i];
        }
        return 0;
    }

    public double minSpeed() {
        double min = Speeds[0];
        for (int i = 1; i < 5; i++) {
            if (Languages[i] != null) {
                if (Speeds[i] < min) min = Speeds[i];
            }
        }
        return min;
    }

    public double maxSpeed() {
        double max = Speeds[0];
        for (int i = 1; i < 5; i++) {
            if (Languages[i] != null) {
                if (Speeds[i] > max) max = Speeds[i];
            }
        }
        return max;
    }

    public void addNewLanguage(String Language) {
        for (int i = 0; i < 5; i++) {
            if (Languages[i] == null) {
                Languages[i] = Language;
                Speeds[i] = 100 + minSpeed() / 2;
                break;
            }
        }
    }

    public void printOverTime(String Language, int Time) {
        for (int i = 0; i < 5; i++) {
            if (Languages[i].equals(Language)) {
                Scores[i] += Time * Speeds[i];
                Times[i] += Time;
                break;
            }
        }
    }

    public double maxTime() {
        double max = Times[0];
        for (int i = 1; i < 5; i++) {
            if (max < Times[i]) max = Times[i];
        }
        return max;
    }

    public double howMuchToImprove() {
        return (maxSpeed() - minSpeed()) * 200;
    }
}
```
### 3.Тест
Ввод:
```java
import java.io.PrintStream;
import java.util.Scanner;
public class Main {
    public static Scanner in = new Scanner(System.in);
    public static PrintStream out = System.out;
    public static void main(String[] args) {
        fastWriting a = new fastWriting();
        a.addNewLanguage("en");
        out.println(a);
        a.Test("en", 200);
        out.println(a);
        out.println(a.getSpeed("ru"));
        a.printOverTime("ru", 60);
        out.println(a);
        out.println(a.maxTime());
        out.println(a.howMuchToImprove());
    }
}
```
Вывод
```
Ln Sp Sc Tm
ru 100.0 100 1.0
en 100.0 0 0.0
null 0.0 0 0.0
null 0.0 0 0.0
null 0.0 0 0.0

Ln Sp Sc Tm
ru 100.0 100 1.0
en 100.0 200 2.0
null 0.0 0 0.0
null 0.0 0 0.0
null 0.0 0 0.0

100.0
Ln Sp Sc Tm
ru 100.0 6100 61.0
en 100.0 200 2.0
null 0.0 0 0.0
null 0.0 0 0.0
null 0.0 0 0.0

61.0
0.0
```
