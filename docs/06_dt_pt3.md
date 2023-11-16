# data.table pt3 {-}
## Запись занятия {-}

Запись занятия 14 октября:

<iframe width="560" height="315" src="https://www.youtube.com/embed/GFUtbhB1tgE?si=HsYCs_sIdgM_ZyEv" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Разбор домашней работы {-}

### level 3 (HMP) {-}

 - изучите справку по функции `ifelse()` (или `fifelse()` в data.table)

 - создайте копию колонки `gender`, назовите ее `gender_2`. Замените в ней все `n/a` и `hermaphrodite` на `other`. Посчитайте количество персонажей в зависимости от пола (`gender_2`):


```r
library(data.table)
sw <- fread('http://bit.ly/39aOUne')

sw[, gender_2 := gender]
sw[gender_2 %in% c('n/a', 'hermaphrodite'), gender_2 := 'other']
sw[, .N, by = gender_2]
```

```
##    gender_2  N
## 1:     male 57
## 2:    other  4
## 3:   female 16
```

 - выполните предыдущее задание без создания промежуточной колонки `gender_2` 
 

```r
# два альтернативных решения
# sw[gender %in% c('n/a', 'hermaphrodite'), gender := 'other']
sw[, gender := fifelse(gender %in% c('n/a', 'hermaphrodite'), 'other', gender)]
sw[, .N, by = gender]
```

```
##    gender  N
## 1:   male 57
## 2:  other  4
## 3: female 16
```
 
 
### level 4 (UV) {-}

 - сделайте сводную таблицу `planet_chars` по персонажам каждой планеты, где в колонках будет количество персонажей, их средний рост и вес (оригинальный и скорректированный).
 
 - округлите значения до 1 знака после запятой

Первые 5 строк результата:

```r
sw[, mass_corrected := mass]
sw[, mass_mean := mean(mass, na.rm = TRUE), by = list(planet_name, gender)]
sw[is.na(mass), mass_corrected := mass_mean]
planet_chars <- sw[, list(
        n_chars = .N,
        height_mn = round(mean(height), 1),
        mass_mn = round(mean(mass, na.rm = TRUE), 1),
        mass_corrected_mn = round(mean(mass_corrected, na.rm = TRUE), 1)
), by = planet_name]
planet_chars[1:5]
```

```
##    planet_name n_chars height_mn mass_mn mass_corrected_mn
## 1:    Tatooine      10     169.8    85.4              85.8
## 2:      Kamino       3     208.3    83.1              83.1
## 3:    Geonosis       1     183.0    80.0              80.0
## 4:      Utapau       1     206.0    80.0              80.0
## 5:    Kashyyyk       2     231.0   124.0             124.0
```


## Манипуляции с таблицами

### merge() {-}
Одна из самых, наверное, важных операций при работе с таблицами - построчное слияние двух или нескольких таблиц. При использовании функции `merge()` каждому значению в ключевой колонке первой таблицы сопоставляется строка параметров наблюдения другой таблицы, с таким же значением в ключевой колонке, как и в первой таблице. В других языках программирования, в SQL, в частности, аналогичная функция может называться `join`.
Несмотря на сложность формулировки, выглядит это достаточно просто:

```r
# создаем датасет 1, в синтасисе data.table
dt1 <- data.table(key_col = c('r1', 'r2', 'r3'),
                  col_num = seq_len(3))

# создаем датасет 2, в синтасисе data.table
dt2 <- data.table(key_col = c('r3', 'r1', 'r2'),
                  col_char = c('c', 'a', 'b'))

# сливаем построчно по значениям в колонке key_col
merge(x = dt1, y = dt2, by = 'key_col')
```

```
##    key_col col_num col_char
## 1:      r1       1        a
## 2:      r2       2        b
## 3:      r3       3        c
```

Здесь первая таблица задается аргументом `x`, вторая таблица - аргументом `y`, а колонка (или колонки), по значениям которой происходит слияние таблиц, задается аргументом `by`. Если аргумент `by` не указан, то слияние происходит по тем колонкам, которые имеют одинаковое название в сливаемых таблицах.  Притом, таблицы можно сливать по значениям колонок разными именами, тогда надо отдельно указать, по значениям каких колонок в первой и второй таблице происходит слияние, и для этого вместо общего аргумента `by` используют аргументы `by.x` и `by.y` для первой и второй таблицы соответственно.

В первом приближении операция слияния `merge()` похожа на результат работы функции `cbind()`. Однако, из-за того, что при слиянии происходит сопоставление по значениям ключевых колонок, в результате получается решается проблема слияния колонок, в которых разный порядок строк. Сравните:


```r
cbind(dt1, dt2)
```

```
##    key_col col_num key_col col_char
## 1:      r1       1      r3        c
## 2:      r2       2      r1        a
## 3:      r3       3      r2        b
```

```r
merge(x = dt1, y = dt2, by = 'key_col')
```

```
##    key_col col_num col_char
## 1:      r1       1        a
## 2:      r2       2        b
## 3:      r3       3        c
```


Второе существенное отличие от `cbind()` - обработка ситуаций, когда в таблицах разное количество наблюдений. Например, в первой таблице данные по первой волне опросов, а во второй - данные по тем, кто из принявших участие в первой волне, принял участие и во второй волне, а так же какие-то новые опрошенные респонденты. Разное количество наблюдений в сливаемых таблицах порождает четыре варианта слияния, все они задаются аргументом `all` с постфикасми.

Варианты направлений слияния (мерджа) таблиц:

- `all` = FALSE. Значение аргумента по умолчанию, в результате слияния будет таблица с наблюдениями, которые есть и в первой, и во второй таблице. То есть, наблюдения из первой таблицы, которым нет сопоставления из второй таблицы, отбрасываются. В примере с волнами это будет таблица только по тем, кто принял участи и в первой, и во второй волнах опросов:

```r
# сливаем так, чтобы оставить только тех, кто был в обеих таблицах, это значение по умолчанию
merge(x = dt1, y = dt2, by = 'key_col', all = FALSE)
```

```
##    key_col col_num col_char
## 1:      r1       1        a
## 2:      r2       2        b
## 3:      r3       3        c
```

- `all.x` = TRUE. Всем наблюдениям из первой таблицы сопоставляются значения из второй. Если во второй таблице нет соответствующих наблюдений, то пропуски заполняются `NA`-значениями (в нашем примере в колонке `col2`):

```r
merge(x = dt1, y = dt2, by = 'key_col', all.x = TRUE)
```

```
##    key_col col_num col_char
## 1:      r1       1        a
## 2:      r2       2        b
## 3:      r3       3        c
```

- `all.y` = TRUE. Обратная ситуация, когда всем наблюдениям из второй таблицы сопоставляются значения из первой, и пропущенные значения заполняются `NA`-значениями (в нашем примере в колонке `co12`):

```r
merge(x = dt1, y = dt2, by = 'key_col', all.y = TRUE)
```

```
##    key_col col_num col_char
## 1:      r1       1        a
## 2:      r2       2        b
## 3:      r3       3        c
```

- `all` = TRUE. Объединение предыдущих двух вариантов - создается таблица по всему набору уникальных значений из ключевых таблиц, по которым происходит слияние. и если в какой-то из таблиц нет соответствующих наблюдений, то пропуски также заполняются `NA`-значениями:


```r
merge(x = dt1, y = dt2, by = 'key_col', all = TRUE)
```

```
##    key_col col_num col_char
## 1:      r1       1        a
## 2:      r2       2        b
## 3:      r3       3        c
```

При работе с несколькими таблицами можно столкнуться с ограничением, что базовая функциz `merge()` работает только с парами таблиц. То есть, если вдруг необходимо слить по одному ключу сразу несколько таблиц (например, не две волны опросов, а пять), то придется строить последовательные цепочки попарных слияний.


**Alarm!**
Необходимо помнить, что в ситуациях, когда одному значению ключа в первой таблице соответствует одна строка, а во второй таблице - несколько строк, то в результате слияния таблиц значения из первой таблицы размножатся по количеству строк во второй таблице:

Создаем данные:

```r
# создаем датасет 1, в синтасисе data.table
dt1 <- data.table(key_col = c('r1', 'r2', 'r3', 'r4', 'r1'),
                  col_num = seq_len(5))

# создаем датасет 2, в синтасисе data.table
dt2 <- data.table(key_col = c('r3', 'r1', 'r2', 'r5', 'r1', 'r1'),
                  col_char = c('c', 'a', 'b', 'd', 'e', 'f'))
```

Тут мы видим, что в первой таблице есть две строки с `key_col = r1`, а во второй таблице -- три строки. 

Сливаем и получаем размножение значений из первой таблицы для ключа `key_col = r1`, значения `1` и `5` из колонки `col1` теперь встречается три раза. А значения `a`, `e` и `f`  встречаются по два раза:

```r
merge(dt1, dt2, by = 'key_col', all.x = TRUE)
```

```
##    key_col col_num col_char
## 1:      r1       1        a
## 2:      r1       1        e
## 3:      r1       1        f
## 4:      r1       5        a
## 5:      r1       5        e
## 6:      r1       5        f
## 7:      r2       2        b
## 8:      r3       3        c
## 9:      r4       4     <NA>
```

То есть три значения из второй таблицы повторились для каждого значения из первой таблицы. В результате у нас получилось шесть строк.

### long / wide-форматы данных {-}

Обычная форма представления данных в таблицах --- когда одна строка является одним наблюдением, а в значениях колонок отражены те или иные характеристики этого наблюдения. Такой формат традиционно называется `wide`-форматом, потому что при увеличении количества характеристик таблица будет расти вширь, путем увеличения числа колонок. Пример таблицы в `wide`-формате.

```r
# создаем таблицу с идентификатором респондента, его возрастом, ростом и весом
dt_wide <- data.table(
  wave = paste0('wave_', rep(1:2, each = 2)),
  id = paste0('id_', rep(1:2)),
  age = c(45, 68, 47, 69),
  height = c(163, 142, 164, 140),
  weight = c(55, 40, 50, 47))
dt_wide
```

```
##      wave   id age height weight
## 1: wave_1 id_1  45    163     55
## 2: wave_1 id_2  68    142     40
## 3: wave_2 id_1  47    164     50
## 4: wave_2 id_2  69    140     47
```

Тем не менее, нередко встречается другой формат, в котором на одно наблюдение может приходиться несколько строк (по количеству измеренных характеристик этого наблюдения). В таком случае таблица состоит из колонки, в которой содержится какой-то идентификатор объекта, одной или нескольких колонок, в которых содержатся идентификаторы характеристик объекта, и колонки, в которой содержатся значения этих характеристик. Такой формат называется длинным, `long`-форматом данных, потому что при увеличении количества измеряемых характеристик таблица будет расти в длину увеличением строк.


```r
# создаем таблицу с идентификатором респондента, его возрастом, ростом и весом
dt_long <- data.table(
  # две волны, по два респондента в каждой
  wave = paste0('wave_', rep(1:2, each = 6)),
  # на каждого респондента задаем три строки
  id = paste0('id_', rep(rep(1:2, each = 3), 2)),
  # три характеристики повторяем для четырех респондентов
  variable = rep(c('age', 'height', 'weight'), 4),
  # задаем значения характеристик, с учетом того, как упорядочены первые две колонки
  value = c(45, 163, 55,
            68, 142, 40,
            47, 164, 50,
            69, 140, 47))

dt_long
```

```
##       wave   id variable value
##  1: wave_1 id_1      age    45
##  2: wave_1 id_1   height   163
##  3: wave_1 id_1   weight    55
##  4: wave_1 id_2      age    68
##  5: wave_1 id_2   height   142
##  6: wave_1 id_2   weight    40
##  7: wave_2 id_1      age    47
##  8: wave_2 id_1   height   164
##  9: wave_2 id_1   weight    50
## 10: wave_2 id_2      age    69
## 11: wave_2 id_2   height   140
## 12: wave_2 id_2   weight    47
```

<br>

### dcast() {-}

Для того, чтобы трансформировать `long`-формат в `wide`-формат, используется функция `dcast()` пакета `data.table` (либо `cast()` пакета `reshape2`). Также можно использовать функцию `reshape()` из базового набора функций R, однако эта функция достаточно медленная по скорости работы.

Для того, чтобы превратить созданную выше таблицу в `long`-формате в широкий формат, выражение будет выглядеть следующим образом (сама операция называется решейп):

```r
dcast(data = dt_long, formula = wave + id ~ variable, value.var = 'value')
```

```
##      wave   id age height weight
## 1: wave_1 id_1  45    163     55
## 2: wave_1 id_2  68    142     40
## 3: wave_2 id_1  47    164     50
## 4: wave_2 id_2  69    140     47
```

Здесь аргумент `data` - определяет таблицу, которую мы хотим трансформировать. 

Аргумент `formula` задает, что в результирующей таблице будет задавать уникальное наблюдение, и значения какой колонки будут разделены на самостоятельные колонки. Формулу можно прочитать как `строки ~ колонки` в результирующей таблице. В нашем случае уникальное наблюдение мы задаем парой переменных `wave` и `id`, поэтому мы их указываем до тильды через `+`. Колонки же мы создаем по значениям переменной `variable`, после тильды. Следует отметить, что ситуация, когда строка задается несколькими переменными через оператор `+`, весьма частая, а вот в правой части формулы несколько переменных встречаются достаточно редко, обычно все же на колонки раскладывают по значениям одной переменной. 

Аргумент `value.var` содержит текстовое название переменной, значения которой будут отражены в результирующей таблице по колонкам для каждого наблюдения. 


Иногда случаются ситуации, когда необходимо провести сначала агрегацию по одной из колонок, описывающих наблюдение. Например, вычислить средние значения возраста, роста и веса для каждой волны. Это можно сделать в два этапа - сначала провести агрегацию, и потом решейп. Также можно сразу сделать решейп, и воспользоваться дополнительным аргументом `fun.aggregate`, который сразу, при решейпе, агрегирует данные. Например, если использовать сначала агрегацию, а потом трансформацию в `wide`-формат:

```r
# агрегируем наблюдения по волнам и характеристикам
tmp <- dt_long[, list(value = mean(value)), by = list(wave, variable)]
tmp
```

```
##      wave variable value
## 1: wave_1      age  56.5
## 2: wave_1   height 152.5
## 3: wave_1   weight  47.5
## 4: wave_2      age  58.0
## 5: wave_2   height 152.0
## 6: wave_2   weight  48.5
```

```r
# трансформируем в wide-формат. колонки id уже нет в таблице, поэтому удаляем из формулы
dcast(data = tmp, formula = wave ~ variable, value.var = 'value')
```

```
##      wave  age height weight
## 1: wave_1 56.5  152.5   47.5
## 2: wave_2 58.0  152.0   48.5
```

Аналогично, но с использованием аргумента `fun.aggregate`. В значения аргумента передаём название функции без кавычек и скобок, в нашем случае это `fun.aggregate = mean`:

```r
dcast(data = tmp, formula = wave ~ variable, value.var = 'value', fun.aggregate = mean)
```

```
##      wave  age height weight
## 1: wave_1 56.5  152.5   47.5
## 2: wave_2 58.0  152.0   48.5
```

<br>

### melt() {-}
Обратная трансформация также возможна, из `wide`-формата в `long`-формат. Для этого используется функция `melt()`: 


```r
melt(data = dt_wide, 
     id.vars = c('wave', 'id'),
     measure.vars = c('age', 'height', 'weight'),
     variable.name = 'variable',
     value.name = 'value')
```

```
##       wave   id variable value
##  1: wave_1 id_1      age    45
##  2: wave_1 id_2      age    68
##  3: wave_2 id_1      age    47
##  4: wave_2 id_2      age    69
##  5: wave_1 id_1   height   163
##  6: wave_1 id_2   height   142
##  7: wave_2 id_1   height   164
##  8: wave_2 id_2   height   140
##  9: wave_1 id_1   weight    55
## 10: wave_1 id_2   weight    40
## 11: wave_2 id_1   weight    50
## 12: wave_2 id_2   weight    47
```

Здесь аргумент `id.vars` задает переменные, которые будут использоваться для уникальной идентификации наблюдения. Аргумент `measure.vars` определяет те колонки, которые войдут длинную таблицу как значения переменной характеристик наблюдений (когда каждая строка --- отдельная характеристика наблюдения, несколько строк на одного пользователя). Аргументы `variable.name` и `value.name`  задают, соответственно, названия колонок характеристик наблюдения и значений этих характеристик в финальной таблице.

## Соотношение list() и `:=` в операциях над колонками.

На занятии я заметил, что многие путаются в синтаксисе создания новых колонок и в выражении `list()`. Различие следующее:

```r
sw[, new_value := 'bla-bla-bla']
sw[1:5]
```

```
##                  name height mass skin_color gender planet_name gender_2
## 1:     Luke Skywalker    172   77       fair   male    Tatooine     male
## 2:              C-3PO    167   75       gold  other    Tatooine    other
## 3:        Darth Vader    202  136      white   male    Tatooine     male
## 4:          Owen Lars    178  120      light   male    Tatooine     male
## 5: Beru Whitesun lars    165   75      light female    Tatooine   female
##    mass_corrected mass_mean   new_value
## 1:             77     100.2 bla-bla-bla
## 2:             75      53.5 bla-bla-bla
## 3:            136     100.2 bla-bla-bla
## 4:            120     100.2 bla-bla-bla
## 5:             75      75.0 bla-bla-bla
```

Здесь выражение `sw[, new_value := 'bla-bla-bla']` можно прочитать как `в таблице sw создай новую колонку new_value и запиши в нее значение 'bla-bla-bla'`. Одинарное значение будет размножено по количеству строк. Вместо `'bla-bla-bla'` также может быть и какая-нибудь функция, которая создает вектор такой же длины, сколько строк в таблице (если больше или меньше, то выдаст ошибку):

```r
# в таблице 77 строк, поэтому можем просто указать 77:1
sw[, new_value2 := 77:1]
sw[1:5]
```

```
##                  name height mass skin_color gender planet_name gender_2
## 1:     Luke Skywalker    172   77       fair   male    Tatooine     male
## 2:              C-3PO    167   75       gold  other    Tatooine    other
## 3:        Darth Vader    202  136      white   male    Tatooine     male
## 4:          Owen Lars    178  120      light   male    Tatooine     male
## 5: Beru Whitesun lars    165   75      light female    Tatooine   female
##    mass_corrected mass_mean   new_value new_value2
## 1:             77     100.2 bla-bla-bla         77
## 2:             75      53.5 bla-bla-bla         76
## 3:            136     100.2 bla-bla-bla         75
## 4:            120     100.2 bla-bla-bla         74
## 5:             75      75.0 bla-bla-bla         73
```

Выражение *sw[, new_value2 := 77:1]* можно прочитать как **в таблице sw создай новую колонку new_value2 и запиши в нее вектор, который получится в результате выполнения выражения 77:1**.


Конструкция с `list()` используется тогда, когда на основе существующей таблицы надо создать новую таблицу. Фактически это создание нового списка на основе колонок таблицы, просто в результате будет таблица и класс data.table:

```r
new_dt <- sw[, list(total_users = uniqueN(name), 
                    height_mn = mean(height, na.rm = TRUE))]
new_dt
```

```
##    total_users height_mn
## 1:          77  176.2078
```

Здесь выражение **new_dt <- sw[, list(total_users = uniqueN(name), height_mn = mean(height, na.rm = TRUE))]** можно прочитать следующим образом: **на основе таблицы sw создай таблицу, в которой в колонку total_users запиши количество уникальных значений из колонки name, а в height_mn - среднее значение по колонке height Полученную таблицу запиши в объект new_dt**.
Надо помнить, что `total_users` и `height_mn` - это колонки, которые будут в новой таблице, в `sw` их нет. 

Соответственно, использовать `:=` вместе с `list()` некорректно. Точно также использовать знак `=` неправильно для создания новых колонок в уже существующей таблице, интерпретатор вернет ошибку.

<br>

## Домашнее задание {-}

### level 1 (IATYTD) {-}

Посчитайте, сколько пользователей (`user_pseudo_id`) в приложение, с разбивкой по платформам (`platform`). 
Датасет: <https://gitlab.com/hse_mar/mar211f/-/raw/main/data/installs.csv>


```
##    platform n_users
## 1:  ANDROID   86112
## 2:      IOS   39484
```

Проверьте, нет ли дублей в таблице (когда несколько записей на одного пользователя). 
Подумайте, как можно от них избавиться.


### level 2 (HNTR) {-}

Посчитайте, сколько было платящих пользователей (`n_payers`), сколько они сделали платежей (`n_purchases`) и на какую сумму (`gross`), сколько в среднем сделал платежей каждый пользователь (`purchases_per_user`), средний размер платежа (`purchase_mn`) и сколько в среднем заплатил каждый пользователь (`ARPPU`). 
Датасет: <https://gitlab.com/hse_mar/mar211f/-/raw/main/data/payments_custom.csv>


```
##    n_payers    gross n_purchases purchases_per_user purchase_mn  ARPPU
## 1:     7228 291734.2       35585               4.92         8.2 40.362
```

### level 3 (HMP) {-}

Сделайте предыдущее задание, только добавьте разбивку по платформам.
Добавьте `total` (то есть статистику по всей выборке, без разбивки).


```
##    platform n_payers  gross n_purchases purchases_per_user purchase_mn  ARPPU
## 1:  ANDROID     3420  95069       12752               3.73        7.46 27.798
## 2:      IOS     3808 196665       22833               6.00        8.61 51.645
## 3:      All     7228 291734       35585               4.92        8.20 40.362
```


### level 4 (UV) {-}

Сделайте предыдущее задание, только добавьте разбивку по полю `media_source` из таблицы инсталлов (для сопоставления нужен `user_pseudo_id`). Имейте в виду, в `payments` все платежи, а нам нужны только по тем пользователям, кто установил приложение (т.е. есть в таблице `installs`)

Пропущенные значения и `other` в поле `media_source` перекодируйте в `organic`.
Аналогично, добавьте `total`.


```
##         media_source n_payers  gross n_purchases purchases_per_user purchase_mn
## 1:           organic     1329  60673        6742               5.07        9.00
## 2:      unityads_int      273   7783        1176               4.31        6.62
## 3:      applovin_int     1190  51544        6203               5.21        8.31
## 4:      Facebook Ads       54   1051         118               2.19        8.91
## 5: googleadwords_int      169   5367         643               3.80        8.35
## 6:               All     3015 126418       14882               4.94        8.49
##     ARPPU
## 1: 45.653
## 2: 28.510
## 3: 43.314
## 4: 19.468
## 5: 31.756
## 6: 41.930
```


### level 5 (N) {-}

Возьмите пользователей, которые пришли в июне.
Оставьте только те платежи, которые были сделаны в период первых 30 дней от инсталла (pay_dt - install_dt < 30). Метрика количества дней от инсталла называется лайфтайм (`lifetime`). 

Создайте таблицу, в которой будет всего пользователей, установивших приложение, все из монетизационные метрика (из level 2) + доля платящих от общего числа пользователей (`conversion`) и средний платеж каждого пользователя когорты, независимо, платящий он или нет (`ARPU`).

У меня в результатах почищены данные по инсталлам.


```
##         media_source total_users n_payers    gross conversion ARPU  ARPPU
## 1:      Facebook Ads        1297       53   884.43      0.041 0.68 16.687
## 2:      applovin_int       36714     1115 38133.02      0.030 1.04 34.200
## 3: googleadwords_int        7767      160  4302.90      0.021 0.55 26.893
## 4:           organic       43070     1000 38867.72      0.023 0.90 38.868
## 5:      unityads_int       21932      263  6099.07      0.012 0.28 23.190
##    n_purchases purchases_per_user purchase_mn
## 1:         101               1.91        8.76
## 2:        4529               4.06        8.42
## 3:         509               3.18        8.45
## 4:        4336               4.34        8.96
## 5:         969               3.68        6.29
```






