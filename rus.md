Кирпичная раскладка на чистом CSS, делаем раскладку в шахматном порядке без использования Javascript.
===

Один из популярных видов раскладки элементов - "кирпичная кладка". Знакомы с такой? Вспоминайте Pinterest, Metro в Windowsб и т.д. В двух словах: эффектное размещение блоков разного размера и с различным контентом, часто в шахматном порядку
<figure name="7372" id="7372" class="graf--figure graf--
layoutOutsetLeft graf-after--p
">

![][12]</figure>

Кирпичная раскладка далеко не новинка. Так зачем говорить о ней снова? Cуществует несколько отличных решений для организации кирпичной раскладки. Однако, можем ли мы получить решение исключительно CSS? Возможно ли используя Flexbox получить желаемый эффект.

Для тех, кто TL;DR - переходите сразу к коду, который я объединил [здесь][10](github) или [здесь][9].

Базовый эффект
===

Давайте начнем с базовой раскладки и представим вот такую структуру DOM :

    <div class="masonry-layout">
      <div class="masonry-layout__panel">
        <div class="masonry-layout__panel-content">
          <-- Контент -->
        </div>
      </div>
      <div class="masonry-layout__panel">
        <div class="masonry-layout__panel-content">
          <-- Контент -->
        </div>
      </div>
      <div class="masonry-layout__panel">
        <div class="masonry-layout__panel-content">
          <-- Контент -->
        </div>
      </div>
      <-- Остальные блоки страницы -->
    </div>

Нам необходимо разбить наш контент на колонки. Опираясь на нашу структуру DOM, можем ли мы получить базовый эффект кирпичной кладки используя CSS3 правила для multi-column или Flexbox?

Что это за правила - multi-column и Flexbоx?
====
Оба правила появились в CSS3, оба - колоночная раскладка и Flexbоx, управляют способами раскладки (организации) контента. Оба свойства имеют много синонимичных названии. Свойство multi-column дает нам возможность организовать контент в виде колонок, не прилагая особых усилий. Flexbox (flex в переводе гибкий) обеспечивает то, что вы подумали из названия, гибкие блоки для вашего лейаута, они могут сжиматься, увеличиваться и меняться так как вы ожидаете. Я не буду вдаваться в подробности в этой статье, так что рекомендую ознакомиться самостоятельно перед дальнейшим чтением.

Возможность Использовать мультиколоночный лейаут появилась в CSS3 вместе с спецификациями column-count и column-gap. Это уже дает возможность получить эффект "кирпичной кладки" довольно просто. Обратите внимание, необходимо использовать правило break-inside для блоков контента внутри колонок. Это будет гарантий того, что содержимое блока не разделится, и не распределится между колонками. Начальный код CSS выглядит следующим образом.

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

Увидеть его в действии можно [тут][13]. Это хороший старт начало, доказывающий, что мульти-колоночная раскладка готова взять на себя самую тяжелую часть лейаута.

Использование Flexbox.
===
Можем ли мы достичь подобного используя Flexbox? Тут все не так просто и однозначно, как в случае мультиколоночной раскладки. Я выявил несколько проблем в своих попытках добиться нужного эффекта используя Flexbox.

В первой попытке, я пытался имитировать колонки используя свойство "flex-direction:column". У свойства "column" есть проблема, вам необходимо указать высоту колонок, чтобы колонки начали разбиваться. Но этого приводит к тому, что не помещающиеся блоки располагаются горизонтально, и содержимое начинает прокручиваться горизонтально. Так что необходимо подбирать высоту, чтобы поместились все блоки, так, как вы этого хотели. В случае, если вы работаете с динамичным контентом, размеры блоков могут меняться при каждом изменении содержимого, и вам придется подбирать высоту снова. Что идет в разрез с идеологией Flexbox, на мой взгляд.

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

Полученный результат можно увидеть [тут][14]. Обратите внимание, что контент начал прокручиваться горизонтально, а мы этого не хотели. Другой подход, это использование "flex-direction: row". Но и это не особо работает. Возможно указать количество колонок, ограничив максимальную ширину элементов, но появляются пробелы, в зависимости от того, как вы укажете свойство "align-items". Совсем не тот результат, который ожидали. Пример использования "flex-direction: row" и "align-items:start" можно увидеть [здесь][13].

Если же нужен больший контроль над разметкой и структурой страницы, то результат можно получить и используя flexbox. Например используя подобную структуру:

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

Добавив колонки в свою разметку, вы получите больше контроля над разметкой. В любом случае, вам нужно хорошенько обдумать, как и где будут находится элементы вашего контента. Увидеть подобную реализацию в действии можно [здесь][15].

Использование свойства multi-column в разметке.
===

Для получения базовой "кирпичной кладки", на мой взгляд, свойство multi-column подходит больше всего. Мое мнение основывается на простоте и минимуме разметки. И в итоге, все работает. Если базовой "кладки" достаточно, то нижеприведенный код - это все, что вам нужно. У каждого из подходов есть свои преимущества.

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

Сделав небольшой рефакторинг, и наши панели стали блоками сами по себе. В итоге у нас будет вот такой код:

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

Наша базовая "кирпичная кладка" все еще имеет потенциал. Давайте её улучшать. И для этого нам отлично подходит flexbox.

Вложенные кластеры контента.
===

Использование вложенных кластеров контента помогает разбить контент на блоки и изменить стандартный поток, что нам и нужно. В итоге наш контент будет выглядеть разбитым на колонки.

Вложенные кластеры контента по сути это разметка в разметке, которая может "течь" и вертикально, и горизотально. И в данном случае,  Flexbox отличное решение.
<figure name="81a5" id="81a5">
![][1]<figcaption class="imageCaption"> Двухколоночный кластер с вертикальным потоком</figcaption>
</figure>

Наша базовая раскладка не вписывается точно в четырехугольник, но наши кластеры **обязаны**. Мы ведь не хотим, чтобы у нас были пробелы в нашей кирпичной раскладке. Возьмем для примера размеры:

    .masonry-layout-panel--small {  
      height: 200px;  
    }  
    .masonry-layout-panel--medium {  
      height: 300px;  
    }  
    .masonry-layout-panel--large {  
      height: 400px;  
    }

И это неправильно. Указывание конкретных размеров, может не сработать в каждом конкретном случае. Лучшим вариантом будет использование колоночных контейнеров, которые мы обсудили ранее. Давайте для примера возьмем такую раскладку:

    <div class=”masonry-layout-panel masonry-layout-cluster masonry-layout-cluster--vertical”>  
      <div class=”masonry-layout-cluster__segment masonry-layout-cluster__segment--column”>  
        <div class=”masonry-layout-panel masonry-layout-cluster__segment”>  
          <div class=”masonry-layout-panel__content”>  
            <h1>Привет</h1>  
          </div>  
        </div>  
      </div>  
      <div class=”masonry-layout-cluster__segment masonry-layout-cluster__segment--column”>  
        <div class=”masonry-layout-panel masonry-layout-cluster__segment”>  
          <div class=”masonry-layout-panel__content”>  
            <h1>Да</h1>  
          </div>  
        </div>  
        <div class=”masonry-layout-panel masonry-layout-cluster__segment”>  
          <div class=”masonry-layout-panel__content”>  
            <p>  
              Здесь клевые фотки.  
            </p>  
          </div>  
        </div>  
      </div>  
    </div>

Кластеры могут располагаться как вертикально, так и горизонтально. Если кластер горизонтальный, мы указываем что колонки должны располагаться вертикально, и наоборот.
Мы можем изменить поток используя "*flex-direction*". Сегменты кластера увеличиваются/сжимаются, полностью заполняя свободное пространство, превращая кластеры в блоки.

Для большего контроля над сегментами кластера, мы можем указать для них классы и задать проценты flex-basis . Нам не нужны полноценно изолированные классы, так как мы можем добавит только один блок в контейнер, и он займет все пространство по высоте и ширине. Так что, классы вроде "half-size" вполне подойдет для указания 50% для flex-basis.

Приятным моментом такой реализации будет факт, что количество рядов и колонок расположится в соответствии с вашим контентом.

Как выглядит код такой реализации? Например так:

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

Посмотреть код в действии можно [здесь][2]. Вложенные кластеры довольно хитрая концепция для объяснения, убрав их, мы можем решать более простые задачи!

Оформление и изображения
======

Цель состоит в том, чтобы изолировать нашу структуру и раскладку от стилей влияющих на внешний вид. Это обеспечит простоту использование в других проектах. Если вы внимательно изучили код выше, вы заметили, что у панелей скругленные углы, и т.д. Этого можно добиться применив минимальное оформление. <figure name="7371" id="7371" class="graf--figure graf--
layoutOutsetLeft graf-after--p
">

![][3]<figcaption class="imageCaption">Блок с изображением</figcaption></figure>

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

![][5]<figcaption class="imageCaption">Блок с вращением</figcaption></figure>

А что если добавить реакцию панелей при наведении? Возможно сделать, что панель перевернется и покажет дополнительную информацию, при наведении на нее. Так что, нам необходимо сделать панель двусторонней. Добавив контейнер для передней и задней части, мы уже можем указать какая информация должна быть видима, а какая сзади. Чтобы сделать это, необходимо указать высоту наших панелей, можно использовать предопределенные классы, с заранее указанной высотой.  Например:

    .masonry-layout-flip--medium {
      height: 300px;  
    }

Также, нам нужен новый элемент для задней части:

**Разметка **

    <div class=”masonry-layout-panel masonry-layout-flip--medium masonry-layout-flip”>  
      <div class=”masonry-layout-panel__content masonry-layout-flip__content”>  
        <img src=”img/photo-1.jpg” class=”masonry-layout-flip__panel masonry-layout-flip__panel--front masonry-layout-flip__panel--img”/>  
        <div class=”masonry-layout-flip__panel masonry-layout-flip__panel--back”>  
          <p>Здесь изображение перевернулось…</p>  
        </div>  
      </div>  
    </div>

    **/* ВНИМАНИЕ: в реальных проектах, можно избежать длинных названий классов, использую сокращения, например "ml-flp" */**

Используя переходы и трансформации CSS3, мы можем применить анимацию к наведению, и наша панель перевернется и покажет содержимое обратной стороны.


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

Если вы уже зашли так далеко, то скорее всего обратили внимание, что кирпичная раскладка выглядит "не очень" на маленьких экранах. Давайте добавим адаптивности. Как мы хотим чтоб раскладка менялась? По мере увеличения экрана должно увеличиваться количество колонок. При уменьшении экрана, кроме уменьшения количества колонок, мы хотим, чтобы располагающиеся рядом блоки увеличивались, и занимали всю ширину. Также, при написании наших стилей мы их сделаем "mobile-first". Давайте так и сделаем:

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
    
Для располагающихся рядом кластеров, нужно игнорировать расположение в ряд и flex-basis на небольших размерах экрана. Необходимо чтобы кластеры объединились и сегменты заняли всю ширину экрана, не зависимо от того колонки они, ряды, и т.п.

    **/* Игнорирование потока кластеров для маленьких экранов */**

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
    /* FLEX-BASIS игнорируется на маленьких экранах */**

    [@media][8] (min-width: 768px) {  
      .masonry-layout-cluster__segment--half {  
        flex-basis: 50%;  
      }  
      .masonry-layout-cluster__segment--quarter {  
        flex-basis: 25%;  
      }  
    }  

Демо работающей адаптивной раскладки можно посмотреть [здесь][9]

Стоит отметить, написание CSS происходит гораздо проще, если вы используете CSS препроцессоры. Я настоятельно рекомендую использовать один из них, вместе с инструментами добавляющими вендорные префиксы. Лично я использую Stylus и мой исходный код был значительно меньше, по сравнению с кодом на выходе.

Итак, просто для информации, вот финальный код CSS, используемый для этой статьи (*без вендорных префиксов и цветового оформления для экономии места*). В нем используются сокращения, чтобы избежать громоздкости.


    **/* CSS Для кирпичной раскладки */**  
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

Мы создали кирпичную раскладку на чистом CSS. Лично я, доволен полученным результатом. Кирпичная раскладка на CSS имеет право на жизнь. Хотя я и не назову ее безупречной. Я вижу несколько потенциально проблемных сценариев. Например: если вам необходимо показать содержимое блога отсортированное по дате публикации, это будет не просто.  Думаю, это потребует даже некоторого переосмысления. Одним из решений может быть использование колонок, и последующее расположение их по датам. Результатом будет самый последний элемент в колонке один, следующий в колонке два и так далее.

Я планирую продолжать работу над кодом, вы можете наблюдать за тем, к чему я приду [здесь][10](github). Можете сделать форк или экспериментировать с кодом [здесь][9].

(***Внимание: **** Если вы дочитали о конца, то посмотрев демо, вы заметили что я добавил несколько плюшек, например фокус при наведении*).

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
[12]: img/1-TfCvIdT79TwK8zcCeG-qSQ.png 
[13]: https://jsbin.com/yofikeveba/2/edit?css,output
[14]: https://jsbin.com/qacawerica/2/edit?html,css,output
[15]: https://jsbin.com/zurapetono/1/edit?html,css,output
