---
title: Holograms 101E：介绍Emulator（模拟器）
date: 2017-10-26 10:04:33
comments: true
reward: true
tags:
  - Holograms
  - Emulator
---

<img src="/assets/holoLens/Holograms101E.jpg">

### 1. 前言

本教程将引导你创建一个构建于Unity的完整项目，演示核心Windows全息和HoloLens功能，包括gaze(凝视)、gestures(手势)、voice input(语音输入)、spatial sound(空间声音)、spatial mapping(空间映射)，本教程将需要大约一小时。

<!-- more -->

### 2. 准备

安装了正确工具的Windows 10 的电脑。

### 3. 项目文件

 *-- 下载项目所需的[文件](https://github.com/Microsoft/HolographicAcademy/archive/Holograms-101.zip)*，需要Unity2017.2或更高版本。
  * 如果你使用Unity5.6，*-- 请使用[这个版本](https://github.com/Microsoft/HolographicAcademy/archive/v1.5.6-101.zip)*。
  * 如果你使用Unity5.5，*-- 请使用[这个版本](https://github.com/Microsoft/HolographicAcademy/archive/v1.5.5-101.zip)*。
  * 如果你使用Untiy5.4，*-- 请使用[这个版本](https://github.com/Microsoft/HolographicAcademy/archive/v1.5.4-101.zip)*。

把文件放到桌面，或者其他容易找到的位置，文件名改为Origami

### 4. 内容

#### 第一章-"Holo" World

本章，我们将创建一个Unity项目，逐步完成构建和部署过程。

##### 目标

* 创建一个基于Unity的项目进行holographich开发。
* 制作一个Hologram（全息图：一种三维图像）。
* 看你做的Hologram。

##### 说明

* 启动Unity。
* 选择Select。
* 输入你刚才下载的Origami文件的本地目录。
* 选择Origami并点击Select Folder。
* 保存新的scene：File->Save Scene As。
* 命名scene为Origami并点击Save按钮。

##### 设置Main Camera

* 在Hierarchy面板，选择Main Camera。
* 在Inspector设置它的Transform Position为0，0，0。
* 找到Clear Flags属性，并且把Skybox改成Solid clolr。
* 点击Background代开一个颜色选择器。
* 设置R,G,B和A都为0。

##### 设置scene

* 在Hierarchy面板，点击Create和Create Empty。
* 右击新的GameObject选中Rename，重命名为OrigamiCollection。
* 在Project面板中从Holograms文件：
  * 拖Stage到Hierarchy面板的OrigamiCollection下。
  * 拖Sphere1到Hierarchy面板的OrigamiCollection下。
  * 拖Sphere2到Hierarchy面板的OrigamiCollection下。
* 右击Hierarchy面板中的Directional Light，选择删除。
* 从Holograms文件，拖Lights到Hierarchy面板的根目录下。
* 在Hierarchy面板中，选中OrigamiCollection。
* 在Inspector，设置Transform Position 为0，-0.5，2.0。
* 点击Unity中的Play按钮，预览你的全息图。
* 你应该在预览窗口看到Origami对象。
* 再次按下Play按钮，停止预览窗口。

##### 把项目从Unity导出到Visual Studio

* 在Unity选择File->Build Settings。
* 在Platform列表选中Windows Store并点击Switch Platform。
* 设置SDK为Universal 10，Build Type为D3D。
* 点击Unity C# Projects。
* 点击Add Open Scenes添加scene。
* 点击Player Settings。
* 在Inspector面板中选择Windows Store logo。然后选择Publishing Settings。
* 在Capabilities部分，选择Microphone和SpatialPerception功能。
* 返回到Build Settings窗口，点击Build。
* 创建一个文件夹，命名为“App”。
* 单击App文件。
* 点击Select Folder。
* 当Unity完成之后，会出现一个资源管理器窗口。
* 打开App文件。
* 使用Visual Studio打开Origami。
* 在Visual Studio的顶部工具栏，把Debug改为Release，把ARM改为X86。
 * 点击Device按钮旁边的旁边的向下的箭头，选择HoloLens Emulator。
 * 点击Debug->Start without debugging或者按Ctrl+F5。
 * 一段时间后Origami启动，第一次启动模拟器，它启动的时间最长可达15分钟，一旦开始，不要关闭它。

#### 第二章-Gaze(凝视)

这一章，我们将介绍三种与你的全息图交互的方法 --凝视。

##### 目标

* 使用world-locked光标可视化你的目光。

##### 说明

* 返回你的Unity项目，如果Build Settings一直开着，则把它关掉。
* 在Porject面板中选中Holograms文件。
* 拖Cursor对象到Hierarchy面板中的根目录。
* 双击Cursor去仔细查看。
* 在Project面板右击Scripts文件。
* 点击子菜单中的Create。
* 选择C# Script。
* 把script命名为WorldCursor。注意：名字是区分大小写的，你不需要添加.cs文件。
* 在Hierarchy面板中选择Cursor。
* 将WorldCursor拖到Inspector面板中。
* 双击WorldCursor打开到Visual Studio。
* 把下面的代码复制粘贴到WorldCursor.cs并保存。
```
using UnityEngine;
public class WorldCursor : MonoBehaviour
{
    private MeshRenderer meshRenderer;
    // Use this for initialization
    void Start()
    {
        // Grab the mesh renderer that's on the same object as this script.
        meshRenderer = this.gameObject.GetComponentInChildren<MeshRenderer>();
    }
    // Update is called once per frame
    void Update()
    {
        // Do a raycast into the world based on the user's
        // head position and orientation.
        var headPosition = Camera.main.transform.position;
        var gazeDirection = Camera.main.transform.forward;
        RaycastHit hitInfo;
        if (Physics.Raycast(headPosition, gazeDirection, out hitInfo))
        {
            // If the raycast hit a hologram...
            // Display the cursor mesh.
            meshRenderer.enabled = true;
            // Move thecursor to the point where the raycast hit.
            this.transform.position = hitInfo.point;
            // Rotate the cursor to hug the surface of the hologram.
            this.transform.rotation = Quaternion.FromToRotation(Vector3.up, hitInfo.normal);
        }
        else
        {
            // If the raycast did not hit a hologram, hide the cursor mesh.
            meshRenderer.enabled = false;
        }
    }
}
```

* 重新运行App，File->Build Settings。
* 返回以前用于部署到模拟器的Visual Studio解决方案。
* 当提示“Reload All”，选中。
* 点击Debug->Start without debugging或者是按Ctrl+F5。
* 用Xbox来查看周围的场景，注意游标如何与对象的形状进行交互。

#### 第三章-Gestures(手势)

这一章，我们将添加对手势的支持。当用户选中一个纸球时，我们将使用Unity的物理引擎是球体落下。

##### 目标

用选择手势控制你的全息图。

##### 说明

我们将首先创建一个脚本，而不是检测选择的手势。
* 在Script文件，创建一个脚本，命名为GazeGestureManager。
* 把GazeGestureManager脚本拖到Hierarchy面板中的OrigamiCollection对象中。
* 使用Visual Studio打开GazeGestureManager脚本，并添加一下代码。

###### GazeGestureManager.cs
```
using UnityEngine;
using UnityEngine.XR.WSA.Input;
public class GazeGestureManager : MonoBehaviour
{
    public static GazeGestureManager Instance { get; private set; }
    // Represents the hologram that is currently being gazed at.
    public GameObject FocusedObject { get; private set; }
    GestureRecognizer recognizer;
    // Use this for initialization
    void Start()
    {
        Instance = this;
        // Set up a GestureRecognizer to detect Select gestures.
        recognizer = new GestureRecognizer();
        recognizer.Tapped += (args) =>
        {
            // Send an OnSelect message to the focused object and its ancestors.
            if (FocusedObject != null)
            {
                FocusedObject.SendMessageUpwards("OnSelect", SendMessageOptions.DontRequireReceiver);
            }
        };
        recognizer.StartCapturingGestures();
    }
    // Update is called once per frame
    void Update()
    {
        // Figure out which hologram is focused this frame.
        GameObject oldFocusObject = FocusedObject;
        // Do a raycast into the world based on the user's
        // head position and orientation.
        var headPosition = Camera.main.transform.position;
        var gazeDirection = Camera.main.transform.forward;
        RaycastHit hitInfo;
        if (Physics.Raycast(headPosition, gazeDirection, out hitInfo))
        {
            // If the raycast hit a hologram, use that as the focused object.
            FocusedObject = hitInfo.collider.gameObject;
        }
        else
        {
            // If the raycast did not hit a hologram, clear the focused object.
            FocusedObject = null;
        }
        // If the focused object changed this frame,
        // start detecting fresh gestures again.
        if (FocusedObject != oldFocusObject)
        {
            recognizer.CancelGestures();
            recognizer.StartCapturingGestures();
        }
    }
}
```

* 在Script文件创建另一个脚本，这次命名为SphereCommands。
* 在Hierarchy视图中展开OrigamiCollection对象。
* 把SphereCommands脚本拖到Hierarchy面板中的Sphere1对象中。
* 把SphereCommands脚本拖到Hierarchy面板中的Sphere2对象中。
* 用Visual Studio打开脚本编辑，用下面 的代码替换默认的代码：

###### SphereCommands.cs
```
using UnityEngine;
public class SphereCommands : MonoBehaviour
{
    // Called by GazeGestureManager when the user performs a Select gesture
    void OnSelect()
    {
        // If the sphere has no Rigidbody component, add one to enable physics.
        if (!this.GetComponent<Rigidbody>())
        {
            var rigidbody = this.gameObject.AddComponent<Rigidbody>();
            rigidbody.collisionDetectionMode = CollisionDetectionMode.Continuous;
        }
    }
}
```

* 导出，构建和部署App到HoloLens模拟器上。
* 环顾四周，并围绕其中的一个球体。
* 按下Xbox控制器上的A按钮，或者按空格来模拟选择手势。

#### 第四章-声音

这一章，我们将添加声音命令的支持：“Reset World”使掉落的球体回到原来的位置，“Drop sphere”使球体落下。

##### 目标

* 添加总是在后台监听的语音命令。
* 创建一个响应语音命令的全息图。

##### 说明

* 在Script文件，创建一个名为SpeechManager的脚本。
* 把SpeechManager脚本拖到Hierarchy面板的OrigamiCollection对象中。
* 使用Visual Studio代开SpeechManager脚本。
* 复制粘贴以下代码带SpeechManager.cs并保存：

###### SpeechManager.cs
```
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using UnityEngine.Windows.Speech;
public class SpeechManager : MonoBehaviour
{
    KeywordRecognizer keywordRecognizer = null;
    Dictionary<string, System.Action> keywords = new Dictionary<string, System.Action>();
    // Use this for initialization
    void Start()
    {
        keywords.Add("Reset world", () =>
        {
            // Call the OnReset method on every descendant object.
            this.BroadcastMessage("OnReset");
        });
        keywords.Add("Drop Sphere", () =>
        {
            var focusObject = GazeGestureManager.Instance.FocusedObject;
            if (focusObject != null)
            {
                // Call the OnDrop method on just the focused object.
                focusObject.SendMessage("OnDrop", SendMessageOptions.DontRequireReceiver);
            }
        });
        // Tell the KeywordRecognizer about our keywords.
        keywordRecognizer = new KeywordRecognizer(keywords.Keys.ToArray());
        // Register a callback for the KeywordRecognizer and start recognizing!
        keywordRecognizer.OnPhraseRecognized += KeywordRecognizer_OnPhraseRecognized;
        keywordRecognizer.Start();
    }
    private void KeywordRecognizer_OnPhraseRecognized(PhraseRecognizedEventArgs args)
    {
        System.Action keywordAction;
        if (keywords.TryGetValue(args.text, out keywordAction))
        {
            keywordAction.Invoke();
        }
    }
}
```

* 用Visual Studio打开SphereCommands脚本。
* 更新脚本代码如下：

##### SphereCommands.cs
```
using UnityEngine;
public class SphereCommands : MonoBehaviour
{
    Vector3 originalPosition;
    // Use this for initialization
    void Start()
    {
        // Grab the original local position of the sphere when the app starts.
        originalPosition = this.transform.localPosition;
    }
    // Called by GazeGestureManager when the user performs a Select gesture
    void OnSelect()
    {
        // If the sphere has no Rigidbody component, add one to enable physics.
        if (!this.GetComponent<Rigidbody>())
        {
            var rigidbody = this.gameObject.AddComponent<Rigidbody>();
            rigidbody.collisionDetectionMode = CollisionDetectionMode.Continuous;
        }
    }
    // Called by SpeechManager when the user says the "Reset world" command
    void OnReset()
    {
        // If the sphere has a Rigidbody component, remove it to disable physics.
        var rigidbody = this.GetComponent<Rigidbody>();
        if (rigidbody != null)
        {
            rigidbody.isKinematic = true;
            Destroy(rigidbody);
        }
        // Put the sphere back into its original local position.
        this.transform.localPosition = originalPosition;
    }
    // Called by SpeechManager when the user says the "Drop sphere" command
    void OnDrop()
    {
        // Just do the same logic as a Select gesture.
        OnSelect();
    }
}
```

* 导出，构建并部署App到HoloLens模拟器上。
* 这个模拟器是支持你电脑的麦克风去响应你的声音：调整视图，使光标在一个球体上，并说“Drop Sphere”。
* 说“Reset World”是他们回到原来的位置上。

#### 第五章-空间立体声

这一章，我们将在应用里添加音乐，然后触发声音对某些动作起作用。我们将使用空间立体声在三维空间给声音一个特定的位置。

##### 目标

* 在你的世界听全息图。

##### 说明

* 在Unity顶部菜单选中 Edit->Project Settings->Audio
* 找到Spatializer Plugin设置，并选中MS HRTF Spatializer。
* 从Holograms文件，拖一个Ambience对象放到Hierarchy面板的OrigamiCollection对象中。
* 选择OrigamiCollection并找到Audio Source部分，更改一些属性：
  * 选中Spatialize属性。
  * 选中Play On Awake。
  * 把Spatial Blend一直拖到右边。
  * 选中Loop。
  * 展开3D Sound Settings，在Doppler Level输入0.1。
  * 把Volume Rolloff设置为Logarithmic Rolloff。
  * 设置Max Distance为20。
* 在Script文件，创建一个名为SphereSounds的脚本。
* 拖SphereSound到Hierarchy面板的Sphere1和Sphere2中。
* 在Visual Studio中打开SphereSounds，修改代码并保存。

##### SphereSounds.cs
```
using UnityEngine;
public class SphereSounds : MonoBehaviour
{
    AudioSource impactAudioSource = null;
    AudioSource rollingAudioSource = null;
    bool rolling = false;
    void Start()
    {
        // Add an AudioSource component and set up some defaults
        impactAudioSource = gameObject.AddComponent<AudioSource>();
        impactAudioSource.playOnAwake = false;
        impactAudioSource.spatialize = true;
        impactAudioSource.spatialBlend = 1.0f;
        impactAudioSource.dopplerLevel = 0.0f;
        impactAudioSource.rolloffMode = AudioRolloffMode.Logarithmic;
        impactAudioSource.maxDistance = 20f;
        rollingAudioSource = gameObject.AddComponent<AudioSource>();
        rollingAudioSource.playOnAwake = false;
        rollingAudioSource.spatialize = true;
        rollingAudioSource.spatialBlend = 1.0f;
        rollingAudioSource.dopplerLevel = 0.0f;
        rollingAudioSource.rolloffMode = AudioRolloffMode.Logarithmic;
        rollingAudioSource.maxDistance = 20f;
        rollingAudioSource.loop = true;
        // Load the Sphere sounds from the Resources folder
        impactAudioSource.clip = Resources.Load<AudioClip>("Impact");
        rollingAudioSource.clip = Resources.Load<AudioClip>("Rolling");
    }
    // Occurs when this object starts colliding with another object
    void OnCollisionEnter(Collision collision)
    {
        // Play an impact sound if the sphere impacts strongly enough.
        if (collision.relativeVelocity.magnitude >= 0.1f)
        {
            impactAudioSource.Play();
        }
    }
    // Occurs each frame that this object continues to collide with another object
    void OnCollisionStay(Collision collision)
    {
        Rigidbody rigid = gameObject.GetComponent<Rigidbody>();
        // Play a rolling sound if the sphere is rolling fast enough.
        if (!rolling && rigid.velocity.magnitude >= 0.01f)
        {
            rolling = true;
            rollingAudioSource.Play();
        }
        // Stop the rolling sound if rolling slows down.
        else if (rolling && rigid.velocity.magnitude < 0.01f)
        {
            rolling = false;
            rollingAudioSource.Stop();
        }
    }
    // Occurs when this object stops colliding with another object
    void OnCollisionExit(Collision collision)
    {
        // Stop the rolling sound if the object falls off and stops colliding.
        if (rolling)
        {
            rolling = false;
            impactAudioSource.Stop();
            rollingAudioSource.Stop();
        }
    }
}
```

* 保存脚本，返回Unity。
* 导出，构建并且部署App到HoloLens模拟器上。
* 戴上耳机才能得到充分的效果，离舞台越来越近，听到声音的变化。

#### 第六章-空间映射

现在，我们将使用空间映射来把游戏板放置在现实世界的真是物体上。

##### 目标

* 把你的真是世界带入到虚拟世界中。
* 把你的全息图放在最重要的地方。

##### 说明

* 在Project面板中点击Holograms文件。
* 拖Spatial Mapping(空间映射)到Hierarchy面板的根目录。
* 在Hierarchy面板中点击Spatial Mapping。
* 在Inspector 面板，改变以下属性：
  * 选中Draw Visual Meshes（视觉化网格）.
  * 点位到Draw Material（绘制材料）并且点击右边的圆圈。在上边的搜索栏中查找“wireframe”类型。点击结果并关闭窗口。
* 导出，构建并部署App到HoloLens模拟器。
* 当程序运行时，先前扫描的真实客厅的网格则会出现在线框中。
* 看到一个滚动的球体从舞台上落下，落在地板上。

现在，我们将为你展示将OrigamiCollection移动到新的位置。
* 在Script文件，创建一个名为TapToPlaceParent的脚本。
* 在Hierarchy面板中，展开OrigamiCollection并选中Stage对象。
* 拖TapToPlaceParent脚本到Stage对象。
* 用Visual Studiodakai TapToPlaceParent，并修改代码，如下：

###### TapToPlaceParent.cs
```
using UnityEngine;
public class TapToPlaceParent : MonoBehaviour
{
    bool placing = false;
    // Called by GazeGestureManager when the user performs a Select gesture
    void OnSelect()
    {
        // On each Select gesture, toggle whether the user is in placing mode.
        placing = !placing;
        // If the user is in placing mode, display the spatial mapping mesh.
        if (placing)
        {
            SpatialMapping.Instance.DrawVisualMeshes = true;
        }
        // If the user is not in placing mode, hide the spatial mapping mesh.
        else
        {
            SpatialMapping.Instance.DrawVisualMeshes = false;
        }
    }
    // Update is called once per frame
    void Update()
    {
        // If the user is in placing mode,
        // update the placement to match the user's gaze.
        if (placing)
        {
            // Do a raycast into the world that will only hit the Spatial Mapping mesh.
            var headPosition = Camera.main.transform.position;
            var gazeDirection = Camera.main.transform.forward;
            RaycastHit hitInfo;
            if (Physics.Raycast(headPosition, gazeDirection, out hitInfo,
                30.0f, SpatialMapping.PhysicsRaycastMask))
            {
                // Move this object's parent object to
                // where the raycast hit the Spatial Mapping mesh.
                this.transform.parent.position = hitInfo.point;
                // Rotate this object's parent object to face the user.
                Quaternion toQuat = Camera.main.transform.localRotation;
                toQuat.x = 0;
                toQuat.z = 0;
                this.transform.parent.rotation = toQuat;
            }
        }
    }
}
```

* 导出，构建并部署App。
* 现在，你可以通过使用选择手势（A或者空格键），然后移动到新的位置，再次使用手势，将游戏放在特定的位置。

#### 结束

这是本教程的尾声。

你学了：
* 如何在Unity中创建一个全息应用。
* 如何使用凝视、手势、语音、声音、和空间映射。
* 如何在Visual Studio构建部署程序。
