# Overview of sound systems

The interface of the sound system is mainly for two types of requirements, one category is "music" that is longer in length and is used for loop play, the other category is "sound effect" that is shorter in length and is used one-time play. <br>
All audio resources are imported into the editor in the format of **audioClip** resources. If you want to play audio, you first need to create **AudioSourceComponent** in the scene.

For music, **audioClip** resources can be assigned directly to the `clip` property on **AudioSourceComponent**. If you want to play music you can set the `playOnAwake` property of the **AudioSourceComponent** to true or call its `play` method in the script. <br>
For sound effects, you can call the `playOneShot` method of **AudioSourceComponent** in the script, calling this method requires incoming sound clips and the volume of playback.

Note that although the **audioClip** resource itself also has direct interface implementations such as `play`, but **AudioSourceComponent** is the usual entry point, try to use components to component the workflow.

## Audio related events
All **AudioClip** resource objects are one EventTarget：
* The `started` event is sent when the audio starts to actually play.
* When the audio playback ends naturally, the `ended` event will be issued.

## Platform differentiation

Web audio API, DOM audio, WeChat mini game audio are currently supported, although the audio interface implementations of each platform are not fully unified at runtime, <br>
We've tried to minimize this difference in the engine, but there's still some incongruity in the engine:
* The DOM audio mode on iOS platform does not support adjusting the volume, and all volume related attributes will not be valid.
* Only Web Audio mode supports multiple playback of the same audio. In other modes, to avoid repeated creation, the default behavior of calling `playOneShot` multiple times is to replay it from scratch.
* At present, most platforms have complied with the latest [auto play police](https://www.chromium.org/audio-video/autoplay), Even if `playOnAwake` is set, it will only start playing when the user input is first received.