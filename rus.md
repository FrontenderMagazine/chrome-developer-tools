# Использование консоли

Консоль предоставляет две функции для разработчиков для тестирования веб-страниц и
приложений:

* в консоль можно выводить отладочную информацию используя такие методы
[Console API][1], как [console.log()][2] и [console.profile()][3].
* оболочка для работы в консоли, в которой вы можете вводить команды, в частности
используя автодополнение, и оперировать document и Chrome DevTools.
Вы можете выполнять JavaScript-выражения прямо в консоли и использовать
методы [Command Line API][4], например, [$()][5] для создания выборки элементов,
или [profile()][6] для запуска CPU-профайлера.

В статье мы рассмотрим наиболее распространённые способы использования
[Console API][7] и [Command Line API][8]. Более подробно о них вы сможете
узнать в документации.

* [Базовые операции](#--1)
	* [Открытие консоли](#--2)
	* [Очистка консоли](#--3)
	* [Настройки консоли](#--4)
* [Использование API консоли](#-api-)
	* [Вывод в консоль](#--)
	* [Ошибки и предупреждения](#---1)
	* [Проверки](#assertions)
	* [Фильтрация вывода в консоли](#filtering-console-output)
	* [Группирование вывода](#grouping-output)
	* [Замена строк и их форматирование](#string-substitution-and-formatting)
	* [Представление DOM-элементов как JavaScript-объекты](#formatting-dom-elements-as-javascript-objects)
	* [Стилизация вывода консоли с помощью CSS](#styling-console-output-with-css)
	* [Измерение временных затрат](#measuring-how-long-something-takes)
	* [Корреляция с панелью Timeline](#marking-the-timeline)
	* [Создание точек останова](#setting-breakpoints-in-javascript)
* [Using the Command Line API](#using-the-command-line-api)
	* [Выполнение выражений](#evaluating-expressions)
	* [Выделение элементов](#selecting-elements)
	* [Инспектирование DOM-элементов и JavaScript объектов](#inspecting-dom-elements-and-javascript-heap-objects)
	* [Доступ к недавним элементам или объектам](#accessing-recently-selected-elements-and-objects)
	* [Отслеживание событий](#monitoring-events)
	* [Контроль за CPU-профайлером](#controlling-the-cpu-profiler)




<a href="" id="--1"></a>
## Базовые операции ##


<a href="" id="--2"></a>
### Открытие консоли ###

Консоль в Chrome DevTools доступна в двух вариантах: вкладка Console и
в виде минимизированной версии, доступной из любой другой вкладки.

Для того, чтобы открыть вкладку Console вы можете:

* используйте хоткей **Command - Option - J** (Mac) или **Control - Shift - J**
(Windows/Linux);
* Выберите **View > Developer > JavaScript Console**.

![Console panel view][9]

Для того, что бы вызвать или скрыть минимизированную версии консоли в других
панелях нажмите клавишу **Esc** или кликните на иконку **Show/Hide Console** в
нижнем левом углу DevTools. Скриншот показывает минимизированную консоль в
панели Elements.

![Console split-view][10]


<a href="" id="--3"></a>
### Очистка консоли ###

Для очистки консоли:

* через контекстное меню панели консоли (клик правой кнопки мыши) выберите пункт
**Clear Console**.
* Введите [clear()][11] — команду из Command Line API в консоли.
* Вызовите [console.clear()][12] (команду из Console API) из скрипта.
* Используйте хоткеи **⌘K** или **⌃L** (Mac) **Control - L** (Windows и Linux).

По умолчанию, история консоли очищается при переходе на другую страницу.
Вы можете отменить очистку включив **Preserve log upon navigation** в разделе
консоли в настройках DevTools (см. [настройки Консоли][13]).


<a href="" id="--4"></a>
### Настройки консоли ###

Консоль имеет две главные опции, которые вы можете настраивать в главной вкладке
настроек DevTools:

* **Log XMLHTTPRequests** — определяет логировать ли XMLHTTPRequest
в панели консоли.
* **Preserve log upon navigation** — определяет сбрасываться ли истории консоли
при переходе на новую страницу. По умолчанию, обе из этих опций отключены

Вы можете поменять эти настройки в контекстном меню консоли, кликнув правой
кнопкой мыши.

![Console panel view][14]



<a href="" id="-api-"></a>
## Использование API консоли ##

Console API — набор методов доступных из объекта `console` объявленного из
DevTools. Одно из главных назначений API — [логировать информацию][15](значение
свойства, целый объект или DOM-элемент) в консоль во время работы вашего
приложения. Вы также можете группировать вывод, чтобы избавиться от визуального
мусора.


<a href="" id="--"></a>
### Вывод в консоль ###

Метод [console.log()][16] принимает один и более параметров и выводит их текущие
значения на консоль. Например:

	var a = document.createElement('p');
	a.appendChild(document.createTextNode('foo'));
	a.appendChild(document.createTextNode('bar'));
	console.log("Node count: " + a.childNodes.length);

![Console log output][17]

Вместо того, чтобы конкатенировать параметры функции с помощью оператора `+` (как
показано выше), вы можете ввести каждый параметр друг за другом через запятую и
их значения будут выведены в одну строку разделённые пробелом.

	console.log("Node count:", a.childNodes.length, "and the current time is:", Date.now());

![Console log output][18]

<a href="" id="---1"></a>
### Ошибки и предупреждения ###

Метод [console.error()][19] выводит красную иконку рядом с сообщением красного
цвета.

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

<a href="" id="assertions"></a>
### Проверки ###

Метод [console.assert()][23] выводит сообщение об ошибке (это второй аргумент)
только в том случае, если первый аргумент равен `false`. К примеру, в следующем
примере сообщение об ошибке появится только, если количество дочерних элементов
DOM-элемента `list` больше пятисот.

	console.assert(list.childNodes.length < 500, "Node count is > 500");

![Example of console.assert()][24]


### Фильтрация вывода в консоли ###

Вы можете быстро фильтровать сообщения в консоли по их типу (уровню) — ошибки,
предупреждения и стандартный лог — выбрав один из доступных опций внизу консоли:

![Only show console.error() output][25]

Возможные фильтры:

* **All** — без фильтрации.
* **Errors** — сообщения `console.error()`.
* **Warnings** — сообщения `console.warn()`.
* **Logs** — сообщения `console.log()`, `console.info()` и
`console.debug()`.
* **Debug** — сообщения `console.timeEnd()` и остальных функций
консольного вывода.


### Группирование вывода ###

Вы можете визуально группировать вывод в консоли с помощью команд
[console.group()][26] и [groupEnd()][27].

	var user = "jsmith", authenticated = false;
	console.group("Authentication phase");
	console.log("Authenticating user '%s'", user);
	// authentication code here...
	if (!authenticated) {
	    console.log("User '%s' not authenticated.", user)
	}
	console.groupEnd();

![Logging group example][28]

Также вы можете вкладывать группы логов друг в друга. В следующем примере группа
логов создаётся для этапа аутентификации в процессе логина. Если пользователь
аутентифицирован, то создаётся вложенная группа для этапа авторизации.

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

Для создания изначально свёрнутой группы используйте [console.groupCollapsed()][30]
вместо `console.group()`:

	console.groupCollapsed("Authenticating user '%s'", user);
	if (authenticated) {
	  //…
	}

![Initially collapsed group][31]


### Замена строк и их форматирование ###

Первый параметр передаваемый в любой метод консоли (например, `log()` или
`error()`) может содержать *модификаторы форматирования*. Модификатор вывода
состоит из символа `%` сразу за которым следует буква, сообщающая о том, какое
форматирование должно быть применено (например, `%s` — для строк). Модификатор
форматирования определяет куда подставить значение переданное из следующих
параметров функции.

В следующем примере используется строчный и числовой модификаторы `%s` (string) и
`%d` (decimal) для замены значений в выводимой строке.

	console.log("%s has %d points", "Sam", "100");

Результатом будет «Sam has 100 points».

Приведённая таблица содержит поддерживаемые модификаторы форматирования и их
значения:

<table class="test-table">
	<thead>
		<tr>
			<th>Модификатор форматирования
			<th>Описание
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>
				<code>%s</code>
			</td>
			<td>
				Форматирует значение как строку.
			</td>
		</tr>
		<tr>
			<td>
				<code>%d</code> или <code>%i</code>
			</td>
			<td>
				Форматирует значение как целое число (decimal и integer).
			</td>
		</tr>
		<tr>
			<td>
				<code>%f</code>
			</td>
			<td>
				Форматирует объект как число с плавающей точкой.
			</td>
		</tr>
		<tr>
			<td>
				<code>%o</code>
			</td>
			<td>
				Форматирует значение как открывающийся DOM-элемент (также как в панели Elements).
			</td>
		</tr>
		<tr>
			<td>
				<code>%O</code>
			</td>
			<td>
				Форматирует значение как открывающийся JavaScript-объект.
			</td>
		</tr>
		<tr>
			<td>
				<code>%c</code>
			</td>
			<td>
				Применяет CSS-стили к выводимой строке, переданные вторым параметром.
			</td>
		</tr>
	</tbody>
</table>

В следующем примере модификатор форматирования `%d` заменяется на значение
`document.childNodes.length` и форматируется как целое число; модификатор `%f`
заменяется на значение, возвращаемое `Date.now()` и форматируется как число с
плавающей точкой.

	console.log("Node count: %d, and the time is %f.", document.childNodes.length, Date.now());

![Using format specifiers][32]

### Представление DOM-элементов как JavaScript-объекты ###

По умолчанию, когда вы логируете DOM-элемент в консоль, он выводится в XML-
формате, как в панели Elements:

	console.log(document.body.firstElementChild);

![][33]

Вы можете вывести DOM-элемент в JavaScript-представлении с помощью метода
`console.dir()`:

	console.dir(document.body.firstElementChild);

![][34]

Точно также вы можете использовать [модификатор вывода][35] `%0` в методе
`console.log()`:

	console.log("%O", document.body.firstElementChild);


### Стилизация вывода консоли с помощью CSS ###

Вы можете использовать модификатор `%c`, чтобы применить СSS-правила, к любой
строке, выводимой с помощью [console.log()][36] или похожих методов.

	console.log("%cThis will be formatted with large, blue text", "color: blue; font-size: x-large");

![Стилизация вывода консоли с помощью CSS][37]


### Измерение временных затрат ###

Вы можете использовать методы [console.time()][38] и [console.timeEnd()][39]
для измерения того, как много времени потребовалось функции или операции для
выполнения. Вы вызываете `console.time()` что бы запустить таймер и
`console.timeEnd()` для его остановки. Время прошедшее между вызовами этих
функций будет выведено в консоль.

	console.time("Array initialize");
	var array= new Array(1000000);
	for (var i = array.length - 1; i >= 0; i--) {
	    array[i] = new Object();
	};
	console.timeEnd("Array initialize");

![Example of using console.time() and timeEnd()][40]

> Внимание: необходимо использовать одинаковые строки в параметрах
`console.time()` and `timeEnd()` для ожидаемой работы таймера — считайте эту
строку уникальным идентификатором таймера.


### Корреляция с панелью Timeline ###

Панель [Timeline][41] предоставляет подробный обзор того, куда было потрачено
время работы вашего приложения. Метод [console.timeStamp()][42] создаёт отдельную
отметку в момент своего исполнения. Это помогает легко и непринуждённо соотносить
события в приложении с браузерными событиями reflow и repaint.

> Внимание: метод `console.timeStamp()` выполняется только при записи событий в
панели Timeline.

В следующем примере в панели Timeline появляется отметка «Adding result» в тот
момент, когда поток выполнения программы доходит до `console.timeStamp("Adding
result")`

	function AddResult(name, result) {
	  console.timeStamp("Adding result");
	  var text = name + ': ' + result;
	  var results = document.getElementById("results");
	  results.innerHTML += (text + "<br>");
	}

Как проиллюстрировано в скриншоте, вызов `timeStamp()` отмечен в следующих
местах:

* жёлтая вертикальная линия в панели Timeline.
* Запись добавлена в список записанных событий.

![Timeline showing custom timestamp][43]


### Создание точек останова ###

Вы можете начать отладку вашего кода вызвав команду [debugger][44]. К примеру, в
следующем коде отладка начинается сразу после вызова метода `brightness()`:

	brightness : function() {
	    debugger;
	    var r = Math.floor(this.red*255);
	    var g = Math.floor(this.green*255);
	    var b = Math.floor(this.blue*255);
	    return (r * 77 + g * 150 + b * 29) >> 8;
	}

![Example of using debugger command][45]

## Использование Command Line API ##

Кроме того, что консоль это место вывода логов вашего приложения, вы можете
вводить в нее команды, определенные в [Command Line API][46].  Это API дает
следующие возможности:

* удобные функции для выделения DOM-элементов.
* Методы управления CPU-профайлером.
* Псевдонимы для некоторых методов Console API.
* Отслеживание событий.
* Просмотр обработчиков событий объекта.


### Выполнение выражений ###

Консоль стремится выполнить любой JavaScript-код, который вы в неё введёте, сразу
после нажатия кнопки Return или Enter. В консоли работает автокомплит сам по себе
и по табу. По мере того, как вы печатаете названия переменных и свойств
предлагаются автоматически. Если существуют несколько одинаково начинающихся
свойств, вы можете выбирать между ними нажимая Tab. Нажав стрелку вправо вы
выберете текущую подсказку. Также текущая подсказка будет выбрана и нажатием по
табу, если подсказка только одна.

![][47]

Для того, чтобы вводить многострочные JavaScript-выражения используйте сочетание
клавиш Shift+Enter между строчек.

![][48]


### Выделение элементов ###

Command Line API предоставляет набор методов для доступа к DOM-элементам в вашем
приложении. К примеру, метод [$()][49] возвращает первый элемент, соответствующий
объявленному CSS-селектору, идентично с [document.querySelector()][50]. Следующий
код вернёт элемент с ID «loginBtn».

	$('#loginBtn');

![][51]

Метод [$$()][52] возвращает массив элементов, соответствующих указанному CSS-
селектору, идентично [document.querySelectorAll()][53]. Следующий код покажет все
кнопки с классом `loginBtn`:

	$$('button.loginBtn');

![][54]

Наконец-то, метод [x()][55] принимает XPath-путь в качестве параметра и возвращает
массив элементов, подпадающих под указанный путь. Например, этот код вернёт все
элементы `<script>`, являющиеся дочерними по отношению к элементу `<body>`:

	$x('/html/body/script');


### Инспектирование DOM-элементов и JavaScript объектов ###

Метод [inspect()][56] принимает ссылку на DOM-элемент (или JavaScript-объект) в
качестве параметра и отображает элемент или объект в соответствующей панеле: DOM-
элемент в панели Elements и JavaScript-объект в панели Profile.

К примеру, в следующем скриншоте функция `$()` использована, чтобы получить ссылку
на элемент `<li>`. Затем последнее исполненное выражение ([$_][57]) передаётся в
`inspect()`, чтобы открыть этот элемент в панели Elements.

![][58]


### Доступ к недавним элементам или объектам ###

Часто во время тестирования вы выбираете DOM-элементы либо непосредственно во
панели Elements, либо используя соответствующий инструмент (иконка —
увеличительное стекло), что бы работать с этими элементами. Также вы можете
выбрать снимок использования памяти в панели Profiles, для дальнейшего изучения
этого объекта.

Консоль запоминает последние пять элементов (или объектов), которые вы выбирали и
к ним можно обратиться используя свойства [$0, $1, $2, $3][59] и [$4][60].
Последний выбранный элемент или объект доступен как **`$0`**, второй — **`$1`** и
так далее.

Следующий скриншот показывает значения этих свойств после выбора трех различных
элементов подряд из панели Elements:

![Recently selected elements][61]

![Недавно выбранные элементы][61]

> Внимание: В любом случае вы можете кликнуть правой кнопкой мыли или кликнуть с
зажатой клавишей Control в консоли и выбрать пункт «Reveal in Elements Panel»



### Отслеживание событий ###

Метод [monitorEvents()][62] позволяет отслеживать определенные события объекта.
При возникновении события оно выводится в консоль. Вы определяете объект и
отслеживаемые события. Например, следующий код позволяет отслеживать событие
"resize" объекта окна.

	monitorEvents(window, "resize");

![Monitoring window resize events][63]

![Отслеживание события изменения размеров окна][63]

Что бы отслеживать несколько событий одновременно можно передать в качестве
второго аргумента массив имен событий. Следующий код отслеживает одновременно
события "mousedown" и "mouseup" элемента `body`:

	monitorEvents(document.body, ["mousedown", "mouseup"]);

Кроме того, вы можете передать один из поддерживаемых «типов событий», которые
DevTools сами преобразуют в реальные имена событий. Например, тип события touch
позволит отслеживать события touchstart, touchend, touchmove, и touchcancel.

	monitorEvents($('#scrollBar'), "touch");

Что бы узнать какие типы событий поддерживаются — ознакомьтесь с [monitorEvents()][64]
из Console API.

Что бы прекратить отслеживать событие вызовите `unmonitorEvents()` с объектом
в качестве аргумента.

	unmonitorEvents(window);


### Контроль за CPU-профайлером ###

С помощью методов [profile()][65] и [profileEnd()][66] можно создавать
JavaScript профили CPU. По желанию можно задать профилю имя.

Ниже вы видите пример создания нового профиля с именем назначенным по умолчанию:

![][67]

Новый профиль появляется в панели Profiles с именем Profile 1:

![][68]

Если вы обозначите имя для нового профиля, то оно будет использоваться в качестве
его заголовка. Если вы создаете множество профилей с одинаковыми именами, то они
группируются под одним заголовком:

![][69]

Результат в панели Profiles:

![][70]

Профили CPU могут быть вложенными, например:

	profile("A");profile("B");profileEnd("B")profileEnd("A")

Вызовы методов остановки и запуска профилирования не обязательно должны быть
корректно вложены друг в друга. Например, этот пример будет работать так же, как и
предыдущий:

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
