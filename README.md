# 1 ЗАДАНИЕ

# 2 ЗАДАНИЕ

## Ответы на вопросы:

### - Какие дыры это может создать?

Использовать такой код очень рискованно и черевато из-за отсутствия какой-либо защиты данных от стороннего вмешательства
и неконтролируемого потока данных.

Очевидные примеры дыр:

- Файлы, содержащие вредоносный код, могут быть загружены на сервер, что может привести к самым разным пагубным
  последствиям
  (от падения сервера до утечки персональных данных);
    - Размер файлов никак не регламентируется, что в последствии приведет к огромной нагрузке на бэк;
    - Файлы с одинаковым именем перезаписываются, а значит есть риски случайно потерять данные.

### - Как бороться?

Лучший способ бороться со всем этим - валидация формы, т.е.
нужно фильтровать загруженные файлы в соответствии с разрешенными расширениями и размером.
Например, можно использовать функцию pathinfo() для проверки расширения файла
и установить ограничения на максимальный размер файла.

(Или использовать библиотеки и фреймворки, где уже под капотом есть инструменты для валидации)

*p.s. Логику формы вынес всё-таки в отдельный файл, чтобы почище смотрелось.*

*p.s.2 Сколько котов из 10?*

# 3 Задание

У нас есть четыре сущности: **`Спортсмены`**, **`Результаты`**, **`Призёры`**, **`Соревнования`**,
Необходимо указать все возможные связи между ними с помощью ER-диаграммы.

Дальше есть несколько развитий событий, выделим **полегче** и **посложнее** варианты.

### Вариант Полегче "**Первый День Олимпиады**":

В этом варианте спортсмены являются профессионалами и каждый из них защищает честь страны
в конкретном виде спорта, который представлен соревнованиями.

- Каждый спортсмен на профессиональном уровне может участвовать только в 1 виде спорта т.е. только в 1 соревновании,
  следовательно, связь `1 к 1`.
    - У каждого спортсмена может быть несколько результатов на соревновании, т.к. во многих видах спорта даётся
      несколько
      попыток,
      значит связь спортсмена к результатам `1 к n`
    - Так как это только первый день олимпиады, то 1 спортсмен может стать призёром только в 1 виде спорта, поэтому
      связь `1 к 1`

Итого:
[Easy схема в папке task3](task3/easy.drawio.png)

### Вариант Посложнее "Школьные соревнования ЗВЁЗДОЧКА":

В этом варианте спортсменами выступают ученики 9-х классов школы №5.
К сожалению современное поколение Z плохо ладит со спортом, поэтому со всего класса
мы имеем лишь несколько Гераклов, здоровье которых позволяет принимать участие в спортивных мероприятиях,
а значит каждый из них будет отстаивать честь класса сразу в нескольких дисциплинах.

- Каждый спортсмен привязан к нескольким соревнованиям, следовательно, связь `n к n`;
    - Каждый спортсмен может иметь несколько результатов, значит `1 к n`;
    - У каждого соревнования может быть 3 призёра `1 к n`;
    - Каждый спортсмен может быть призёром в нескольких соревнованиях - `1 к n`;

Итого:
[Hard схема в папке task3](task3/hard.drawio.png)

# 4 Задание

MySQL-запрос для создания таблицы спортсменов:

```mysql
CREATE TABLE `спортсмены`
(
    `id`                             INT,
    `ФИО`                            VARCHAR(255),
    `e-mail`                         VARCHAR(255),
    `телефон`                        VARCHAR(11),
    `дата рождения`                  DATE,
    `возраст`                        TINYINT UNSIGNED,
    `дата и время создания записи`   TIMESTAMP,
    `номер паспорта`                 VARCHAR(6),
    `среднее место на соревнованиях` DECIMAL(3, 2),
    `биография`                      TEXT,
    `видеопрезентация`               LONGBLOB,
    PRIMARY KEY (`id`)
);
```

топ 5 ФИО спортсменов, больше остальных посетивших соревнований (одним SQL-запросом и без вложенных SELECT’ов):

```sql
SELECT `спортсмены`.`ФИО`, COUNT(`результаты`.`id_соревнование`) as `количество посещенных соревнований`
FROM `результаты`
         INNER JOIN `спортсмены` ON `результаты`.`id_спортсмена` = `спортсмены`.`id`
GROUP BY `результаты`.`id_спортсмен`
ORDER BY `количество посещенных соревнований` DESC
LIMIT 5;

```
