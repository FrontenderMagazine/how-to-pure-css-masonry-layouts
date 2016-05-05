Использование вложеных кластеров контента может разбить контент на блоки и изменить стандартный поток, чего мы и добиваемся. Это даст эффект, что контент рзмещен колонками.

Вложеные кластеры контента сами по себе вложеные блоки контента, который может течь как вертикально, так и горизотально. И Flexbox отличное решение в этом случае.<figure name="81a5
" id="81a5" class="graf--figure graf--layoutOutsetLeft graf-after--p
">

![][1]<figcaption class="imageCaption">Кластер с вертикальным потоком в две колонки</figcaption></figure>

Наша базовая раскладка не вписывается точно в четырехуголник, но наши кластеры **обязаны**. Мы ведь не хотим, чтобы у нас были пробелы в нашей кирпичной раскладке. Учитывая такие размеры для примера:

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

Кластеры могут распологаться как вертикально, так и горизонтально. Если кластер рапологается горизонтально, мы указываем что колонки должны располагться вертикально, и наоборот.
Мы можем изменить поток используя "*flex-direction*". Сегменты кластера увеличиваются/сжимаются для того, чтобы полностью заполнить свободное пространство, превращая кластеры в блоки.

Если же мы хотим больше контроля над сегментами кластера, мы можем указать для них классы и задать проценты flex-basis для них. Нам не нужно полноценные изолированые класыЮ так как мы можем добавит только один блок в контейнер, и он займет все пространство по высоте и ширине. Так что, что-то вроде "half-size" вполне подойдет для указания 50% для flex-basis.

Приятным моментом такой реализации будет то, что количество рядов и колонок расположится  в соответсвии с вашим контетном.

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

Цель состоит в том, чтобы изолировать нашу структуру и раскладку от стилей влияющих на внешний вид. Это обеспечивает более простое использование в других проектах. Если вы присматривались к коду выше, вы заметили что у панелей скругленные углы, и т.д. Этого можно добитьс применив минимальное оформление. <figure name="7371" id="7371" class="graf--figure graf--
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

Посмотрите как оно выглядит теперь [здесь][4]. ***Внимание:*** так как названия класов становятся довольно громоздкими, далее будут использованы сокращенияб например: “*masonry-layout”* превратится в  “*ml*”, “*masonry-layout-cluster*” превратится в  “*ml-clstr*”, и т.д. Кроме того, плейсхолдеры станут желтыми.

Анимация панелей при наведении
===

 <figure name="2482" id
="2482" class="graf--figure graf--layoutOutsetLeft graf-after--h3
">

![][5]<figcaption class="imageCaption">Panel mid-flip</figcaption></figure>

А что если сделать реакцию панелей при наведении? Мы можем сделать так, что панель перевернеться и покажет дополнительную информацию, когда мы наведем на нее. Так что нам необходимо сделать панель двустронней. Добавив контейнер для передней и задней части, мы уже можем указать какая информация должна быть навиду, а какая сзади. Чтобы сделать это, нам необходимо указать высоту для наших панелей, мы можем использовать предопределенные класы, с зарнее указанной высотой.  Например:

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

    **/* ВНИМАНИЕ: в реальных проектах, можно избежать дляииных названий класов, использую сокращения, например "ml-flp" */**

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

Мы можем не только переварачивать панелию Мы можем использовать различные эффекты и действия при взаимодействии с панелью, например изменене при попадании в фокус. В финальный код я добавлю немного бонусных фич.

Если вы зашли так далеко, то скорее всего обратили внимание, что кирпична раскладко выгядит "неочень" на маленьких экранах. Давайте добавим адаптивности. Как мы хотим чтоб расладка менялась? По мере увеличения экрана должно увеличиваться количество колонок. Пр уменьшении экрана, кроме уменьшения количества колонк, мы хотим, чтобы распологающиеся рядом блоки увеличивались, и занимали всю шрину.  Также, при написании наших стилей мы их сделаем "mobile-first". Давайте сделаем так:

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
Для распологающихся рядом кластеров, мы хотим игнорировать расположение в ряд и flex-basis при небольших размерах экрана.  Нам нужно чтобы кластеры объеденились и сегменты заняли всю ширину экрана, не зависимо от того они колонки, ряды, и т.п.

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

Стоит отметить, что написание CSS происходит гораздо проще, если вы используете CSS препроцоссоры. Я настоятельно рекомендую использовать один из них, вместе с инструментами доавляющими вендорные префиксы. .лично я использую Stylus и мой исходный код был значительно меньше, по сравнению с кодом на выходе.

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

Мы разработали кирпичную раскладку на чистом CSS. Kbxyj z ljdjkty gjkextyysv htpekmnfnjv. Кирпичная раскладка на CSS имеет право на жизнь. Хотя я и не назову ее безупречной. Я вижу несколько потенциально проблемных сценариев. Например: если вам необходимо показать содержимое блога отсортированное по дате публикаци, это будет не просто.  Думаю что это потребует даже некоторого переосмысления.  Одним из решиний может быть использование колонок, и последуюющее расположение их по датам. Резудьтатом будет самый последний элемент в колонке один, следующий в колонке два и так далее.

Я планирую продолжать работу над кодом, вы можете наблюдать за тем, к чему я приду [здесь][10](github). Можете форкнуть или поэксперементировать с кодом [здесь][9].

(***Внимание: **** Если вы добрались так далеко, то посмотрев демо, вы замитете что я добавил несколько фич, например фокус при наведении*).

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
