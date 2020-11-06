# First Extension

Through this article, we will learn to create a **Cocos Creator 3D** extension and execute a custom script through the extension.

## Create and Install Extension

Find `~/.CocosEditor3D/packages` Windows user is `C:\Users\${your user name}\.CocosEditor3D\packages)`, or `${your project path}/packages` folder, if the folder does not exist, create a new one.
Create an empty folder in this folder, name it `hello-world`, and create two files `browser.js` and `package.json` in the folder.
The structure of the directory where the extension is located is roughly as follows:

```
hello-world
  |--browser.js
  |--package.json
```

## Define the Description File package.json

Every extension needs a package.json file to describe the purpose of the extension. Only after the description file `package.json` is fully defined, the **Cocos Creator 3D** editor can know the specific functions defined in this extension, loading entry and other information.

Although the definition of many fields in `package.json` is similar to that of `node.js`'s npm package, they are obviously customized for different products and services. So the npm module downloaded from the npm community cannot be directly put into **Cocos Creator 3D** to become an extension, but we can use the modules in the npm community in the **Cocos Creator 3D** extension.

To continue the operation just now, fill in the content in the newly created `package.json` file:

```json
{
    "name": "hello-world",
    "version": "1.0.0",
    "main": "./browser.js",
    "description": "A simple extension",
    "contributions": {
        "menu": [{
            "path": "Develop",
            "label": "test",
            "message": "log"
        }],
        "messages": {
            "log": {
                "methods": ["log"]
            }
        }
    }
}
```

Now, it is needed to define a messages object in contributions, which is the method of editor message registration. This message can be bound to one or more methods defined in the extension.
For more definition data, please refer to the [Message](./contributions-messages.md) documentation.

Next, define a menu array in contributions to provide basic information of a menu to the menu component. Finally bind this menu to a message. For more details, please refer to the [Menu](./contributions-menu.md) documentation.

Careful that after the menu is pressed, the triggered action is notified by the message between the extensions, and the message system is the way of interaction between the extensions.

For a detailed `package.json` format definition, please refer to the [Extension](./define.md) documentation.

## Entry program browser.js

After defining the description file, the next step is to write the entry program `browser.js`.

The content is as follows:

```javascript
'use strict';

// The method defined in the extension
exports.methods = {
    log() {
        console.log('Hello World');
    },
};

// Execute when the extension is started
exports.load = {};

// Execute when the extension is closed
exports.unload = {};
```

This entry program will be loaded during the startup of **Cocos Creator 3D**. The methods defined in methods will be used as the operation interface, which can be called across extensions through [messages](./messages.md) or communicate with the panel.

## Run extension

Now, we can open **Cocos Creator 3D**, find and open the **Extension --> Extension Manager** at the top, and select the extension location (global or project) on the panel.

Next, find the **Refresh** button at the top and click to manually update the extended list information at that location. Then the extension list will show the extensions that have been found, we can start, close, or restart the corresponding extensions in the list control.

If the extension has been started, a Develop menu will appear in the top menu area with a test menu item. After clicking, the message will be triggered according to the definition, and the corresponding method in the extension will be executed according to the message definition, and then the log information of `Hello World` will be printed out on the console.

Congratulations you have written your first simple editor extension.