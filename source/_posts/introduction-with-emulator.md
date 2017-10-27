---
title: Holograms 101E：介绍Emulator（模拟器）
date: 2017-10-26 10:04:33
comments: true
reward: true
tags:
  -Emulator
  -Holograms
---

<img src="/assets/IntroductionWithEmulator.jpeg" width="350px" height="350px">

### 1. 前言

朋友今天说，和我聊天全靠猜......

他说聊天不是做阅读理解、完型填空......

我能怎么办呢......

我只能......

<!-- more -->

哈哈哈哈哈......


本教程将引导你创建一个构建于Unity的完整项目，演示核心Windows全息和HoloLens功能，包括gaze(注视)、gestures(手势)、voice input(语音输入)、spatial sound(空间声音)、spatial mapping(空间映射)，本教程将需要大约一小时。

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

#### 第二章-Gaze(注视)

这一章，我们将介绍三种与你的全息图交互的方法 --注视。

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
>
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

* 重新运行App，File->Build Settings。
* 返回以前用于部署到模拟器的Visual Studio解决方案。
* 当提示“Reload All”，选中。
* 点击Debug->Start without debugging或者是按Ctrl+F5。
* 用Xbox来查看周围的场景，注意游标如何与对象的形状进行交互。

#### 第三章-Gestures(手势)
