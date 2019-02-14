# docs-3d
This is the Cocos3D docs repository.

## How to use?
We have several command could help you publish your modify immediately.

Firstly you need to init your environment. Please execute:

```
npm install

gitbook install
```

1. `npm run build` this command is used when you want to publish your modify officially.
2. `npm run preview -- -o file1,file2,file3` this command will help you preview those files quickly with ignoring other files automatically.
3. `npm run upload` you can set the dest path in package.json manually.
4. `npm run publish` this command contains `npm run build` and `npm run upload` both commands.

## How to add a new docs?

Add a new .md file in `\zh` and `\en` both fold, and modify the SUMMARY.md increase a corresponding index.

## How to add a new version switch

Modify the book.json file which is existed in `\zh` and `\en` fold. You can increase new version switch through adding a new link object at the `version.link` array.