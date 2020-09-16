# Panel and extension communication

Some useful tools or simple functions can be written directly on the panel, but the panel is not a reliable data storage location. The window may be closed at any time, and the panel will also be closed.

The most common example is that a panel is dragged and docked to the main window. At this time, the panel will be closed first and then reopened in the main window. If the data in the memory used on the panel is not stored and backed up, it will be lost with restart.

At this time, a certain degree of data interaction is required with the extended main body.

Before reading this chapter, you need to have a certain degree of understanding of [Message System](./messages.md).

## Define the method of extending the top and panel

First, we define a package.json:

```json
{
    "name": "hello-world",
    "main": "./browser.js",
    "panels": {
        "default": {
            "title": "hw",
            "main": "./panel.js"
        }
    },
    "contributions": {
        "messages": {
            "upload": {
                "methods": ["saveData"]
            },
            "query": {
                "methods": ["queryData"]
            }
        }
    }
}
```

Then define the extended main file browser.js:

```javascript
exports.methods = {
    saveData(path, data) {
        // Cache it after receiving the data
        this.cache[path] = data;
    },
    queryData(path) {
        const result = this.cache[path];
        delete this.cache[path];
        return result;
    },
};

exports.load = function() {};
exports.unload = function() {};
```

Then define the main file of the panel:

 ```javascript
 exports.ready = async () => {
    const tab = await Editor.Message.send('hello-world','query','tab');
    const subTab = await Editor.Message.send('hello-world','query','subTab');

    // Print the queried data
    console.log(tab, subTab):
    // TODO uses these two data to initialize
 };
 exports.close() {
        // Upload the data to the extension process after receiving the data
        Editor.Message.send('hello-world','upload','tab', 1);
        Editor.Message.send('hello-world','upload','subTab', 0);
    },
 };
 ```

 ## Send a message

 After we define the extension and the panels in the extension, we can try to trigger these messages.

 Press ctrl(cmd) + shift + i to open the console. Open the panel in the console:

 ```javascript
 // default can be omitted, if the panel name is not default, you need to fill in'hello-world.xxx'
 Editor.Panel.open('hello-world');
 ```

After opening the panel, the console will print out a sentence:

```
undefined, undefined
```

This is because the data has not yet been submitted. Let's close this panel and open it again. At this time, the console prints out the data:

 ```
1, 0
 ```

 Because when the panel is closed, two messages are sent:

```javascript
Editor.Message.send('hello-world','upload','tab', 1);
Editor.Message.send('hello-world','upload','subTab', 0);
```

Through these two messages, the Message system first saves the data to the extension process according to the upload definition in messages ("methods": ["saveData"]).

When opening the panel again, pass:

```javascript
const tab = await Editor.Message.send('hello-world','query','tab');
const subTab = await Editor.Message.send('hello-world','query','subTab');
```

Query the data just saved, initialize the interface, and print to the console.

So far, we have completed the interaction between the panel and the expansion process.