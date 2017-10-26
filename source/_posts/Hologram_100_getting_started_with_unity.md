### Holograms 100
本教程将引导您创建基于Unity的基本混合现实应用程序。

#### 先决条件
在Windows10的电脑上安装正确的恐惧

#### 第一章-创建一个新项目

使用Unity构建一个应用程序，首先你需要创建一个项目。该项目由一些文件组成，其中最重要的是Assets文件。它保存了从三维计算机图形软件（比如：Maya、Max Cinema 4D 或者Photoshop）导入的所有资源文件，你使用Visual Studio或者你喜欢的代码编辑器，以及在编辑器中Unity创建Scenes的一些内容文件，动画，和其他的Unity资源类型。

去构建和部署UWP应用程序，Unity可以导出项目作为一个包含必要资源和代码文件的Visual Studio项目。
1. 启动Unity
2. 选择New
3. 输入项目名称（比如：“MixedRealityIntroduction”）
4. 输入保存项目的本地路径
5. 选择Create project

恭喜，现在你可以开始使用混合现实自定义。

#### 第二章-设置相机

Unity主相机可以处理头跟踪和立体渲染。对主要相机进行一些更改将其与混合现实配合使用。

1. 选择File -> 选择 New Scene

首先，如果你将用户的初始位置设为（x:0,y:0,z:0），则更容易布局应用程序。由于主相机是根据用户的头移动，所以用户的起始位置可以根据主摄像机的起始位置来设置。
1. 在Hierarchy面板中选择Main Camera
2. 在Inspector面板中，找Transform，将Position从（x:0,y:1,z:-10）更改为（x:0,y:0,z:0）

第二，默认的Camera背景需要一些想法。
对于HoloLens应用，真实世界应该出现在相机渲染所有内容的后边，而不是一个Skybox结构。
1. 仍然在Hierarchy面板中选择Main Camera，在Inspector面板中找到Camera，并且把Clear Flags的Skybox改成Solid Color。
2. 选择Background颜色选择器，更改RGBA值为（0，0，0，0）
对于沉浸式耳机的混合现实应用，我们可以使用Unity默认的Skybox结构。
1. 仍然在Hierarchy面板中选择Main Camera，在Inspector面板中找到Camera，保持Clear Flags下拉框的值为Skybox。

第三，让我们考虑Unity中的近剪切平面，当用户靠近过对象或者是对象靠近用户时，防止对象被渲染的太靠近用户的眼睛。
对于HoloLens应用，近剪切平面可以设置为HoloLens推荐的0.85米
1. 仍然在Hierarchy面板中选择Main Camera，在Inspector面板中找到Camera，并且把Near Clip Plan从默认的0.3更改为HoloLens推荐的0.85。

最后，让我们保存到目前为止的配置。要保存scene的更改，选择File->Save Scene As, 命名为Main，选择Save保存。

#### 第三章-设置项目设置
在这章，我们将设置一些Unity项目设置，以便于帮助我们找到Windows Holographic SDK进行开发。对于应用，我们将设置一些质量设置。最后，我们将确保我们的构建目标是设置为Windows Store。

##### Unity性能和质量设置

由于在HoloLens上保持高帧率很重要，我们希望把质量设置调整到最快的性能，有关更多的性能信息，请参考Unity性能推荐。
1. 选择Edit->Project Settings-> Quality
2. 选择Windows Store图标下边的下拉按钮，选择Fastest。当Windows Store列和Fastest行是绿色的，那么，你设置正确。
![质量](/assets/UnityQualitySettings.png)
对于遮挡显示的混合现实应用，你可以将质量设置保留为默认值。

Target Windows10 SDK
我们需要让Unity知道我们试图导出的应用程序应该创建一个全息视图，而不是2D视图。我们通过在面向Windows 10 SDK的Unity启用VirtualReality 来达到此目的。
1. 选择Edit-> Project Settings-> Player
2. 在Inspector面板点击Windows Store
3. 展开Other Settings
4. 在Rendering部分，点击Virtual Reality Supported 添加新的Virtual Reality SDKs列表，并确认Windows Mixed Reality被列为支持的SDK。

很好的完成了项目的所有设置。接下来，让我们添加一个全息图。

#### 第四章-创建一个立方体

创建一个立方体，就好像创建Unity的其他对象一样，在用户前面放置一个立方体是容易的，因为Unity的坐标系就是映射在现实世界-Unity中的一米相当于真实世界的一米。
1. 在Hierarchy面板的左上角，选择Create下拉菜单，选择3D Object->Cube
2. 在Hierarchy面板选择创建一个新的Cube
3. 在Inspector找到Transform部分，并且更改Position为（x:0,y:0,z:2）。这个立方体位于用户前方两米的地方。
4. 在Transform部分，更改Rotation为（x:45,y:45,z:45），以及更改Scale为（x:0.25,y:0.25,z:0.25）。这个立方体将缩放到0.25米。
5. 保存scene修改，选择File->Save Scene

#### 第五章-用Unity编辑器验证设备
现在我们已经创建了立方体，现在是快速检验设备的时候了，你可以直接用Unity编辑器进行此操作。

##### 对于HoloLens使用Unity Remoting
1. 在你的HoloLens，安装并运行Holographic Remoting Player，可从Windows Store获得。在设备上启动应用，当进入等待状态并显示设备的IP地址时，记下IP。
2. 在你开发的电脑上，在Unity，打开Window-> Holographic Emulation。
3. 把 Emulation mode 从 None 改为 Remote to Device。
4. 在 Remote Machine，输入刚才提到的HoloLens的IP地址。
5. 点击Connect。
6. 确保Connection Status 改变为绿色的Connected。
7. 现在，你可以在Unity点击Play。

你现在将可以在设备和编辑器中查看立方体。你可以像在编辑器中运行应用程序一样暂停，查看对象，调试，但是，是通过主机和设备之间德尔网络来回传输视频，音频和设备输入。

##### 对于其他混合现实支持的耳机

1. 在你开发的电脑上，使用USB和HDMI或者显示器端口电缆连接。
2. 启动混合现实门户，确保你已经完成了第一次运行的体验。
3. 从Unity，你现在可以点击播放按钮了。
现在，你可以在混合现实耳机和编辑器中查看立方体渲染了。

#### 第六章-从Visual Studio 构建和部署到设备上

我们现在准备编译项目到Visual Studio ，并且部署到我们的设备上。

##### 导出到Visual Studio
1. 打开File-> Build Settings窗口。
2. 点击Add OpenScenes 去添加场景。
3. 把Platform改为 Windows Store，并且点击Switch Platform。
4. 在 Windows Store设置中，确保SDK是Universal 10。
5. 对于目标设备，离开任何设备，以遮挡显示或者切换到HoloLens。
6. UWP Build Type 应该是D3D。
7. UWP SDK可以在Latest installed。
8. 检查正在调试的Unity C#项目。
9. 点击Build。
10. 在资源文件管理中，点击New Folder并命名为”“App”。
11. 选中App文件夹之后，点击Select Folder按钮。
12. Unity构建完成之后，讲出现一个Windows浏览窗口。
13. 在文件浏览器中打开App文件。
14. 打开生成的Visual Studio解决方案（本例中为：MixedRealityIntroduction.sln）

##### 编译Visual Studio解决方案

最后，我们将编译导出的Visual Studio解决方案，并将其部署在设备上。
使用Visual Studio中的顶部工具栏，将目标从Debug更改为Release，从ARM更改为X86。
对于部署到设备与仿真器的说明不同。按照与您的设置相匹配的说明。

##### 通过Wi-Fi部署到混合现实设备

单击Local Machine按钮旁边的箭头，并将部署目标更改为Remote Machine。
输入混合现实设备的IP地址，并将其他设备的HoloLens和Windows的Authentication Mode更改为Universal（未加密协议）。
单击Debug->Start without debugging。
对于HoloLens，如果这是首次部署到您的设备，则需要使用Visual Studio配对。

##### 通过USB部署到混合现实设备

确保通过USB电缆插入设备。
对于HoloLens，单击Local Machine按钮旁边的箭头，并将部署目标更改为Device。
要定位连接到PC的遮挡设备，请将设置保留到本地机器。确保您有Mixed Reality Portal运行。
单击Debug->Start without debugging。

##### 部署到仿真器

单击Device按钮旁边的箭头，从下拉列表中选择HoloLens Emulator。
单击Debug->Start without debugging。

##### 尝试你的应用程序

现在您的应用程序已部署，请尝试移动多维数据集周围，并观察它在您面前的世界。
