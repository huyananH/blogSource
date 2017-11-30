---
title: Holograms 210：Gaze（凝视）
date: 2017-10-30 16:24:37
comments: true
reward: true
tags:
  - Holograms
---

<img src="/assets/holoLens/Holograms210.jpg">

### 1. 前言

凝视是输入的第一种形式，它揭示了用户的意图和意识。Holograms 210（又名Project Explorer）深入研究了与凝视相关的Windows全息图的概念。我们将为我们的游标和全息图添加上下文感知，充分利用你的应用去了解用户的凝视。

<!-- more -->

我们有一个友好的宇航员去帮助我们学习凝视的概念。在Holograms 101，我们有在你的凝视下的一个简单的光标。今天我们要做一个超简单的光标。
* 我们做光标和全息图凝视：两者都将根据用户正在看或者用户不在看的位置进行修改，这使他们具有上下文感知能力。
* 我们将向你展示定位技术来帮助用户点击更小的目标。
* 我们将向你展示如何使用方向指示器将用户的注意力吸引到你的全息图上。
* 我们将教你如何在用户的世界各地移动时，将用户的全息图带给你。

### 2. 准备

* *-- [安装了正确工具](https://developer.microsoft.com/en-us/windows/mixed-reality/install_the_tools)* 的Windows 10电脑。
* 一些基础的C#编程能力。
* 你应该完成 *-- [Holograms 101](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_101)* 。
* 配置 *-- [用于开发](https://developer.microsoft.com/en-us/windows/mixed-reality/Using_Visual_Studio.html#enabling_developer_mode)* 的HoloLens设备。

### 3. 项目文件

* *-- 下载项目所需的[文件](https://github.com/Microsoft/HolographicAcademy/archive/Holograms-210-Gaze.zip)* 需要Unity2017.02或以上的版本。
  * 如果你还需要Unity5.6的支持， *-- 请使用[这个版本](https://github.com/Microsoft/HolographicAcademy/archive/v1.5.6-210.zip)*
  * 如果你还需要Unity5.5的支持， *-- 请使用[这个版本](https://github.com/Microsoft/HolographicAcademy/archive/v1.5.5-210.zip)*
  * 如果你还需要Unity5.4的支持， *-- 请使用[这个版本](https://github.com/Microsoft/HolographicAcademy/archive/v1.5.4-210.zip)*
* 解压文件放到你的桌面或者其他容易找到的路径。

### 4. 排错和提示

* 需要在Tools（工具）-> Options（选项） ->Debugging（调试）下禁用（取消选中）“Just My Code”（仅我的代码），以便在代码中触发断点

### 5. 内容

#### 第一章-Unity设置

##### 目标

* 为HoloLens开发优化Unity。
* 导入资源文件并设置场景。
* 在HoloLens中看宇航员。

#### 说明

* 启动Unity。
* 选择New Project（创建项目）。
* 该项目命名为ModelExplorer。
* 输入该项目路径为先前解压文件的路径。
* 确认项目为3D。
* 点击Create Project(创建项目)。

#### 对于HoloLens,Unity的设置

我们需要让Unity知道我们试图导出应用程序应该创建一个全息图，而不是2D视图。我们用添加HoloLens为虚拟现实设备来实现这一点。
* 选择Edit->Project Settings->Player(查看播放器设置)
* 在Player的Inspector面板中选择Windows Store的图标。
* 展开Other Settings（其他设置）。
* 在Rendering（渲染）部分，选中“Virtual Reality Supported”复选框添加Virtual Reality SDKs列表。（我在实际操作中，是点击Virtual Reality moved to XR Settings，然后再选中“Virtual Reality Supported”）。
* 验证列表是否出现Windows Holographic，如果没有，则选择+按钮，然后选择 Windows Holographic。（我在实际操作中，出现的是Windows Mixed Reality）。
* 选择Edit ->Project Settings->Quality。
* 点击Windows Store图标下边Default中的向下箭头。
* 对于Windows Store APP选择Fastest。

#### 导入图形资源文件

* 右击Project面板的Assets（资源文件）。
* 点击Import Package(导包)->Custom Package(自定义包)。
* 进入你先前解压的文件目录下，进入Starting目录下，点击ModelExplorer.unitypackage。
* 点击Open（打开）。
* 包加载完之后，点击Import（导入）按钮。

#### 设置场景

* 在Hierarchy面板，删除Main Camera。
* 在Project面板中的HoloToolkitwenjian，打开Utilities文件，然后打开Prefabs文件。
* 把Prefabs文件夹下的Main Camera拖到Hierarchy面板中。
* 右击Hierarchy面板中的Directional Light并选择删除。
* 在Project面板中的Holograms文件中，将一下资源文件拖到Hierarchy面板中。
  * AstroMan（宇航员）
  * Lights (方向光)
  * SpaceAudioSource（空间音频资源）。
  * SpaceBackground(空间背景)。
* 点击Play（运行）去看宇航员。
* 再次点击Play（运行）去停止。
* 在Project面板中，找到Fitbox资源文件拖到Hierarchy面板的根目录下。
* 在Hierarchy面板中选择Fitbox。
* 把AstroMan集合拖到Inspector面板中的Hologram Collection属性中。

#### 构建项目

* 保存新的场景：File->Save Scenes As。
* 点击New Folder（新建文件夹）并把文件夹的名字改为Scenes。
* 把项目名字设为“ModelExplorer”并保存到Scenes文件中。
* 在Unity中选择File->Build Settings。
* 点击Add Open Scenes去添加场景。
* 在Platform（平台）列表中，选择Windows Store并点击Switch Platform（更换平台）。
* 把SDK设为Universal 10 ，并把Build Type设为D3D。
* 选中Unity C# Projects。
* 点击Build。
* 创建一个名为App的文件夹。
* 单击App文件夹。
* 点击选中文件夹。
* 到Unity执行完之后，会出现一个文件资源管理器窗口。
* 打开App文件夹。
* 打开ModelExplorer visual Studio解决方案。（就是双击App文件夹下的ModelExplorer.sln文件）
* 在Visual Studio中，右击解决方案资源管理器中的Pack.appxmanifest，并选择View Code（查看代码）。
* 找到指定TargetDeviceFamily行，并修改Name=Windows.Universal为Name=Windows.Holographic。
* 保存Pack.appxmanifest文件。
* 在Visual Studio的顶部工具栏中，把Debug改为Release，把ARM改为x86。
* 点击Device旁边的向下箭头，选择Remote Machine。
* 输入你设备的IP地址，并设置Authentication Mode（认证模式）为Universal（Unencrypted Protocol）未加密协议。点击选择。如果你不知道你设备的IP地址，在Settings->Network & Internet-> Advanced Options中查看。
* 在顶部菜单中，点击Debug（调试）->Start Without debugging（开始执行（不调试）） 或者按Ctrl+F5，如果这是第一次在你的设备上部署，你将需要与Visual Studio中匹配。

#### 第二章-光标和目标的反馈

##### 目标

* 光标的视觉设计和行为。
* 基于凝视的光标反馈。
* 基于凝视的全息图反馈。

我们将以一些光标设计原则为基础，换句话说：
  >
  1. 光标总是存在的。
  2. 不要让光标太小或太大。
  3. 避免遮挡内容。

##### 说明

* 点击Hierarchy面板中的Create菜单。
* 选中Create Empty。
* 右击新创建的GameObject，并重命名为“Managers”。
* 在Inspector面板中，点击Add Component按钮。
* 在菜单中，在搜索框中查询Gaze Manager。选中搜索的结果。
* 在Inspector面板中，选中RaycastLayerMask下拉，并取消选中TransparentFX。
* 在Project面板中的HoloToolkit/Input/Prefabs文件，找到Cursor资源文件。
* 把Cursor拖到Hierarchy面板中。
* 在Hierarchy面板中，选中Cursor。
* 在Inspector面板中，点击Add Component按钮。
* 在菜单搜索框中，输入Cursor Manager。选中搜索结果。
* 展开Hierarchy面板中的Cursor。
* 把CursorOnHolograms拖到Inspector面板中的Cursor On Holograms。
* 把CursorOffHolograms拖到Inspector面板中的Cursor Off Holograms。

此时，我们需要编辑GazeManager.cs文件，并编辑它完成以下任务：
  * 完成一个物理射线。
  * 存储射线交叉点的位置和法线。
  * 如果这条射线没有击中任何东西，把位置和法线设为默认。
你可以在GazeManager.cs注解中定位“Coding Exercise”去写你自己的代码-每一行注解对应一行代码。你可以通过这种方式去完成这个文件：

##### GazeManager.cs

```
using UnityEngine;

namespace Academy.HoloToolkit.Unity
{
    /// <summary>
    /// GazeManager determines the location of the user's gaze, hit position and normals.
    /// </summary>
    public class GazeManager : Singleton<GazeManager>
    {
        [Tooltip("Maximum gaze distance for calculating a hit.")]
        public float MaxGazeDistance = 5.0f;

        [Tooltip("Select the layers raycast should target.")]
        public LayerMask RaycastLayerMask = Physics.DefaultRaycastLayers;

        /// <summary>
        /// Physics.Raycast result is true if it hits a Hologram.
        /// </summary>
        public bool Hit { get; private set; }

        /// <summary>
        /// HitInfo property gives access
        /// to RaycastHit public members.
        /// </summary>
        public RaycastHit HitInfo { get; private set; }

        /// <summary>
        /// Position of the user's gaze.
        /// </summary>
        public Vector3 Position { get; private set; }

        /// <summary>
        /// RaycastHit Normal direction.
        /// </summary>
        public Vector3 Normal { get; private set; }

        private GazeStabilizer gazeStabilizer;
        private Vector3 gazeOrigin;
        private Vector3 gazeDirection;

        void Awake()
        {
            /* TODO: DEVELOPER CODING EXERCISE 3.a */

            // 3.a: GetComponent GazeStabilizer and assign it to gazeStabilizer.

        }

        private void Update()
        {
            // 2.a: Assign Camera's main transform position to gazeOrigin.
            gazeOrigin = Camera.main.transform.position;

            // 2.a: Assign Camera's main transform forward to gazeDirection.
            gazeDirection = Camera.main.transform.forward;

            // 3.a: Using gazeStabilizer, call function UpdateHeadStability.
            // Pass in gazeOrigin and Camera's main transform rotation.


            // 3.a: Using gazeStabilizer, get the StableHeadPosition and
            // assign it to gazeOrigin.


            UpdateRaycast();
        }

        /// <summary>
        /// Calculates the Raycast hit position and normal.
        /// </summary>
        private void UpdateRaycast()
        {
            /* TODO: DEVELOPER CODING EXERCISE 2.a */

            // 2.a: Create a variable hitInfo of type RaycastHit.
            RaycastHit hitInfo;

            // 2.a: Perform a Unity Physics Raycast.
            // Collect return value in public property Hit.
            // Pass in origin as gazeOrigin and direction as gazeDirection.
            // Collect the information in hitInfo.
            // Pass in MaxGazeDistance and RaycastLayerMask.
            Hit = Physics.Raycast(gazeOrigin,
                           gazeDirection,
                           out hitInfo,
                           MaxGazeDistance,
                           RaycastLayerMask);

            // 2.a: Assign hitInfo variable to the HitInfo public property
            // so other classes can access it.
            HitInfo = hitInfo;

            if (Hit)
            {
                // If raycast hit a hologram...

                // 2.a: Assign property Position to be the hitInfo point.
                Position = hitInfo.point;
                // 2.a: Assign property Normal to be the hitInfo normal.
                Normal = hitInfo.normal;
            }
            else
            {
                // If raycast did not hit a hologram...
                // Save defaults ...

                // 2.a: Assign Position to be gazeOrigin plus MaxGazeDistance times gazeDirection.
                Position = gazeOrigin + (gazeDirection * MaxGazeDistance);
                // 2.a: Assign Normal to be the user's gazeDirection.
                Normal = gazeDirection;
            }
        }
    }
}
```

接下来，你需要编辑CursorManager.cs，为了实现以下几点：
  >
  1. 决定哪个光标被激活。
  2. 根据它是否在全息图上去修改光标。
  3. 把光标定位到用户凝视的位置。

此外，欢迎你通过定位注解中的“Coding Exercise”去写你自己的代码，或者使用以下代码：

##### CursorManager.cs

```
using UnityEngine;

namespace Academy.HoloToolkit.Unity
{
    /// <summary>
    /// CursorManager class takes Cursor GameObjects.
    /// One that is on Holograms and another off Holograms.
    /// Shows the appropriate Cursor when a Hologram is hit.
    /// Places the appropriate Cursor at the hit position.
    /// Matches the Cursor normal to the hit surface.
    /// </summary>
    public class CursorManager : Singleton<CursorManager>
    {
        [Tooltip("Drag the Cursor object to show when it hits a hologram.")]
        public GameObject CursorOnHolograms;

        [Tooltip("Drag the Cursor object to show when it does not hit a hologram.")]
        public GameObject CursorOffHolograms;

        void Awake()
        {
            if (CursorOnHolograms == null || CursorOffHolograms == null)
            {
                return;
            }

            // Hide the Cursors to begin with.
            CursorOnHolograms.SetActive(false);
            CursorOffHolograms.SetActive(false);
        }

        void Update()
        {
            /* TODO: DEVELOPER CODING EXERCISE 2.b */

            if (GazeManager.Instance == null || CursorOnHolograms == null || CursorOffHolograms == null)
            {
                return;
            }

            if (GazeManager.Instance.Hit)
            {
                // 2.b: SetActive true the CursorOnHolograms to show cursor.
                CursorOnHolograms.SetActive(true);
                // 2.b: SetActive false the CursorOffHolograms hide cursor.
                CursorOffHolograms.SetActive(false);
            }
            else
            {
                // 2.b: SetActive true CursorOffHolograms to show cursor.
                CursorOffHolograms.SetActive(true);
                // 2.b: SetActive false CursorOnHolograms to hide cursor.
                CursorOnHolograms.SetActive(false);
            }

            // 2.b: Assign gameObject's transform position equals GazeManager's instance Position.
            gameObject.transform.position = GazeManager.Instance.Position;

            // 2.b: Assign gameObject's transform up vector equals GazeManager's instance Normal.
            gameObject.transform.up = GazeManager.Instance.Normal;
        }
    }
}

```

* 通过File->Build Settings去重新构建应用。
* 打开App文件夹。
* 用visual Studio打开ModelExplorer解决方案。
* 点击Debug->Start Without Debugging 或者按Ctrl+F5。
* 观察光标是怎么画的，并且如果触碰到全息图，它是如何改变它的外观的。
* 在Hierarchy面板选择Managers。
* 在Inspector面板，点击Add Component按钮。
* 在菜单，搜索框输入Interactible Manager，选中搜索结果。
* 在Hierarchy面板，选中AstroMan对象。
* 在Inspector面板，点击Add Component按钮。
* 在菜单，搜索框输入Interactible，选中搜索关系。

你需要编辑InteractibleManager.cs和Interactible.cs来实现以下几点：
  >
  1. 在InteractibleManager.cs中，获得凝视射线投射点并保存相关的GameObject。
  2. 如果凝视一个可以和你交互的物体，发送GazeEntered消息。
  3. 如果实现离开那个和你交互的物体，则发送GazeExited消息。
  4. 在Interactible.cs中处理GazeEntered和GazeExited的回调函数。

试着自己在InteractibleManager.cs和Interactible.cs中做代码练习，或者使用一下解决方案：

##### InteractibleManager.cs

```
using Academy.HoloToolkit.Unity;
using UnityEngine;

/// <summary>
/// InteractibleManager keeps tracks of which GameObject
/// is currently in focus.
/// </summary>
public class InteractibleManager : Singleton<InteractibleManager>
{
    public GameObject FocusedGameObject { get; private set; }

    private GameObject oldFocusedGameObject = null;

    void Start()
    {
        FocusedGameObject = null;
    }

    void Update()
    {
        /* TODO: DEVELOPER CODING EXERCISE 2.c */

        oldFocusedGameObject = FocusedGameObject;

        if (GazeManager.Instance.Hit)
        {
            RaycastHit hitInfo = GazeManager.Instance.HitInfo;
            if (hitInfo.collider != null)
            {
                // 2.c: Assign the hitInfo's collider gameObject to the FocusedGameObject.
                FocusedGameObject = hitInfo.collider.gameObject;
            }
            else
            {
                FocusedGameObject = null;
            }
        }
        else
        {
            FocusedGameObject = null;
        }

        if (FocusedGameObject != oldFocusedGameObject)
        {
            ResetFocusedInteractible();

            if (FocusedGameObject != null)
            {
                if (FocusedGameObject.GetComponent<Interactible>() != null)
                {
                    // 2.c: Send a GazeEntered message to the FocusedGameObject.
                    FocusedGameObject.SendMessage("GazeEntered");
                }
            }
        }
    }

    private void ResetFocusedInteractible()
    {
        if (oldFocusedGameObject != null)
        {
            if (oldFocusedGameObject.GetComponent<Interactible>() != null)
            {
                // 2.c: Send a GazeExited message to the oldFocusedGameObject.
                oldFocusedGameObject.SendMessage("GazeExited");
            }
        }
    }
}
```

##### Interactible.cs

```
using UnityEngine;

/// <summary>
/// The Interactible class flags a Game Object as being "Interactible".
/// Determines what happens when an Interactible is being gazed at.
/// </summary>
public class Interactible : MonoBehaviour
{
    [Tooltip("Audio clip to play when interacting with this hologram.")]
    public AudioClip TargetFeedbackSound;
    private AudioSource audioSource;

    private Material[] defaultMaterials;

    void Start()
    {
        defaultMaterials = GetComponent<Renderer>().materials;

        // Add a BoxCollider if the interactible does not contain one.
        Collider collider = GetComponentInChildren<Collider>();
        if (collider == null)
        {
            gameObject.AddComponent<BoxCollider>();
        }

        EnableAudioHapticFeedback();
    }

    private void EnableAudioHapticFeedback()
    {
        // If this hologram has an audio clip, add an AudioSource with this clip.
        if (TargetFeedbackSound != null)
        {
            audioSource = GetComponent<AudioSource>();
            if (audioSource == null)
            {
                audioSource = gameObject.AddComponent<AudioSource>();
            }

            audioSource.clip = TargetFeedbackSound;
            audioSource.playOnAwake = false;
            audioSource.spatialBlend = 1;
            audioSource.dopplerLevel = 0;
        }
    }

    /* TODO: DEVELOPER CODING EXERCISE 2.d */

    void GazeEntered()
    {
        for (int i = 0; i < defaultMaterials.Length; i++)
        {
            // 2.d: Uncomment the below line to highlight the material when gaze enters.
            defaultMaterials[i].SetFloat("_Highlight", .25f);
        }
    }

    void GazeExited()
    {
        for (int i = 0; i < defaultMaterials.Length; i++)
        {
            // 2.d: Uncomment the below line to remove highlight on material when gaze exits.
            defaultMaterials[i].SetFloat("_Highlight", 0f);
        }
    }

    void OnSelect()
    {
        for (int i = 0; i < defaultMaterials.Length; i++)
        {
            defaultMaterials[i].SetFloat("_Highlight", .5f);
        }

        // Play the audioSource feedback when we gaze and select a hologram.
        if (audioSource != null && !audioSource.isPlaying)
        {
            audioSource.Play();
        }

        /* TODO: DEVELOPER CODING EXERCISE 6.a */
        // 6.a: Handle the OnSelect by sending a PerformTagAlong message.

    }
}
```

* 和以前一样，构建项目并部署到HoloLens上。
* 观察当凝视一个物体时会发生什么，当不凝视的时候，会发生什么。

#### 第三章-Targeting Techniques(目标技术)

##### 目标

* 使目标全息图更容易。
* 稳定自然头部运动。

##### 说明

  1. 在Hierarchy面板中，选中Managers。
  2. 在Inspector面板中，点击Add Component按钮。
  3. 在菜单，搜索框中输入Gaze Stabilizer。并选中搜索结果。

现在，我们将使用GazeStabilizer修改我们的GazeManager.
  1. 用Visual Studio打开GazeManager。
  2. 复制一下的代码到GazeManager.cs，或者你自己完成代码练习3.a。

##### GazeManager.cs

```
using UnityEngine;

namespace Academy.HoloToolkit.Unity
{
    /// <summary>
    /// GazeManager determines the location of the user's gaze, hit position and normals.
    /// </summary>
    public class GazeManager : Singleton<GazeManager>
    {
        [Tooltip("Maximum gaze distance for calculating a hit.")]
        public float MaxGazeDistance = 5.0f;

        [Tooltip("Select the layers raycast should target.")]
        public LayerMask RaycastLayerMask = Physics.DefaultRaycastLayers;

        /// <summary>
        /// Physics.Raycast result is true if it hits a Hologram.
        /// </summary>
        public bool Hit { get; private set; }

        /// <summary>
        /// HitInfo property gives access
        /// to RaycastHit public members.
        /// </summary>
        public RaycastHit HitInfo { get; private set; }

        /// <summary>
        /// Position of the user's gaze.
        /// </summary>
        public Vector3 Position { get; private set; }

        /// <summary>
        /// RaycastHit Normal direction.
        /// </summary>
        public Vector3 Normal { get; private set; }

        private GazeStabilizer gazeStabilizer;
        private Vector3 gazeOrigin;
        private Vector3 gazeDirection;

        void Awake()
        {
            /* TODO: DEVELOPER CODING EXERCISE 3.a */

            // 3.a: GetComponent GazeStabilizer and assign it to gazeStabilizer.
            gazeStabilizer = GetComponent<GazeStabilizer>();
        }

        private void Update()
        {
            // 2.a: Assign Camera's main transform position to gazeOrigin.
            gazeOrigin = Camera.main.transform.position;

            // 2.a: Assign Camera's main transform forward to gazeDirection.
            gazeDirection = Camera.main.transform.forward;

            // 3.a: Using gazeStabilizer, call function UpdateHeadStability.
            // Pass in gazeOrigin and Camera's main transform rotation.
            gazeStabilizer.UpdateHeadStability(gazeOrigin, Camera.main.transform.rotation);

            // 3.a: Using gazeStabilizer, get the StableHeadPosition and
            // assign it to gazeOrigin.
            gazeOrigin = gazeStabilizer.StableHeadPosition;

            UpdateRaycast();
        }

        /// <summary>
        /// Calculates the Raycast hit position and normal.
        /// </summary>
        private void UpdateRaycast()
        {
            /* TODO: DEVELOPER CODING EXERCISE 2.a */

            // 2.a: Create a variable hitInfo of type RaycastHit.
            RaycastHit hitInfo;

            // 2.a: Perform a Unity Physics Raycast.
            // Collect return value in public property Hit.
            // Pass in origin as gazeOrigin and direction as gazeDirection.
            // Collect the information in hitInfo.
            // Pass in MaxGazeDistance and RaycastLayerMask.
            Hit = Physics.Raycast(gazeOrigin,
                           gazeDirection,
                           out hitInfo,
                           MaxGazeDistance,
                           RaycastLayerMask);

            // 2.a: Assign hitInfo variable to the HitInfo public property
            // so other classes can access it.
            HitInfo = hitInfo;

            if (Hit)
            {
                // If raycast hit a hologram...

                // 2.a: Assign property Position to be the hitInfo point.
                Position = hitInfo.point;
                // 2.a: Assign property Normal to be the hitInfo normal.
                Normal = hitInfo.normal;
            }
            else
            {
                // If raycast did not hit a hologram...
                // Save defaults ...

                // 2.a: Assign Position to be gazeOrigin plus MaxGazeDistance times gazeDirection.
                Position = gazeOrigin + (gazeDirection * MaxGazeDistance);
                // 2.a: Assign Normal to be the user's gazeDirection.
                Normal = gazeDirection;
            }
        }
    }
}

```

#### 第四章-Directional indicator（方向指示器）

##### 目标

* 在光标上添加一个方向指示器去帮助找到全息图。

##### 说明

我们将使用DirectionalIndicator.cs文件，它将：
  1. 如果用户没有凝视全息图，展示方向指示器。
  2. 如果用户凝视全息图，隐藏方向指示器。
  3. 更改方向指示器去指向全息图。

让我们开始吧。
* 单击Hierarchy面板中的AstroMan的箭头，展开它。
* 在Hierarchy面板中，选中AstroMan下边的DirectionalIndicator。
* 在Inspector面板中，点击Add Component按钮。
* 在菜单，搜索框中输入Direction Indicator。选中搜索结果。
* 在Hierarchy面板中，把Cursor到Inspector面板中的Cursor属性中。
* 在Project面板中，在Holograms文件夹，把DirectionalIndicator资源文件拖到Inspector面板的Directional Indicator属性中。
* 构建部署应用到HoloLens。
* 看观察方向指示器怎么帮助你找到宇航员。

#### 第五章-公告板技术

##### 目标

* 使用公告板技术使全息图总是面对你。

我们将使用BillBoard.cs文件，保持GameObject的朝向，比如它总是面向用户。
* 在Hierarchy面板，选中AstroMan对象。
* 在Inspector面板中，点击Add Component按钮。
* 在菜单，搜索框中输入Billboard。选中搜索结果。
* 在Inspector面板中，把Pivot Axis设置为Y。
* 试一下，像以前一样构建并部署应用到HoloLens。
* 无论你如何改变你的视角，Billboard物体总是面向你的。
* 现在从AstroMan中删除脚本。

#### 第六章-Tag-Along

##### 说明

* 使用Tag-Along使我们的全息图跟着我们在房间周围。

在这个问题上，我们将遵循以下设计约束：
* 内容应该一目了然。
* 内容不应妨碍。
* Head-Looking内容不舒服。

这里使用的解决方案是“Tag-Along”方法。

Tag-Along对象永远不会完全离开用户的视线。你可以将一个Tag-Along看成是橡皮筋贴在用户头上的物体。随着用户的移动，内容将通过向视图边缘滑动而不会完全离开你的视线，当你凝视Tag-Along，它会更加完整的呈现。
* 在Hierarchy面板中，选中Managers。
* 在Inspector面板中，点击Add Component按钮。
* 在菜单，搜索框中输入Gesture Manager，选中搜索结果。
我们将使用SimpleTagalong.cs文件，它将：
  1. 确定Tag-Along对象是否在相机范围内。
  2. 如果不在视图中，则将Tag-Along放入视图中。
  3. 否则，将Tag-Along定位到距离用户的默认位置。

为了做到这一点：
* 在Holograms文件夹点击Tagalong资源文件。
* 在Inspector面板中，点击Tag下拉框，并点击Add Tag...
* 点击+号，并把Tag0命名为TagAlong。
* 在Holograms文件点击Tagalong资源文件，并点击Tag下拉框。
* 选择TagAlong标签。
* 我们首先修改Interactible.cs脚本为InteractibleAction发送一个消息。
* 通过完成代码练习Interactible.cs编辑或者是使用以下代码：

##### Interactible.cs

```
using UnityEngine;

/// <summary>
/// The Interactible class flags a Game Object as being "Interactible".
/// Determines what happens when an Interactible is being gazed at.
/// </summary>
public class Interactible : MonoBehaviour
{
    [Tooltip("Audio clip to play when interacting with this hologram.")]
    public AudioClip TargetFeedbackSound;
    private AudioSource audioSource;

    private Material[] defaultMaterials;

    void Start()
    {
        defaultMaterials = GetComponent<Renderer>().materials;

        // Add a BoxCollider if the interactible does not contain one.
        Collider collider = GetComponentInChildren<Collider>();
        if (collider == null)
        {
            gameObject.AddComponent<BoxCollider>();
        }

        EnableAudioHapticFeedback();
    }

    private void EnableAudioHapticFeedback()
    {
        // If this hologram has an audio clip, add an AudioSource with this clip.
        if (TargetFeedbackSound != null)
        {
            audioSource = GetComponent<AudioSource>();
            if (audioSource == null)
            {
                audioSource = gameObject.AddComponent<AudioSource>();
            }

            audioSource.clip = TargetFeedbackSound;
            audioSource.playOnAwake = false;
            audioSource.spatialBlend = 1;
            audioSource.dopplerLevel = 0;
        }
    }

    /* TODO: DEVELOPER CODING EXERCISE 2.d */

    void GazeEntered()
    {
        for (int i = 0; i < defaultMaterials.Length; i++)
        {
            // 2.d: Uncomment the below line to highlight the material when gaze enters.
            defaultMaterials[i].SetFloat("_Highlight", .25f);
        }
    }

    void GazeExited()
    {
        for (int i = 0; i < defaultMaterials.Length; i++)
        {
            // 2.d: Uncomment the below line to remove highlight on material when gaze exits.
            defaultMaterials[i].SetFloat("_Highlight", 0f);
        }
    }

    void OnSelect()
    {
        for (int i = 0; i < defaultMaterials.Length; i++)
        {
            defaultMaterials[i].SetFloat("_Highlight", .5f);
        }

        // Play the audioSource feedback when we gaze and select a hologram.
        if (audioSource != null && !audioSource.isPlaying)
        {
            audioSource.Play();
        }

        /* TODO: DEVELOPER CODING EXERCISE 6.a */
        // 6.a: Handle the OnSelect by sending a PerformTagAlong message.
        SendMessage("PerformTagAlong");
    }
}
```

当你凝视全息图时，在Interactible.cs脚本中完成自定义动作。让我们更新他，以便于使用Tag-Along。
* 在Script文件中，点击Interactible.cs资源文件，用Visual Studio打开。
* 完成代码练习或者改变这些：
  * 在Hierarchy面板的顶部，搜索框中输入ChestButton_Center并选择结果。
  * 在Inspector面板中，点击Add Component按钮。
  * 在菜单，搜索框中输入Interactible Action。选中搜索结果。
  * 在Holograms文件找到Tagalong资源文件。
  * 在Hierarchy面板选择ChestButton_Center。从Project面板中拖TagAlong到Inspector面板中的Object to TagAlong属性中。
  * 栓剂InteractibleAction脚本，用Visual Studio打开。

我们需要添加一下内容：
* 在InteractibleAction脚本中的PerformTagAlong函数添加功能。
* 添加BillBoarding到gazed-upon物体，并设置pivot axis为Free。
* 然后添加简单的Tag-Along到Object中。

这是我们的解决方案：

##### InteractibleAction.cs

```
using Academy.HoloToolkit.Unity;
using UnityEngine;

/// <summary>
/// InteractibleAction performs custom actions when you gaze at the holograms.
/// </summary>
public class InteractibleAction : MonoBehaviour
{
    [Tooltip("Drag the Tagalong prefab asset you want to display.")]
    public GameObject ObjectToTagAlong;

    void PerformTagAlong()
    {
        if (ObjectToTagAlong == null)
        {
            return;
        }

        // Recommend having only one tagalong.
        GameObject existingTagAlong = GameObject.FindGameObjectWithTag("TagAlong");
        if (existingTagAlong != null)
        {
            return;
        }

        GameObject instantiatedObjectToTagAlong = GameObject.Instantiate(ObjectToTagAlong);

        instantiatedObjectToTagAlong.SetActive(true);

        /* TODO: DEVELOPER CODING EXERCISE 6.b */

        // 6.b: AddComponent Billboard to instantiatedObjectToTagAlong.
        // So it's always facing the user as they move.
        instantiatedObjectToTagAlong.AddComponent<Billboard>();

        // 6.b: AddComponent SimpleTagalong to instantiatedObjectToTagAlong.
        // So it's always following the user as they move.
        instantiatedObjectToTagAlong.AddComponent<SimpleTagalong>();

        // 6.b: Set any public properties you wish to experiment with.
    }
}
```

* 试一下，构建并部署应用到HoloLens。
* 观察内容如何跟随凝视点的重心，但不要连续，也不要阻挡他。
