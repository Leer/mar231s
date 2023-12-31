# HW 2  {-}

## Общие замечания  {-}

- Срок сдачи работы: 26 декабря 2023 включительно.

- Домашнее задание должно быть выполнено в виде R-скрипта или Rmd-скрипта c чанками, кому что удобнее. 

- Свой файл с кодом решения назовите по структуре `mar231_hw2_<ваша фамилия латиницей>.Rmd` или `mar231_hw2_<ваша фамилия латиницей>.R` и пришлите в личных сообщениях в slack.  

- Старайтесь комментировать каждую значимую строчку кода (т.е., в которой происходит сложное или не очень прозрачное преобразование). Комментарии нужны, в первую очередь, для того, чтобы вы могли продемонстрировать, что понимаете, что и зачем делаете. Если некоторые операции однозначны и очевидны, комментарии можно опустить. В частности, при подключении пакетов можно ограничиться одним комментарием ко всем командам подключения пакетов. Если используете какое-то выражение или функцию, которое нагуглили, объясните, зачем и приложите ссылку.

- Соблюдайте [гайд](http://adv-r.had.co.nz/Style.html) по стилю оформления кода и/или используйте автоформатирование RStudio (ctr+shift+A на выделенном коде для Win/*nix). Отсутствие комментариев, неопрятность и/или нечитаемость кода, несоблюдение конвенций гайда по стилю --- на все это я буду обращать внимание и, в случае существенных помарок, снижать оценку на один балл.

- Выполняйте задание самостоятельно. Если у меня возникнут затруднения в объективной оценке, то договоримся о созвоне и я попрошу прокомментировать то или иное решение, или же дам небольшое задание из аналогичных, чтобы сравнить стиль решения.

- Если при выполнении задания все же возникнут какие-то вопросы - можете спросить меня (все вопросы в слаке: либо в личке, либо в канале #random). Не гарантирую, что отвечу максимально подробно, но дать минимальную подсказку или прояснить неясность задания постараюсь. 

- Все задания основаны на уже пройденных нами материалах, ничего запредельно сложного для вас быть не должно. Функции, примеры и алгоритмы можно найти на сайте в материалах. Если вы не знаете, как подступиться к задаче - попробуйте ее разложить на подзадачи, цепочку операций.

- Для того, чтобы не писать полный адрес к файлам данных, файлы должны лежать в той же папке, что и R/Rmd-файл, который использует эти файлы. Тогда импортировать можно без указание пути, просто через указание названия файла и его расширения. Например, my_fun_import`('file_to_import.csv')`

- Я рассчитываю, что вы будете использовать в работе какой-то один стиль синтаксиса - data.table (которому я вас учил), либо data.frame / tidyverse - если вы в них чувствуете себя комфортно. Пожалуйста, если вы не планируете использовать data.table, сообщите мне это заранее. 

- Пожалуйста, избегайте зоопарка пакетов и стилей.




## Задание 1 {-}

Внимательно изучите документацию сайта https://swapi.dev. Найдите способ, как можно с сайта импортировать данные по героям саги. Всего героев 83 (точнее, 82, так как ссылка на 17 героя не работает, но это можно игнорировать). 

Импортируйте и представьте в виде таблицы под названием таблицу `sw_chars_table` информацию по героям. В колонках должны быть имя, пол, дата рождения, цвет волос, цвет кожи и родной мир персонажа (name, gender, hair_color, skin_color, birth_year, homeworld). Есть несколько вариантов решения, предпочтительнее использовать API сервиса, но можно и по-другому. Также решение можно сделать изящнее с помощью циклов и анонимных функций (например, lapply), как мы делали при скрапинге статей журнала.

API сервиса не требует ключей авторизации и т. д., что сильно упрощает работу. Главное, найти соответствующий метод в документации.

Пример таблицы (оформление можно не воспроизводить):

|name           |gender |hair_color |skin_color  |birth_year |homeworld                        |
|:--------------|:------|:----------|:-----------|:----------|:--------------------------------|
|Luke Skywalker |male   |blond      |fair        |19BBY      |https://swapi.dev/api/planets/1/ |
|C-3PO          |n/a    |n/a        |gold        |112BBY     |https://swapi.dev/api/planets/1/ |
|R2-D2          |n/a    |n/a        |white, blue |33BBY      |https://swapi.dev/api/planets/8/ |
|Darth Vader    |male   |none       |white       |41.9BBY    |https://swapi.dev/api/planets/1/ |
|Leia Organa    |female |brown      |light       |19BBY      |https://swapi.dev/api/planets/2/ |

## Задание 2 {-}
Замените в таблице `sw_chars_table` значения в поле `homeworld` с ссылки на нормальное значение. Либо добавьте отдельное поле `planet` с нормальным названием. Всего планет 60.

Стратегий решения может быть несколько. Постарайтесь выбрать наиболее лаконичное и прозрачное по логике.


|homeworld |name               |gender |hair_color  |skin_color |birth_year |
|:---------|:------------------|:------|:-----------|:----------|:----------|
|Tatooine  |Luke Skywalker     |male   |blond       |fair       |19BBY      |
|Tatooine  |C-3PO              |n/a    |n/a         |gold       |112BBY     |
|Tatooine  |Darth Vader        |male   |none        |white      |41.9BBY    |
|Tatooine  |Owen Lars          |male   |brown, grey |light      |52BBY      |
|Tatooine  |Beru Whitesun lars |female |brown       |light      |47BBY      |

## Задание 3 {-}

Напишите функцию, которая на вход принимает имя героя (полностью или частично) и возвращает информации по нему. Ответ может быть в виде списка, необязательно перерабатывать в табличный вид. Для пробы героев можно взять из выгруженного ранее списка. Для того, чтобы обработать пробел в имени, вам потребуется `URLencode()`.

Решение не должно включать в себя весь датасет по персонажам, которых собирали в первом задании (решайте так, будто первого задания не было).



Пример работы функции:

```r
search_hero('Obi-Wan Kenobi')
```

```
## $name
## [1] "Obi-Wan Kenobi"
## 
## $height
## [1] "182"
## 
## $mass
## [1] "77"
## 
## $hair_color
## [1] "auburn, white"
## 
## $skin_color
## [1] "fair"
## 
## $eye_color
## [1] "blue-gray"
## 
## $birth_year
## [1] "57BBY"
## 
## $gender
## [1] "male"
## 
## $homeworld
## [1] "https://swapi.dev/api/planets/20/"
## 
## $films
## $films[[1]]
## [1] "https://swapi.dev/api/films/1/"
## 
## $films[[2]]
## [1] "https://swapi.dev/api/films/2/"
## 
## $films[[3]]
## [1] "https://swapi.dev/api/films/3/"
## 
## $films[[4]]
## [1] "https://swapi.dev/api/films/4/"
## 
## $films[[5]]
## [1] "https://swapi.dev/api/films/5/"
## 
## $films[[6]]
## [1] "https://swapi.dev/api/films/6/"
## 
## 
## $species
## list()
## 
## $vehicles
## $vehicles[[1]]
## [1] "https://swapi.dev/api/vehicles/38/"
## 
## 
## $starships
## $starships[[1]]
## [1] "https://swapi.dev/api/starships/48/"
## 
## $starships[[2]]
## [1] "https://swapi.dev/api/starships/59/"
## 
## $starships[[3]]
## [1] "https://swapi.dev/api/starships/64/"
## 
## $starships[[4]]
## [1] "https://swapi.dev/api/starships/65/"
## 
## $starships[[5]]
## [1] "https://swapi.dev/api/starships/74/"
## 
## 
## $created
## [1] "2014-12-10T16:16:29.192000Z"
## 
## $edited
## [1] "2014-12-20T21:17:50.325000Z"
## 
## $url
## [1] "https://swapi.dev/api/people/10/"
```

## Альтернативное задание

Если какое-то из трех заданий выше вам не дается, вы можете вместо него сделать следующее задание:

Напишите функцию, которая принимает на вход название пакета в строковом виде, а на выходе возвращает табличку с колонками: 

 - package (название пакета), 
 - publish_date (дата публикации), 
 - version (версию пакета), 
 - reference_manual (ссылку на мануал). 
 
 Вся информация берется с страницы пакета на сайте CRAN. Список пакетов здесь: https://cran.r-project.org/web/packages/available_packages_by_name.html

В функции рекомендую использовать каноничную форму (canonical form) ссылки на пакет (ее пример можно найти в самом низу страницы каждого пакета).

Подсказка. Чтобы получить работающую ссылку на reference_manual, нужно склеить ссылку на страницу пакета в каноничной форме, '/' и ссылку на мануал из раздела `Documentation`.
Вместо ссылки в каноничной форме можно также поставить `https://cran.r-project.org/web/packages/`




```r
library(rvest)
library(data.table)

get_package_info('data.table')
```

```
##       package publish_date version
## 1: data.table   2023-12-08 1.14.10
##                                                reference_manual
## 1: https://CRAN.R-project.org/package=data.table/data.table.pdf
```
