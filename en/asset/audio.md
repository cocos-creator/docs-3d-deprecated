# Audio

__Sound__ resources are __audio files__. An __audio system__ has two main functions: __playing background music__ and __playing short sound effects__.
For sound resources, there is no difference between the two. After all, audio resources are imported into the editor, `AudioClip` resources perform related audio operations through the `AudioSourceComponent` system component. To use the audio system, please refer to the [Audio System](../audio-system/overview.md) documentation.

## Supported audio resource formats

Currently, the engine's audio system can support the following formats:
  - `.ogg`
  - `.mp3`
  - `.wav`
  - `.mp4`
  - `.m4a`

## Use of sound resources

After adding an `AudioSourceComponent` to a `Node`, drag the imported audio resource from the __Resource Manager__ to the `Clip` of the node `AudioSourceComponent` to control the sound resource:

![](audio/audiocilp.gif)