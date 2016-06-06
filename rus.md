Кирпичная раскладка на чистом CSS, делаем раскладку в шахматном порядке без использования Javascript.
===

Один из популярных видов раскладки элементов - "кирпичная кладка". Не знакомы с такой? Вспомните Pinterest,  Metro в Windowsб и т.д. В двух словах: эффектное размещение блоков разного размера и с различным контентом, часто в шахматном порядку

Кирпичная раскладка далеко не новинка. Так зачем говорить о ней снова. Уже существует несколько отличных решений для организации кирпичной раскладки. Однако, можем ли мы получить решение CSS? Возможно  ли используя Flexbox получить желаемый эффект.

Для тех, кто  TL;DR - переходите сразу к коду, который я обьеденил здесь или здесь.

Базовый эффект
===

Давайте начнем с базовой раскладки. Давайте представим вот такую структуру DOM :
    <div class="masonry-layout">
      <div class="masonry-layout__panel">
        <div class="masonry-layout__panel-content">
          <-- CONTENT HERE -->
        </div>
      </div>
      <div class="masonry-layout__panel">
        <div class="masonry-layout__panel-content">
          <-- CONTENT HERE -->
        </div>
      </div>
      <div class="masonry-layout__panel">
        <div class="masonry-layout__panel-content">
          <-- CONTENT HERE -->
        </div>
      </div>
      <-- FOLLOWING CONTENT PANELS -->
    </div>

Нам необходимо разбить наш контент на колонки. Опираясь на нашу структуру DOM, можем ли мы получить  базовый эффект кирпичной кладки используя CSS3 правила для multi-column или Flexbox?

О правилах multi-column  и Flexbоx?
===
Оба правила появились в CSS3, оба - колоночная раскладка и Flexbоx, управляют способами раскладки (организации) контента. Оба свойства имеют много синонимов в названии. Свойство multi-column дает нам возможность организовать контент в виде колонок,  не прилагая особых усилий. Flexbox (flex в переводе гибкий) обеспечивает то, что вы подумали из названия, гибкие блоки для вашего лейаута, они могут сжиматься, увеличиваться и меняться так как вы этого хотите. Я не буду вдаваться в подробности  в этой статье,  так что рекомендую ознакомиться самостоятельно перед дальнейшим чтением.

Возможность Использовать мультиколоночный лейаут появилась в CSS3 вместе с спецификациями column-count и column-gap. Что дает возможность получить эффект "кирпичной кладки" довольно просто. Обратите внимание, что необходимо использовать правило break-inside для блоков контента внутри колонок. Это гарантирует нам, что содержимое блока не разделиться, и не распределиться между колонками. Начальный код CSS выглядит следующим образом.

    .masonry-layout {
      column-count: 3;
      column-gap: 0;
    }
    .masonry-layout__panel {
      break-inside: avoid;
      padding: 5px;
    }
    .masonry-layout__panel-content {
      padding: 10px;
      border-radius: 10px;
    }

Увидеть его в действии можо тут.  Это хорошее начало, показывающее, роказывающее, что мульти-колоночная раскладка, кготова взять на себя самую тяжелую часть лейаута.

Использование Flexbox.
===
Можем ли мы достичь подобного используя Flexbox? Это не так просто и однозначно , как использование мультиколоночной раскладки. Я обнаружил несколько проблем в своих попытках добиться такого используя Flexbox.

В первый раз, я попытался имитировать колонки используя свойство "flex-direction:column". Проблема с "column" заключается в том, что вам необходимо указать высоту колонок, чтобы колонки начали разбиваться. Но этого приводит к тому, что не помещающиеся блоки располагаются горизонтально, и содержимое начинает прокручиваться горизонтально. Так что вам нужно подбирать высоту так, чтобы у вас поместились все блоки, так, как вы этого хотели. В случае, если вы работаете с динамичным содержимым, размеры блоков могут меняться при каждом изменении содержимого, а вам придется подбирать высоту снова. Что идет в разрез с идеологией Flexbox, на мой взгляд.

Для Flexbox я использовал следующий код:

  .masonry-layout {
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    padding: 10px;
    height: 100vw;
  }
  .masonry-layout__panel {
    display: flex;
    flex: 1 1 auto;
    width: 33.3%;
    margin-bottom: 10px;
    border-radius: 10px;
  }

Это даст нам результат, который можно посмотреть здесь. Обратите внимание, что контент начал прокручиваться горизонтально, а мы этого не хотели.  Другой подход, это использование "flex-direction: row". Это тоже не особо работает. Возможно указать количество колонок, ограничив максимальную ширину элементов, но появляются пробелы, в зависимости от того, как вы укажете свойство "align-items". Совсем не то, что нам надо. Пример использования "flex-direction: row" и "align-items:start" можно увидеть здесь.

Если же вы хотите иметь больше контроля над разметкой и структурой страницы, то результат можно получить и используя flexbox. Например используя подобную структуру:

    <div class="masonry-layout">
      <div class="masonry-layout__column">
        <div class="masonry-layout__panel></div>
        <div class="masonry-layout__panel></div>
      </div>
      <div class="masonry-layout__column">
        <div class="masonry-layout__panel></div>
        <div class="masonry-layout__panel></div>
      </div>
    </div>

Добавля колонки в свою разметку, у вас будет немного больше контроля над разметкой. В любом случае, вам нужно хрошенько поразмыслить над тем, как и где будут находится элементы вашего контента. Увидеть подобную реализацию в действии можно здесь.

Использование свойства multi-column в разметке.
===

Для поолучения базовой "кирпичной кладки", свойство multi-column подходит болше всего, на мой взгляд. Мое мнение основывается на простоте и минимальной разметке. И все работает в итоге. Если базовой "кладки" вам достаточно, то нижепреведенный код - именно то, что вам нужно. В любом случае, у каждого из подходов есть свои преимущества.

    .masonry-layout {
      column-count: 3;
      column-gap: 0;
    }
    .masonry-layout__panel {
      break-inside: avoid;
      padding: 5px;
    }
    .masonry-layout__panel-content {
      padding: 10px;
      border-radius: 10px;
    }

Сделаем небольшой рефакторинг, чтобы наши панели были блоками сами по себе. Так что мы получим следующий код:

    .masonry-layout {
      column-count: 3;
      column-gap: 0;
    }
    .masonry-layout-panel {
      break-inside: avoid;
      padding: 5px;
    }
    .masonry-layout-panel__content {
      padding: 10px;
      border-radius: 10px;
    }

Наша базовая "кирпичная кладка" все еще имеет потенциал. Давайте попробуем ее улучшить. В этом нам отлично подходит flexbox.

Вложенные кластеры контента.
===

Использование вложеных кластеров контента может разбить контент на блоки и изменить стандартный поток, чего мы и добиваемся. Это даст эффект, что контент рзмещен колонками.

Вложеные кластеры контента сами по себе вложеные блоки контента, который может течь как вертикально, так и горизотально. И Flexbox отличное решение в этом случае.<figure name="81a5
" id="81a5" class="graf--figure graf--layoutOutsetLeft graf-after--p
">

![][1]<figcaption class="imageCaption">Кластер с вертикальным потоком в две колонки</figcaption></figure>

Наша базовая раскладка не вписывается точно в четырехугольник, но наши кластеры **обязаны**. Мы ведь не хотим, чтобы у нас были пробелы в нашей кирпичной раскладке. Учитывая такие размеры для примера:

    .masonry-layout-panel--small {  
      height: 200px;  
    }  
    .masonry-layout-panel--medium {  
      height: 300px;  
    }  
    .masonry-layout-panel--large {  
      height: 400px;  
    }

Но это довольно неправильно. Указывание конкретных размеров может не сработать в каждом конкретном случае. Лучшим вараинтом может быть использование колоночных контейнеров, которые мы обсуждали панее. Давайте представим раскладку вроде такой:

    <div class=”masonry-layout-panel masonry-layout-cluster masonry-layout-cluster--vertical”>  
      <div class=”masonry-layout-cluster__segment masonry-layout-cluster__segment--column”>  
        <div class=”masonry-layout-panel masonry-layout-cluster__segment”>  
          <div class=”masonry-layout-panel__content”>  
            <h1>hello</h1>  
          </div>  
        </div>  
      </div>  
      <div class=”masonry-layout-cluster__segment masonry-layout-cluster__segment--column”>  
        <div class=”masonry-layout-panel masonry-layout-cluster__segment”>  
          <div class=”masonry-layout-panel__content”>  
            <h1>yes</h1>  
          </div>  
        </div>  
        <div class=”masonry-layout-panel masonry-layout-cluster__segment”>  
          <div class=”masonry-layout-panel__content”>  
            <p>  
              Some cool pics.  
            </p>  
          </div>  
        </div>  
      </div>  
    </div>

Кластеры могут располагаться как вертикально, так и горизонтально. Если кластер располагается горизонтально, мы указываем что колонки должны располагаться вертикально, и наоборот.
Мы можем изменить поток используя "*flex-direction*". Сегменты кластера увеличиваются/сжимаются для того, чтобы полностью заполнить свободное пространство, превращая кластеры в блоки.

Если же мы хотим больше контроля над сегментами кластера, мы можем указать для них классы и задать проценты flex-basis для них. Нам не нужно полноценные изолированные классы. так как мы можем добавит только один блок в контейнер, и он займет все пространство по высоте и ширине. Так что, что-то вроде "half-size" вполне подойдет для указания 50% для flex-basis.

Приятным моментом такой реализации будет то, что количество рядов и колонок расположится  в соответствии с вашим контетом.

Как будет выглядеть код? Например такой вариант:

    .masonry-layout-cluster {  
      display: flex;  
      padding: 0;  
    }  
    .masonry-layout-cluster--vertical {  
      flex-direction: row;  
    }  
    .masonry-layout-cluster--horizontal {  
      flex-direction: column;  
    }  
    .masonry-layout-cluster__segment {  
      display: flex;  
      flex: 1 1 auto;  
    }  
    .masonry-layout-cluster__segment--row {  
      flex-direction: row;  
    }  
    .masonry-layout-cluster__segment--column {  
      flex-direction: column;  
    }  
    .masonry-layout-cluster__segment--half {  
      flex-basis: 50%;  
    }  
    .masonry-layout-cluster__segment--quarter {  
      flex-basis: 25%;  
    }

вы можете посмотреть код в действии [здесь][2]. Вложенные кластеры довольно хитрая концепция для объяснения, убрав их, мы можем решать более простые задачи!

Оформление и изображения
======

Цель состоит в том, чтобы изолировать нашу структуру и раскладку от стилей влияющих на внешний вид. Это обеспечивает более простое использование в других проектах. Если вы присматривались к коду выше, вы заметили что у панелей скругленные углы, и т.д. Этого можно добиться применив минимальное оформление. <figure name="7371" id="7371" class="graf--figure graf--
layoutOutsetLeft graf-after--p
">

![][3]<figcaption class="imageCaption">A picture panel</figcaption></figure>

    .masonry-layout-panel__content {  
      border-radius: 10px;  
      overflow: hidden;  
      padding: 10px;  
    }
А что с изображениями? Вполне объяснимо, что вы захотите отображать изображения как цельную панель в своей кирпичной раскладке. И если мы поместим изображение в середину панели, то у нас есть проблема с padding который мы указали в нашем оформлении ранее. Так почему бы нам не создать модификатор для класса "*masonry-layout__panel-content*" с учетом изображений?

    <img src="img/photo.jpg" class="masonry-layout-panel__content--img">

Уже почти, но еще не совсем. Если мы добавим немного стилей непосредственно для изображений, то можем получить нечто следующее.

    .masonry-layout-panel__content--img {  
      max-width: 100%;  
      padding: 0;  
    }

Посмотрите как оно выглядит теперь [здесь][4]. ***Внимание:*** так как названия классов становятся довольно громоздкими, далее будут использованы сокращения, например: “*masonry-layout”* превратится в  “*ml*”, “*masonry-layout-cluster*” превратится в  “*ml-clstr*”, и т.д. Кроме того, плейсхолдеры станут желтыми.

Анимация панелей при наведении
===

 <figure name="2482" id
="2482" class="graf--figure graf--layoutOutsetLeft graf-after--h3
">

![][5]<figcaption class="imageCaption">Panel mid-flip</figcaption></figure>

А что если сделать реакцию панелей при наведении? Мы можем сделать так, что панель перевернется и покажет дополнительную информацию, когда мы наведем на нее. Так что нам необходимо сделать панель двусторонней. Добавив контейнер для передней и задней части, мы уже можем указать какая информация должна быть видима, а какая сзади. Чтобы сделать это, нам необходимо указать высоту для наших панелей, мы можем использовать предопределенные классы, с заранее указанной высотой.  Например:

    .masonry-layout-flip--medium {  
      height: 300px;  
    }
Также нам нужен новый элемент для задней части:

**Разметка **

    <div class=”masonry-layout-panel masonry-layout-flip--medium masonry-layout-flip”>  
      <div class=”masonry-layout-panel__content masonry-layout-flip__content”>  
        <img src=”img/photo-1.jpg” class=”masonry-layout-flip__panel masonry-layout-flip__panel--front masonry-layout-flip__panel--img”/>  
        <div class=”masonry-layout-flip__panel masonry-layout-flip__panel--back”>  
          <p>Here is a flpped image…</p>  
        </div>  
      </div>  
    </div>

    **/* ВНИМАНИЕ: в реальных проектах, можно избежать длинных названий классов, использую сокращения, например "ml-flp" */**

Используя переходы и трансформации CSS3, мы можем применить анимацию к наведению, так что наша панель перевернется и покажет содержимое обратной стороны.


    **/* CSS FOR FLIPPER PANELS */**

    .masonry-layout-flip {  
      perspective: 1000;  
    }  
    .masonry-layout-flip:hover .masonry-layout-flip__content {  
      transform: rotateY(180deg);  
    }  
    .masonry-layout-flip--md {  
      height: 300px;  
    }  
    .masonry-layout-flip__panel {  
      backface-visibility: hidden;  
      border-radius: 10px;  
      height: 100%;  
      left: 0;  
      overflow: hidden;  
      position: absolute;  
      top: 0;  
      width: 100%;  
    }  
    .masonry-layout-flip__panel--front {  
      transform: rotateY(0deg);  
      z-index: 2;  
    }  
    .masonry-layout-flip__panel--back {  
      transform: rotateY(180deg);  
    }  
    .masonry-layout-flip__content {  
      height: 100%;  
      overflow: visible;  
      position: relative;  
      transform-style: preserve-3d;  
      transition: 0.25s;  
    }

Наша раскладка приобретает форму, можно посмотреть в действии [здесь][6].<figure
name="d116" id="d116" class="graf--figure graf--layoutOutsetLeft graf-after--p
">

![][7]<figcaption class="imageCaption">Эффект при фокусе панели</figcaption></
figure
>

Мы можем не только переворачивать панели. Мы можем использовать различные эффекты и действия при взаимодействии с панелью, например изменение при попадании в фокус. В финальный код я добавлю немного бонусных плюшек.

Если вы зашли так далеко, то скорее всего обратили внимание, что кирпичная раскладка выглядит "не очень" на маленьких экранах. Давайте добавим адаптивности. Как мы хотим чтоб раскладка менялась? По мере увеличения экрана должно увеличиваться количество колонок. Пр уменьшении экрана, кроме уменьшения количества колонок, мы хотим, чтобы располагающиеся рядом блоки увеличивались, и занимали всю ширину.  Также, при написании наших стилей мы их сделаем "mobile-first". Давайте сделаем так:

    **/* Изменение числа колонок */**

    .masonry-layout {  
      column-count: 1;  
      column-gap: 0;  
    }  
    [@media][8] (min-width: 768px) {  
      .masonry-layout {  
        column-count: 2;  
      }  
    }  
    [@media][8] (min-width: 1200px) {  
      .masonry-layout {  
        column-count: 3;  
      }  
    }
Для располагающихся рядом кластеров, мы хотим игнорировать расположение в ряд и flex-basis при небольших размерах экрана.  Нам нужно чтобы кластеры объединились и сегменты заняли всю ширину экрана, не зависимо от того они колонки, ряды, и т.п.

    **/* CLUSTER FLOW IGNORED AT LOWER VIEWPORT */**

    [@media][8] (min-width: 768px) {  
      .masonry-layout-cluster__segment--row {  
        flex-direction: row;  
      }  
    }  
    [@media][8] (min-width: 768px) {  
      .masonry-layout-cluster--vertical {  
        flex-direction: row;  
      }  
    }

    **  
    /* FLEX-BASIS IGNORED AT LOWER VIEWPORT */**

    [@media][8] (min-width: 768px) {  
      .masonry-layout-cluster__segment--half {  
        flex-basis: 50%;  
      }  
      .masonry-layout-cluster__segment--quarter {  
        flex-basis: 25%;  
      }  
    }  

Демо работающей адаптивной раскладки можно посмотреть [здесь][9]/

Стоит отметить, что написание CSS происходит гораздо проще, если вы используете CSS препроцессоры. Я настоятельно рекомендую использовать один из них, вместе с инструментами добавляющими вендорные префиксы. .лично я использую Stylus и мой исходный код был значительно меньше, по сравнению с кодом на выходе.

Итак, просто для информации, вот финальный код CSS, используемый для этой статьи (*без вендорных префиксов и цветового оформления для экономии места*). В нем используются сокращения, чтобы избежать громоздкости.


    **/* MASONRY CSS */**  
    .ml {  
      box-sizing: border-box;  
      column-count: 1;  
      column-gap: 0;  
      position: relative;  
    }  
    .ml * {  
      box-sizing: border-box;  
    }  
    [@media][8] (min-width: 768px) {  
      .ml {  
        column-count: 2;  
      }  
    }  
    [@media][8] (min-width: 1200px) {  
      .ml {  
        column-count: 3;  
      }  
    }  
    .ml-pnl {  
      margin: 0;  
      padding: 5px;  
    }
    [@media][8] (min-width: 768px) {  
      .ml-pnl {  
        break-inside: avoid;  
      }  
    }  
    .ml-pnl__cntnt {  
      border-radius: 10px;  
      overflow: hidden;  
      padding: 10px;  
      width: 100%;  
    }  
    .ml-pnl__cntnt--img {  
      max-width: 100%;  
      padding: 0;  
    }  
    .ml-flp {  
      perspective: 1000;  
    }  
    .ml-flp:hover .ml-flp__cntnt {  
      transform: rotateY(180deg);  
    }  
    .ml-flp--sm {  
      height: 200px;  
    }  
    .ml-flp--md {  
      height: 300px;  
    }  
    .ml-flp--lg {  
      height: 400px;  
    }  
    .ml-flp__pnl {  
      backface-visibility: hidden;  
      border-radius: 10px;  
      height: 100%;  
      left: 0;  
      overflow: hidden;  
      position: absolute;  
      top: 0;  
      width: 100%;  
    }  
    .ml-flp__pnl--frnt {  
      transform: rotateY(0deg);  
      z-index: 2;  
    }  
    .ml-flp__pnl--bck {  
      transform: rotateY(180deg);  
    }  
    .ml-flp__cntnt {  
      height: 100%;  
      overflow: visible;  
      position: relative;  
      transform-style: preserve-3d;  
      transition: 0.25s;  
    }  
    .ml-clstr {  
      display: flex;  
      padding: 0;  
    }  
    .ml-clstr--vrt {  
      flex-direction: column;  
    }  
    [@media][8] (min-width: 768px) {  
      .ml-clstr--vrt {  
        flex-direction: row;  
      }  
    }  
    .ml-clstr--hrz {  
      flex-direction: column;  
    }  
    .ml-clstr__sgmnt {  
      display: flex;  
      overflow: hidden;  
      flex: 1 1 auto;  
    }  
    .ml-clstr__sgmnt--rw {  
      display: flex;  
      flex-direction: column;  
    }  
    [@media][8] (min-width: 768px) {  
      .ml-clstr__sgmnt--rw {  
        flex-direction: row;  
      }  
    }  
    .ml-clstr__sgmnt--clmn {  
      flex-direction: column;  
    }  
    [@media][8] (min-width: 768px) {  
      .ml-clstr__sgmnt--hlf {  
        flex-basis: 50%;  
      }  
      .ml-clstr__sgmnt--qrt {  
        flex-basis: 25%;  
      }  
    }

Мы разработали кирпичную раскладку на чистом CSS. Я, личноб  доволен полученным результатом. Кирпичная раскладка на CSS имеет право на жизнь. Хотя я и не назову ее безупречной. Я вижу несколько потенциально проблемных сценариев. Например: если вам необходимо показать содержимое блога отсортированное по дате публикации, это будет не просто.  Думаю что это потребует даже некоторого переосмысления.  Одним из решений может быть использование колонок, и последующее расположение их по датам. Результатом будет самый последний элемент в колонке один, следующий в колонке два и так далее.

Я планирую продолжать работу над кодом, вы можете наблюдать за тем, к чему я приду [здесь][10](github). Можете сделать форк или эксперементировать с кодом [здесь][9].

(***Внимание: **** Если вы добрались так далеко, то посмотрев демо, вы замитете что я добавил несколько плюшек, например фокус при наведении*).

Это была однозначно длинная статья, и как всегда, если у вас есть вопросы и предложения, напишите мне в твиттере[@_jh3y][11]!

[1]: img/1*XaERXcb0ysiyMTNxva-j4Q.png
[2]: https://jsbin.com/tuxoqusaxu/2/edit?css,output
[3]: img/1*n2mjkAnx5NNAK7w-I1zJxw.png
[4]: https://jsbin.com/zamabonuhe/2/edit?html,css,output
[5]: img/1*sDjPLMU-Q2I3KpWOiuO8Gw.png
[6]: https://jsbin.com/vixaxofato/3/edit?html,css,output
[7]: img/1*8Y__HtUYHClzv9jTMvxLAQ.png
[8]: http://twitter.com/media "Twitter profile for @media"
[9]: https://jsbin.com/loronibali/2/edit?css,output
[10]: https://github.com/jh3y/driveway
[11]: http://twitter.com/_jh3y
