## 9/27/2021
- Found [Holographic Remoting](https://docs.microsoft.com/en-us/windows/mixed-reality/develop/unity/unity-play-mode?tabs=openxr) for Unity - super useful for quickly testing changes in the HoloLens!
- Followed [how to add near interactivity](https://docs.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/features/input/how-to-add-near-interactivity?view=mrtkunity-2021-05) to add custom input (touch) handling for [mesh deformation](https://catlikecoding.com/unity/tutorials/mesh-deformation/). This work is a starting point for terrain interaction/wave synthesis.
- Added mesh volume calculation as a volume/pitch modifier to an looping AudioClip (one sphere for volume, one for pitch). Would be cool to use the waveform as a live texture somewhere.
![Mesh Deformation](imgs/Mesh_Deformation_Touch_Demo.gif)

#### Procedural Terrain/Mesh Generation in Unity
- Perlin noise, random noise, simplex noise

#### Procedural Audio Generation in Unity
- Unity script for programmatic `AudioClip` generation from [API doc](https://docs.unity3d.com/ScriptReference/AudioClip.Create.html).
```
using UnityEngine;
using System.Collections;

public class ExampleClass : MonoBehaviour
{
    public int position = 0;
    public int samplerate = 44100;
    public float frequency = 440;

    void Start()
    {
        AudioClip myClip = AudioClip.Create("MySinusoid", samplerate * 2, 1, samplerate, true, OnAudioRead, OnAudioSetPosition);
        AudioSource aud = GetComponent<AudioSource>();
        aud.clip = myClip;
        aud.Play();
    }

    void OnAudioRead(float[] data)
    {
        int count = 0;
        while (count < data.Length)
        {
            data[count] = Mathf.Sin(2 * Mathf.PI * frequency * position / samplerate);
            position++;
            count++;
        }
    }

    void OnAudioSetPosition(int newPosition)
    {
        position = newPosition;
    }
}
```
- Another example using `OnAudioFilterRead`
```
void OnAudioFilterRead(float[] data, int channels) {
    for (int i = 0; i < data.Length; i++) {
        data[i] = Mathf.Sin(2 * Mathf.PI * frequency * i / samplerate);
    }
}
```


## 9/25/2021
- https://www.konsfik.com/procedural-audio-made-in-unity3d/
- https://www.linkedin.com/pulse/building-modular-synthesizer-unity-using-pure-data-heavy-rothmann/
- In Unity, edit audio clips using `AudioClip.SetData` and `AudioClip.GetData`
- https://www.youtube.com/watch?v=O33YV4ooHSo
- http://sonology.org/wp-content/uploads/2019/10/Ioannis-Tsirikoglou.pdf
- https://phaidra.kug.ac.at/open/o:60753

#### WebXR
- Looked into WebXR technology for the HoloLens. Found this [website](https://hololens.glitch.me/). Looks like Firefox Reality for HoloLens 2 is no longer supported or maintained, so best option is Beta Edge Browser.
- Was unable to install [Edge Beta](https://docs.microsoft.com/en-us/hololens/hololens-new-edge) through the Hololens, so tried using the Device Portal from this [article](https://docs.microsoft.com/en-us/windows/mixed-reality/develop/platform-capabilities-and-apis/using-the-windows-device-portal#installing-an-app). It installed successfully but wouldn't run. After some debugging, I realized my HoloLens wasn't up to date.
- Played around with some WebXR samples. Most were for VR, which the HoloLens also supports.
- [This](https://www.servoexperiments.com/webxr-particles/) was a super cool example using AR and interaction (made w/ Three.js). 

#### Research Mode
- Looked into [Research mode](https://docs.microsoft.com/en-us/windows/mixed-reality/develop/platform-capabilities-and-apis/research-mode) and [samples](https://github.com/microsoft/HoloLens2ForCV).
- Had some difficulties getting the samples to run at first, but then figured out that I hadn't applied Research Mode changes.
- The [SensorVisualization](https://github.com/microsoft/HoloLens2ForCV/tree/main/Samples/SensorVisualization) sample was super cool. It's a Holographic UWP application - built starting from a [DX11 template app](https://docs.microsoft.com/en-us/windows/mixed-reality/develop/native/creating-a-holographic-directx-project) in C++.
![Research Mode Support](imgs/ResearchModeSupport.PNG)


#### Waveform Terrain Synthesis
A method for generating complex, evolving waveforms using a representation of a three-dimensional surface. The surface may be artificially constructed, or taken from actual geospatial data. To generate the wave, an imaginary traveller traverses an orbit, or closed path of some sort, about the terrain; a simple example is a circle about the surface's center or origin point. The traveller moves from point to point about the path, and at each point it moves to, the value of the output waveform is the elevation of the terrain at that point. One complete traversal of the path constitutes one cycle of the output waveform. Evolving waveforms are created by varying the path; for example, in the case of a circular path, the radius can be varied [(Electronic Music, Website)](https://electronicmusic.fandom.com/wiki/Wave_terrain_synthesis)

#### Wavetable Synthesis
Wavetable synthesis is a sound synthesis technique used to create periodic waveforms.[(Wavetable Synthesis, Wikipedia)](https://en.wikipedia.org/wiki/Wavetable_synthesis)

#### Object Recognition
- https://azure.microsoft.com/en-us/services/cognitive-services/custom-vision-service/
- https://ieeexplore.ieee.org/document/9057151
- https://core.ac.uk/download/pdf/326909176.pdf
- Lab Assist Vision, Object Recognition/Tracking for the HoloLens 2 (https://github.com/cookieofcode/LabAssistVision)
- VisionLib Object Tracking
- https://localjoost.github.io/labeling-toy-aircraft-in-3d-space-using/



## 9/21/2021
- Created a custom [pulse shader](https://docs.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/features/rendering/pulse-shader?view=mrtkunity-2021-05) for spatial mapping
- Created, built, and deployed a basic Unity w/ MRTK app. Added an `NearInteractionGrabbable`, `Object Manipulator` and `Contstraint Manager` component to a basic cube.
![Unity App](imgs/20210927_205044_HoloLens.jpg)

- Investigated the Device Portal (accessed through the HoloLens IP address in a browser). There's a cool 3D View feature that shows the current spatial mapping model. It can be even exported as an `.obj` file.
![Device Portal](imgs/DevicePortal.PNG)
![System Performance](imgs/SystemPerformance.PNG)