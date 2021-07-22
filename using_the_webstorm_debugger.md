## Using the WebStrom debugger

Refer to https://github.com/dic-iit/element_software-engineering/issues/42#issuecomment-879486507

### Issues
- Issue: there is a know issue with the heap memory used by the tool. When using the debug mode, the used memory quickly increases and reaches the maximum defined size. Once that happens, the application hangs repeatedly and becomes unusable.
- solution/workaround: increase the maximum allowed size of the heap memory...
    1.  Go to Help->Change Memory Settings
    
    <img width="937" alt="image" src="https://user-images.githubusercontent.com/6848872/125541114-7458f2f5-4b67-4216-917d-9d2c01cf8418.png">
    
    2. Increase the "Maximum Heap Size". On MacOS platform, for this project, the max heap recommended size is around 16GB.

### Debugging

#### Debugging session and Live Edit
https://www.jetbrains.com/help/webstorm/starting-the-debugger-session.html#starting_debug_session
https://www.jetbrains.com/help/webstorm/starting-the-debugger-session.html

#### Debug the client application running on the browser

https://www.jetbrains.com/help/webstorm/debugging-javascript-in-chrome.html#debugging_js_on_external_web_server

1. Start the `iCubTelemVizServer` and `openmctStaticServer` servers from the terminal running `npm start`.
2. Create a "Javascript Debugger" configuration:
    - Select "Edit Configurations" (or "Add Configuration" if none exists) on the right hand corner...
    <img width="634" alt="image" src="https://user-images.githubusercontent.com/6848872/125662316-6d20ea3e-e430-400b-a152-c6ecea7b0727.png">
    - Add "Javascript Debugger" configuration, defining it as follows:
    <img width="904" alt="image" src="https://user-images.githubusercontent.com/6848872/125662245-a5ad8abd-4e9e-428d-b019-c349f9ffd969.png">
3. Run the debug session by clicking on the debug green button or use the key shortcut ^+D.

Alternatively, you can:
1. Start the `iCubTelemVizServer` server from the terminal running `npm start`.
2. In the WebStrom app, create a "npm" debug configuration, set for running the Open MCT visualizer server: `node server.js` under the folder `openmctStaticServer`.
    <img width="912" alt="image" src="https://user-images.githubusercontent.com/6848872/125720286-efc8a7de-e284-40da-ac80-a8f8cbe11874.png">
3. Select the configurtion and run the debug session. This runs the web server hosted on `http://localhost:8080`.
4. Run the web client by clicking on that address (SHIFT+META+click). This opens Chrome and navigates to `http://localhost:8080`, displaying the visualizer page.


#### Debug the server application running on the localhost

Running the same way a debug session on `iCubTelemVizServer` instead of `openmctStaticServer` does not work:
- The breakppoints are not effective.
- The server stops after a few minutes although it publishes the data as expected while it's running.

#### Live Edit

Make the updates on the Javascript, CSS or HTML files without having to refresh the browser: https://www.jetbrains.com/help/webstorm/live-editing.html#ws_live_edit_activate_procedure


#### Notes

These guidelines don't work: https://www.jetbrains.com/help/webstorm/starting-the-debugger-session.html#starting_debug_session

[//]: <On the WebStorm debugger, the first breakpoint you set cannot be removed nor muted by clicking on it. You have to go to "View breakponts..." on the left toolbar in the "Debug">


## Debugging the Server Application on localhost

### Run Modes

https://nodejs.org/en/docs/guides/debugging-getting-started

When you run the mode.js process with the `--debug` option, it listens to a debugging client. The option `--debug=<listened-port>` specifies the port the process is listening to. You can then have the debugging client attach to the node.js process via the `<listened-port>` port.

When you run the mode.js process with the `--debug-brk[=<listened-port>]` option, the process stops at tthe first line of the app script, waiting for the debugging client.

Running the process with the `debug` option (without `--`) does the two following steps:
1. node --debug-brk <app>.
2. attach the Node.js debugging client to the process (this debugging client has limited features).

### Node.js Debugging Client

#### The debugger interface

https://nodejs.org/api/debugger.html#debugger_stepping

##### Commands
- Inserting the statement debugger; into the source code of a script will enable a breakpoint at that position in the code.
- To begin/stop watching an expression, type `watch('my_expression')`/`unwatch('my_expression')`.
- Stepping
    - cont, c: Continue execution
    - next, n: Step next
    - step, s: Step in
    - out, o: Step out
    - pause: Pause running code (like pause button in Developer Tools)
- Breakpoints
    - setBreakpoint(), sb(): Set breakpoint on current line
    - setBreakpoint(line), sb(line): Set breakpoint on specific line
    - setBreakpoint('fn()'), sb(...): Set breakpoint on a first statement in function's body
    - setBreakpoint('script.js', 1), sb(...): Set breakpoint on first line of script.js
    - setBreakpoint('script.js', 1, 'num < 4'), sb(...): Set conditional breakpoint on first line of script.js that only - breaks when num < 4 evaluates to true
    - clearBreakpoint('script.js', 1), cb(...): Clear breakpoint in script.js on line 1
- The `repl` command creates a REPL (Read-Eval-Print-Loop) instance evaluates user inputs and so can be used to evaluate variables or expressions from the code.

#### The REPL interface

https://nodejs.org/api/repl.html

##### Commands and Keys
- break: When in the process of inputting a multi-line expression, enter the .break command (or press Ctrl+C) to abort further input or processing of that expression.
- clear: Resets the REPL context to an empty object and clears any multi-line expression being input.
- exit: Close the I/O stream, causing the REPL to exit.
- help: Show this list of special commands.

##### Notes
We can get all methods of an object under the REPL session, by running:
```js
Object.getPrototypeOf(<object_instance>);
```
Example under REPL, after pausing at a breakpoint:
```js
> image.getObjType(image)
'image'
> Object.getPrototypeOf(image)
{ copy: [Function],
  toBinary: [Function],
  getCompressionType: [Function],
  getObjType: [Function],
  getHeight: [Function],
  getWidth: [Function],
  constructor: [Function] }
```
