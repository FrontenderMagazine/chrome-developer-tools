Using the Console
============================================================

The JavaScript Console provides two primary functions for developers  testing
web pages and applications:

* [Basic operation](#basic-operation)
	* [Opening the Console](#opening-the-console)
	* [Clearing the console history](#clearing-the-console-history)
	* [Console settings](#console-settings)
* [Using the Console API](#using-the-console-api)
	* [Writing to the console](#writing-to-the-console)
	* [Errors and warnings](#errors-and-warnings)
	* [Assertions](#assertions)
	* [Filtering console output](#filtering-console-output)
	* [Grouping output](#grouping-output)
	* [String substitution and formatting](#string-substitution-and-formatting)
	* [Formatting DOM elements as JavaScript objects](#formatting-dom-elements-as-javascript-objects)
	* [Styling console output with CSS](#styling-console-output-with-css)
	* [Measuring how long something takes](#measuring-how-long-something-takes)
	* [Marking the Timeline](#marking-the-timeline)
	* [Setting breakpoints in JavaScript](#setting-breakpoints-in-javascript)
* [Using the Command Line API](#using-the-command-line-api)
	* [Evaluating expressions](#evaluating-expressions)
	* [Selecting elements](#selecting-elements)
	* [Inspecting DOM elements and JavaScript heap objects](#inspecting-dom-elements-and-javascript-heap-objects)
	* [Accessing recently selected elements and objects](#accessing-recently-selected-elements-and-objects)
	* [Monitoring events](#monitoring-events)
	* [Controlling the CPU profiler](#controlling-the-cpu-profiler)

* A place to log diagnostic information using methods provided by the [Console
API][1], such as [console.log()][2], or [console.profile()][3].
* A shell prompt where you can enter commands and interact with the document and
the Chrome DevTools. You can evaluate expressions directly in the Console, and
can also use the methods provided by the [Command Line API][4], such as [$()][5]
command for selecting elements, or [profile()][6] to start the CPU profiler.

This documentation provides an overview and common uses of these two APIs. You
can also browse the [Console API][7] and [Command Line API][8] reference guides.



## Basic operation ##

### Opening the Console ###

The JavaScript Console is available in two modes within Chrome DevTools: the
primary Console tab, or as a split-view you can display while on another tab
(such as Elements or Sources).

To open the Console tab, do one of the following:

* Use the keyboard shortcut **Command - Option - J** (Mac) or **Control -Shift
-J** (Windows/Linux).
* Select **View > Developer > JavaScript Console**.

![Console panel view][9]

To toggle a split-view of the Console on another tab, press the **Esc** key on
your keyboard, or click the **Show/Hide Console** button in the bottom left
corner of the Chrome DevTools window. In the following screenshot the Console
split-view is shown with the Elements panel.

![Console split-view][10]

### Clearing the console history ###

To clear the console's history, do one of the following:

* Right-click or Ctrl-click anywhere in the Console and choose **Clear Console**
from the context menu that appears.
* Enter the [clear()][11] Command Line API at the shell prompt.
* Invoke [console.clear()][12] Console API from JavaScript.
* Use the keyboard shortcut **⌘K** or **⌃L** (Mac) **Control - L** (Windows and
Linux).

By default, the console history is cleared when you navigate to another page.
You can change this behavior by enabling **Preserve log upon navigation** in the
Console area of the Settings dialog (see [Console preferences][13]).

### Console settings ###

The Console has two global settings you can modify in the General tab of the
DevTools Settings dialog:

* **Log XMLHTTPRequests**—determines if each XMLHTTPRequest is logged to the
Console panel.
* **Preserve log upon navigation**—determines if console history for the current
page is preserved when you navigate to another page. By default, both of these
settings are disabled.

You can also change these settings by right-clicking anywhere in the Console to
bring up the context menu.

![Console panel view][14]


## Using the Console API ##

The Console API is collection of methods provided by the global `console` object
defined by DevTools. One of the API's main purposes is to [log information][15]
(such as a property value, or an entire objects or DOM element) to the console
while your application is running. You can also group output visually in the
console to reduce visual clutter.

### Writing to the console ###

The [console.log()][16] method takes one or more expressions as parameters and
writes their current values to the console. For example:

	var a = document.createElement('p');a.appendChild(document.createTextNode('foo'));a.appendChild(document.createTextNode('bar'));console.log("Node count: " + a.childNodes.length);

![Console log output][17]

Instead of concatenating expressions together with the "+" operator (as shown
above), you can put each in its own method parameter and they will be joined
together in a space-delimited line.

	console.log("Node count:", a.childNodes.length, "and the current time is:", Date.now());

![Console log output][18]

### Errors and warnings ###

The [console.error()][19] method displays a red icon along with the message
text, which is colored red.

	function connectToServer() {    console.error("Error: %s (%i)", "Server is  not responding",500);}connectToServer();

![][20]

The [console.warn()][21] method displays a yellow warning icon with the message
text.

	if(a.childNodes.length < 3 ) {    console.warn('Warning! Too few nodes (%d)', a.childNodes.length);}

![Example of console.warn()][22]

### Assertions ###

The [console.assert()][23] method conditionally displays an error string (its
second parameter) only if its first parameter evaluates to `false`. For
instance, in the following example an error message is written to the console
only if the number of child nodes belonging to the `list` element is greater
than 500.

	console.assert(list.childNodes.length < 500, "Node count is > 500");

![Example of console.assert()][24]

### Filtering console output ###

You can quickly filter console output by its severity level--errors, warning, or
standard log statements--by selecting one of the filter options along the bottom
of the Console, as shown below.

![Only show console.error() output][25]

Filter options:

* **All**—Shows all console output.
* **Errors**—Only show output from `console.error()`
* **Warnings**—Only show output from `console.warn()`
* **Logs**—Only show output from `console.log()`, `console.info()` and
`console.debug()`.
* **Debug**—Only show output from `console.timeEnd()` and other console output.

### Grouping output ###

You can visually group related console output statements together in the console
with the [console.group()][26] and [groupEnd()][27] commands.

	var user = "jsmith", authenticated = false;console.group("Authentication phase");console.log("Authenticating user '%s'", user);// authentication code here...if (!authenticated) {    console.log("User '%s' not authenticated.", user)}console.groupEnd();

![Logging group example][28]

You can also nest logging groups. In the following example a logging group is
created for the authentication phase of a login process. If the user is
authenticated, a nested group is created for the authorization phase.

	var user = "jsmith", authenticated = true, authorized = true;// Top-level groupconsole.group("Authenticating user '%s'", user);if (authenticated) {    console.log("User '%s' was authenticated", user);    // Start nested group    console.group("Authorizing user '%s'", user);    if (authorized) {        console.log("User '%s' was authorized.", user);    }    // End nested group    console.groupEnd();}// End top-level groupconsole.groupEnd();console.log("A group-less log trace.");

![Nested logging group example][29]

To create a group that is initially collapsed, use
[console.groupCollapsed()][30] instead of `console.group()`, as shown below:

	console.groupCollapsed("Authenticating user '%s'", user);if (authenticated) {  ...}

![Initially collapsed group][31]

### String substitution and formatting ###

The first parameter you pass to any of the console's logging methods (`log()` or
`error()`, for example) may contain one or more *format specifiers*. A format
specifier consists of a **`%`** symbol followed by a letter that indicates the
formatting that should be applied to the inserted value (`%s` for strings, for
example). The format specifier identifies where to substitute a value provided
by a subsequent parameter value.

The following example using the `%s` (string) and `%d` (integer) formatters to
insert values into the output string.

	console.log("%s has %d points", "Sam", "100");

This would result in "Sam has 100 points" being logged to the console.

The following table lists the supported format specifiers and the formatting
they apply:



In the following example the `%d` format specifier is substituted with the value
of `document.childNodes.length` and formatted as an integer; the `%f` format
specifier is substituted with the value returned by `Date.now()`, which is
formatted as a floating point number.

	console.log("Node count: %d, and the time is %f.", document.childNodes.length, Date.now());

![Using format specifiers][32]

### Formatting DOM elements as JavaScript objects ###

By default, when you log a DOM element to the console it's displayed in an XML
format, as in the Elements panel:

	console.log(document.body.firstElementChild)

![][33]

You can also log an element's JavaScript representation with the `console.dir()`
method:

	console.dir(document.body.firstElementChild);

![][34]

Equivalently, you can us the `%O` [format specifier][35] with `console.log()`:

	console.log("%O", document.body.firstElementChild);

### Styling console output with CSS ###

You use the `%c` format specifier to apply custom CSS rules to any string you
write to the Console with [console.log()][36] or related methods.

	console.log("%cThis will be formatted with large, blue text", "color: blue; font-size: x-large");

![Styling console output with CSS][37]

### Measuring how long something takes ###

You can use the [console.time()][38] and [console.timeEnd()][39] methods to measure how long a function or operation in your code takes to complete. You call `console.time()` at the point in your code where you want to start the timer and `console.timeEnd()` to stop the timer. The elapsed time between these two calls is displayed in the console.

	console.time("Array initialize");var array= new Array(1000000);for (var i = array.length - 1; i >= 0; i--) {    array[i] = new Object();};console.timeEnd("Array initialize");

![Example of using console.time() and timeEnd()][40]



### Marking the Timeline ###

The [Timeline panel][41] gives you a complete overview of where time is spent
when loading and using your web app or page. The [console.timeStamp()][42]
method marks the Timeline at the moment it was executed. This provides an easy
way to correlate events in your application with other browser-related events,
such as layout or paints.



In the following example the Timeline is marked when the application enters the
`AddResult()` function's implementation.

	function AddResult(name, result) {  console.timeStamp("Adding result");  var text = name + ': ' + result;  var results = document.getElementById("results");  results.innerHTML += (text + "<br>");}

As shown in the following screenshot, the `timeStamp()` command annotates the
Timeline in the following places:

* A yellow vertical line in the Timeline's summary and detail views.
* A record is added to the list of recorded events.

![Timeline showing custom timestamp][43]

### Setting breakpoints in JavaScript ###

You can start a debugging session from your JavaScript code by calling the
[debugger][44] command. For instance, in the following example the JavaScript
debugger is opened when an object's `brightness()` function is invoked:

	brightness : function() {    debugger;    var r = Math.floor(this.red*255);    var g = Math.floor(this.green*255);    var b = Math.floor(this.blue*255);    return (r * 77 + g * 150 + b * 29) >> 8;}

![Example of using debugger command][45]


## Using the Command Line API ##

In addition to being a place where you can log information from your
application, the Console is also a shell prompt where you can directly evaluate
expressions or issue commands provided by the [Command Line API][46]. This API
provides the following features:

* Convenience functions for selecting DOM elements
* Methods for controlling the CPU profiler
* Aliases for a number of Console API methods
* Monitoring events
* View event listeners registered on objects

### Evaluating expressions ###

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

### Selecting elements ###

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

### Inspecting DOM elements and JavaScript heap objects ###

The [inspect()][56] method takes a DOM element reference (or JavaScript
reference) as a parameter and displays the element or object in the appropriate
panel—the Elements panel for DOM elements, or the Profile panel for a JavaScript
object.

For example, in the following screenshot the `$()` function is used to get a
reference to an `<li>` element. Then the last evaluated expression property
([$_][57]) is passed to `inspect()` to open that element in the Elements panel.

![][58]

### Accessing recently selected elements and objects ###

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



### Monitoring events ###

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

### Controlling the CPU profiler ###

You can create JavaScript CPU profiles from the command line with the
[profile()][65] and [profileEnd()][66] commands. You can optionally specify a
name that's applied to the profile you create.

For example, the following shows an example of creating a new profile with the
default name:

![][67]

The new profile appears in the Profiles panel under the name "Profile 1":

![][68]

If you specify a label for the new profile, it is used as the new profile's
heading. If you create multiple profiles with the same name, they are grouped as
individual runs under the same heading:

![][69]

The result in the Profiles panel:

![][70]

CPU profiles can be nested, for example:

	profile("A");profile("B");profileEnd("B")profileEnd("A")

The calls to stop and start profiling do not need be properly nested. For
example, the following works the same as the previous example:

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
[37]: ./img/format-string.png "Styling console output with CSS"
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
