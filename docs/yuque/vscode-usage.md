版本: 1.35.1 (user setup)

## 1. 通过配置文件设置

VS Code 的配置文件默认为：settings.json，我们可以通过下面的方式打开该文件进行自定义配置：
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1561099788662-7327baa6-1373-468b-9e8f-92918417bc77.png#align=left&display=inline&height=227&originHeight=310&originWidth=1019&size=35777&status=done&style=none&width=746)
打开 settings.json 方式一：VSCode 1.35.1 (user setup)

![setting_json.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1587345536635-f2bc6bc5-8c1d-4f21-b2c6-4e283a086936.png#align=left&display=inline&height=492&originHeight=629&originWidth=953&size=76016&status=done&style=none&width=746)
打开 settings.json 方式一：VSCode 1.44.2 (user setup)

![setting-json-2.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1587345967895-9e3895eb-5ad3-42ff-a626-112aa0d398e8.png#align=left&display=inline&height=334&originHeight=427&originWidth=953&size=50039&status=done&style=none&width=746)
打开 settings.json 方式二：VSCode 1.44.2 (user setup)

- **C:\Users[USER_NAMD]\AppData\Roaming\Code\User\settings.json**

```json
{
  "editor.minimap.enabled": false,
  "editor.fontSize": 12,
  "workbench.statusBar.feedback.visible": false,
  "terminal.integrated.shell.windows": "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
  "workbench.colorTheme": "Visual Studio Dark",
  "[python]": {},
  "breadcrumbs.enabled": false,
  "workbench.editor.centeredLayoutAutoResize": false,
  "editor.wordWrap": "on",
  "editor.minimap.showSlider": "always",
  "window.menuBarVisibility": "visible",
  "workbench.sideBar.location": "left",
  "editor.accessibilitySupport": "off",
  //显示头部导航栏
  "workbench.editor.showTabs": true,
  "window.zoomLevel": 0,
  //Ctrl+滚轮实现代码的缩放
  "editor.mouseWheelZoom": true,
  "workbench.editor.tabSizing": "shrink"
}
```

## 2. 编辑器选项卡

当 vscode 打开很多文件，如果 "**设置 → 工作台 → 编辑器管理 →Tab Sizing**" 为 "**fit**"，编辑器选项卡将使用滚动隐藏的方式显示，想要显示打开的编辑器可以使用 `edt`  的命令或者设置 "**Show All Editors**" 的快捷键。
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1565228295004-83f73509-9e84-457b-b30d-3ed359d89860.png#align=left&display=inline&height=320&originHeight=320&originWidth=772&size=86248&status=done&style=none&width=772)

![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1561098903958-6b478420-5bdc-4a29-ab67-24af40d370a9.png#align=left&display=inline&height=225&originHeight=225&originWidth=1019&size=30265&status=done&style=none&width=1019)

也可以将  "**设置 → 工作台 → 编辑器管理 →Tab Sizing**" 设置为 "**shrink**"：
![image.png](https://cdn.nlark.com/yuque/0/2019/png/126032/1561099277202-15bc9e25-1f38-4c13-8a1e-48cf7cd696de.png#align=left&display=inline&height=313&originHeight=313&originWidth=684&size=27747&status=done&style=none&width=684)

## 3. 禁用 Tab 转换为空格

VS Code 中会默认把 Tab 键转换成 4 个空格，当然你也可以自己定义转换的空格数。或者你也可以禁止把 Tab 键转换为空格，具体设置如下截图：**“设置”**→**“常规设置”**→**“Editor:Tab Size”**→**“Editor:Insert Spaces”。**
![vscode-tab-setting.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1587086399599-33f96d84-3fc2-4a4e-b64b-efd1b170903d.png#align=left&display=inline&height=728&originHeight=728&originWidth=1140&size=96759&status=done&style=none&width=1140)

###

## 4. 无法创建和打开任何文件

VSCode 无法创建和打开任何问题文件，重装和重启后都没办法解决，异常信息如下“**Unable to open 'xxx': Assertion Failed: argument is undefined or null**”。
![vscode-open-error.png](https://cdn.nlark.com/yuque/0/2020/png/126032/1587087181723-84ef65cf-2980-4e35-ae7a-be5a04063204.png#align=left&display=inline&height=656&originHeight=1560&originWidth=1775&size=157310&status=done&style=none&width=746)
解决方法参考：
[Windows: Both insider and normal version: Unable to open ‘xxx’: Assertion Failed: argument is undefined or null · Issue #93679 · microsoft/vscode](https://github.com/microsoft/vscode/issues/93679)

> I just had the same issue and this is how I fixed:
> go to -> **C:\Users[USER_NAMD]\AppData\Roaming\Code\User\settings.json**
> and look for `"editor.quickSuggestions": null`. Change the value to either `false` or `true` .
