# Использование консоли Google Chrome

Консоль предоставляет две функции для разработчиков для тестирования веб-страниц и
приложений:

* в консоль можно выводить отладочную информацию, используя такие методы
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
	* [Фильтрация вывода в консоли](#---)
	* [Группирование вывода](#--5)
	* [Замена строк и их форматирование](#----)
	* [Представление DOM-элементов как JavaScript-объекты](#-dom---javascript-)
	* [Стилизация вывода консоли с помощью CSS](#-----css)
	* [Измерение временных затрат](#---2)
	* [Корреляция с панелью Timeline](#---timeline)
	* [Создание точек останова](#---3)
* [Использование the Command Line API](#-command-line-api)
	* [Выполнение выражений](#--6)
	* [Выделение элементов](#--7)
	* [Инспектирование DOM-элементов и JavaScript объектов](#-dom---javascript--1)
	* [Доступ к недавним элементам или объектам](#-----)
	* [Отслеживание событий](#--8)
	* [Контроль за CPU-профайлером](#--cpu-)




<a id="--1"></a>
## Базовые операции ##


<a id="--2"></a>
### Открытие консоли ###

Консоль в Chrome DevTools доступна в двух вариантах: вкладка Console и
в виде разделённой версии, доступной из любой другой вкладки.

Для того, чтобы открыть вкладку Console вы можете:

* использовать хоткей **Command - Option - J** (Mac) или **Control - Shift - J**
(Windows/Linux);
* выбрать пункт меню **View > Developer > JavaScript Console**.

![Скриншот][Консоль]

Для того, чтобы вызвать или скрыть разделённую версию консоли в других
вкладках, нажмите клавишу **Esc** или кликните на иконку **Show/Hide Console** в
нижнем левом углу DevTools. Скриншот показывает разделённый вариант консоли во
вкладке Elements.

![Скриншот][Разделённый режим консоли]


<a id="--3"></a>
### Очистка консоли ###

Для очистки консоли:

* через контекстное меню вкладки консоли (клик правой кнопки мыши) выберите пункт
**Clear Console**.
* Введите [clear()][11] — команду из Command Line API в консоли.
* Вызовите [console.clear()][12] (команду из Console API) из скрипта.
* Используйте хоткеи **⌘K** или **⌃L** (Mac) **Control - L** (Windows и Linux).

По умолчанию, история консоли очищается при переходе на другую страницу.
Вы можете отменить очистку включив **Preserve log upon navigation** в разделе
консоли в настройках DevTools (см. [настройки Консоли][13]).


<a id="--4"></a>
### Настройки консоли ###

Консоль имеет две главные опции, которые вы можете настраивать в главной вкладке
настроек DevTools:

* **Log XMLHTTPRequests** — определяет, логировать ли XMLHTTPRequest
в панели консоли.
* **Preserve log upon navigation** — определяет, сбрасываться ли истории консоли
при переходе на новую страницу. По умолчанию обе из этих опций отключены.

Вы можете поменять эти настройки в контекстном меню консоли, кликнув правой
кнопкой мыши.

![Скриншот][Контекстное меню консоли]



<a id="-api-"></a>
## Использование API консоли ##

Console API — набор методов, доступных из объекта `console`, объявленного из
DevTools. Одно из главных назначений API — [логировать информацию][15] (значение
свойства, целый объект или DOM-элемент) в консоль во время работы вашего
приложения. Вы также можете группировать вывод, чтобы избавиться от визуального
мусора.


<a id="--"></a>
### Вывод в консоль ###

Метод [console.log()][16] принимает один и более параметров и выводит их текущие
значения на консоль. Например:

	var a = document.createElement('p');
	a.appendChild(document.createTextNode('foo'));
	a.appendChild(document.createTextNode('bar'));
	console.log("Количество дочерних элементов: " + a.childNodes.length);

![Скриншот][Пример console.log()]

Вместо того, чтобы конкатенировать параметры функции с помощью оператора `+` (как
показано выше), вы можете ввести каждый параметр друг за другом через запятую и
их значения будут выведены в одну строку, разделённые пробелом.

	console.log("Количество дочерних элементов: ", a.childNodes.length,  "; текущее время: ", Date.now() );

![Скриншот][Пример console.log() с несколькими аргументами и с модификаторами форматирования]


<a id="---1"></a>
### Ошибки и предупреждения ###

Метод [console.error()][19] выводит красную иконку рядом с сообщением красного
цвета.

	function connectToServer() {
		console.error("Ошибка: %s (%i)", "Сервер не отвечает",500);
	}
	connectToServer();

![Скриншот][Пример console.error()]

Метод [console.warn()][21] выводит жёлтую иконку рядом с текстом сообщения.

	if(a.childNodes.length < 3 ) {
	    console.warn('Внимание! Слишком мало дочерних элементов (%d)', a.childNodes.length);
	}

![Скриншот][Пример console.warn()]

<a id="assertions"></a>
### Проверки ###

Метод [console.assert()][23] выводит сообщение об ошибке (это второй аргумент)
только в том случае, если первый аргумент равен `false`. К примеру, в следующем
примере сообщение об ошибке появится, только если количество дочерних элементов
DOM-элемента `list` больше пятисот.

	console.assert(list.childNodes.length < 500, "Количество дочерних элементов > 500");

![Скриншот][Пример console.assert()]


<a id="---"></a>
### Фильтрация вывода в консоли ###

Вы можете быстро фильтровать сообщения в консоли по их типу (уровню) — ошибки,
предупреждения и стандартный лог — выбрав один из доступных опций внизу консоли:

![Скриншот][Вывод сообщений только console.error()]

Возможные фильтры:

* **All** — без фильтрации.
* **Errors** — сообщения `console.error()`.
* **Warnings** — сообщения `console.warn()`.
* **Logs** — сообщения `console.log()`, `console.info()` и
`console.debug()`.
* **Debug** — сообщения `console.timeEnd()` и остальных функций
консольного вывода.


<a id="--5"></a>
### Группирование вывода ###

Вы можете визуально группировать вывод в консоли с помощью команд
[console.group()][26] и [groupEnd()][27].

	var user = "Вася Пупкин", authenticated = false;
	console.group("Этап аутентификации");
	console.log("Аутентификация пользователя '%s'", user);
	// Код авторизации…
	if (!authenticated) {
	    console.log("Пользователь '%s' не был аутентифицирован.", user)
	}
	console.groupEnd();

![Скриншот][Пример группировки сообщений]

Также вы можете вкладывать группы логов друг в друга. В следующем примере группа
логов создаётся для этапа аутентификации в процессе логина. Если пользователь
аутентифицирован, то создаётся вложенная группа для этапа авторизации.

	var user = "Вася Пупкин", authenticated = true, authorized = true;
	// Внешняя группа
	console.group("Аутентификация пользователя '%s'", user);
	if (authenticated) {
	    console.log("Пользователь '%s' был аутентифицирован.", user)
	    // Начало вложенной группы
	    console.group("Авторизация пользователя '%s'", user);
	    if (authorized) {
	        console.log("Пользователь '%s' был авторизован.", user)
	    }
	    // Конец вложенной группы
	    console.groupEnd();
	}
	// Конец внешней группы
	console.groupEnd();
	console.log("Обычный вывод без групп.");

![Скриншот][Пример создания вложенных групп сообщений]

Для создания изначально свёрнутой группы используйте [console.groupCollapsed()][30]
вместо `console.group()`:

	console.groupCollapsed("Аутентификация пользователя '%s'", user);
	if (authenticated) {
		//…
	}

![Скриншот][Группа сообщений, свёрнутая по умолчанию]


<a id="----"></a>
### Замена строк и их форматирование ###

Первый параметр, передаваемый в любой метод консоли (например, `log()` или
`error()`), может содержать *модификаторы форматирования*. Модификатор вывода
состоит из символа `%`, сразу за которым следует буква, сообщающая о том, какое
форматирование должно быть применено (например, `%s` — для строк). Модификатор
форматирования определяет, куда подставить значение, переданное из следующих
параметров функции.

В следующем примере используется строчный и числовой модификаторы `%s` (string) и
`%d` (decimal) для замены значений в выводимой строке.

	console.log("%s купил %d бочонков мёда", "Саша", "100");

Результатом будет «Саша купил 100 бочонков мёда».

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
				Форматирует значение как DOM-элемент (также как в панели Elements).
			</td>
		</tr>
		<tr>
			<td>
				<code>%O</code>
			</td>
			<td>
				Форматирует значение как JavaScript-объект.
			</td>
		</tr>
		<tr>
			<td>
				<code>%c</code>
			</td>
			<td>
				Применяет переданные в качестве второго аргумента CSS-стили к выводимой строке.
			</td>
		</tr>
	</tbody>
</table>

В следующем примере модификатор форматирования `%d` заменяется на значение
`document.childNodes.length` и форматируется как целое число; модификатор `%f`
заменяется на значение, возвращаемое `Date.now()` и форматируется как число с
плавающей точкой.

	console.log("Количество дочерних элементов: %d; текущее время: %f", a.childNodes.length, Date.now() );

![Скриншот][Пример console.log() с несколькими аргументами и с модификаторами форматирования]

<a id="-dom---javascript-"></a>
### Представление DOM-элементов как JavaScript-объекты ###

По умолчанию, когда вы логируете DOM-элемент в консоль, он выводится в XML-
формате, как в панели Elements:

	console.log(document.body.firstElementChild);

![Скриншот][Пример вывода DOM-элемента]

Вы можете вывести DOM-элемент в JavaScript-представлении с помощью метода
`console.dir()`:

	console.dir(document.body.firstElementChild);

![Скриншот][Пример вывода DOM-элемента в JavaScript-представлении]

Точно также вы можете использовать [модификатор вывода][35] `%0` в методе
`console.log()`:

	console.log("%O", document.body.firstElementChild);


<a id="-----css"></a>
### Стилизация вывода консоли с помощью CSS ###

Можно использовать модификатор `%c`, чтобы применить СSS-правила, к любой
строке, выводимой с помощью [console.log()][36] или похожих методов.

	console.log("%cЭта надпись будет отформатирована крупным голубым текстом", "color: blue; font-size: x-large");

![Скриншот][Стилизация вывода консоли с помощью CSS]

<a id="---2"></a>
### Измерение временных затрат ###

Методы [console.time()][38] и [console.timeEnd()][39] используются для
измерения того, как много времени потребовалось для выполнения скрипта.
`console.time()` вызывается для запуска таймера, а `console.timeEnd()` — для его
остановки. Время, прошедшее между вызовами этих функций, будет выведено в
консоль.

	console.time("Инициализация массива");
	var array= new Array(1000000);
	for (var i = array.length - 1; i >= 0; i--) {
	    array[i] = new Object();
	};
	console.timeEnd("Инициализация массива");

![Скриншот][Пример использования console.time() и console.timeEnd()]

> Внимание: необходимо использовать одинаковые строки в параметрах
`console.time()` и `timeEnd()` для ожидаемой работы таймера — считайте эту
строку его уникальным идентификатором.


<a id="---timeline"></a>
### Корреляция с панелью Timeline ###

Панель [Timeline][41] предоставляет подробный обзор того, куда было потрачено
время работы вашего приложения. Метод [console.timeStamp()][42] создаёт отдельную
отметку в момент своего исполнения. Это помогает легко и непринуждённо соотносить
события в приложении с браузерными событиями *reflow* и *repaint*.

> Внимание: метод `console.timeStamp()` выполняется только при записи событий в
панели Timeline.

В следующем примере в панели Timeline появляется отметка «Adding result» в тот
момент, когда поток выполнения программы доходит до `console.timeStamp("Adding
result")`

	function AddResult(name, result) {
		console.timeStamp("Добавление результатов");
		var text = name + ': ' + result;
		var results = document.getElementById("results");
		results.innerHTML += (text + "<br>");
	}

Как проиллюстрировано в скриншоте, вызов `timeStamp()` отмечен в следующих
местах:

* жёлтая вертикальная линия в панели Timeline.
* Запись добавлена в список записанных событий.

![Скриншот][Рукотворная отметка в панели Timeline]

<a id="---3"></a>
### Создание точек останова ###

Вы можете начать отладку вашего кода, вызвав команду [debugger][44]. К примеру, в
следующем коде отладка начинается сразу после вызова метода `brightness()`:

	function brightness() {
	    debugger;
	    var r = Math.floor(this.red*255);
	    var g = Math.floor(this.green*255);
	    var b = Math.floor(this.blue*255);
	    return (r * 77 + g * 150 + b * 29) >> 8;
	}

![Скриншот][Пример использования команды debugger]



<a id="-command-line-api"></a>
## Использование Command Line API ##

Кроме того, что консоль — это место вывода логов вашего приложения, вы можете
вводить в нее команды, определенные в [Command Line API][46].  Это API дает
следующие возможности:

* удобные функции для выделения DOM-элементов.
* Методы управления CPU-профайлером.
* Псевдонимы для некоторых методов Console API.
* Отслеживание событий.
* Просмотр обработчиков событий объекта.


<a id="--6"></a>
### Выполнение выражений ###

Консоль выполнит любой JavaScript-код, который вы в неё введёте, сразу
после нажатия кнопки Return или Enter. В консоли действует автодополнение
и дополнение по табу.  По мере ввода выражений и свойств вам предлагают
возможные варианты. Если существуют несколько одинаково начинающихся
свойств, вы можете выбирать между ними с помощью кнопки Tab. Нажав стрелку
вправо вы выберете текущую подсказку. Если подсказка одна, то нажатие Tab
тоже приведет к ее выбору.

![Скриншот][Пример выполнения команд в консоли]

Для того, чтобы вводить многострочные JavaScript-выражения, используйте сочетание
клавиш Shift+Enter для перевода строки.

![Скриншот][Пример выполнения многострочных команд]


<a id="--7"></a>
### Выделение элементов ###

Command Line API предоставляет набор методов для доступа к DOM-элементам в вашем
приложении. К примеру, метод [$()][49] возвращает первый элемент, соответствующий
объявленному CSS-селектору, идентично с [document.querySelector()][50]. Следующий
код вернёт первый элемент с ID «loginBtn».

	$('#loginBtn');

![Скриншот][Выделение DOM-элемента]

Метод [$$()][52] возвращает массив элементов, соответствующих указанному CSS-
селектору, идентично [document.querySelectorAll()][53]. Чтобы получить все
кнопки с классом `loginBtn`, нужно ввести:

	$$('button.loginBtn');

![Скриншот][Выделение нескольких DOM-элементов]

И, наконец, метод [x()][55] принимает XPath-путь в качестве параметра и возвращает
массив элементов, соответствующих этому пути. Например, этот код вернёт все
элементы `<script>`, являющиеся дочерними по отношению к элементу `<body>`:

	$x('/html/body/script');


<a id="-dom---javascript--1"></a>
### Инспектирование DOM-элементов и объектов ###

Метод [inspect()][56] принимает ссылку на DOM-элемент (или объект) в
качестве параметра и отображает элемент или объект в соответствующей панели:
DOM-элемент в панели Elements и JavaScript-объект в панели Profile.

К примеру, в следующем скриншоте функция `$()` использована, чтобы получить ссылку
на элемент `<li>`. Затем последнее исполненное выражение ([$_][57]) передаётся в
`inspect()`, чтобы открыть этот элемент в панели Elements.

![Скриншот][Инспектирование DOM-элементов и объектов]

<a id="-----"></a>
### Доступ к недавно вызванным DOM-элементам или объектам ###

Часто во время тестирования вы выбираете DOM-элементы либо непосредственно в
панели Elements, либо используя соответствующий инструмент (иконка —
увеличительное стекло), чтобы работать с этими элементами. Также вы можете
выбрать снимок использования памяти в панели Profiles для дальнейшего изучения
этого объекта.

Консоль запоминает последние пять элементов (или объектов), которые вы выбирали,
и к ним можно обратиться используя свойства [$0, $1, $2, $3][59] и [$4][60].
Последний выбранный элемент или объект доступен как **`$0`**, второй — **`$1`** и
так далее.

Следующий скриншот показывает значения этих свойств после выбора трех различных
элементов подряд из панели Elements:

![Скриншот][Недавно выбранные элементы]

> Внимание: В любом случае вы можете кликнуть правой кнопкой мыши или кликнуть с
зажатой клавишей Control в консоли и выбрать пункт «Reveal in Elements Panel»


<a id="--8"></a>
### Отслеживание событий ###

Метод [monitorEvents()][62] позволяет отслеживать определенные события объекта.
При возникновении события оно выводится в консоль. Вы определяете объект и
отслеживаемые события. Например, следующий код позволяет отслеживать событие
"resize" объекта окна.

	monitorEvents(window, "resize");

![Скриншот][Отслеживание события изменения размеров окна]

Чтобы отслеживать несколько событий одновременно, можно передать в качестве
второго аргумента массив имен событий. Следующий код отслеживает одновременно
события "mousedown" и "mouseup" элемента `body`:

	monitorEvents(document.body, ["mousedown", "mouseup"]);

Кроме того, вы можете передать один из поддерживаемых «типов событий», которые
DevTools сами преобразуют в реальные имена событий. Например, тип события touch
позволит отслеживать события touchstart, touchend, touchmove, и touchcancel.

	monitorEvents($('#scrollBar'), "touch");

Чтобы узнать, какие типы событий поддерживаются — ознакомьтесь с [monitorEvents()][64]
из Console API.

Чтобы прекратить отслеживать событие вызовите `unmonitorEvents()` с объектом
в качестве аргумента.

	unmonitorEvents(window);

<a id="--cpu-"></a>
### Контроль за CPU-профайлером ###

С помощью методов [profile()][65] и [profileEnd()][66] можно создавать
JavaScript профили CPU. По желанию можно задать профилю имя.

Ниже вы видите пример создания нового профиля с именем назначенным по умолчанию:

![Скриншот][Пример создания нового профиля с именем назначенным по умолчанию]

Новый профиль появляется в панели Profiles с именем Profile 1:

![Скриншот][Новый профиль появляется в панели Profiles с именем Profile 1]

Если вы обозначите имя для нового профиля, то оно будет использоваться в качестве
его заголовка. Если вы создаете множество профилей с одинаковыми именами, то они
группируются под одним заголовком:

![Скриншот][Пример создания нескольких профилей]

Результат в панели Profiles:

![Скриншот][Результат создания нескольких профилей]

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
[11]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#clear
[12]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoleclear
[13]: https://developers.google.com/chrome-developer-tools/docs/console#consolepreferences
[15]: https://developers.google.com/chrome-developer-tools/docs/console#writing_to_the_console
[16]: https://developers.google.com/chrome-developer-tools/docs/console-api#consolelogobject_object
[19]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoleerrorobject_object
[21]: https://developers.google.com/chrome-developer-tools/docs/console-api#consolewarnobject_object
[23]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoleassertexpression_object
[26]: https://developers.google.com/chrome-developer-tools/docs/console-api#consolegroupobject_object
[27]: https://developers.google.com/chrome-developer-tools/docs/console-api#consolegroupend
[30]: https://developers.google.com/chrome-developer-tools/docs/console-api#consolegroupcollapsed
[35]: https://developers.google.com/chrome-developer-tools/docs/console#string_substitution_and_formatting
[36]: https://developers.google.com/chrome-developer-tools/docs/console#writingtotheconsole
[38]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoletimelabel
[39]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoletimeendlabel
[41]: https://developers.google.com/chrome-developer-tools/docs/timeline
[42]: https://developers.google.com/chrome-developer-tools/docs/console-api#consoletimestamplabel
[44]: https://developers.google.com/chrome-developer-tools/docs/console-api#debugger
[46]: https://developers.google.com/chrome-developer-tools/docs/commandline-api
[49]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#selector
[50]: http://docs.webplatform.org/wiki/css/selectors_api/querySelector
[52]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#selector_1
[53]: http://docs.webplatform.org/wiki/css/selectors_api/querySelectorAll
[55]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#xpath
[56]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#inspectobject
[57]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#_
[59]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#0_-_4
[60]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#0_-_4
[62]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#monitoreventsobject_events
[64]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#monitoreventsobject_events
[65]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#profilename
[66]: https://developers.google.com/chrome-developer-tools/docs/commandline-api#profileendname




[Консоль]: ./img/console1.png "Консоль"
[Разделённый режим консоли]: ./img/console-split-view.png "Разделённый режим консоли"
[Контекстное меню консоли]: ./img/console-context-menu.png "Контекстное меню консоли"
[Пример console.log()]: ./img/log-basic.png "Пример console.log()"
[Пример console.log() с несколькими аргументами и с модификаторами форматирования]: ./img/log-multiple.png "Пример console.log() с несколькими аргументами и с модификаторами форматирования"
[Пример console.error()]: ./img/error-server-not-resp.png "Пример console.error()"
[Пример console.warn()]: ./img/warning-too-few-nodes.png "Пример console.warn()"
[Пример console.assert()]: ./img/assert-failed.png "Пример console.assert()"
[Вывод сообщений только console.error()]: ./img/filter-errors.png "Вывод сообщений только console.error()"
[Пример группировки сообщений]: ./img/group.png "Пример группировки сообщений"
[Пример создания вложенных групп сообщений]: ./img/nestedgroup.png "Пример создания вложенных групп сообщений"
[Группа сообщений, свёрнутая по умолчанию]: ./img/groupcollapsed.png "Группа сообщений, свёрнутая по умолчанию"
[Пример вывода DOM-элемента]: ./img/log-element.png "Пример вывода DOM-элемента"
[Пример вывода DOM-элемента в JavaScript-представлении]: ./img/dir-element.png "Пример вывода DOM-элемента в JavaScript-представлении"
[Стилизация вывода консоли с помощью CSS]: ./img/format-string.png "Стилизация вывода консоли с помощью CSS"
[Пример использования console.time() и console.timeEnd()]: ./img/time-duration.png "Пример использования console.time() и console.timeEnd()"
[Рукотворная отметка в панели Timeline]: ./img/timestamp2.png "Рукотворная отметка в панели Timeline"
[Пример использования команды debugger]: ./img/debugger.png "Пример использования команды debugger"
[Пример выполнения команд в консоли]: ./img/evaluate-expressions.png "Пример выполнения команд в консоли"
[Пример выполнения многострочных команд]: ./img/multiline-expression.png "Пример выполнения многострочных команд"
[Выделение DOM-элемента]: ./img/select-login-btn.png "Выделение DOM-элемента"
[Выделение нескольких DOM-элементов]: ./img/select-multiple-login.png "Выделение нескольких DOM-элементов"
[Инспектирование DOM-элементов и объектов]: ./img/inspect2.png "Инспектирование DOM-элементов и объектов"
[Недавно выбранные элементы]: ./img/recent-selection.png "Недавно выбранные элементы"
[Отслеживание события изменения размеров окна]: ./img/monitor-resize.png "Отслеживание события изменения размеров окна"
[Пример создания нового профиля с именем назначенным по умолчанию]: ./img/profile-console.png "Пример создания нового профиля с именем назначенным по умолчанию"
[Новый профиль появляется в панели Profiles с именем Profile 1]: ./img/profile-panel.png "Новый профиль появляется в панели Profiles с именем Profile 1"
[Пример создания нескольких профилей]: ./img/profile-console-2.png "Пример создания нескольких профилей"
[Результат создания нескольких профилей]: ./img/profile-panel-2.png "Результат создания нескольких профилей"
