# WeChat Mini Games Engine Plugin Instructions

The **Game Engine Plugin** is a new feature added to **WeChat v7.0.7**, which has the official version of the Cocos Creator 3D engine built in. If the plugin is enabled in the first game the player experiences, all games that also have the plugin enabled do not need to download the Cocos Creator 3D engine again, just use the same version of the engine directly from the public plugin library, or incremental update the engine.

For example, when a player has played an A game developed using Cocos Creator 3D v1.0.2, and the A game already enabled this plugin. Then he played the B Game, also developed by v1.0.2, and would not have needed to redownload the Cocos Creator 3D engine if the B game had also enabled this plugin. Even if the B Game is developed using Cocos Creator 3D v1.0.3, WeChat only needs to incremental update the difference between the two engine versions. This will drastically reduce the download counts of mini games, and improve the startup speed of mini games by 0.5-2s for a better user experience.

## How to use

Cocos Creator 3D has supported this feature since **v1.0.2**. Simply check the **Separate Engine** option in the **Build** panel, and then build and release as normal, without additional manual operation. (This feature is only available when the built-in engine is used and the build is in non-debug mode.)

![](./wechatgame-plugin/build-options.jpg)

## FAQ

Q: Does the engine plugin feature support engine customization?<br/>
A: Not supported. If the version does not match or the engine customization is enabled during the build, the built package will not actually use the engine plugin feature properly, although the editor will continue to build after an error occurs.

Q: The project enable the engine module clipping, should I need to disable it when using the engine plugin?<br/>
A: No, the project can continue to use the engine module clipping as before. The engine plugin provides a complete engine that is compatible with all clipping settings without affecting the original project package.

Q: After the engine plugin is enabled, will the engine code still be counted into the first package?<br/>
A: According to WeChat's rules, it will still be counted.

Q: After the engine plugin is enabled, can I remove all modules in **Project Setting -> Modules** of editor to reduce the package size?<br/>
A: No, because WeChat only supports engine plugin since v7.0.7, if the engine is clipped randomly, the game may not be able to run on a lower version of WeChat.

Q: When the engine plugin is enabled, prompt "Code package unpacking failed" or "Login user is not the developer of the Mini Program" in the WeChat DevTools, while the physical device previews correctly?<br/>
A: The default appid in the **Build** panel is a common test id, and according to WeChat's rules, you need to fill in the **appid** applied for yourself to test the engine plugin.

Q: When the engine plugin is enabled, prompt "Unauthorized plugin, `Add plugin`" in the WeChat DevTools?<br/>
A: Click the `Add plugin` in the prompt, then select add CocosCreator3D plugin and recompile. If prompt "There are no plugins to add" when you add the plugin, you can select the **Clear Cache -> Clear All** option in the WeChat DevTools and try again.

## Reference documentation

- [WeChat Mini Games Engine Plugin Development Documentation](https://developers.weixin.qq.com/minigame/dev/guide/base-ability/game-engine-plugin.html)
