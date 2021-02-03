# Total.js Flow

[![Professional Support](https://www.totaljs.com/img/badge-support.svg)](https://www.totaljs.com/support/) [![Chat with contributors](https://www.totaljs.com/img/badge-chat.svg)](https://messenger.totaljs.com)

__Total.js Flow 6.1__ is a visual programming interface. It's available as a package and can be added to any applications based on __Total.js framework__. Flow can be used to add missing or changing already implemented functionality to already existing applications without having to write any code as well as creating new applications. It can be used for connecting *Internet of Things*, home automation, etc.

- [__Documentation__](https://wiki.totaljs.com/?q=flow)
- [Website](https://www.totaljs.com/flow/)

---

Flow comes pre-installed with components such as:
- `HTTP route` for creating web endpoints
- `WebSockets` for real-time communication
- `MQTT` for connecting IoT devices
- `Template` for formating the output
- `Email` and `SMS` sender for sending emails and text messages
- `NoSQL` for saving and retrieving data (uses Total.js embedded nosql db engine)
- `Analitycs` for analyzing tha data
- __And many more to come__

If You miss some specific component You can always write it by yourself :)

__Terminology__:
- `Component` is a single unit which recieves data, processes them and responds with the result
- `Flow` is a series of components connected together
- `FlowData` is an instance of data recieved by component, see `data` event in component instance
- `Designer` is a user interface available at `/$flow` (can be changed in config)

## Installation

- Total.js `+v3.3.0`
- download and copy `flow.package` into the `/packages/` directory __or create a definition file with:__

```javascript
var options = {};

// ====================================
// COMMON (OPTIONAL)
// ====================================

// options.url = '/$flow/';

// A maximum length of request:
// options.limit = 150;

// Predefined set of components (default value):
// options.templates = 'https://cdn.totaljs.com/flow/templates.json';

// +v4.0.2 Second user-defined set of components (default value is !!! EMPTY !!!):
// It must have same structure as "options.template", example:
// options.templates2 = 'https://cdn.totaljs.com/flow/templates.json';

// +v4.0.0
// Default light theme
// options.dark = false;

// +v4.1.0
// Enables backing up of Flow designer
// options.backup = true;
// default: false

// +v4.1.0
// Enables real-time monitoring components (their files)
// options.debug = true;
// default: false

// +v5.0.0
// Enables crash mode (the flow won't be applied after start)
// options.crashmode = true;
// default: false
// IMPORTANT: crash mode can be enabled via command line "node yourapp.js --crashmode"

// +v5.0.0
// Enables "check new updates" for installed components
// options.updates = true;
// default: true

// +v5.1.0
// Enables "logging" to plain text file
// options.logging = true;
// default: false

// +v5.2.0
// options.type = 'bundle'; // 'bundle', 'server', 'client'
// "bundle" -> loads server and client together
// "server" -> loads server only
// "client" -> loads client only but it needs external URL (look to "options.external")

// +v5.2.0
// options.external = 'ws://yourserver.com/$dashboard/';
// Can contain an external URL for WebSocket

// ====================================
// Security (OPTIONAL)
// ====================================

// HTTP Basic auth:
// options.auth = ['admin:admin', 'name:password'];

// Standard "authorize" flag
// options.auth = true;

// IP restrictions:
// options.restrictions = ['127.0.0.1', '138.201', '172.31.33'];

// options.token = ['OUR_COMPANY_TOKEN'];
// you can open flow using : /$flow/?token=OUR_COMPANY_TOKEN

INSTALL('package', 'https://cdn.totaljs.com/flow.package', options);
```

- __IMPORTANT__: it doesn't support `UPTODATE` mechanism

## Flow

- `FLOW` is a global variable
- URL address `http://127.0.0.1:8000/$flow/` (default, can be changed in config)

__Properties__:

```javascript
FLOW.variables;
// Returns all custom variables defined by user key/value
// returns {Object}
// +v3.0.0
```

__Methods__:

```javascript
FLOW.emit2('hello', 'arg1', 'arg..N');
// Emits event to all component instances

FLOW.emit('hello', 'arg1', 'arg..N');
// Emits event in main Flow instance

FLOW.send(message);
// Sends a message to designer via WebSocket

FLOW.debug(message);
// Sends a debug message
// message: {String} - HTML string

FLOW.set(key, value);
// Writes a value into the key-value store (data are stored on HDD)

FLOW.get(key);
// Reads a value from the key-value store (data are stored on HDD)

FLOW.rem(key);
// Removes value from the key-value store (data are stored on HDD)

FLOW.find(function(instance, definition){
	// return true if this instance satisfies your search criteria
});
// Finds instances
// returns {Array of Components}

FLOW.findByReference('STRING');
FLOW.findByReference(REGULAR_EXPRESSION);
// Finds all instances by reference
// returns {Array of Components}

FLOW.findByName('STRING');
FLOW.findByName(REGULAR_EXPRESSION);
// Finds all instances by name
// returns {Array of Components}

FLOW.findByComponent('STRING');
FLOW.findByComponent(REGULAR_EXPRESSION);
// Finds all instances by component name
// returns {Array of Components}

FLOW.findById('ID');
// Finds an instance by its ID
// returns {Component}

FLOW.hasComponent(name);
// Does have a flow registered component?
// returns {Boolean}

FLOW.hasInstance(id);
// Does have a flow registered instance?
// returns {Boolean}

FLOW.install(url, [callback]);
FLOW.install(filename, body, [callback]);
// Registers a new component and stores its content into the `/flow/` directory
// returns {FLOW}

FLOW.variable(key);
// Returns a value of variable by key
// return {Object}
// +v3.0.0

FLOW.arg(str);
FLOW.arg(obj);
// This method can bind global Flow variables to the current object or string.
// FLOW.arg('This pharse {variable-key} will be replaced.') returns {String}
// FLOW.arg({ name: '{variable-key}', secret: '{variable-secret}' }) returns {Object} (new instance)
```

__Prototype overrides__

- changes in v6.1.3 allows custom logic for saving and reading designer config as well as variables.

```javascript
FLOW.read_designer = function(callback) {
	// read the data somehow
	// return an err if any and an object in the callback
	callback(<err | null>, <obj>);
};

FLOW.save_designer = function(data, callback, backup) {
	// save the data somehow
	// return err if any in the callback
	callback(<err>);

	// if the backup is enabled in the config
	// you should also create a backup somehow
};

FLOW.read_variables = function(callback) {
	// read variables somehow
	// return an err if any and string in the callback
	// string = total.js framework config file syntax
	callback(<err | null>, <string>);
};

FLOW.save_variables = function(data, callback) {
	// save the data(string) somehow
	// return err if any in the callback
	callback(<err>);
};
```

__Events__:

```javascript
ON('flow.register', function(declaration) {
	// New component has been registered
});

ON('flow.init', function(count) {
	// Init FLOW system
	// "count" a count of all new instances
});

ON('flow.reset', function(count) {
	// Components have been reset
});

ON('flow.open', function(instance) {
	// New instance of a component has been created
});

ON('flow.save', function(instance) {
	// Flow is saved/applied
	// +v1.2.0
});

ON('flow.close', function(instance) {
	// A component instance will be closed
});

ON('flow.options', function(instance) {
	// A component instance has changed options
	// +v3.0.0
});

ON('flow.variables', function(variables) {
	// Changed flow variables
	// +v3.0.0
});
```

## Component

- __IMPORTANT__: `exports.id` can contain `a-z` `0-9` chars only and lower-case.

```javascript
// {String}, IMPORTANT (lower case without diacritics)
exports.id = 'component';

// {String}, IMPORTANT a component version
exports.version = '1.0.0';

// {String}, optional (default: "component name")
exports.title = 'A component name (for human)';

// {String}, optional (default: "#656D78")
exports.color = '#656D78'; // use darker colors because the font color is "white"

// {Boolean}, optional (default: false)
exports.click = true;

// {Number}, optional (default: 0)
// +v3.0.0
exports.input = 0;

// or {Array of Colors}, input will have 2 inputs (red and blue)
// +v3.0.0
exports.input = ['red', 'blue'];

// or {Array of Colors}, input will have 2 inputs with tooltips (red and blue)
// +v5.1.0
exports.input = ['red | ToolTip Red', 'blue | ToolTip Blue'];
exports.input = ['ToolTip Input 1', 'ToolTip Input 2'];

// {Number}, optional (default: 0)
exports.output = 1;

// or {Array of Colors}, output will have 2 outputs (red and blue)
exports.output = ['red', 'blue'];

// or {Array of Colors}, input will have 2 inputs with tooltips (red and blue)
// +v5.1.0
exports.output = ['red | ToolTip Red', 'blue | ToolTip Blue'];
exports.output = ['ToolTip Output 1', 'ToolTip Output 2'];

// {String}, optional (default: "Common")
exports.group = 'Common';

// {String}, optional (default: "Unknown")
exports.author = 'Peter Širka';

// {String}, optional (default: "")
// Font-Awesome icon without "fa-"
exports.icon = 'home';

// {String or Object}, optional (default: undefined)
exports.state = 'DEFAULT STATUS TEXT';
// or
exports.state = { text: 'DEFAULT STATUS TEXT', color: 'red' };

// {String Array}
// optional (default: undefined), NPM dependencies
exports.npm = ['sqlagent', 'mqtt'];

// {Object}, optional (default "undefined")
// Default options for new and existing instances
exports.options = { enabled: true };

// Disables data cloning
exports.cloning = false;

// {Boolean}, optional (default: true)
// +v4.0.0
// hides stats under component box in designer UI
exports.traffic = false;

// {String}, optional (format: 'yyyy-MM-dd HH:mm')
// +v4.0.0
// Updated date
exports.dateupdated = '2017-17-10';

// {Boolean}, optional (default: false)
// +v4.1.1
// Flow executes "options" event if global "variables" are changed
exports.variables = false;

// +v5.0.0
// Flow evaluates "extension" as a raw JavaScript on the client-side when the user opens Flow designer
// {String}, optional (default: null)
exports.extension = 'alert("OK")';

exports.install = function(component) {

	// =====================
	// DELEGATES
	// =====================

	// A close delegate (optional)
	// - "callback" argument must be executed!
	component.close = function(callback) {
		// This instance will be killed.
		// use this if some asyncronous work needs to be done
		// alternatively use component.on('close',...
	};

	// =====================
	// EVENTS
	// =====================

	component.on('click', function() {
		// optional
		// the component was clicked on in the designer
		// usefull for enabling/disabling some behavior or triggering some actions
	});

	component.on('data', function(message, next) {

		// RAW DATA
		// returns {Object}
		message.data;

		// Write value to data repository
		// returns {Message}
		message.set('key', 'value');

		// Read value from data repository
		// returns {Object}
		message.get('key');

		// Remove value from data repository
		// returns {Message}
		message.rem('key');

		// {Object} Parent component (first component which started the flow)
		message.parent;

		// {Boolean} Is completed?
		message.completed;

		// {DateTime}
		message.begin;

		// How can I modify data?
		message.data = { newdata: true };

		// +v4.0.2
		// Replaces {KEY1} {KEY2} {KEY..N} according to the message repository
		// returns {String}
		message.arg('Dynamic arguments {name} according to {message} repository.');

		// +v4.1.1
		// This method can bind global data repository to the current object
		// message.arg({ name: '{repository-key}', secret: '{repository-secret}' }) returns {Object} (new instance)

		// send this message :-)
		component.send(message);

		// or use the next param, it's a reference to component.send
		// or a function passed in component.callback
		next(message);

	});

	component.on('<input-number>', function(message) {
		// message as specified above in 'data' event
		// input 0 to event '0' and so on
	});

	component.on('options', function(new_options, old_options) {
		// optional
		// options have changed in the designer
		// instance.options holds the new_options already
	});

	component.on('variables', function(variables) {
		// +v3.0.0
		// optional
		// global variables have been changed
		// instance.variable(key)
	});

	component.on('close', function() {
		// optional
		// This instance will be killed
	});

	component.on('reinit', function() {
		// optional
		// Designer has been updated, but this instance still persists
		// This instance can have new connections.
	});

	component.on('signal', function(data, parent) {
		// optional
		// Captured signal
		// @data {Object} - optional, can be "null", or "undefined"
		// @parent {Component} - a component which created this signal
	});

	component.on('service', function(counter) {
		// optional
		// Service called each 1 minute
	});

	component.on('pause', function(is) {
		// +v5.1.0
		// optional
		// Is Flow paused?
	});

	// =====================
	// METHODS
	// =====================

	component.status(message, [color]);
	// Sends a status to designer
	// @message: {String/Object} - string will be formatted as markdown and object as JSON
	// color: {String} - "black" (default: "gray")

	component.debug(message, [style]);
	// Sends a debug message
	// @message: {String/Object} - string will be formatted as markdown and object as JSON
	// style: {String} - "info", "warning", "error" (default: "info")

	component.hasConnection(index);
	// Calculates connections
	// @index: {Number}
	// returns {Number}

	component.send([index], flowdata);
	// Sends data
	// @index: {Number} - optional, the output index (otherwise all outputs)
	// @flowdata: {instance of FlowData} - use component.make(data); to create it if needed
	// returns Message;

	var message = component.send2([index], data);
	if (message) {
		// message will be sent
	} else {
		// no connections
	}
	// +v3.0.0
	// Alias for component.send() but with a check of connections

	component.callback([index], flowdata, function(flowdata){ ... }, param);
	// +v6.0.0
	// Sends data to connected component and recieves a response back.
	// @index: {Number} - optional, the output index (defualt output 0)
	// @flowdata: {instance of FlowData} - same instance will be recieved in callback function
	// @function: {Function} - callback function
	// @param: {Any} - aditinal data
	// expects only 1 component to be connected to the given output, if not only first component will recieve data
	// returns {Component}


	component.set(key, value);
	// Writes a value to a private key-value store (data are stored on HDD)
	// @key {String}
	// @value {Object}
	// returns {Component}

	component.get(key);
	// Reads a value from a private key-value store (data are stored on HDD)
	// @key {String}
	// returns {Object}

	component.make(data);
	// Creates a new FlowData/Message instance.
	// @data {Object}
	// returns {Message}

	component.rem(key);
	// Removes a value from a private key-value store (data are stored on HDD)
	// @key {String}
	// returns {Component}

	component.variable(key);
	// +v3.0.0
	// Reads a value from global variables
	// @key {String}
	// returns {Object}

	component.signal([index], [data]);
	// Sends a signal to first connection (it emits "signal" event in target connection)
	// @index {Number} - optional, an output index (default: "undefined" --> all connections)
	// @data {Object} - optional, an additional data
	// returns {Component}

	component.click();
	// Performs click event.
	// returns {Component}

	component.log([a], [b], [c], [d]);
	// Writes some info into the log file
	// returns {Component}

	component.error(err, [parent|response|component]);
	// Creates error
	// returns {Component}

	component.save();
	// Saves current options, useful when options are changed internally. Options from settings form are saved automatically
	// returns {Component}

	component.reconfig();
	// If the component options changes on the server (not by recieving new options from designer) then use this to update options in designer

	component.arg(str);
	component.arg(obj);
	// This method can bind global Flow variables to the current object or string.
	// component.arg('This pharse {variable-key} will be replaced.') returns {String}
	// component.arg({ name: '{variable-key}', secret: '{variable-secret}' }) returns {Object} (new instance)

	component.throw(data);
	// +v5.0.0
	// Sends data through the error output

	component.next([name]);
	// @name {String} optional, name of component (lower-case), e.g. "trigger"
	// returns {Component} or "undefined"
	// Returns a component instance (if exists) in the outputs
	// +v5.0.0

	component.prev([name]);
	// @name {String} optional, name of component (lower-case), e.g. "trigger"
	// returns {Component} or "undefined"
	// Returns a component instance (if exists) in the inputs
	// +v5.0.0

	component.outputs();
	// returns {Array Component}
	// Returns all components in the outputs
	// +v5.0.0

	component.inputs();
	// returns {Array Component}
	// Returns all components in the inputs
	// +v5.0.0

	component.isDisabled([io], [index]);
	// @io {String} optional, "input" or "output"
	// @index {Number} optional, input/output index
	// returns {Boolean} if io and index passed in
	// returns {Array of inputs or outputs} if only io passed in
	// returns {Object} e.g. { inputs: [disabled inputs], outputs: [disabled outputs] } if nothing passed in
	// +v5.0.0

	component.reoptions(new_options);
	// returns {Component}
	// The method reconfigures the entire component again with new options
	// "new_options" argument can contain properties for changing internal meta data of the component: NAME:String, REFERENCE:String, COLOR:String, NOTES:String, OUTPUT:Number/Array, INPUT:Number/Array
	// +v6.1.5

	// =====================
	// PROPERTIES
	// =====================

	component.paused;
	// {Boolean} - determines whether the Flow is paused
	// +v5.1.0

	component.duration;
	// {Number} - time of data processing between input and output in milliseconds
	// +v5.0.0

	component.countinput;
	// {Number} - count of message on input
	// +v5.0.0

	component.countoutput;
	// {Number} - count of message on output
	// +v5.0.0

	component.custom;
	// {Object} - empty object for custom variables and methods

	component.name;
	// {String} - readonly, a component name (USER-DEFINED)

	component.reference;
	// {String} - readonly, a component reference (USER-DEFINED)

	component.options;
	// {Object} - readonly, custom settings

	component.state;
	// {Object} - readonly, latest state

	component.connections;
	// {Object} - readonly, all connections
};

exports.uninstall = function() {
	// OPTIONAL
};
```

## Message

When is the message instance created?

```javascript
// FIRST CASE:
component.on('data', function(message) {
	// Properties:
	message.id;               // {Number} A message identificator
	message.index;            // {Number} An input number
	message.begin;            // {Date} when it started
	message.data;             // {Anything} user defined data
	message.completed;        // {Boolean} is sending completed?
	message.parent;           // {Component} a parent instance

	// Methods (private message repository):
	message.set(key, value);  // Sets a key-value to message repository (doesn't modify data)
	message.get(key);         // Gets a key-value (doesn't read data from "data")
	message.rem(key);         // Removes a key-value (doesn't read data from "data")
	message.rewrite(data);    // Rewrites the current with new
});

// SECOND CASE
var message = component.send('YOUR-DATA-TO-CHILD-CONNECTIONS');
```

## Multiple inputs

```javascript
// data from all inputs go to 'data' event
component.on('data', function(message, next) {
	// message as specified above
	message.index; // Input number
});

// data from specific input go also to the corresponding event -> input 0 to event '0'
component.on('0', function(message) {
	// message as specified above
});
```

---

## Client-Side

### Events

```javascript
ON('open.componentname', function(component, options) {
	// Settings will be open
});

ON('save.componentname', function(component, options) {
	// Settings will be save
});

ON('select.componentname', function(component) {
	// A component has been selected in designer.
});

ON('click.componentname', function(component) {
	// Performed "click"
});

ON('add.componentname', function(component) {
	// A component has been added.
});

ON('rem.componentname', function(component) {
	// A component has been removed.
});

ON('apply', function() {
	// Designer will be sent to server and then will be applied
});
```

### Good to know

__How to change count of outputs/inputs dynamically?__

`v3.0.0` This is possible on client-side only.

```javascript
ON('save.componentname', function(component, options) {

	component.output = 5;
	// component.input = 3;

	// or
	component.output = ['green', 'red', 'blue'];
	// component.input = ['green', 'red', 'blue'];

	// or set output to default
	component.output = null;
	// component.input = null;
});
```

### Components: jComponent +v17

Bellow jComponents can be used in `Settings form`:

- autocomplete (declared `body`)
- binder (declared in `body`)
- calendar (declared in `body`)
- checkbox
- checkboxlist
- codemirror
- colorpicker (declared in `body`)
- confirm (declared in `body`)
- contextmenu (declared in `body`)
- dropdown
- dropdowncheckbox
- error
- exec (declared in `body`)
- form
- importer
- keyvalue
- loading
- message (declared in `body`)
- nosqlcounter
- repeater
- repeater-group
- search
- selectbox
- textbox
- textboxlist
- validation
- visible
- multioptions
- dragdropfiles
- filereader
- input
- directory

__References:__

- [Componentator.com](https://componentator.com/)
- [jComponents on Github](https://github.com/totaljs/jComponent)

### Triggers

- trigger executes a trigger on the server-side which can return some data

__Server-side__:

```javascript
// Register trigger
FLOW.trigger('name', function(next, data) {
	// Data sent from client-side
	console.log(data);

	// Sends back some data
	next([1, 2, 3, 4]);
});

// Unregister trigger
FLOW.trigger('name', null);
```

__Client-side__:

```javascript
// TRIGGER(trigger_name, [data], callback(data))

// Call the trigger
TRIGGER('name', function(data) {
	console.log(data); // outputs [1, 2, 3, 4]
});

TRIGGER('name', 'path.to.bind.response'); // window.path.to.bind.response === [1, 2, 3, 4]
```

### Extending of designer

```javascript
// Context-menu settings will be open
ON('settings.open', function(items) {
	// now you can add some item into the items
});

ON('settings.select', function(item) {
	// item has been selected
});

// Features will be open
ON('features.open', function(items) {
	// now you can add some item into the items
});

ON('features.select', function(item) {
	// item has been selected
});

// flow;
// {Object} contains designer meta-data

// common;
// {Object} contains global meta-data
```

## Contributors

| Contributor | Type | E-mail |
|-------------|------|--------|
| [Peter Širka](https://github.com/petersirka) | author + support | <petersirka@gmail.com> |
| [Martin Smola](https://github.com/molda) | contributor + support | <smola.martin@gmail.com> |
| [John Graves](https://github.com/gravesjohnr) | contributor | <john.graves@oracle.com> |

## Components contributors

| Contributor | Type | E-mail |
|-------------|------|--------|
| Torlone Gianfranco | contributor | <torloneg@gmail.com> |

## Contact

- (c) 2017-2018 by Peter Širka - <petersirka@gmail.com>
- contact form <https://www.totaljs.com/contact/>
- <info@totaljs.com>
