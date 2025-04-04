---
title: Введение в события
slug: Learn_web_development/Core/Scripting/Events
---

{{LearnSidebar}}{{PreviousMenuNext("Learn/JavaScript/Building_blocks/Return_values","Learn/JavaScript/Building_blocks/Image_gallery", "Learn/JavaScript/Building_blocks")}}

События — это действия или случаи, возникающие в программируемой вами системе, о которых система сообщает вам для того, чтобы вы могли с ними взаимодействовать. Например, если пользователь нажимает кнопку на веб-странице, вы можете ответить на это действие, отобразив информационное окно. В этой статье мы обсудим некоторые важные концепции, связанные с событиями, и посмотрим, как они работают в браузерах. Эта статья не является исчерпывающим источником по этой теме — здесь только то, что вам нужно знать на этом этапе.

| Предпосылки: | Базовая компьютерная грамотность, базовое понимание HTML и CSS, [Первые шаги в JavaScript](/ru/docs/conflicting/Learn_web_development/Core/Scripting). |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Задача:      | Понять фундаментальную теорию событий, как они работают в браузерах и как события могут различаться в разных средах программирования.                  |

## Серия удачных событий

При возникновении **события** система генерирует сигнал, а также предоставляет механизм, с помощью которого можно автоматически предпринимать какие-либо действия (например, выполнить определённый код), когда происходит событие. Например, в аэропорту, когда взлётно-посадочная полоса свободна для взлёта самолёта, сигнал передаётся пилоту, и в результате они приступают к взлёту.

![](mdn-mozilla-events-runway.png)

В Web события запускаются внутри окна браузера и, как правило, прикрепляются к конкретному элементу, который в нем находится. Это может быть один элемент, набор элементов, документ HTML, загруженный на текущей вкладке, или все окно браузера. Существует множество различных видов событий, которые могут произойти, например:

- Пользователь кликает мышью или наводит курсор на определённый элемент.
- Пользователь нажимает клавишу на клавиатуре.
- Пользователь изменяет размер или закрывает окно браузера.
- Завершение загрузки веб-страницы.
- Отправка данных через формы.
- Воспроизведение видео, пауза или завершение воспроизведения.
- Произошла ошибка.

Подробнее о событиях можно посмотреть в [Справочнике по событиям](/ru/docs/Web/Events).

Каждое доступное событие имеет **обработчик событий** — блок кода (обычно это функция JavaScript, вводимая вами в качестве разработчика), который будет запускаться при срабатывании события. Когда такой блок кода определён на запуск в ответ на возникновение события, мы говорим, что мы **регистрируем обработчик событий**. Обратите внимание, что обработчики событий иногда называют слушателями событий (от англ. event listeners). Они в значительной степени взаимозаменяемы для наших целей, хотя, строго говоря, они работают вместе. Слушатель отслеживает событие, а обработчик — это код, который запускается в ответ на событие.

> [!NOTE]
> Важно отметить, что веб-события не являются частью основного языка JavaScript. Они определены как часть JavaScript-API, встроенных в браузер.

### Простой пример

Рассмотрим простой пример. Вы уже видели события и обработчики событий во многих примерах в этом курсе, но давайте повторим для закрепления информации. В этом примере у нас есть кнопка {{htmlelement ("button")}}, при нажатии которой цвет фона изменяется случайным образом:

```html
<button>Change color</button>
```

```css hidden
button {
  margin: 10px;
}
```

JavaScript выглядит так:

```js
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

btn.onclick = function () {
  const rndCol =
    "rgb(" + random(255) + "," + random(255) + "," + random(255) + ")";
  document.body.style.backgroundColor = rndCol;
};
```

В этом коде мы сохраняем ссылку на кнопку внутри переменной `btn` типа `const`, используя функцию {{domxref ("Document.querySelector()")}}. Мы также определяем функцию, которая возвращает случайное число. Третья часть кода — [обработчик события](#свойства_обработчика_событий). Переменная `btn` указывает на элемент `<button>` — для этого типа объекта существуют возникающие при определённом взаимодействии с ним события, а значит, возможно использование обработчиков событий. Мы отслеживаем момент возникновения события при щелчке мышью, связывая свойство обработчика события `onclick` с анонимной функцией, генерирующей случайный цвет RGB и устанавливающей его в качестве цвета фона элемента `<body>`.

Этот код теперь будет запускаться всякий раз, когда возникает событие при нажатии на элемент `<button>` — всякий раз, когда пользователь щёлкает по нему.

Пример вывода выглядит следующим образом:

{{ EmbedLiveSample('Простой_пример', '100%', 200, "") }}

### События не только для веб-страниц

События, как понятие, относятся не только к JavaScript — большинство языков программирования имеют модель событий, способ работы которой часто отличается от модели в JavaScript. Фактически, даже модель событий в JavaScript для веб-страниц отличается от модели событий для просто JavaScript, поскольку используются они в разных средах.

Например, [Node.js](/ru/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs) — очень популярная среда исполнения JavaScript, которая позволяет разработчикам использовать JavaScript для создания сетевых и серверных приложений. [Модель событий Node.js](https://nodejs.org/docs/latest-v5.x/api/events.html) основана на том, что существуют обработчики, отслеживающие события, и эмиттеры (передатчики), которые периодически генерируют события. В общем-то, это похоже на модель событий в JavaScript для веб-страниц, но код совсем другой. В этой модели используется функция `on()` для регистрации обработчиков событий, и функция `once()` для регистрации обработчика событий, который отключается после первого срабатывания. Хорошим примером использования являются протоколы событий [HTTP connect event docs](https://nodejs.org/docs/latest-v8.x/api/http.html#http_event_connect).

Вы также можете использовать JavaScript для создания кросс-браузерных расширений — улучшения функциональности браузера с помощью технологии [WebExtensions](/ru/docs/Mozilla/Add-ons/WebExtensions). В отличии от модели веб-событий здесь свойства обработчиков событий пишутся в так называемом регистре [CamelCase](https://ru.wikipedia.org/wiki/CamelCase) (например, `onMessage`, а не `onmessage`) и должны сочетаться с функцией `addListener`. См. [runtime.onMessage page](/ru/docs/Mozilla/Add-ons/WebExtensions/API/runtime/onMessage#examples) для примера.

На данном этапе обучения вам не нужно особо разбираться в различных средах программирования, однако важно понимать, что принцип работы _событий_ в них отличается.

## Способы использования веб-событий

Существует множество различных способов добавления кода обработчика событий на веб-страницы так, чтобы он срабатывал при возникновении соответствующего события. В этом разделе мы рассмотрим различные механизмы и обсудим, какие из них следует использовать.

### Свойства обработчика событий

В этом курсе вы уже сталкивались со свойствами, связываемыми с алгоритмом работы обработчика событий. Вернёмся к приведённому выше примеру:

```js
const btn = document.querySelector("button");

btn.onclick = function () {
  var rndCol =
    "rgb(" + random(255) + "," + random(255) + "," + random(255) + ")";
  document.body.style.backgroundColor = rndCol;
};
```

В данной ситуации свойство [`onclick`](/ru/docs/Web/API/Element/click_event) — это свойство обработчика события. В принципе это обычное свойство кнопки как элемента (наравне с [`btn.textContent`](/ru/docs/Web/API/Node/textContent) или [`btn.style`](/ru/docs/Web/API/HTMLElement/style)), но оно относится к особому типу. Если вы установите его равным какому-нибудь коду, этот код будет запущен при возникновении события (при нажатии на кнопку).

Для получения того же результата, вы также можете присвоить свойству обработчика имя уже описанной функции (как мы видели в статье [Создайте свою функцию](/ru/docs/Learn_web_development/Core/Scripting/Build_your_own_function)):

```js
const btn = document.querySelector("button");

function bgChange() {
  const rndCol =
    "rgb(" + random(255) + "," + random(255) + "," + random(255) + ")";
  document.body.style.backgroundColor = rndCol;
}

btn.onclick = bgChange;
```

Давайте теперь поэкспериментируем с другими свойствами обработчика событий.

Создайте локальную копию файла [random-color-eventhandlerproperty.html](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/events/random-color-eventhandlerproperty.html) и откройте её в своём браузере. Это всего лишь копия простого примера про случайные цвета, который мы уже разобрали в этой статье. Теперь попробуйте изменить `btn.onclick` на следующие значения и понаблюдайте за результатами:

- [`btn.onfocus`](/ru/docs/Web/API/Window/focus_event) и [`btn.onblur`](/ru/docs/Web/API/Window/blur_event) — Цвет изменится, когда кнопка будет сфокусирована или не сфокусирована (попробуйте нажать Tab, чтобы выбрать кнопку или убрать выбор). Эти свойства часто применяются для отображения информации о том, как заполнить поля формы, когда они сфокусированы, или отобразить сообщение об ошибке, если поле формы было заполнено с неправильным значением.
- [`btn.ondblclick`](/ru/docs/Web/API/Element/dblclick_event) — Цвет будет изменяться только при двойном щелчке.
- [`window.onkeypress`](/ru/docs/Web/API/Element/keypress_event), [`window.onkeydown`](/ru/docs/Web/API/Element/keydown_event), [`window.onkeyup`](/ru/docs/Web/API/Element/keyup_event) — Цвет будет меняться при нажатии клавиши на клавиатуре, причём `keypress` ссылается на обычное нажатие (нажатие кнопки и последующее её отпускание _как одно целое_), в то время как `keydown` и `keyup` _разделяют_ действия на нажатие клавиши и отпускание, и ссылаются на них соответственно. Обратите внимание, что это не работает, если вы попытаетесь зарегистрировать этот обработчик событий на самой кнопке - его нужно зарегистрировать на объекте [window](/ru/docs/Web/API/Window), который представляет все окно браузера.
- [`btn.onmouseover`](/ru/docs/Web/API/Element/mouseover_event) и [`btn.onmouseout`](/ru/docs/Web/API/Element/mouseout_event) — Цвет будет меняться при наведении указателя мыши на кнопку или когда указатель будет отводиться от кнопки соответственно.

Некоторые события очень общие и доступны практически в любом месте (например, обработчик `onclick` может быть зарегистрирован практически для любого элемента), тогда как некоторые из них более конкретны и полезны только в определённых ситуациях (например, имеет смысл использовать [onplay](/ru/docs/Web/API/HTMLMediaElement/play_event) только для определённых элементов, таких как {{htmlelement ("video")}}).

### Встроенные обработчики событий - не используйте их

Самый ранний из введённых в сеть Web методов регистрации _обработчиков событий_ базируется на **HTML атрибутах** (**встроенные обработчики событий**):

```html
<button onclick="bgChange()">Press me</button>
```

```js
function bgChange() {
  const rndCol =
    "rgb(" + random(255) + "," + random(255) + "," + random(255) + ")";
  document.body.style.backgroundColor = rndCol;
}
```

> [!NOTE]
> Вы можете найти [полный исходник кода](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/events/random-color-eventhandlerattributes.html) из этого примера на GitHub (также [взгляните на его выполнение](https://mdn.github.io/learning-area/javascript/building-blocks/events/random-color-eventhandlerattributes.html)).

Значение атрибута — это буквально код JavaScript, который вы хотите запустить при возникновении события. В приведённом выше примере вызывается функция, определённая внутри элемента {{htmlelement ("script")}} на той же странице, но вы также можете вставить JavaScript непосредственно внутри атрибута, например:

```html
<button onclick="alert('Hello, this is my old-fashioned event handler!');">
  Press me
</button>
```

Для многих свойств обработчика событий существуют эквиваленты в виде атрибутов HTML. Однако, не рекомендуется их использовать — это считается плохой практикой. Использование атрибутов для регистрации обработчика событий кажется простым и быстрым методом, но такое описание обработчиков также скоро становится неудобным и неэффективным.

Более того, не рекомендуется смешивать HTML и JavaScript файлы, так как в дальнейшем такой код становится сложнее с точки зрения обработки (парсинга). Лучше держать весь JavaScript в одном месте. Также, если он находится в отдельном файле, вы можете применить его к нескольким документам HTML.

Даже при работе только в одном файле использование встроенных обработчиков не является хорошей идеей. Ладно, если у вас одна кнопка, но что, если у вас их 100? Вам нужно добавить в файл 100 атрибутов; обслуживание такого кода очень быстро превратится в кошмар. С помощью JavaScript вы можете легко добавить функцию обработчика событий ко всем кнопкам на странице независимо от того, сколько их было.

Например:

```js
const buttons = document.querySelectorAll("button");

for (var i = 0; i < buttons.length; i++) {
  buttons[i].onclick = bgChange;
}
```

Обратите внимание, что для перебора всех элементов, которые содержит объект [`NodeList`](/ru/docs/Web/API/NodeList), можно воспользоваться встроенным методом [`forEach()`](/ru/docs/Web/API/NodeList/forEach):

```js
buttons.forEach(function (button) {
  button.onclick = bgChange;
});
```

> [!NOTE]
> Разделение логики вашей программы и вашего контента также делает ваш сайт более дружественным к поисковым системам.

### Функции addEventListener() и removeEventListener()

Новый тип механизма событий определён в спецификации [Document Object Model (DOM) Level 2 Events](https://www.w3.org/TR/DOM-Level-2-Events/), которая предоставляет браузеру новую функцию — [`addEventListener()`](/ru/docs/Web/API/EventTarget/addEventListener). Работает она аналогично свойствам обработчика событий, но синтаксис совсем другой. Наш пример со случайным цветом мог бы выглядеть и так:

```js
var btn = document.querySelector("button");

function bgChange() {
  var rndCol =
    "rgb(" + random(255) + "," + random(255) + "," + random(255) + ")";
  document.body.style.backgroundColor = rndCol;
}

btn.addEventListener("click", bgChange);
```

> [!NOTE]
> Вы можете найти [исходный код](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/events/random-color-addeventlistener.html) из этого примера на GitHub (также [взгляните на его выполнение](https://mdn.github.io/learning-area/javascript/building-blocks/events/random-color-addeventlistener.html)).

Внутри функции `addEventListener()` мы указываем два параметра — имя события, для которого мы хотим зарегистрировать этот обработчик, и код, содержащий функцию обработчика, которую мы хотим запустить в ответ. Обратите внимание, что будет целесообразно поместить весь код внутри функции `addEventListener()` в анонимную функцию, например:

```js
btn.addEventListener("click", function () {
  var rndCol =
    "rgb(" + random(255) + "," + random(255) + "," + random(255) + ")";
  document.body.style.backgroundColor = rndCol;
});
```

Этот механизм имеет некоторые преимущества по сравнению с более старыми механизмами, рассмотренными ранее. Например, существует аналогичная функция [`removeEventListener()`](/ru/docs/Web/API/EventTarget/removeEventListener), которая удаляет ранее добавленный обработчик. Это приведёт к удалению набора обработчиков в первом блоке кода в этом разделе:

```js
btn.removeEventListener("click", bgChange);
```

Это не важно для простых небольших программ, но для более крупных и более сложных программ он может повысить эффективность очистки старых неиспользуемых обработчиков событий. Кроме того, это позволяет вам иметь одну и ту же кнопку, выполняющую различные действия в разных обстоятельствах — все, что вам нужно сделать, это добавить/удалить обработчики событий, если это необходимо.

Также вы можете зарегистрировать несколько обработчиков для одного и того же события на элементе. Следующие два обработчика не будут применяться:

```js
myElement.onclick = functionA;
myElement.onclick = functionB;
```

Поскольку вторая строка будет перезаписывать значение `onclick`, установленное первой. Однако, если:

```js
myElement.addEventListener("click", functionA);
myElement.addEventListener("click", functionB);
```

Обе функции будут выполняться при щелчке элемента.

Кроме того, в этом механизме событий имеется ряд других мощных функций и опций. Эта тема выходит за рамки данной статьи, но если вы хотите изучить подробнее, переходите по ссылкам: [Метод EventTarget.addEventListener()](/ru/docs/Web/API/EventTarget/addEventListener) и [Метод EventTarget.removeEventListener()](/ru/docs/Web/API/EventTarget/removeEventListener).

### Какой механизм мне использовать?

Из трёх механизмов определённо не нужно использовать атрибуты событий HTML. Как упоминалось выше, это устаревшая и плохая практика.

Остальные два являются относительно взаимозаменяемыми, по крайней мере для простых целей

- Свойства обработчика событий имеют меньшую мощность и параметры, но лучше совместимость между браузерами (поддерживается ещё в Internet Explorer 8). Вероятно, вам следует начать с них, когда вы учитесь.
- События уровня 2 DOM (`addEventListener()` и т. д.) являются более мощными, но также могут стать более сложными и хуже поддерживаться (поддерживается ещё в Internet Explorer 9). Вам также стоит поэкспериментировать с ними и стремиться использовать их там, где это возможно.

Основные преимущества третьего механизма заключаются в том, что при необходимости можно удалить код обработчика событий, используя `removeEventListener()`, и так же можно добавить несколько элементов-обработчиков того же типа к элементам. Например, вы можете вызвать `addEventListener('click', function() {...})` для элемента несколько раз, с разными функциями, указанными во втором аргументе. Это невозможно при использовании свойств обработчика событий, поскольку любые последующие попытки установить свойство будут перезаписывать более ранние, например:

```js
element.onclick = function1;
element.onclick = function2;
etc.
```

> [!NOTE]
> Если вам требуется поддержка браузеров старше Internet Explorer 8 в вашем проекте, вы можете столкнуться с трудностями, так как такие старые браузеры используют старые модели событий из более новых браузеров. Но не бойтесь, большинство библиотек JavaScript (например, `jQuery`) имеют встроенные функции, которые абстрагируют различия между браузерами. Не беспокойтесь об этом слишком много на этапе вашего учебного путешествия.

## Другие концепции событий

Рассмотрим некоторые современные концепции, имеющие отношение к событиям. На данный момент не обязательно понимать их полностью, но представление о них поможет лучше понять некоторые модели кода, с которыми вы, вероятно, столкнётесь.

### Объекты событий

Иногда внутри функции обработчика событий вы можете увидеть параметр, заданный с таким именем, как `event`, `evt` или просто `e`. Называется он **объектом события** и он автоматически передаётся обработчикам событий для предоставления дополнительных функций и информации. Например, давайте немного перепишем наш пример со случайным цветом:

```js
function bgChange(e) {
  var rndCol =
    "rgb(" + random(255) + "," + random(255) + "," + random(255) + ")";
  e.target.style.backgroundColor = rndCol;
  console.log(e);
}

btn.addEventListener("click", bgChange);
```

> [!NOTE]
> Вы можете найти [исходник кода](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/events/random-color-eventobject.html) для этого примера на GitHub (также [взгляните на его выполнение](https://mdn.github.io/learning-area/javascript/building-blocks/events/random-color-eventobject.html)).

Итак в коде выше мы включаем объект события **`e`** в функцию, а в функции — настройку стиля фона для `e.target`, который является кнопкой. Свойство объекта события `target` всегда является ссылкой на элемент, с которым только что произошло событие. Поэтому в этом примере мы устанавливаем случайный цвет фона на кнопке, а не на странице.

> [!NOTE]
> Вместо `e`/`evt`/`event` можно использовать любое имя для объекта события, которое затем можно использовать для ссылки на него в функции обработчика событий. `e`/`evt`/`event` чаще всего используются разработчиками, потому что они короткие и легко запоминаются. И хорошо придерживаться стандарта.

`e.target` применяют, когда нужно установить один и тот же обработчик событий на несколько элементов и, когда на них происходит событие, применить определённое действие к ним ко всем. Например, у вас может быть набор из 16 плиток, которые исчезают при нажатии. Полезно всегда иметь возможность просто указать, чтобы объект исчез, как `e.target`, вместо того, чтобы выбирать его более сложным способом. В следующем примере (см. исходный код на [useful-eventtarget.html](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/events/useful-eventtarget.html),а как он работает можно посмотреть [здесь](https://mdn.github.io/learning-area/javascript/building-blocks/events/useful-eventtarget.html)), мы создаём 16 элементов {{htmlelement ("div")}} с использованием JavaScript. Затем мы выберем все из них, используя {{domxref ("document.querySelectorAll()")}}, и с помощью цикла `for` выберем каждый из них, добавив обработчик `onclick` к каждому так, чтобы случайный цвет применялся к каждому клику:

```js
var divs = document.querySelectorAll("div");

for (var i = 0; i < divs.length; i++) {
  divs[i].onclick = function (e) {
    e.target.style.backgroundColor = bgChange();
  };
}
```

Результат выглядит следующим образом (попробуйте щёлкнуть по нему):

```html hidden
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Useful event target example</title>
    <style>
      div {
        background-color: #ff6600;
        height: 100px;
        width: 25%;
        float: left;
      }
    </style>
  </head>
  <body>
    <script>
      for (var i = 1; i <= 16; i++) {
        var myDiv = document.createElement("div");
        document.body.appendChild(myDiv);
      }

      function random(number) {
        return Math.floor(Math.random() * number);
      }

      function bgChange() {
        var rndCol =
          "rgb(" + random(255) + "," + random(255) + "," + random(255) + ")";
        return rndCol;
      }

      var divs = document.querySelectorAll("div");

      for (var i = 0; i < divs.length; i++) {
        divs[i].onclick = function (e) {
          e.target.style.backgroundColor = bgChange();
        };
      }
    </script>
  </body>
</html>
```

{{ EmbedLiveSample('Hidden_example', '100%', 400) }}

Большинство обработчиков событий, с которыми вы столкнулись, имеют только стандартный набор свойств и функций (методов), доступных для объекта события (см. {{domxref("Event")}} для ссылки на полный список). Однако некоторые более продвинутые обработчики добавляют специальные свойства, содержащие дополнительные данные, которые им необходимо выполнять. Например, [Media Recorder API](/ru/docs/Web/API/MediaStream_Recording_API) имеет событие, доступное для данных, которое срабатывает, когда записано какое-либо аудио или видео и доступно для выполнения чего-либо (например, для сохранения или воспроизведения). Соответствующий объект события [ondataavailable](/ru/docs/Web/API/MediaRecorder/dataavailable_event) handler имеет свойство данных, содержащее записанные аудио- или видеоданные, чтобы вы могли получить к нему доступ и что-то сделать с ним.

### Предотвращение поведения по умолчанию

Иногда бывают ситуации, когда нужно остановить событие, выполняющее то, что оно делает по умолчанию. Наиболее распространённым примером является веб-форма, например, пользовательская форма регистрации. Когда вы вводите данные и нажимаете кнопку отправки, естественное поведение заключается в том, что данные должны быть отправлены на указанную страницу на сервере для обработки, а браузер перенаправляется на страницу с сообщением об успехе (или остаться на той же странице, если другое не указано).

Но если пользователь отправил данные не правильно, как разработчик, вы хотите остановить отправку на сервер и выдать сообщение об ошибке с информацией о том, что не так и что нужно сделать. Некоторые браузеры поддерживают функции автоматической проверки данных формы, но, поскольку многие этого не делают, вам не следует полагаться на них и выполнять свои собственные проверки валидации. Давайте посмотрим на простой пример.

Простая форма HTML, в которой требуется ввести ваше имя и фамилию:

```html
<form>
  <div>
    <label for="fname">Имя: </label>
    <input id="fname" type="text" />
  </div>
  <div>
    <label for="lname">Фамилия: </label>
    <input id="lname" type="text" />
  </div>
  <div>
    <input id="submit" type="submit" />
  </div>
</form>
<p></p>
```

```css hidden
div {
  margin-bottom: 10px;
}
```

В JavaScript мы реализуем очень простую проверку внутри обработчика события [onsubmit](/ru/docs/Web/API/HTMLFormElement/submit_event) (событие "отправить" запускается в форме, когда оно отправлено), который проверяет, пусты ли текстовые поля. Если они пусты, мы вызываем функцию [`preventDefault()`](/ru/docs/Web/API/Event/preventDefault) объекта события, которая останавливает отправку формы, а затем выводит сообщение об ошибке в абзаце ниже нашей формы, чтобы сообщить пользователю, что не так:

```js
var form = document.querySelector("form");
var fname = document.getElementById("fname");
var lname = document.getElementById("lname");
var submit = document.getElementById("submit");
var para = document.querySelector("p");

form.onsubmit = function (e) {
  if (fname.value === "" || lname.value === "") {
    e.preventDefault();
    para.textContent = "Оба поля должны быть заполнены!";
  }
};
```

Очевидно, что это довольно слабая проверка формы - это не помешает пользователю отправить форму с пробелами или цифрами, введёнными в поля, но для примера подходит. Вывод выглядит следующим образом:

{{ EmbedLiveSample('Предотвращение_поведения_по_умолчанию', '100%', 140) }}

> [!NOTE]
> Чтобы увидеть исходный код, откройте [preventdefault-validation.html](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/events/preventdefault-validation.html) (также [запустите](https://mdn.github.io/learning-area/javascript/building-blocks/events/preventdefault-validation.html) здесь).

### Всплытие и перехват событий

Последним предметом для рассмотрения в этой теме является то, с чем вы не часто будете сталкиваться, но это может стать настоящей головной болью, если вы не поймёте, как работает следующий механизм. _Всплытие_ и _перехват событий_ — два механизма, описывающих, что происходит, когда два обработчика одного и того же события активируются на одном элементе. Посмотрим на пример. Чтобы сделать это проще — откройте пример [show-video-box.html](https://mdn.github.io/learning-area/javascript/building-blocks/events/show-video-box.html) в одной вкладке и [исходный код](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/events/show-video-box.html) в другой вкладке. Он также представлен ниже:

```html hidden
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Show video box example</title>
    <style>
      div {
        position: absolute;
        top: 50%;
        transform: translate(-50%, -50%);
        width: 550px;
        height: 420px;
        border-radius: 10px;
        background-color: #eee;
        background-image: linear-gradient(
          to bottom,
          rgba(0, 0, 0, 0.1),
          rgba(0, 0, 0, 0.4)
        );
      }

      .hidden {
        left: -50%;
      }

      .showing {
        left: 50%;
      }

      div video {
        display: block;
        width: 400px;
        margin: 50px auto;
      }
    </style>
  </head>
  <body>
    <button>Display video</button>

    <div class="hidden">
      <video>
        <source
          src="https://raw.githubusercontent.com/mdn/learning-area/master/javascript/building-blocks/events/rabbit320.mp4"
          type="video/mp4" />
        <source
          src="https://raw.githubusercontent.com/mdn/learning-area/master/javascript/building-blocks/events/rabbit320.webm"
          type="video/webm" />
        <p>
          Your browser doesn't support HTML5 video. Here is a
          <a href="rabbit320.mp4">link to the video</a> instead.
        </p>
      </video>
    </div>

    <script>
      var btn = document.querySelector("button");
      var videoBox = document.querySelector("div");
      var video = document.querySelector("video");

      btn.onclick = function () {
        displayVideo();
      };

      function displayVideo() {
        if (videoBox.getAttribute("class") === "hidden") {
          videoBox.setAttribute("class", "showing");
        }
      }

      videoBox.addEventListener("click", function () {
        videoBox.setAttribute("class", "hidden");
      });

      video.addEventListener("click", function () {
        video.play();
      });
    </script>
  </body>
</html>
```

{{ EmbedLiveSample('Hidden_video_example', '100%', 500) }}

Это довольно простой пример, который показывает и скрывает {{htmlelement ("div")}} с элементом {{htmlelement ("video")}} внутри него:

```html
<button>Display video</button>

<div class="hidden">
  <video>
    <source src="rabbit320.mp4" type="video/mp4" />
    <source src="rabbit320.webm" type="video/webm" />
    <p>
      Your browser doesn't support HTML5 video. Here is a
      <a href="rabbit320.mp4">link to the video</a> instead.
    </p>
  </video>
</div>
```

При нажатии на кнопку {{htmlelement ("button")}}, изменяется атрибут класса элемента `<div>` с `hidden` на `showing` (CSS примера содержит эти два класса, которые размещают блок вне экрана и на экране соответственно):

```css
div {
        position: absolute;
        top: 50%;
        transform: translate(-50%,-50%);
        ...
      }
.hidden {
   left: -50%;
  }
.showing {
   left: 50%;
  }
```

```js
var btn = document.querySelector("button");
btn.onclick = function () {
  videoBox.setAttribute("class", "showing");
};
```

Затем мы добавляем ещё пару обработчиков событий `onclick.` Первый к `<div>`, а второй к `<video>`. Идея заключается в том, чтобы при щелчке по области `<div>` вне зоны видео поле снова скрылось, а при клике в области `<video>` видео начало воспроизводиться.

```js
var videoBox = document.querySelector("div");
var video = document.querySelector("video");

videoBox.onclick = function () {
  videoBox.setAttribute("class", "hidden");
};

video.onclick = function () {
  video.play();
};
```

Но есть проблема: когда вы нажимаете на видео, оно начинает воспроизводиться, но одновременно вызывает скрытие `<div>`. Это связано с тем, что видео находится внутри `<div>,` это часть его, поэтому нажатие на видео фактически запускает оба вышеуказанных обработчика событий.

#### Всплытие и перехват событий — концепция выполнения

Когда событие инициируется элементом, который имеет родительские элементы (например, {{htmlelement ("video")}} в нашем случае), современные браузеры выполняют две разные фазы — фазу **захвата** и фазу **всплытия**.

На стадии **захвата** происходит следующее:

- Браузер проверяет, имеет ли самый внешний элемент ({{htmlelement ("html")}}) обработчик события `onclick`, зарегистрированный на нем на этапе захвата и запускает его, если это так.
- Затем он переходит к следующему элементу внутри `<html>` и выполняет то же самое, затем следующее и так далее, пока не достигнет элемента, на который на самом деле нажали.

На стадии **всплытия** происходит полная противоположность:

- Браузер проверяет, имеет ли элемент, который был фактически нажат, обработчик события `onclick`, зарегистрированный на нем в фазе всплытия, и запускает его, если это так.
- Затем он переходит к следующему непосредственному родительскому элементу и выполняет то же самое, затем следующее и так далее, пока не достигнет элемента `<html>`.

[![](bubbling-capturing.png)](bubbling-capturing.png)

(Нажмите на изображение, чтобы увеличить диаграмму)

В современных браузерах по умолчанию все обработчики событий регистрируются в фазе **_всплытия_**. Итак, в нашем текущем примере, когда вы нажимаете видео, событие click вызывается из элемента `<video>` наружу, в элемент `<html>`. По пути:

- Он находит обработчик `video.onclick...` и запускает его, поэтому видео сначала начинает воспроизводиться.
- Затем он находит обработчик `videoBox.onclick...` и запускает его, поэтому видео также скрывается.

#### Исправление проблемы с помощью stopPropagation()

Чтобы исправить это раздражающее поведение, стандартный объект события имеет функцию, называемую [`stopPropagation()`](/ru/docs/Web/API/Event/stopPropagation), которая при вызове в обработчике событий объекта делает так, чтобы обработчик выполнялся, но событие не всплывало дальше по цепочке, поэтому не будут запускаться другие обработчики.

Поэтому мы можем исправить нашу текущую проблему, изменив вторую функцию-обработчик в предыдущем блоке кода:

```js
video.onclick = function (e) {
  e.stopPropagation();
  video.play();
};
```

Вы можете попробовать создать локальную копию [show-video-box.html](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/events/show-video-box.html) и попробовать его самостоятельно исправить или просмотреть исправленный результат в [show-video-box-fixed.html](https://mdn.github.io/learning-area/javascript/building-blocks/events/show-video-box-fixed.html) (также см. [исходный код здесь](https://github.com/mdn/learning-area/blob/master/javascript/building-blocks/events/show-video-box-fixed.html)).

> [!NOTE]
> Зачем беспокоиться как с захватом, так и с всплыванием? Что ж, в старые добрые времена, когда браузеры были менее совместимы, чем сейчас, Netscape использовал только захват событий, а Internet Explorer использовал только всплывающие события. Когда W3C решил попытаться стандартизировать поведение и достичь консенсуса, они в итоге получили эту систему, которая включала в себя и то, и другое, что реализовано в одном из современных браузеров.

> [!NOTE]
> Как упоминалось выше, по умолчанию все обработчики событий регистрируются в фазе всплытия и это имеет смысл в большинстве случаев. Если вы действительно хотите зарегистрировать событие в фазе захвата, вы можете сделать это, зарегистрировав обработчик с помощью [`addEventListener()`](/ru/docs/Web/API/EventTarget/addEventListener) и установив для третьего дополнительного свойства значение `true`.

#### **Делегирование** события

Всплытие также позволяет нам использовать **делегирование событий.** Если у какого-либо родительского элемента есть множество дочерних элементов, и вы хотите, чтобы определённый код выполнялся при щелчке (событии) на каждом из дочерних элементов, можно установить обработчик событий на родительском элементе и события, происходящие на дочерних элементах будут всплывать до их родителя. При этом не нужно устанавливать обработчик событий на каждом дочернем элементе.

Хорошим примером является серия элементов списка. Если вы хотите, чтобы каждый из них, например, отображал сообщение при нажатии, вы можете установить обработчик событий `click` для родительского элемента `<ul>` и он будет всплывать в элементах списка.

Эту концепцию объясняет в своём блоге Дэвид Уолш, где приводит несколько примеров. (см. [How JavaScript Event Delegation Works](https://davidwalsh.name/event-delegate).)

## Вывод

Это все, что вам нужно знать о веб-событиях на этом этапе. Как уже упоминалось, события не являются частью основного JavaScript — они определены в веб-интерфейсах браузера ([Web API](/ru/docs/Web/API)).

Кроме того, важно понимать, что различные контексты, в которых используется JavaScript, обычно имеют разные модели событий — от веб-API до других областей, таких как браузерные расширения и Node.js (серверный JavaScript). Может сейчас вы не особо в этом разбираетесь, но по мере изучения веб-разработки начнёт приходить более ясное понимание тематики.

Если у вас возникли вопросы, попробуйте прочесть статью снова или [обратитесь за помощью к нам](/ru/docs/Learn_web_development#contact_us).

## Смотрите также

- [Event order](https://www.quirksmode.org/js/events_order.html) (обсуждение захвата и всплытий) — превосходно детализированная часть от Peter-Paul Koch.
- [Event accessing](https://www.quirksmode.org/js/events_access.html) (discussing of the event object) — another excellently detailed piece by Peter-Paul Koch.
- [Event reference](/ru/docs/Web/Events)

{{PreviousMenuNext("Learn/JavaScript/Building_blocks/Return_values","Learn/JavaScript/Building_blocks/Image_gallery", "Learn/JavaScript/Building_blocks")}}
