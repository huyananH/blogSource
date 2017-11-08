---
title: holograms 211：gesture
date: 2017-11-01 14:56:44
comments: true
reward: true
tags:
---

<img src="/assets/holoLens/Holograms211.jpg" width="350px" height="350px">

### 1. 前言

手势将用户意识转变为动作。用手势，用户能与全息图交互。在这个教程中，我们将学习如何跟踪用户的瘦，响应用户的输入，并根据手的状态和位置给用户反馈。

*-- 在[Holograms 101](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_101)*，我们用一个简单的air-tap手势与我们的全息图进行交互。现在，我们将超越air-tap手势，探索新概念：
* 检测当用户的手被跟踪的时候，向用户提供反馈。
* 用navigation导航手势去旋转我们的全息图。
* 当用户的手即将离开视图，提供反馈。
* 用操作事件允许用户用他们的手移动全息图。

 *-- 在这个教程中，我们将再用我们在[Holograms 210](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_210)创建的Unity项目 ModelExplorer* 。我们的宇航员朋友回来帮助我们探索这些新的手势概念。

 <!-- more -->

### 2. 准备

* *-- [安装了正确工具](https://developer.microsoft.com/en-us/windows/mixed-reality/install_the_tools)* 的Windows 10电脑。
* 一些基础的C#编程能力。
* 你应该完成 *-- [Holograms 101](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_101)* 。
* 你应该完成 *-- [Holograms 210](https://developer.microsoft.com/en-us/windows/mixed-reality/holograms_210)*
* 配置 *-- [用于开发](https://developer.microsoft.com/en-us/windows/mixed-reality/Using_Visual_Studio.html#enabling_developer_mode)* 的HoloLens设备。

### 3. 项目文件

* *-- 下载项目所需的[文件](https://github.com/Microsoft/HolographicAcademy/archive/Holograms-211-Gesture.zip)* 需要Unity2017.02或以上的版本。
  * 如果你还需要Unity5.6的支持， *-- 请使用[这个版本](https://github.com/Microsoft/HolographicAcademy/archive/v1.5.6-211.zip)*
  * 如果你还需要Unity5.5的支持， *-- 请使用[这个版本](https://github.com/Microsoft/HolographicAcademy/archive/v1.5.5-211.zip)*
  * 如果你还需要Unity5.4的支持， *-- 请使用[这个版本](https://github.com/Microsoft/HolographicAcademy/archive/v1.5.4-211.zip)*
* 解压文件放到你的桌面或者其他容易找到的路径。

### 4. 排错和提示

* 需要在Tools（工具）-> Options（选项） ->Debugging（调试）下禁用（取消选中）“Just My Code”（仅我的代码），以便在代码中触发断点

### 5. 内容

#### Unity设置

##### 说明

* 启动Unity。
* 选择Open。
* 导航到你刚才解压好的Gesture文件夹下。
* 找到并选中ModelExplorer文件。
* 点击Select Folder按钮。
* 在Project面板，展开Scenes文件。
* 双击ModelExplorer场景加载到Unity中。
* 在Unity中选择File->BuildSettings。
* 如果scenes/ModelExplorer没有构建在场景里，点击Add Open Scenes添加到场景。
* 在Platform列表中选中Windows Store并点击Switch Platform。
* 设置SDK为Universal 10，设置UWP Build Type为D3D。
* 选中Unity C# Projects。
* 点击Build。
* 创建一个名为App的文件夹。
* 单击App文件夹。
* 点击Select Folder，Unity将开始为Visual Studio构建项目。
* 当Unity构建完之后，会出现一个文件资源管理器。
* 打开App文件夹。
* 用Visual Studio打开ModelExplorer解决方案。
* 使用顶部附近的下拉选项，把Debug改为Release，把ARM改为x86。
* 点击下拉选项赴京的“本地设备”，并选择“Remote Machine”（远程计算机）。
* 将Address（地址）设置为你的HoloLens的IP地址。如果你不知道你设备的IP地址，查看Settings->Network & Internet-> Advanced Options或者问Cortana,"你好，Cortana,我的IP地址是什么"。
* 点击Debug->Start without debugging或者按Ctrl+F5，如果这是你的设备第一次进行部署，它将需要Visual Studio匹配。
* 提示，你可能会注意到在Visual Studio Errors面板中有一些红色错误。这些错误是安全的可以被忽略。切换到Output面板查看实际的构建进度。Output（输出面板）中的错误将要求您进行修复（通常它们是由脚本中的错误引起的）。

#### 第一章-检测手的反馈

##### 目标

* 绑定手的跟踪事件。
* 当手被跟踪的时候，用光标反馈给用户。

##### 说明

* 在Hierarchy面板中，选择Manager对象。
* 在Inspector面板中，点击Add Component按钮。
* 在菜单，搜索框中输入Hands Manager。选中搜索结果。

在HandsManager.cs脚本完成这些操作：
  1. 绑定InteractionSourceDetected和InteractionSourceLost事件。
  2. 设置HandDetected状态。
  3. 未绑定来自SourceDetected和SourceLost事件。
* 在Hierarchy面板中，选中Cursor对象。
* 在Inspector面板中，点击Add Component按钮。
* 在菜单，搜索框中输入Cursor Feedback。并选中搜索结果。
* 在Project面板中的Holograms文件，找到HandDetectedFeedback资源文件。
* 把HandDetectedFeedBack资源文件拖到Cursor FeedBack(Script)组件中的Hand Detected Asset属性中。
* 在Hierarchy面板中，展开Cursor对象。
* 拖CursorBillboard到Cursor FeedBack(Script)组件中的FeedBack Parent属性中。
* 在CursorFeedBack.cs脚本中是这样工作的。
  1. 实例化HandDetectedFeedBack资源。
  2. 将HandDetectedFeedBack的父项设为光标介绍对象。
  3. 根据HandDetected的状态激活/取消激活HandDetectedFeedBack资源。

##### 构建和部署

* 在Unity中，使用File->Build Settings来重构应用。
* 打开App文件夹。
* 如果尚未打开，请代开ModelExplorer Visual Studio解决方案。
  * 如果你在Visual Studio设置过程中构建/部署了，你可以打开VS的实例，并在出现提示时，点击“Reload All”。
* 在Visual Studio，点击Debug->Start Without debugging或者是按Ctrl+F5。
* 之后应用部署到HoloLens，使用air-tap来关闭合适的框。
* 将你的手指移动到视图中，并将您的食指指向天空，开始手动跟踪。
* 向左、右、上、下移动你的手。
* 观察当你的手被跟踪和不在视图时，光标的改变。

#### 第二章-Navigation（导航）

##### 目标

* 用导航手势时间去旋转宇航员。

##### 说明

* 在我们的应用中使用导航手势，我们将编辑四个脚本完成以下工作。
  1. 焦点对象被HandsManager.cs跟踪。
  2. 导航事件将被GazeGestureManager.cs处理。
  3. 当导航手势旋转物体时，将被GestureAction.cs处理。
  4. 光标将通过CursorAction.cs向用户提供导航反馈。
开始
* 用Visual Studio打开HandsManager.cs。

我们在HandsManager.cs中跟踪相互作用的对象。复制下面已完成的代码到HandsManager.cs，或者你根据标注的代码练习自己完成代码。

##### HandsManager.cs
```
using UnityEngine;
using UnityEngine.XR.WSA.Input;

namespace Academy.HoloToolkit.Unity
{
    /// <summary>
    /// HandsManager keeps track of when a hand is detected.
    /// </summary>
    public class HandsManager : Singleton<HandsManager>
    {
        [Tooltip("Audio clip to play when Finger Pressed.")]
        public AudioClip FingerPressedSound;
        private AudioSource audioSource;

        /// <summary>
        /// Tracks the hand detected state.
        /// </summary>
        public bool HandDetected
        {
            get;
            private set;
        }

        // Keeps track of the GameObject that the hand is interacting with.
        public GameObject FocusedGameObject { get; private set; }

        void Awake()
        {
            EnableAudioHapticFeedback();

            InteractionManager.InteractionSourceDetected += InteractionManager_InteractionSourceDetected;
            InteractionManager.InteractionSourceLost += InteractionManager_InteractionSourceLost;

            /* TODO: DEVELOPER CODE ALONG 2.a */

            // 2.a: Register for SourceManager.SourcePressed event.
            InteractionManager.InteractionSourcePressed += InteractionManager_InteractionSourcePressed;

            // 2.a: Register for SourceManager.SourceReleased event.
            InteractionManager.InteractionSourceReleased += InteractionManager_InteractionSourceReleased;

            // 2.a: Initialize FocusedGameObject as null.
            FocusedGameObject = null;
        }

        private void EnableAudioHapticFeedback()
        {
            // If this hologram has an audio clip, add an AudioSource with this clip.
            if (FingerPressedSound != null)
            {
                audioSource = GetComponent<AudioSource>();
                if (audioSource == null)
                {
                    audioSource = gameObject.AddComponent<AudioSource>();
                }

                audioSource.clip = FingerPressedSound;
                audioSource.playOnAwake = false;
                audioSource.spatialBlend = 1;
                audioSource.dopplerLevel = 0;
            }
        }

        private void InteractionManager_InteractionSourceDetected(InteractionSourceDetectedEventArgs obj)
        {
            HandDetected = true;
        }

        private void InteractionManager_InteractionSourceLost(InteractionSourceLostEventArgs obj)
        {
            HandDetected = false;

            // 2.a: Reset FocusedGameObject.
            ResetFocusedGameObject();
        }

        private void InteractionManager_InteractionSourcePressed(InteractionSourcePressedEventArgs hand)
        {
            if (InteractibleManager.Instance.FocusedGameObject != null)
            {
                // Play a select sound if we have an audio source and are not targeting an asset with a select sound.
                if (audioSource != null && !audioSource.isPlaying &&
                    (InteractibleManager.Instance.FocusedGameObject.GetComponent<Interactible>() != null &&
                    InteractibleManager.Instance.FocusedGameObject.GetComponent<Interactible>().TargetFeedbackSound == null))
                {
                    audioSource.Play();
                }

                // 2.a: Cache InteractibleManager's FocusedGameObject in FocusedGameObject.
                FocusedGameObject = InteractibleManager.Instance.FocusedGameObject;
            }
        }

        private void InteractionManager_InteractionSourceReleased(InteractionSourceReleasedEventArgs hand)
        {
            // 2.a: Reset FocusedGameObject.
            ResetFocusedGameObject();
        }

        private void ResetFocusedGameObject()
        {
            // 2.a: Set FocusedGameObject to be null.
            FocusedGameObject = null;

            // 2.a: On GestureManager call ResetGestureRecognizers
            // to complete any currently active gestures.
            GestureManager.Instance.ResetGestureRecognizers();
        }

        void OnDestroy()
        {
            InteractionManager.InteractionSourceDetected -= InteractionManager_InteractionSourceDetected;
            InteractionManager.InteractionSourceLost -= InteractionManager_InteractionSourceLost;

            // 2.a: Unregister the SourceManager.SourceReleased event.
            InteractionManager.InteractionSourceReleased -= InteractionManager_InteractionSourceReleased;

            // 2.a: Unregister for SourceManager.SourcePressed event.
            InteractionManager.InteractionSourcePressed -= InteractionManager_InteractionSourcePressed;
        }
    }
}
```
