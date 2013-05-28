Ипользование консоли
============================================================

Джаваскрипт консоль предоставляет две функции для разработчиков для тестирования
веб-страниц и приложений:

* в консоль можно выводить отладочную информацию используя [Console API][1]:
такие методы как [console.log()][2] и [console.profile()][3];
* оболочка для работы в консоли, в которой вы можете вводить команды,
использовать автодополнение и оперировать функциями document и Chrome DevTools.
Вы можете выполнять выражения прямо в консоли, также вы можете использовать
методы из [Command Line API][4], такие как [$()][5] для выделения элементов, или
[profile()][6] для CPU-профайлера.

Эта документация расскажет про распространённые способы использования этих двух
API. Также вы можете посмотреть [Console API][7] and [Command Line API][8] в
качестве справочного материала.

* [Базовые операции](#basic-operation)
	* [Открытие консоли](#opening-the-console)
	* [Очистка консоли](#clearing-the-console-history)
	* [Настройки консоли](#console-settings)
* [Использование API консоли](#using-the-console-api)
	* [Вывод в консоль](#writing-to-the-console)
	* [Ошибки и предупреждения](#errors-and-warnings)
	* [Проверки](#assertions)
	* [Фильтрация вывода в консоли](#filtering-console-output)
	* [Группирование вывода](#grouping-output)
	* [Замена строк и их форматирование](#string-substitution-and-formatting)
	* [Представление DOM-элементов как джаваскрипт-объекты](#formatting-dom-elements-as-javascript-objects)
	* [Стилизация вывода консоли с помощью CSS](#styling-console-output-with-css)
	* [Измерение временных затрат](#measuring-how-long-something-takes)
	* [Корреляция с панелью Timeline](#marking-the-timeline)
	* [Создание точек останова](#setting-breakpoints-in-javascript)
* [Using the Command Line API](#using-the-command-line-api)
	* [Выполнение выражений](#evaluating-expressions)
	* [Выделение элементов](#selecting-elements)
	* [Инспектирование DOM-элементов и джаваскрипт объектов](#inspecting-dom-elements-and-javascript-heap-objects)
	* [Доступ к недавним элементам или объектам](#accessing-recently-selected-elements-and-objects)
	* [Отслеживание событий](#monitoring-events)
	* [Контроль за CPU-профайлером](#controlling-the-cpu-profiler)


## Базовые операции ##

### Открытие консоли ###

Джаваскрипт консоль доступна в двух вариантах в пределах Chrome DevTools (!!!): главная вкладка «Консоль» и также в виде половинчатой версии, доступной из любой другой вкладки.

Для того, чтобы открыть вкладку «Консоль», сделайте что-нибудь из этого списка:

* используйте хоткей **Command - Option - J** (Mac) или **Control - Shift - J**
(Windows/Linux);
* Выберите **View > Developer > JavaScript Console**.

![Console panel view][9]

Для переключения половинчатой версии консоли на других вкладках нажмите клавишу **Esc** или кликните на иконку **Show/Hide Console** в нижнем левом углу DevTools. Скриншот показывает половинчатую консоль в панели Elements.

![Console split-view][10]

### Очистка консоли ###

Для очистки вывода консоли, сделайте что-нибудь из нижеследующего:

* через контекстное меню панели консоли (клик правой кнопки мыши) и пункт **Clear Console**.
* Введите [clear()][11] — команду из Command Line API в консоли.
* Вызовите [console.clear()][12] (команду из Console API) в своём джаваскрипт коде.
* Используйте хоткеи **⌘K** или **⌃L** (Mac) **Control - L** (Windows и Linux).

По умолчанию, история консоли очищается при переходе на другую страницу. Вы можете поменять это включив **Preserve log upon navigation** в разделе консоли в настройках DevTools (см. [настройки Консоли][13]).

### Настройки консоли ###

Консоль имеет две главные опции, которые вы можете настраивать в главной вкладке настроек DevTools:

* **Log XMLHTTPRequests** — определяет логировать ли XMLHTTPRequest в панели консоли.
* **Preserve log upon navigation** — определяет сбрасываться ли истории консоли при переходе на новую страницу. По умолчанию, обе из этих опций отключены.

Вы можете поменять эти настройки в контекстном меню консоли, кликнув правой кнопкой мыши.

![Console panel view][14]


## Использование API консоли ##

Console API — набор методов доступных из объекта `console` объявленного из DevTools. Одним из главных назначений API — [логировать информацию][15](значение свойства, целый объект или DOM-элемент) в консоль во время работы вашего приложения. Вы также можете группировать вывод, чтобы избавиться от визуального мусора.

### Вывод в консоль ###

Метод [console.log()][16] принимает один и более параметров и выводит их текущие значения на консоль. Например:

	var a = document.createElement('p');
	a.appendChild(document.createTextNode('foo'));
	a.appendChild(document.createTextNode('bar'));
	console.log("Node count: " + a.childNodes.length);

![Console log output][17]

Вместо того, чтобы конкатенировать параметры функции с помощью оператора `+` (как показано выше), напротив, вы можете каждый параметр друг за другом через запятую и их значения будут выведены в одну строку разделённые пробелом.

	console.log("Node count:", a.childNodes.length, "and the current time is:", Date.now());

![Console log output][18]

### Ошибки и предупреждения ###

Метод [console.error()][19] выводит красную иконку рядом с сообщением красного цвета.

	function connectToServer() {
	    console.error("Error: %s (%i)", "Server is  not responding",500);
	}
	connectToServer();

![][20]

Метод [console.warn()][21] выводит жёлтую иконку рядом с текстом сообщения.

	if(a.childNodes.length < 3 ) {
	    console.warn('Warning! Too few nodes (%d)', a.childNodes.length);
	}

![Example of console.warn()][22]

### Проверки ###

Метод [console.assert()][23] выводит сообщение об ошибке (это второй аргумент) только в том случае, если первый аргумент равен `false`. К примеру, в следующем примере сообщение об ошибке появится только, если количество дочерних элементов DOM-элемента `list` больше пятисот.

	console.assert(list.childNodes.length < 500, "Node count is > 500");

![Example of console.assert()][24]

### Фильтрация вывода в консоли ###

Вы можете быстро фильтровать сообщения в консоли по их типу (уровню) — ошибки, предупреждения и стандартный лог — выбрав один из доступных опций внизу консоли, как показано ниже.

![Only show console.error() output][25]

Возможные фильтры:

* **All** — без какой-либо фильтрации.
* **Errors** — сообщения только из `console.error()`.
* **Warnings** — сообщения только из `console.warn()`.
* **Logs** — сообщения только из `console.log()`, `console.info()` и
`console.debug()`.
* **Debug** — сообщения только из `console.timeEnd()` и остальных функций консольного вывода.

### Группирование вывода ###

Вы можете визуально группировать вывод в консоли с помощью команд [console.group()][26] и [groupEnd()][27].

	var user = "jsmith", authenticated = false;
	console.group("Authentication phase");
	console.log("Authenticating user '%s'", user);
	// authentication code here...
	if (!authenticated) {
	    console.log("User '%s' not authenticated.", user)
	}
	console.groupEnd();

![Logging group example][28]

Также вы можете вкладывать группы логов друг в друга. В следующем примере группа логов создаётся для этапа аутентификации в процессе логина. Если пользователь ввёл верные данные, то создаётся вложенная группа для этой фазы.

	var user = "jsmith", authenticated = true, authorized = true;
	// Top-level group
	console.group("Authenticating user '%s'", user);
	if (authenticated) {
	    console.log("User '%s' was authenticated", user);
	    // Start nested group
	    console.group("Authorizing user '%s'", user);
	    if (authorized) {
	        console.log("User '%s' was authorized.", user);
	    }
	    // End nested group
	    console.groupEnd();
	}
	// End top-level group
	console.groupEnd();
	console.log("A group-less log trace.");

![Nested logging group example][29]

Для создания изначально свёрнутой групы используйте [console.groupCollapsed()][30] вместо `console.group()` как показано ниже:

	console.groupCollapsed("Authenticating user '%s'", user);
	if (authenticated) {
	  //…
	}

![Initially collapsed group][31]

### Замена строк и их форматирование ###

Первый параметр передаваемый в любой метод консоли (например, `log()` или `error()`) может содержать *модификаторы форматирования*. Модификатор вывода состоит из символа **`%`** сразу за которым следует буква, сообщающая о том, какое форматирование должно быть применено (например, `%s` — для строк). Модификатор форматирования определяет куда подставить значение переданное из следующих параметров функции.

В следующем примере используется строчный и числовой модификаторы `%s` (string) и `%d` (decimal) для замены значений в выводимой строке.

	console.log("%s has %d points", "Sam", "100");

Результатом будет «Sam has 100 points».

Приведённая таблица содержит поддерживаемые модификаторы форматирования и их значения:

<table>
<thead>
<tr>
	<th>Модификатор форматирования <th>Описание
<tbody>
<tr>
	<td><code>%s</code>
	<td>Форматирует значение как строку.
<tr>
	<td><code>%d</code> или <code>%i</code>
	<td>Форматирует значение как целое число (decimal и integer).
<tr>
	<td><code>%f</code>
	<td>Форматирует объект как число с плавающей точкой.
<tr>
	<td><code>%o</code>
	<td>Форматирует значение как открывающийся DOM-элемент (также как в панели Elements).
<tr>
	<td><code>%O</code>
	<td>Форматирует значение как открывающийся джаваскрипт-объект.
<tr>
	<td><code>%c</code>
	<td>Применяет CSS-стили к выводимой строке, переданные вторым параметром.
</table>

В следующем примере модификатор форматирования `%d` заменяется на значение `document.childNodes.length` и форматируется как целое число; модификатор `%f` заменяется на значение, возвращаемое `Date.now()` и форматируется в число с плавающей точкой.

	console.log("Node count: %d, and the time is %f.", document.childNodes.length, Date.now());

![Using format specifiers][32]

### Представление DOM-элементов как джаваскрипт-объекты ###

По умолчанию, когда вы логируете DOM-элемент в консоль, он выводится в XML-формате, как в панели Elements:

	console.log(document.body.firstElementChild);

![][33]

Вы можете вывести DOM-элемент в джаваскрипт-представлении с помощью метода `console.dir()`:

	console.dir(document.body.firstElementChild);

![][34]

Точно также вы можете использовать [модификатор вывода][35] `%0` в методе `console.log()`:

	console.log("%O", document.body.firstElementChild);

### Стилизация вывода консоли с помощью CSS ###

Вы можете использовать модификатор `%c`, чтобы применить СSS-правила, к любой строке, выводимой с помощью [console.log()][36] или похожих методов.

	console.log("%cThis will be formatted with large, blue text", "color: blue; font-size: x-large");

![Стилизация вывода консоли с помощью CSS][37]

### Измерение временных затрат ###

Вы можете использовать методы [console.time()][38] и [console.timeEnd()][39] для измерения того, как много времени потребовалось функции или операции для выполнения. Вы вызываете `console.time()` в точке, где вы хотите запустить таймер и `console.timeEnd()` — остановить. Время прошедшее между вызовами функций таймера будет выведено в консоль.

	console.time("Array initialize");
	var array= new Array(1000000);
	for (var i = array.length - 1; i >= 0; i--) {
	    array[i] = new Object();
	};
	console.timeEnd("Array initialize");

![Example of using console.time() and timeEnd()][40]

> Внимание: необходимо использовать одинаковые строки в параметрах `console.time()` and `timeEnd()` для ожидаемой работы таймера — считайте эту строку уникальным идентификатором таймера.


### Корреляция с панелью Timeline ###

Панель [Timeline][41] предоставляет подробный обзор того, куда было потрачено время работы вашего приложения. Метод [console.timeStamp()][42] создаёт отдельную отметку в момент своего исполнения. Это помогает легко и непринуждённо соотносить события в приложении с браузерными событиями reflow и repaint.

> Внимание: метод `console.timeStamp()` выполняется только при записи событий во вкладке Timeline.

В следующем примере в панели Timeline появляется отметка «Adding result» в тот момент, когда поток выполнения программы доходит до `console.timeStamp("Adding result")`

	function AddResult(name, result) {
	  console.timeStamp("Adding result");
	  var text = name + ': ' + result;
	  var results = document.getElementById("results");
	  results.innerHTML += (text + "<br>");
	}

Как проиллюстрированно в скриншоте, вызов `timeStamp()` отмечен в следующих местах:

* жёлтая вертикальная линия в панели Timeline.
* Запись добавлена в список записанных событий.
* A record is added to the list of recorded events.

![Timeline showing custom timestamp][43]

### Создание точек останова ###

Вы можете начать отладку вашего кода вызвав команду [debugger][44]. К примеру, в следующем коде отладка начинается сразу после вызова метода `brightness()`:

	brightness : function() {
	    debugger;
	    var r = Math.floor(this.red*255);
	    var g = Math.floor(this.green*255);
	    var b = Math.floor(this.blue*255);
	    return (r * 77 + g * 150 + b * 29) >> 8;
	}

![Example of using debugger command][45]


## Using the Command Line API ##

In addition to being a place where you can log information from your
application, the Console is also a shell prompt where you can directly evaluate
expressions or issue commands provided by the [Command Line API][46]. This API
provides the following features:

* Convenience functions for selecting DOM elements
* Methods for Контроль за CPU-профайлером
* Aliases for a number of Console API methods
* Отслеживание событий
* View event listeners registered on objects

### Выполнение выражений ###

The Console attempts to evaluate any JavaScript expression you enter at the
shell prompt, upon pressing the Return or Enter key. The Console provides auto-
completion and tab-completion. As you type expressions, property names are
automatically suggested. If there are multiple properties with the same prefix,
pressing the Tab key cycles through them. Pressing the right arrow key accepts
the current suggestion. The current suggestion is also accepted by pressing the
Tab key if there is only one matched property.

![][47]

To enter a multi-line expression at the shell prompt (such as a function
definition) press Shift+Enter between lines.

![][48]

### Выделение элементов ###

The Command Line API provides several methods to access DOM elements in your
application. For example, the [$()][49] method returns the first element that
matches the specified CSS selector, just like [document.querySelector()][50].
For instance, the following code returns the element with the ID "loginBtn".

	$('#loginBtn');

![][51]

The [$$()][52] command returns an array of all the elements that match the
specified CSS selector, just like [document.querySelectorAll()][53]. For
instance, the following displays selects all `<button>` elements with the CSS
class "loginBtn":

	$$('button.loginBtn');

![][54]

Lastly  the [x()][55] method takes an XPath path as a parameter and returns an
array of all elements that match the specified path. The following returns all
the `<script>` elements that are children of the `<body>` tag:

	$x('/html/body/script');

### Инспектирование DOM-элементов и джаваскрипт объектов ###

The [inspect()][56] method takes a DOM element reference (or JavaScript
reference) as a parameter and displays the element or object in the appropriate
panel—the Elements panel for DOM elements, or the Profile panel for a JavaScript
object.

For example, in the following screenshot the `$()` function is used to get a
reference to an `<li>` element. Then the last evaluated expression property
([$_][57]) is passed to `inspect()` to open that element in the Elements panel.

![][58]

### Доступ к недавним элементам или объектам ###

Often when testing you'll select DOM elements—either directly in the Elements
panel or using the Selection tool (magnifying glass)—so that you can further
inspect the element. Or, when analyzing a memory snapshot in the Profiles panel,
you might select a JavaScript object to further inspect it.

The Console remembers the last five elements (or heap objects) you've selected
and makes them available as properties named [$0, $1, $2, $3][59] and [$4][60].
The most recently selected element or object is available as **`$0`**, the
second most as **`$1`**, and so forth.

The following screenshot shows the values of these properties after selecting
three different elements in turn from the Elements panel:

![Recently selected elements][61]



### Отслеживание событий ###

The [monitorEvents()][62] command monitors an object for one or more specified
events. When an event occurs on the monitored object, the corresponding Event
object is logged to the Console. You specify the object and the events you want
to monitor on that object. For example, the following code enables event
monitoring for every "resize" event on the global window object.

	monitorEvents(window, "resize");

![Monitoring window resize events][63]

To monitor several events, you can pass an array of event names as the second
parameter. The code below monitors both "mousedown" and "mouseup" events on the
body of the document.

	monitorEvents(document.body, ["mousedown", "mouseup"]);

You can also pass one of the supported "event types" that DevTools maps to a set
of actual event names. For example, the "touch" event type cause DevTools to
monitor "touchstart", "touchend", "touchmove", and "touchcancel" events on the
target object.

	monitorEvents($('#scrollBar'), "touch");

See [monitorEvents()][64] in the Console API Reference for a list of supported
event types.

To stop monitoring events call `unmonitorEvents()`, passing the object to stop
monitoring.

	unmonitorEvents(window);

### Контроль за CPU-профайлером ###

You can create JavaScript CPU profiles from the command line with the
[profile()][65] and [profileEnd()][66] commands. You can optionally specify a
name that's applied to the profile you create.

С помощью комманд [profile()][65] и [profileEnd()][66] можно создавать
JavaScript профили CPU. По желанию можно задать профилю имя.

For example, the following shows an example of creating a new profile with the
default name:

Ниже вы видите пример создания нового профиля с именем назначенным по умолчанию.

![][67]

The new profile appears in the Profiles panel under the name "Profile 1":

Новый профиль появляется во вкладке Profiles с именем "Profile 1":

![][68]

If you specify a label for the new profile, it is used as the new profile's
heading. If you create multiple profiles with the same name, they are grouped as
individual runs under the same heading:

Если вы задаете имя для нового профиля, то она она используется в качестве
его заголовка. Если вы создаете множество профилей с одинаковыми именами, то
они групируются под одним заголовком.

![][69]

The result in the Profiles panel:

Результат во вкладке Profiles:

![][70]

CPU profiles can be nested, for example:

Профили CPU могут быть вложенные, например:

	profile("A");profile("B");profileEnd("B")profileEnd("A")

The calls to stop and start profiling do not need be properly nested. For
example, the following works the same as the previous example:

Комманды остановки и запуска профилирования не обязательно должны быть корректно
вложены друг в друга. Например, этот пример будет работать так же, как и предидущий:

	profile("A");profile("B"); profileEnd("A");profileEnd("B");






[1]: https://developers.google.com/chrome-developer-tools/docs/console-api
[2]: https://developers.google.com/chrome-developer-tools/docs/console-api#consolelogobject_object
[3]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoleprofilelabel
[4]: https://developers.google.com/chrome-developer-tools/docs/commandline-api
[5]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#selector
[6]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#profile
[7]: https://developers.google.com/chrome-developer-tools/docs/console-api
[8]: https://developers.google.com/chrome-developer-tools/docs/commandline-api
[9]: ./img/console1.png "Console panel view"
[10]: ./img/console-split-view.png "Console split-view"
[11]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#clear
[12]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoleclear
[13]: https://developers.google.com/chrome-developer-tools/docs/console#consolepreferences
[14]: ./img/console-context-menu.png "Console panel view"
[15]: https://developers.google.com/chrome-developer-tools/docs/console#writing_to_the_console
[16]: https://developers.google.com/chrome-developer-tools/docs/console-api#consolelogobject_object
[17]: ./img/log-basic.png "Console log output"
[18]: ./img/log-multiple.png "Console log output"
[19]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoleerrorobject_object
[20]: ./img/error-server-not-resp.png
[21]: https://developers.google.com/chrome-developer-tools/docs/console-api#consolewarnobject_object
[22]: ./img/warning-too-few-nodes.png "Example of console.warn()"
[23]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoleassertexpression_object
[24]: ./img/assert-failed.png "Example of console.assert()"
[25]: ./img/filter-errors.png "Only show console.error() output"
[26]: https://developers.google.com/chrome-developer-tools/docs/console-api#consolegroupobject_object
[27]: https://developers.google.com/chrome-developer-tools/docs/console-api#consolegroupend
[28]: ./img/group.png "Logging group example"
[29]: ./img/nestedgroup.png "Nested logging group example"
[30]: https://developers.google.com/chrome-developer-tools/docs/console-api#consolegroupcollapsed
[31]: ./img/groupcollapsed.png "Initially collapsed group"
[32]: ./img/format-substitution.png "Using format specifiers"
[33]: ./img/log-element.png
[34]: ./img/dir-element.png
[35]: https://developers.google.com/chrome-developer-tools/docs/console#string_substitution_and_formatting
[36]: https://developers.google.com/chrome-developer-tools/docs/console#writingtotheconsole
[37]: ./img/format-string.png "Стилизация вывода консоли с помощью CSS"
[38]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoletimelabel
[39]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoletimeendlabel
[40]: ./img/time-duration.png "Example of using console.time() and timeEnd()"
[41]: https://developers.google.com/chrome-developer-tools/docs/timeline
[42]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoletimestamplabel
[43]: ./img/timestamp2.png "Timeline showing custom timestamp"
[44]: https://developers.google.com/chrome-developer-tools/docs/console-api#debugger
[45]: ./img/debugger.png "Example of using debugger command"
[46]: https://developers.google.com/chrome-developer-tools/docs/commandline-api
[47]: ./img/evaluate-expressions.png
[48]: ./img/multiline-expression.png
[49]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#selector
[50]: http://docs.webplatform.org/wiki/css/selectors_api/querySelector
[51]: ./img/select-login-btn.png
[52]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#selector_1
[53]: http://docs.webplatform.org/wiki/css/selectors_api/querySelectorAll
[54]: ./img/select-multiple-login.png
[55]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#xpath
[56]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#inspectobject
[57]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#_
[58]: ./img/inspect2.png
[59]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#0_-_4
[60]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#0_-_4
[61]: ./img/recent-selection.png "Recently selected elements"
[62]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#monitoreventsobject_events
[63]: ./img/monitor-resize.png "Monitoring window resize events"
[64]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#monitoreventsobject_events
[65]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#profilename
[66]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#profileendname
[67]: ./img/profile-console.png
[68]: ./img/profile-panel.png
[69]: ./img/profile-console-2.png
[70]: ./img/profile-panel-2.png
