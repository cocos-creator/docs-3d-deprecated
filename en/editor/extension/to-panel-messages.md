# Communicate with the panel

In general, our interaction model is dominated by "extension process" and "panel" for data presentation. To the traditional Web, the "plug-in" function is the server side, and the "panel" function is the browser on the client's computer.

In this case, there is usually no direct data sent to the panel, some of the majority is some state synchronization, using broadcast broadcast can be.

But for simple extensions, or extensions to the browser environment, the actual functionality may be on the panel, and a request needs to be sent to the panel.

Some level of understanding of the [Message System](./messages.md) is required before reading this section.

## Define methods on extensions and panels

First we define the file: package.json

```json
{
    "name": "hello-world",
    "panels": {
        "default": {
            "title": "hw",
            "main": "./panel.js"
        }
    },
    "contributions": {
        "messages": {
            "console": {
                "methods": ["default.console"]
            }
        }
    }
}
```

The method name defined by methods in messages.console is 'default.console'.
Represents a console method issued to the Default panel.
(To send to the plug-in process, fill in methdName directly)

Then define the panel.js file of the panel:

```javascript
exports.template = '';
exports.style = '';

exports.methods = {
    console(str) {
        console.log(str);
    },
};

exports.ready = async function() {};

exports.close = function() {};
```

## Send a message

Once we have defined the extension and the panels within the extension, we can try to trigger these messages.

Press CTRL (CMD) + Shift + I to open the console. Open the panel in the console:

 ```javascript
 Editor.Panel.open('hello-world');
 Editor.Message.send('hello-world', 'console', 'log');
 ```

When the Hello World plug-in receives a message, it passes it to the methods.console in panell.js for processing.

So it prints a string "log" on the console

At this point, we have completed an interaction with the panel.
