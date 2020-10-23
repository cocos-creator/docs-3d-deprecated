# Compose panel

We have written the definition of the panel in [package.json](./panel.md). At this time, we need to implement the logic function of the panel.

At this time, you need to identify the main entry file in the panel definition and fill in its content:

```javascript
'use strict';

// html text
exports.template ='';
// style text
exports.style ='';
// html selector after rendering
exports.$ = {};
// method on the panel
exports.methods = {};
// event triggered on the panel
exports.listeners = {};

// Triggered when the panel is rendered successfully
exports.ready = async function() {};
// Triggered when trying to close the panel
exports.beforeClose = async function() {};
// Triggered when the panel is actually closed
exports.close = async function() {};
```

## template

html string, for example:

```javascript
exports.template = `
<header>
    Header
</header>
<section class="test">
    Section
</section>
`;
```

You can also read an html file directly:

```javascript
const {readFileSync} = require('fs');
const {join} = require('path');
exports.template = readFileSync(join(__dirname,'../static/default.html'),'utf8');
```

When the template is defined, when the panel is opened, the content of the template will be automatically rendered on the interface.

In addition, the editor also provides some custom elements, you can refer to [UI](./editor/extension/ui.md) to use.

## style

With html, you need to customize some styles and you need to use style. Style is a string like template.

```javascript
exports.style = `
header {padding: 10px;}
`;
```

Of course, you can also read a css file:

```javascript
const {readFileSync} = require('fs');
const {join} = require('path');
exports.template = readFileSync(join(__dirname,'../static/default.css'),'utf8');
```

## $

This is an html element selector, directly call querySelector to find the specified element and use it as a shortcut.

```javascript
exports.$ = {
    header:'header',
    test:'.test',
};
```

First define the selector. After the template is rendered, the editor will automatically call document.querySelector to find the corresponding element and hang it on this.$:

```javascript
exports.ready = function() {
    console.log(this.$.header); // <header>
    console.log(this.$.test); // <section class="test">
}
```

## methods

The method defined on the panel. The external functions of the panel need to be encapsulated into methods and provided externally in units of functions. Messages can also directly trigger the methods on the panel. For details, please refer to [Message Communication](./contributions-messages.md)

This object is full of functions, please do not mount other types of objects here.

```javascript
exports.methods = {
    open() {
        Editor.Panel.open('hello-world');
    },
};
```

## listeners

After the basic layout is completed, we sometimes need to update the status on some panels according to some situations. At this time, we need to use the listeners function.

```javascript
exports.listeners = {
    /**
     * Triggered when the panel is hidden
     */
    hide() {
        console.log(this.hidden);
    },
    /**
     * Triggered when the panel is displayed
     */
    show() {
        console.log(this.hidden);
    },
    /**
     * Triggered when the panel size changes
     */
    resize() {
        console.log(this.clientHeight);
        console.log(this.clientWidth);
    },
};
```

## ready

When the panel startup is complete, this life cycle function will be triggered.

## beforeClose

When the panel tries to be closed, this function will be triggered. BeforeClose can be an async function that can be used for asynchronous judgment. If it returns false, the current closing operation will be terminated.

Please do not execute the actual destruction and close-related logic code in this function. This step is just for inquiry. Please put the actual destruction in the close function.

**Please use with caution** If the judgment is wrong, the editor or panel window may not be closed normally.

## close

When all the panels in the window are allowed to be closed, the panel close will be triggered. Once the close is triggered, the window will be forcibly closed after the end, so please save the data in the close. If an abnormal close occurs, please make a backup of the data in order to Restore data as much as possible when restarting.