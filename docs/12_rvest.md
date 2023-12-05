# Web-скрапинг {-}
## Запись занятия {-}

<iframe width="560" height="315" src="https://www.youtube.com/embed/rKZm_RoFJM8?si=2w9TiErvbQJJEyXz" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## HTML интро {-}

### Web-сервер {-}

При попытке открыть каrую-нибудь html-страницу (сайт) происходит запрос к web-серверу, на котором размещена эта страница. Если не возникает ошибок, то сервер возвращает сообщение с кодом страницы (html + javascript). Соответственно, когда браузер получает сообщение, он обрабатывает и выполняет код, "отрисовывает страницу" для пользователя. Инструменты скрапинга (rvest, в частности), аналогично запрашивают web-сервер, однако результат не отрисовывают, а хранят в виде объекта в памяти.
 
<br>

### Структура html-страницы {-}

Все страницы сайтов - это текст, форматированный с помощью языка разметки html. Это значит, что все элементы страницы - тексты, картинки, гиперссылки, заголовки и проч. имеют свою метку, иначе говоря, тэг. Тэги - ключевые слова, обернутые в угловые скобки `<tag_name>`. Тэги всегда представлены в паре открывающий (`<tag_name>`) и закрывающий тэги (`</tag_name>`). Между ними как раз и размещается материал страницы - текст, картинка и проч. 

Стандартная html-страница имеет вид иерархического дерева тэгов, где корневой тэг - `html`. Ниже дан простейший пример кода страницы,в которой есть заголовок и список. Список формируется из общего тэга `<ol></ol>` (ordered list) и элементов списка внутри тэга, каждый в собственной парке тэгов `<li></li>`.

У тэгов есть атрибуты - как правило, это либо дополнительная информация (адрес гиперссылки, адрес картинки), либо класс, который используется в таблицах стилей, чтобы к разным тегам применять разные визуальные эффекты (о таблице стилей ниже). Например, чтобы текст во всех тэгах с классом `my_class` был синего, а с классом `my_class2` - зеленым. Атрибуты тэгов пишутся сразу после названия открывающего тэга, в угловых скобках: `<div class="my_class1"></div>`.

Пример кода страницы:

```js
<!DOCTYPE HTML>
<html>
  <head>
    <link rel="stylesheet" href="./data/my_css.css">
  </head>  
  <body>
    <h2>Правда о лосях</h2>
    <ol>
      <li style='color:red'>Лось — животное хитрое</li>
      <li class='my_class'>...и коварное!</li>
      <li class='my_class2'>а вот <a href="https://ru.wikipedia.org/wiki/%D0%9B%D0%BE%D1%81%D0%B8">ссылка</a> на страницу в вики</li>
    </ol>
    <h2>Второй заголовок уровня h2</h2>
    <p class='my_class'>
      Ло́с] (лат. Alces) — род парнокопытных млекопитающих, самые крупные представители семейства оленевх]. Включает 2 вида: европейский лось (лат. Alces alces) и американский лось (лат. Alces americanus), которые имеют разное число хромосом (68 и 70, соответственно)</p>
    <h2>а это просто пример таблицы</h2>
      <table>
        <tr>
          <th>cell</th>
          <th>colour</th>
        </tr>
        <tr>
          <td>1st</td>
          <td>white</td>
        </tr>
        <tr><td>2nd</td><td>red</td></tr>
        <tr><td>3rd</td><td>black</td></tr>
        <tr><td>4th</td><td class="my_class2">green</td></tr>
      </table>
  </body>
</html>
```

Вот так выглядит эта страница в том виде, как ее присылает веб-сервер:
```
<pre><!DOCTYPE HTML><html><head><link rel="stylesheet" href="./data/my_css.css"></head><body><h2>Правда о лосях</h2><ol><li style='color:red'>Лось — животное хитрое</li><li class='my_class'>...и коварное!</li><li class='my_class2'>а вот <a href="https://ru.wikipedia.org/wiki/%D0%9B%D0%BE%D1%81%D0%B8">ссылка</a> на страницу в вики</li></ol><h2>Второй заголовок уровня h2</h2><p class='my_class'>Ло́с] (лат. Alces) — род парнокопытных млекопитающих, самые крупные представители семейства оленевх]. Включает 2 вида: европейский лось (лат. Alces alces) и американский лось (лат. Alces americanus), которые имеют разное число хромосом (68 и 70, соответственно)</p><h2>а это просто пример таблицы</h2><table><tr><th>cell</th><th>colour</th></tr><tr><td>1st</td><td>white</td></tr><tr><td>2nd</td><td>red</td></tr><tr><td>3rd</td><td>black</td></tr><tr><td>4th</td><td class="my_class2">green</td></tr></table></body></html></pre>
```

Вот так выглядит эта страница в браузере:
<pre><!DOCTYPE HTML><html><head><link rel="stylesheet" href="./data/my_css.css"></head><body><h2>Правда о лосях</h2><ol><li style='color:red'>Лось — животное хитрое</li><li class='my_class'>...и коварное!</li><li class='my_class2'>а вот <a href="https://ru.wikipedia.org/wiki/%D0%9B%D0%BE%D1%81%D0%B8">ссылка</a> на страницу в вики</li></ol><h2>Второй заголовок уровня h2</h2><p class='my_class'>Ло́с] (лат. Alces) — род парнокопытных млекопитающих, самые крупные представители семейства оленевх]. Включает 2 вида: европейский лось (лат. Alces alces) и американский лось (лат. Alces americanus), которые имеют разное число хромосом (68 и 70, соответственно)</p><h2>а это просто пример таблицы</h2><table><tr><th>cell</th><th>colour</th></tr><tr><td>1st</td><td>white</td></tr><tr><td>2nd</td><td>red</td></tr><tr><td>3rd</td><td>black</td></tr><tr><td>4th</td><td class="my_class2">green</td></tr></table></body></html></pre>

### css {-}

Если посмотреть внимательно, то в разделе <head></head> есть подключение css-файла. Это так называемый файл стилей, который позволяет задавать визуальные параметры содержимого тэга не в инлайн-атрибутах (например, `style='color:red'`), а отдельно. И, соответственно, применять эти стили в самым разным тэгам, при необходимости. В примере выше заголовки все желтого цвета, а параграф и элемент упорядоченного списка с `class="my_class"` -- синего.

Заголовок h2 закомментирован, чтобы не покрасить выбранным цветом все h2-заголовки на странице (в том числе и заголовки конспекта).

Содержимое файла my_css.css:

```js
.my_class {
  color: blue;
  font-size: 20;
}

.my_class2 {
  color: green;
}
```

## rvest {-}

### Общая последовательность {-}
Последовательность действий при скрапинге данных с помощью `rvest` такова:

- с помощью функции `read_html()` в рабочее окружение импортируется заданая страница (web-адрес) или XML-документ

- визуально по структуре или визуально в средствах web-разработчика (в частности, SelectorGadget и нативные инструменты браузеров для web-разрботчиков), а так же функциями `html_children()`, `html_attrs()` анализируется структура дерева элементов документа (тэгов и атрибутов)

- с помощью функций `html_element()` и `html_elements()` указывается, какой элемент (элементы) структуры необходимо выбрать

- из выбранного элемента с помощью функций `html_text()`, `html_table` и `html_attr()` извлекаются значения

- при необходмости во время сбора данных с html-страниц процесс масштабируется на прочие схожие разделы сайта или осуществляется в регулярном формате (краулинг)
 
<br>

### импорт страницы {-}
Установка и подключение пакета стандартные. На *nix/MacOS-системах могут потребоваться дополнительные библиотеки. Для импорта таблицы требуется `read_html()`, при импорте может быть необходимо указать кодировку.


```r
library(data.table)
# install.packages('rvest')
library(rvest)


# импорт страницы
url <- read_html('./data/tmp.html', encoding = 'UTF-8')
```
 
<br>

### Выбор узлов и их значений {-}
Для того, чтобы извлечь значение из какого-то тэга, необходимо указать путь к этому тэгу в дереве тэгов страницы. Выбор тэгов осуществляется с помощью функции `html_element()`, где первым аргументом указывают импортированную страницу, а в аргументе `xpath` - путь к тэгу на языке XPath.

В тэгах может быть разная информация - текст или таблица, в некоторых случаях важно не содержание тэга, а его атрибут. После того, как выделяется из дерева тэг, необходимо извлечь его содержание. Это делается с помощью функций:

- `html_text()` если в тэге хранится текстовое значение. По сути, используется всегда, когда в тэге не табличка и не картинка.

```r
# извлекаем текстовое значение - заголовок
html_element(url, xpath = '//h2') %>%
  html_text()
```

```
## [1] "Правда о лосях"
```
<br>
- `html_table()` если в тэге хранится таблица (`<table>`).

```r
# извлекаем таблицу
html_element(url, xpath = '//table') %>%
  html_table()
```

```
## # A tibble: 4 × 2
##   cell  colour
##   <chr> <chr> 
## 1 1st   white 
## 2 2nd   red   
## 3 3rd   black 
## 4 4th   green
```

<br>
- `html_attr('attr_name')` - используется для случаев, когда надо извлечь атрибут тэга. Атрибуты тэга можно узнать, применив к выделенному с помощью `html_element()` тэгу функцию `html_attrs()`.

```r
# смотрим атрибуты тэга <a>
html_element(url, xpath = '//a') %>%
  html_attrs()
```

```
##                                                     href 
## "https://ru.wikipedia.org/wiki/%D0%9B%D0%BE%D1%81%D0%B8"
```

```r
# извлекаем значение атрибута href
html_element(url, xpath = '//a') %>%
  html_attr('href')
```

```
## [1] "https://ru.wikipedia.org/wiki/%D0%9B%D0%BE%D1%81%D0%B8"
```

```r
# извлекаем не атрибут, а значение тэга, 
# то есть, каким текстом видит эту ссылку пользователь
html_element(url, xpath = '//a') %>%
  html_text()
```

```
## [1] "ссылка"
```

 
<br>

## Навигация по дереву тегов {-}

Ключевые элементы языка XPath:

- `%tag_name%`:	название тэга `%tag_name%`

- `/`: Иcпользуется для указания прямого пути к элементу от корневого/родительского тега. Если весь xpath-путь начинается с `/`, то это абсолютный путь от корня дерева. Вот так можно выбрать тэг (ноду, узел) заголовка страницы. 


```r
# длинный путь, от корня через все поколения родителей к тэгу h2
html_element(url, xpath = '/html/body/h2') %>%
  html_text()
```

```
## [1] "Правда о лосях"
```

- `//`: Выбирает теги независимо от места их расположения, при этом может использоваться как в начале пути, так и в середине:

```r
# короткий путь, "пропусти все до первого тэга h2"
html_element(url, xpath = '//h2') %>%
  html_text()
```

```
## [1] "Правда о лосях"
```

- `@`: выбор атрибута узла. Если необходимо использовать атрибут тэга в пути, то атрибут указывается как `@attr_name`. Если указать путь к атрибуту тэга с использованием `@`, то можно не использовать функцию `html_attr()`, а обойтись общей `html_text()`:


```r
html_element(url, xpath = '//a/@href') %>%
  html_text()
```

```
## [1] "https://ru.wikipedia.org/wiki/%D0%9B%D0%BE%D1%81%D0%B8"
```


- `[x]`: фильтрация тэга с помощью номера места в списке тэгов-сиблингов (на одном уровне в иерархии) или по наличию какого-либо атрибута. В целом, очень похоже на фильтрацию таблиц по номеру строки или логическому условию.



```r
### выбираем по номеру тэга в списке 
# первый заголовок h2
html_element(url, xpath = '//h2[1]') %>%
  html_text()
```

```
## [1] "Правда о лосях"
```

```r
# третий заголовок h3
html_element(url, xpath = '//h2[3]') %>%
  html_text()
```

```
## [1] "а это просто пример таблицы"
```

```r
### выбираем по содержанию определенного атрибута
# выбираем значение тэга <p>, у которого есть атрибут класса class="my_class"
html_element(url, xpath = '//p[@class="my_class"]') %>%
  html_text()
```

```
## [1] "\n      Ло́с] (лат. Alces) — род парнокопытных млекопитающих, самые крупные представители семейства оленевх]. Включает 2 вида: европейский лось (лат. Alces alces) и американский лось (лат. Alces americanus), которые имеют разное число хромосом (68 и 70, соответственно)"
```

```r
# выбираем значение тэга <p>, у которого есть атрибут класса class="my_class"
# и используем html_text2(), чтобы убрать нечитаемые символы
html_element(url, xpath = '//p[@class="my_class"]') %>%
  html_text2()
```

```
## [1] "Ло́с] (лат. Alces) — род парнокопытных млекопитающих, самые крупные представители семейства оленевх]. Включает 2 вида: европейский лось (лат. Alces alces) и американский лось (лат. Alces americanus), которые имеют разное число хромосом (68 и 70, соответственно)"
```

- `*`: знак подстановки, "на этом месте может быть что угодно" - используется, когда надо пропустить какой-то длинный кусок дерева тэгов, или точно неизвестно, какой тэг или атрибут используется:


```r
# не знаем тэг, знаем атрибут класс, вместо тэга пишем *
html_element(url, xpath = '//*[@class="my_class"]') %>%
  html_text()
```

```
## [1] "...и коварное!"
```

```r
# знаем тэг, но не знаем, какой атрибут со значением "my_class"
html_element(url, xpath = '//p[@*="my_class"]') %>%
  html_text()
```

```
## [1] "\n      Ло́с] (лат. Alces) — род парнокопытных млекопитающих, самые крупные представители семейства оленевх]. Включает 2 вида: европейский лось (лат. Alces alces) и американский лось (лат. Alces americanus), которые имеют разное число хромосом (68 и 70, соответственно)"
```

## Циклы {-}
Наряду с условными операторами, циклы в R аналогичны циклы в других языках программирования. Три основных вида циклов - `for`, `while` и `repeat`. Циклы задаются с помощью оператора цикла, последовательности или условия, ограничивающих работу цикла и, собственно, выполняемого выражения. Если это одна строка, то выражение можно не заключать в фигурные скобки, во всех прочих случаях фигурные скобки необходимы.

Циклы традиционно редко используются в R, в немалой части это вызвано спецификой использования памяти во время выполнения выражений в цикле, точнее, не очень эффективным кодом. Для циклов существуют альтернативы - векторизованные вычисления и неявные циклы, а так же, собственно оптимизация кода путем преаллокации памяти или параллелизации.
<br>


### for  {-}
Цикл `for`, который используется чаще прочих, использует заданную последовательность, по которой и итерируется. Последовательностью может быть как численный ряд, так и строковый вектор, например, названий файлов при импорте и обработке большого количества файлов в одном цикле. После выполнения цикла используемый итератор имеет значение последнего элемента цикла. В том же случае, если последовательность нулевой длины, то цикл не отрабатывает. 


```r
for (i in letters[1:3]) {
  cat('letter', i, '\n')
}
```

```
## letter a 
## letter b 
## letter c
```

```r
cat('i =', i)
```

```
## i = c
```
<br>


### while (Advanced) {-}
Циклы `while` и `repeat` используются намного реже. Если в цикле `for` количество циклов определяется длиной заданной последовательности, то в `while` количество циклов может быть бесконечным, до тех пор, пока поставленное условие будет верным. 

Для цикла надо задать начальное значение счетчика циклов, задать условие для этого счетчика и не забыть дополнить тело цикла увеличением счетчика при каждой итерации. Либо же добавить любое другое изменение значения счетчика, которое может привести срабатыванию условия. Второй вариант цикла `while` - это сначала создать объект с логическим значением `TRUE` и его поставить у условие, а потом прописать в теле цикла, что при определенных условиях значение сменится на `FALSE`, что и приведет к остановке цикла.

Выведем первые три элемента вектора `letters` с помощью цикла `while`.

```r
i <- 1
while (i < 4) {
  my_l <- letters[i]
  cat('letter', my_l, '\n')
  i <- i + 1
}
```

```
## letter a 
## letter b 
## letter c
```

```r
cat('i =', i)
```

```
## i = 4
```

Как правило, `while` нужен тогда, когда надо подсчитать количество попыток до какого-то результата, либо же неизвестно, сколько попыток модет потребоваться. Самый показательный пример - сбор данных с POST-запросом, когда сервер может не отвечать, соединение может рваться, и так далее.
<br>


### repeat  (Advanced) {-}
Цикл `repeat` схож с циклом `while`, только он выполняется до тех пор, пока пока при выполнении выражения не будет достигнут желаемый результат и не будет вызвана команда прерывания цикла. На том же примере с буквами:

```r
i <- 1
repeat {
    my_l <- letters[i]
    if (i == 4) {
      break()
    } else {
      cat('letter', my_l, '\n')
      i <- i + 1
    }
}
```

```
## letter a 
## letter b 
## letter c
```

```r
cat('i =', i)
```

```
## i = 4
```
<br>


### Прерывание циклов (Advanced) {-}
В какие-то моменты возникает необходимость прервать цикл или же пропустить последующеи действия и начать новую итерацию цикла. Для этих целей используют функции `break()` и `next()`. Выше в цикле `repeat` мы уже использовали `break()`, вот еще один пример цикла с прерыванием:

```r
for (i in letters[1:10]) {
  cat(i, '\n')
  if (i == 'c')
    break()
}
```

```
## a 
## b 
## c
```

Эффективнее всего функции прерывания в вложенных циклах - если прервать выполнение вложенного цикла, то родительский цикл не будет прерван и продолжит итерироваться. 
<br>

## Неявные циклы, семейство \*pply {-#eval-pply}

Циклами в их привычном большинству программистов виде в R пользуются не очень часто. Как правило, это ситуации, когда неизвестно количество возможных циклов или, наоборот, их ничтожное количество (несколько названий файлов). В большинстве же случаев пользуются так называемыми неявными циклами --- функциями семейства \*pply.

Самая распространённая и покрывающая большинство задач функция семейства --- это `lapply()`. Первый аргумент функции --- набор элементов, над которыми должно быть произведено какое-то действие. Буква `l` в названии маркирует, что функция обычно применяется к спискам (`l` = `list`), однако на практике используются и векторы, и списки и т. д. Фактически первый аргумент в функции `lapply()` схож с итератором в цикле `for`. Второй аргумент --- это собственно функция, которая должна быть применена к элементам вектора/списка из первого аргумента.

В качестве результата работы функция `lapply()` возвращает список, где каждый элемент --- результат применения указанной во втором аргументе функции к каждому элементу первого аргумента. В этом заключается одно из отличий от классических циклов, в которых тело цикла всего лишь повторяется определённое количество раз. То есть в классических циклах, в отличие от `lapply()`, нет возможности создать объект с результатами цикла, и надо изменять созданный за пределами цикла объект.


```r
res <- lapply(1:5, sqrt)
str(res)
```

```
## List of 5
##  $ : num 1
##  $ : num 1.41
##  $ : num 1.73
##  $ : num 2
##  $ : num 2.24
```

```r
unlist(res)
```

```
## [1] 1.000000 1.414214 1.732051 2.000000 2.236068
```

Если же у функции, используемой в `lapply()`, есть дополнительные аргументы, то они идут последующими аргументами, так как в списке аргументов `lapply()` заданы `...`, дополнительные аргументы.


```r
# создадим список из векторов
my_list <- list(el1 = c(1, 2, 3, 4), 
                el2 = c(1, 2, NA, 4))

# вычислим среднее и укажем, что NA надо пропускать
lapply(my_list, mean, na.rm = TRUE)
```

```
## $el1
## [1] 2.5
## 
## $el2
## [1] 2.333333
```

Несмотря на определённую гибкость и возможность указывать аргументы функции, чаще всего в `lapply()` используются анонимные функции, в которые в качестве аргумента при выполнении передаётся значения объекта, переданного в первый аргумент (по которому осуществляется итерирование).


```r
res <- lapply(1:5, function(x) {
  z <- x * 2
  z <- sqrt(z)
  z
})
str(res)
```

```
## List of 5
##  $ : num 1.41
##  $ : num 2
##  $ : num 2.45
##  $ : num 2.83
##  $ : num 3.16
```

```r
unlist(res)
```

```
## [1] 1.414214 2.000000 2.449490 2.828427 3.162278
```

Семейство \*pply-функций достаточно велико, вот наиболее часто используемые функции (тут я ориентируюсь на [*список*](https://r-analytics.blogspot.com/2012/11/r-apply.html) С. Мастицкого):

-   `lapply()`: `l` в названии означает `list`, список. Используется в случаях, когда необходимо применить какую-либо функцию к каждому элементу списка и получить результат также в виде списка. На деле обычно служит более удобным аналогом цикла `for`.

-   `sapply()`: `s` в названии означает `simplify`, упрощение. Работает как `lapply()`, только в результате отдаёт именованный вектор.

-   `apply()`: используется в случаях, когда необходимо применить какую-либо функцию ко всем строкам или столбцам матрицы (или массивов большей размерности).

-   `vapply()`: `v` в названии означает `velocity`, скорость. Аналогична `lapply()` и `sapply()`, однако в качестве ещё одного аргумента требует указать тип данных, которые должны быть получены в результате. Это несколько ускоряет работу функции, что и привело к такому названию.

-   `mapply()`: `m` в названии означает `multivariate`, многомерный. Используется в случаях, когда необходимо поэлементно применить какую-либо функцию одновременно к нескольким объектам (например, получить сумму первых элементов векторов, затему сумму вторых элементов векторов и т. д.).

-   `rapply()`: `r` в названии означает `recursively`, рекурсивно. Используется в случаях, когда необходимо применить какую-либо функцию к компонентам вложенного списка.

В настоящее время желательно использовать аналоги *pply-функций базового пакета из пакет `purrr`. Они чуть удобнее и чуть требовательнее к данным. Этот материал за пределами курса, но освоить рекомендую.

Некоторые пакеты имеют свои реализации неявных циклов, например, `parallel::mcmapply()`, которая часто используется для параллелизации кода.


## Скрапинг сайта журнала

### Получение информации о статье и авторе

Анализируем дерево тэгов на странице статьи, и указываем xpath-пути для получения той или иной информации. Сразу все оборачиваем в табличный вид.


```r
page_url <- 'https://ecsoc.hse.ru/2023-24-5/876469050.html'
page <- read_html(page_url)
article_info <- data.table(
  url = page_url,
  autor = html_element(page, xpath = '//h3/i') %>% html_text(),
  title = html_element(page, xpath = '//h2[@class="article-header"]') %>% html_text(),
  annotation = html_element(page, xpath = '//div[@class="annot"]') %>% html_text2(),
  keywords = html_element(page, xpath = '//div[@class="keywords"]') %>% html_text2(),
  doi = html_element(page, xpath = '//div[@class="keywords"][2]') %>% html_text2(),
  volume = html_element(page, xpath = '//div[@class="centercolumn"]/div') %>% html_text2()
)
```


## Получение списка ссылок с страницы рубрики

Мы видим, что на странице статьей рубрики дано до 10 ссылок на статьи, всего таких страниц более 15. Внимательный анализ DOM-дерева тэгов позволяет нам считать, что все ссылки находятся в интервале 3-12 div-тэги.


```r
# страница рубрики
rubric_url <- 'https://ecsoc.hse.ru/rubric/26590246.html'
rubric_page <- read_html(rubric_url)

# первая статья на странице
html_element(rubric_page, xpath = '//div[@class="centercolumn"]/div[3]/small/a[2]') %>% 
  html_attr('href')
```

```
## [1] "https://ecsoc.hse.ru/2023-24-5/876469050.html"
```

```r
# последняя статья на странице
html_element(rubric_page, xpath = '//div[@class="centercolumn"]/div[12]/small/a[2]') %>% 
  html_attr('href')
```

```
## [1] "https://ecsoc.hse.ru/2022-23-3/638089235.html"
```

Пишем короткий цикл, чтобы получить ссылки:


```r
# for-цикл, работа в глобальном окружении
page_urls <- NULL
for (i in 3:12) {
  art_xpath <- paste('//div[@class="centercolumn"]/div[', i, ']/small/a[2]', sep = '')
  tmp_page_url <- html_element(rubric_page, xpath = art_xpath) %>% 
    html_attr('href')    
  page_urls <- c(page_urls, tmp_page_url)
}
print(page_urls[1:3])
```

```
## [1] "https://ecsoc.hse.ru/2023-24-5/876469050.html"
## [2] "https://ecsoc.hse.ru/2023-24-4/861891244.html"
## [3] "https://ecsoc.hse.ru/2023-24-4/861891738.html"
```



```r
# lapply-цикл
res <- lapply(3:12, function(x) {
  art_xpath  <- paste('//div[@class="centercolumn"]/div[', x, ']/small/a[2]', sep = '')
  tmp_page_url <- html_element(rubric_page, xpath = art_xpath) %>% 
    html_attr('href')     
  return(tmp_page_url)
})
print(res[1:3])
```

```
## [[1]]
## [1] "https://ecsoc.hse.ru/2023-24-5/876469050.html"
## 
## [[2]]
## [1] "https://ecsoc.hse.ru/2023-24-4/861891244.html"
## 
## [[3]]
## [1] "https://ecsoc.hse.ru/2023-24-4/861891738.html"
```

Добавляем в цикл сбор данных по статье:


```r
res <- lapply(3:12, function(x) {
  art_xpath  <- paste('//div[@class="centercolumn"]/div[', x, ']/small/a[2]', sep = '')
  page_url <- html_element(rubric_page, xpath = art_xpath) %>% 
    html_attr('href')  
  page <- read_html(page_url)
  article_info <- data.table(
    url = page_url,
    autor = html_element(page, xpath = '//h3/i') %>% html_text(),
    title = html_element(page, xpath = '//h2[@class="article-header"]') %>% html_text(),
    annotation = html_element(page, xpath = '//div[@class="annot"]') %>% html_text2(),
    keywords = html_element(page, xpath = '//div[@class="keywords"]') %>% html_text2(),
    doi = html_element(page, xpath = '//div[@class="keywords"][2]') %>% html_text2(),
    volume = html_element(page, xpath = '//div[@class="centercolumn"]/div') %>% html_text2()
  )  
  return(article_info)
})
print(res[[1]])
```

```
##                                              url        autor
## 1: https://ecsoc.hse.ru/2023-24-5/876469050.html Шевчук А. В.
##                                                                      title
## 1: Теоретизируя цифровые платформы: концептуальная схема для гиг-экономики
##                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      annotation
## 1: Современная социология труда уделяет большое внимание изучению трудового опыта работников, занятых через цифровые платформы. Однако для более глубокого понимания проблем гиг-экономики этот подход необходимо дополнить анализом самих цифровых платформ как организационных структур и социальных акторов. В статье предлагается концептуальная схема подобного анализа, опирающаяся на теоретические наработки в области экономической социологии, институционализма и политэкономии.\nРоль цифровых платформ проблем атизируется с помощью пяти основных категорий (организационная инновация, фирма-медиатор, инфраструктура рынков, частный регулятор, институциональный предприниматель), которые последовательно включаются в анализ. Цифровые платформы представляют собой радикальную организационную инновацию, построенную на технологиях, способных эффективно координировать деятельность рассредоточенных агентов, не требуя их пространственного, темпорального и организационного соприсутствия. Это способствует развитию бизнеса, получающего выгоду от координации внешних по отношению к фирме работников и ресурсов. Предоставляемые платформами средства связи между экономическими агентами постепенно приобретают системный инфраструктурный характер, формируя базовые условия функционирования рынков. Имея возможность в одностороннем порядке устанавливать «правила игры» и осуществлять алгоритмическое управление, платформы превращаются в частных регуляторов рынков, конкурируя с государством. Чтобы укрепить и легитимировать свою власть, платформы активно вовлекаются в политический процесс с целью социальной реорганизации рынков и общей институциональной перестройки. На этом этапе концептуальная схема закольцовывается, возвращаясь к тому, что платформы представляют собой инновацию, в процессе диффузии которой должны быть разрешены наиболее острые социальные противоречия, связанные с бизнес-моделью фирмы-медиатора, её ролью как инфраструктуры, частного регулятора и институционального предпринимателя.\nВ статье продемонстрировано, как предложенные категории могут применяться к анализу различных проблем гиг-экономики.
##                              keywords                                  doi
## 1: Ключевые слова: цифровая экономика DOI: 10.17323/1726-3247-2023-5-11-53
##                                            volume
## 1: 2023. Т. 24. № 5. С. 11–53 [содержание номера]
```



<br>

## Дополнительные материалы  {-}

- Один из лучших [справочников](https://www.w3schools.com/html/default.asp) и туториалов по HTML. Есть примеры написания и использования тэгов. Тэги описаны по функционалу, есть [список](https://www.w3schools.com/tags/default.asp) тэгов

- [Справочник и туториалы](https://html5book.ru/html-tags/), как и w3s, но с некоторыми изменениями/дополнениями и на русском языке.

- [Короткое](https://msiter.ru/tutorials/uchebnik-xml-dlya-nachinayushchih/xml-i-xpath) введение в XPath. Сайт выглядит страшненько, да и примеры все на xml (хотя разница с html минимальна). Скорее, для тех, кто хочет глубже разобраться в особенностях XPath, так как содержит много излишне детальной информации.

- [Страница](https://purrr.tidyverse.org/) пакета `purrr` (для тех, кто хочет использовать современные функции для неявных циклов)

- [Оптимизация циклов в R](https://textbook.rintro.ru/performance.html#performance-loops) или "как надо правильно использовать for-циклы"

<br>

## Домашнее задание {-}

## level 1 (IATYTD) {.unnumbered}

- прочитайте дополнительные материалы. 

- разберите код получения информации о статьях с первой страницы рубрики

## level 2 (HNTR) {.unnumbered}

- научитесь получать, сколько страниц-списков статей есть в рубрике (список 12345...N внизу каждой страницы рубрики, нам нужно вот это N)

## level 3 (HMP) {.unnumbered}

Прочитайте справку по функции `list.files()` и/или `list.dirs()`. Импортируйте названия файлов в какой-нибудь из ваших папок (или названия подпапок). В цикле выведите на печать первые пять названий. Например:


```
## [1] "aggressive_actions.xlsx"
## [1] "artists.csv"
## [1] "artwork.csv"
## [1] "character-predictions_pose.csv"
## [1] "coronavirus_dt.csv"
```

## level 4 (UV) {.unnumbered}

Найдите максимальное значение по каждой колонке в таблице mtcars. Для каждой колонки выведите строку 'Максимальное значение по колонке xxx: zzz', где xxx и zzz - название колонки и максимальное значение соответственно.

Для пятой задачи вам потребуются вот такие конструкции:


```r
library(data.table)
mtcars <- as.data.table(mtcars)
names(mtcars)
```

```
##  [1] "mpg"  "cyl"  "disp" "hp"   "drat" "wt"   "qsec" "vs"   "am"   "gear"
## [11] "carb"
```

```r
# вариант 1. обращение к колонке по номеру
mtcars[1:3, 1]
```

```
##     mpg
## 1: 21.0
## 2: 21.0
## 3: 22.8
```

```r
# вариант 2. обращение к колонке по ее названию, которое хранится в другом объекте
i <- 'mpg'
mtcars[1:3, get(i)]
```

```
## [1] 21.0 21.0 22.8
```

## level 5 (N) {.unnumbered}

Решите получение информации о десяти статьях с первой страницы рубрики с помощью for-списка (у нас в решении используется lapply).
Постарайтесь сделать это решение оптимальным с точки зрения использования памяти.