# HW 1  {-}

## Общие замечания  {-}

- Срок сдачи работы: 26 ноября 2023 включительно.

- Домашнее задание должно быть выполнено в виде R-скрипта или Rmd-скрипта c чанками, кому что удобнее. 

- Свой файл с кодом решения назовите по структуре `mar231_hw1_<ваша фамилия латиницей>.Rmd` или `mar231_hw1_<ваша фамилия латиницей>.R` и пришлите в личных сообщениях в mattermost  

- Старайтесь комментировать каждую значимую строчку кода (т. е., в которой происходит сложное или не очень прозрачное преобразование). Комментарии нужны, в первую очередь, для того, чтобы вы могли продемонстрировать, что понимаете, что и зачем делаете. Если некоторые операции однозначны и очевидны, комментарии можно опустить. В частности, при подключении пакетов можно ограничиться одним комментарием ко всем командам подключения пакетов. Если используете какое-то выражение или функцию, которое нагуглили, объясните, зачем и приложите ссылку.

- Соблюдайте [гайд](http://adv-r.had.co.nz/Style.html) по стилю оформления кода и/или используйте автоформатирование RStudio (ctr+shift+A на выделенном коде для Win/*nix). Отсутствие комментариев, неопрятность и/или нечитаемость кода, несоблюдение конвенций гайда по стилю --- на все это я буду обращать внимание и, в случае существенных помарок, снижать оценку на один балл.

- Выполняйте задание самостоятельно. Если у меня возникнут затруднения в объективной оценке, то договоримся о созвоне и я попрошу прокомментировать то или иное решение, или же дам небольшое задание из аналогичных, чтобы сравнить стиль решения.

- Если при выполнении задания все же возникнут какие-то вопросы - можете спросить меня (все вопросы в mattermost: либо в личке, либо в канале ~random). Не гарантирую, что отвечу максимально подробно, но дать минимальную подсказку или прояснить неясность задания постараюсь. 

- Все задания основаны на уже пройденных нами материалах, ничего запредельно сложного для вас быть не должно. Некоторые моменты могут потребовать гугления, но это минимально.  Функции, примеры и алгоритмы можно найти на сайте в материалах. Если вы не знаете, как подступиться к задаче --- попробуйте ее разложить на подзадачи, цепочку операций.

- Для того, чтобы не писать полный адрес к файлам данных, файлы должны лежать в той же папке, что и R/Rmd-файл, который использует эти файлы. Тогда импортировать можно без указание пути, просто через указание названия файла и его расширения. Например, `my_fun_import('file_to_import.csv')`.

- Я рассчитываю, что вы будете использовать в работе какой-то один стиль синтаксиса - data.table (которому я вас учил), либо data.frame / tidyverse --- если вы в них чувствуете себя комфортно. Пожалуйста, если вы не планируете использовать data.table, сообщите мне это заранее. 

- Если вы увереннее чувствуете себя в Python, можете выполнить задание в нем (также скажите об этом заранее). Но тогда будьте готовы к тому, что я могу попросить созвониться и прокомментировать какие-нибудь выражения из вашего решения.

- Пожалуйста, избегайте зоопарка пакетов и стилей. Чем меньше используется пакетов и чем согласованнее по стилю и лаконичнее код -- тем лучше.

<!-- - В чанках с ggplot-графиками добавьте в чанк параметр fig.width=12, вот так: `{r, fig.width=12}`. В чанки с plotly - `{r, fig.width=9.5}` -->




## Задание 1. Дома в Игре престолов.  {-}

### импорт данных  {-}
Импортируйте датасет по персонажам `Game of Thrones`  (ссылка на файл: https://gitlab.com/hse_mar/mar221s/-/raw/master/data/character-predictions_pose.csv). 
Назовите таблицу `got_chars`




### чистка данных  {-}

Замените пустое название Дома (house == '') на `Unknown`.
<!-- Для того, чтобы изменить пустое название Дома (`house == ''`) используйте выражение (весь мой код в синтаксисе data.table): -->
<!-- got_chars[house == '', house := 'Unknown'] -->
<!-- Напишите в комментариях, как работает это выражение. -->



Удалите строки, в которых возраст отрицательный (age < 0).


### график численности кланов  {-}
Создайте из `got_chars` объект `got_chars_bar`, где будет статистика по количеству людей в клане, а также по количеству мужчин и женщин. 



Нарисуйте в ggplot2 или plotly барчарт по количеству членов в клане, возьмите первые 10 домов по численности, включая `'Unknown'`. Для того, чтобы на графике бары были отсортированы, сначала надо отсортировать датасет по убыванию по количеству членов в клане. После этого выполнить это выражение:

```r
# делаем название клана в две строки
got_chars_bar[, house := gsub('\\s+', '<br>', house)]

# задаем сортировку домов
got_chars_bar[, house := factor(house, levels = house)]
```

Если рисуете в plotly - добавьте ховер (hover) с информацией о названии клана, количестве членов в клане, количестве мужчин и количестве женщин в клане. Если используете ggplot2 --- добавьте какую-нибудь тему не по умолчанию и заголовки графика и осей.

*Дополнительно*: удалите кнопки `plotly` (Zoom/Autoscale и т.д.). Выполнение этого пункта на оценку не влияет.


```{=html}
<div class="plotly html-widget html-fill-item-overflow-hidden html-fill-item" id="htmlwidget-751102d1b4959b761122" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-751102d1b4959b761122">{"x":{"visdat":{"1dd49629b3c3":["function () ","plotlyVisDat"]},"cur_data":"1dd49629b3c3","attrs":{"1dd49629b3c3":{"x":{},"y":{},"text":{},"textposition":"none","hoverinfo":"text","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"bar"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"title":"Численность Домов в GoT","xaxis":{"domain":[0,1],"automargin":true,"title":"","type":"category","categoryorder":"array","categoryarray":["Unknown","Night's<br>Watch","House<br>Frey","House<br>Stark","House<br>Targaryen","House<br>Lannister","House<br>Greyjoy","House<br>Tyrell","House<br>Martell","House<br>Osgrey","Faith<br>of<br>the<br>Seven","House<br>Arryn","House<br>Hightower","House<br>Bracken","House<br>Botley","House<br>Bolton","House<br>Baratheon","House<br>Florent","Brave<br>Companions","House<br>Tully","House<br>Velaryon","House<br>Whent","Brotherhood<br>without<br>banners","House<br>Crakehall","House<br>Waynwood","House<br>Westerling","House<br>Clegane","Stone<br>Crows","House<br>Baratheon<br>of<br>Dragonstone","House<br>Redwyne","House<br>Royce","House<br>Seaworth","House<br>Swyft","House<br>Wylde","House<br>Brax","House<br>Drumm","House<br>Karstark","House<br>Swann","House<br>Mormont","House<br>Manderly","House<br>of<br>Loraq","House<br>Royce<br>of<br>the<br>Gates<br>of<br>the<br>Moon","House<br>Paege","House<br>Baelish","Alchemists'<br>Guild","House<br>Plumm","Brotherhood<br>Without<br>Banners","House<br>Webber","House<br>Dayne","House<br>Tallhart","House<br>Ryswell","House<br>Stokeworth","House<br>Beesbury","House<br>Redfort","House<br>Harlaw","House<br>Goodbrother","House<br>Mallister","House<br>Umber","House<br>Estermont","House<br>Haigh","House<br>Darry","House<br>Blackwood","House<br>Blackfyre","House<br>Ashford","House<br>Hollard","House<br>Connington","House<br>Crabb","Drowned<br>men","Second<br>Sons","House<br>Oakheart","House<br>Caswell","Kingsguard","Chataya's<br>brothel","House<br>Norcross","House<br>Vance<br>of<br>Atranta","House<br>Caron","House<br>Glover","House<br>Crane","House<br>Farwynd<br>of<br>the<br>Lonely<br>Light","Happy<br>Port","Kingswood<br>Brotherhood","House<br>Vance<br>of<br>Wayfarer's<br>Rest","House<br>Tarth","House<br>Darklyn","House<br>Fossoway<br>of<br>Cider<br>Hall","House<br>Ironmaker","House<br>Qorgyle","House<br>Ambrose","House<br>Manwoody","House<br>Corbray","House<br>Hornwood","House<br>Lefford","House<br>Strong","House<br>Lothston","House<br>Farring","House<br>Cassel","House<br>Vance","House<br>Hunter","House<br>Goodbrook","Blacks","House<br>Reed","House<br>Baratheon<br>of<br>King's<br>Landing","House<br>Yronwood","House<br>Kettleblack","House<br>Santagar","House<br>Stout","Stormcrows","House<br>Spicer","House<br>Staunton","House<br>Tarly","House<br>Costayne","Moon<br>Brothers","House<br>Frey<br>of<br>Riverrun","House<br>Sharp","House<br>Butterwell","House<br>Bulwer","House<br>Smallwood","House<br>Payne","House<br>Slynt","House<br>of<br>Galare","R'hllor","House<br>Morrigen","House<br>Grafton","House<br>Fossoway<br>of<br>New<br>Barrel","House<br>Penrose","House<br>Piper","House<br>Blackmont","House<br>Stackspear","House<br>Vypren","House<br>Marbrand","House<br>Hardyng","House<br>Allyrion","House<br>Locke","House<br>Mooton","House<br>Hewett","House<br>Uller","House<br>Humble","House<br>Fell","Burned<br>Men","Kingdom<br>of<br>the<br>Three<br>Daughters","House<br>Nayland","House<br>Rowan","House<br>Lonmouth","House<br>Lynderly","House<br>Heddle","House<br>Ryger","Faceless<br>Men","House<br>Hunt","House<br>Dalt","House<br>Charlton","House<br>Blacktyde","Black<br>Ears","House<br>Codd","House<br>Willum","House<br>Uffering","House<br>Clifton","House<br>Blackberry","House<br>Penny","House<br>Serry","House<br>Lorch","House<br>Meadows","House<br>Mullendore","House<br>Inchfield","House<br>Kenning<br>of<br>Harlaw","House<br>Shepherd","House<br>Wynch","House<br>Belmore","House<br>Brune<br>of<br>Brownhollow","House<br>Rosby","House<br>Cuy","House<br>Cerwyn","House<br>Vaith","House<br>Prester","House<br>Greenfield","House<br>Peake","Good<br>Masters","House<br>Sunglass","House<br>Wull","House<br>Lydden","House<br>Kenning<br>of<br>Kayce","House<br>of<br>Pahl","House<br>Flint<br>of<br>Widow's<br>Watch","House<br>Selmy","House<br>Jordayne","Iron<br>Bank<br>of<br>Braavos","House<br>Wythers","House<br>Norrey","House<br>Thorne","House<br>Flint","House<br>Wells","House<br>Toyne","House<br>Hetherspoon","House<br>Tollett","House<br>Poole","Golden<br>Company","Unsullied","House<br>Merryweather","House<br>Wode","wildling","House<br>Farrow","House<br>Reyne","House<br>Weaver","House<br>Harclay","House<br>Lannister<br>of<br>Lannisport","Antler<br>Men","House<br>Stonetree","House<br>Leygood","House<br>Sparr","House<br>Varner","Peach","Sea<br>watch","Band<br>of<br>Nine","House<br>Bushy","House<br>Cafferen","House<br>Buckler","House<br>Conklyn","House<br>Greenhill","House<br>Deddings","House<br>Risley","Pureborn","House<br>Gaunt","House<br>Grandison","House<br>Sawyer","House<br>Bolling","House<br>Cupps","House<br>Tawney","Windblown","House<br>Roote","House<br>Hardy","Queensguard","House<br>Dondarrion","House<br>Yew","House<br>Mertyns","House<br>Boggs","House<br>Woods","House<br>Pemford","House<br>Staedmon","City<br>Watch<br>of<br>King's<br>Landing","House<br>Cockshaw","House<br>Graceford","House<br>Jast","House<br>Celtigar","House<br>of<br>Ghazeen","House<br>Byrch","House<br>Hawick","House<br>Broom","House<br>Harlaw<br>of<br>Harridan<br>Hill","House<br>Shett<br>of<br>Gull<br>Tower","House<br>Bar<br>Emmon","House<br>Norridge","House<br>Hayford","House<br>Brune<br>of<br>the<br>Dyre<br>Den","House<br>Fowler","House<br>Gower","House<br>Borrell","Citadel","Wise<br>Masters","House<br>Grimm","The<br>Citadel","House<br>Mollen","House<br>Hoare","House<br>Rambton","House<br>Harlaw<br>of<br>the<br>Tower<br>of<br>Glimmering","House<br>Wagstaff","House<br>Vyrwel","House<br>Bettley","House<br>Myre","House<br>Turnberry","House<br>Blackbar","House<br>Woolfield","House<br>Fossoway","House<br>Mallery","House<br>Chyttering","House<br>Lychester","House<br>Vikary","House<br>Volmark","House<br>Merlyn","House<br>Sarsfield","House<br>of<br>Merreq","House<br>Chester","House<br>Goodbrother<br>of<br>Shatterstone","House<br>Toland","House<br>Foote","House<br>Chelsted","House<br>Banefort","House<br>Ball","House<br>Cox","House<br>Ruttiger","House<br>Estren","House<br>Rykker","House<br>Longwaters","House<br>Moreland","House<br>Hogg","House<br>Longthorpe","House<br>Coldwater","House<br>Leek","House<br>Farman","House<br>Harlaw<br>of<br>Harlaw<br>Hall","House<br>Templeton","House<br>Liddle","House<br>Gargalen","House<br>Mudd","House<br>Farwynd","House<br>Sunderland","House<br>Wayn","Maesters","House<br>Blanetree","House<br>Blount","Company<br>of<br>the<br>Cat","House<br>Suggs","Khal","House<br>Nymeros<br>Martell","House<br>Drinkwater","House<br>Harlaw<br>of<br>Grey<br>Garden","Summer<br>Islands","House<br>Condon","House<br>Lannister<br>of<br>Casterly<br>Rock","House<br>Moore","House<br>Trant","House<br>Yarwyck","Undying<br>Ones","House<br>Stonehouse","House<br>Bolton<br>of<br>the<br>Dreadfort","Thenn","House<br>Hasty","House<br>Cole","Graces","House<br>Tyrell<br>of<br>Brightwater<br>Keep","House<br>Dayne<br>of<br>High<br>Hermitage","House<br>Strickland","House<br>Bywater","House<br>Massey","Brotherhood<br>without<br>Banners","House<br>Rhysling","House<br>Potter","House<br>Horpe","House<br>of<br>Kandaq","House<br>of<br>Reznak","House<br>Peckledon","House<br>Dustin","Mance<br>Rayder","House<br>Egen","House<br>Errol","Thirteen","House<br>Erenford","House<br>Grell","brotherhood<br>without<br>banners","Three-eyed<br>crow"]},"yaxis":{"domain":[0,1],"automargin":true,"title":""},"hovermode":"closest","showlegend":false},"source":"A","config":{"modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false,"displayModeBar":false},"data":[{"x":["Unknown","Night's<br>Watch","House<br>Frey","House<br>Stark","House<br>Targaryen","House<br>Lannister","House<br>Greyjoy","House<br>Tyrell","House<br>Martell","House<br>Osgrey"],"y":[427,105,97,72,59,49,41,36,28,21],"text":["house = Unknown<br>total members = 427<br>males = 245<br>females = 182","house = Night's<br>Watch<br>total members = 105<br>males = 79<br>females = 26","house = House<br>Frey<br>total members = 97<br>males = 54<br>females = 43","house = House<br>Stark<br>total members = 72<br>males = 49<br>females = 23","house = House<br>Targaryen<br>total members = 59<br>males = 31<br>females = 28","house = House<br>Lannister<br>total members = 49<br>males = 31<br>females = 18","house = House<br>Greyjoy<br>total members = 41<br>males = 29<br>females = 12","house = House<br>Tyrell<br>total members = 36<br>males = 18<br>females = 18","house = House<br>Martell<br>total members = 28<br>males = 10<br>females = 18","house = House<br>Osgrey<br>total members = 21<br>males = 14<br>females = 7"],"textposition":["none","none","none","none","none","none","none","none","none","none"],"hoverinfo":["text","text","text","text","text","text","text","text","text","text"],"type":"bar","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```
  

## Задание 2. Агрессивность персонажей в книгах о Гарри Поттере.   {-}
Импортируйте файл [aggressive_actions.xlsx](https://gitlab.com/hse_mar/mar221s/-/raw/master/data/aggressive_actions.xlsx). Вам потребуются данные с листа `DataSet`. Объект с данными с листа назовите `actions`.

По возможности напишите код так, чтобы исключить ручное скачивание файла на диск и потом импорт в R, т.е. сделать все средствами R.



В датасете есть орфографическая ошибка, `Ron Wealsey` вместо `Ron Weasley`. Исправьте это.


Отсортируйте по убыванию колонки `tot`, и возьмите первые 15 строк. Выполните следующие два выражения. Попробуйте проинтерпретировать смысл этих действий, что значат аргументы функций (на оценку наличие/корректность интерпретации не влияет).

```r
actions[, names_ordered := gsub('\\s+', '\n', name)]
actions[, names_ordered := factor(names_ordered, levels = names_ordered)]
```

Воспроизведите график ниже. Цвет заливки - `grey90`, цвет контура - `grey85`. Каждый бар должен быть подписан (количество действий), значение должно размещаться на области бара, чуть ниже, но не касаясь верхней границы бара, цвет текстовых меток - `grey30`. Выделите цветом `darkred` метку количества действий персонажей Золотого Трио (`Harry`, `Hermione Granger`, `Ron Weasley`). Выделите цветом `darkblue` метку количества действий тварей (`creature == 1`). Заголовок в две линии (вспомните про разделители линий в операционных системах, мы про это в ETL-блоке говорили), ориентация по центру.

Добавьте на график и отформатируйте `caption`. Содержание подписи - ваша фамилия, курс и тип графика, по аналогии с `mar231_hw1_upravitelev: barchart`. Параметры подписи: `face = "italic", color = 'grey60'`, смещения задайте самостоятельно.

<img src="HW1_files/figure-html/unnamed-chunk-11-1.png" width="768" />

## Задание 3. Динамика среднегодовой температуры в период 1750-2015  {-}
Импортируйте файл global_temperature.csv: https://gitlab.com/hse_mar/mar221s/-/raw/master/data/global_temperature.csv. 
По возможности исключите ручное скачивание файла на диск.

Создайте из даты `dt` переменную года (назовите ее `year`). Для этого можно воспользоваться функцией `year()` пакета `data.table`, или же любым другим методом на ваше усмотрение. Подсчитайте среднюю температуру в год (колонка температуры `LandAverageTemperature`). У вас должен получиться файл с колонками `year` и `temp_mn`, где `temp_mn` - средняя температура за год.



Воспроизведите график среднегодовой температуры. Добавьте линии трендов (методы `loess` и `lm`). Прозрачность диапазона равна 0.1, цвета - `darkred` и `darkblue`. Ваш график не должен вводить в заблуждение по поводу величины вариации между разными годами.

<img src="HW1_files/figure-html/unnamed-chunk-13-1.png" width="576" />

Корректное выполнение этого задания в `plotly` --- плюс 1 балл к оценке (можно без `caption`, только график и линии трендов). Функцией `ggplotly()` пользоваться нельзя.
