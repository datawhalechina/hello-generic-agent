# 第 3 章 基础使用：第一个小时

> **学完本章，你将熟悉 GA 的界面、解锁视觉与文件搜索等扩展能力，并掌握多轮对话技巧。**

## 🎯 学习目标

1. 熟悉 GA 的交互界面，能自如地与 GA 对话
2. 解锁 OCR、视觉、飞书 CLI、Everything CLI 四大扩展能力
3. 掌握多轮对话与上下文管理的最佳实践

---

## 3.1 界面介绍

双击 `launch.pyw` 启动 GA 后，我们会看到一个简洁的对话界面：

![GA 主界面](images/img_1.png)

界面主要由以下几个区域组成：

**❶ 模型选择区**

左上角下拉菜单，用于切换当前使用的大语言模型（LLM）。如果我们在 `mykey.py` 中配置了多个模型，可以在这里自由切换。

**❷ 轮次计数器**

显示当前任务已经进行了多少轮对话。每一轮代表 GA 的一次"思考→工具调用→获得结果"的循环。

![轮次展示](images/img_2.png)

<details>
<summary>💡 关于轮次的更多细节</summary>

每一轮都可以展开查看详细过程。如果 GA 在一轮中调用了多个工具，我们可以展开查看每个工具的输入和输出。

![轮次详情](images/img_3.png)

</details>

**❸ 对话显示区**

GA 的回复和工具调用结果都会显示在这里。多轮任务会自动折叠成可展开的摘要，避免历史信息过长。

**❹ 输入框**

在这里输入指令或问题，用自然语言描述即可。支持多行输入（Shift+Enter 换行），按 Enter 发送。

**❺ 发送按钮**

点击输入框右侧的上箭头按钮，或直接按 Enter 键发送消息。

> ⚠️ **小提示**：如果 GA 正在执行任务，会看到光标闪烁（▌），表示正在生成回复。如果离开超过 30 分钟且开启了"自主行动"，GA 会自动执行预设的自动化任务。

---

## 3.2 视觉能力

GA 由大语言模型驱动，本身不具备视觉能力。但通过配置 OCR 和视觉扩展，我们可以让 GA "看见"屏幕和图片。

### 3.2.1 OCR 能力

OCR（光学字符识别）能让 GA 阅读图片上的文字。配置非常简单，只需一句话：

> 用 rapidocr 配置你的 ocr 能力并存入记忆

![GA 配置 OCR](images/img_20.png)

配置完成后，GA 就能阅读任何图片上的文字了：

> 帮我看看 `F:\data\to_be_ocred.png` 这张图片写了什么

![OCR 识别结果](images/img_22.png)

更实用的是，配置了 OCR 之后，GA 可以了解我们的屏幕内容：

> 总结我当前屏幕上的内容

![GA 识别屏幕内容](images/img_23.png)

### 3.2.2 视觉能力（Vision）

OCR 只能识别文字。如果需要 GA 理解界面布局、识别图标和窗口，我们还需要解锁完整的视觉能力：

> 仿照你的 llmcore，写个调用 vision 的能力并存入记忆

![GA 请求修改代码授权](images/img_24.png)

GA 会请求修改自身代码的授权。由于我们确实希望它获得视觉能力，回复授权即可。

> ⚠️ GA 在申请授权时给出的选项可能不完全一致——这些选项是 GA 自己生成的，并非固定程序。我们可以复制选项发送，也可以用自然语言表达意图。

配置完成后，测试一下：

> 请看看我的屏幕上有哪些应用、窗口和重要组件

![GA 识别桌面内容](images/img_27.png)

现在 GA 已经拥有完整的视觉能力，可以理解屏幕上的窗口和内容了。

---

## 3.3 飞书 CLI

通过飞书 CLI，我们可以把 GA 接入飞书，用文字指令高效操作飞书，随时随地用手机给 Agent 下任务。

> 💡 把 Agent 放进我们每天必用的通讯工具里，就能让 Agent 能力无缝嵌入日常工作流，在熟悉的 App 中随时、随地操纵你的个人助手。

### 第 1 步：安装依赖

GA 应该在首次启动时已经帮你装好了 `lark-oapi` 和 `python-socks`。如果不确定，可以让 GA 自己检查：

```
检查 lark-oapi 与 python-socks 两个包是否已经安装。如果没有，请立即安装
```

![GA 检查依赖](images/img_29.png)

也可以手动安装：

```bash
pip install lark-oapi python-socks
```

### 第 2 步：创建飞书应用并获取密钥

打开 [飞书开放平台](https://open.feishu.cn/)，登录账号，创建企业自建应用，添加机器人能力，配置权限，获取 App ID 和 App Secret。

> 这部分步骤较多，完整流程请参考 [第 5 章 聊天平台接入](../chapter5/) 中的**飞书接入指引**，按步骤操作即可。

<table><tr>
<td><img src="images/img_30.png" width="100%"></td>
<td><img src="images/img_31.png" width="100%"></td>
</tr></table>

<table><tr>
<td><img src="images/img_32.png" width="100%"></td>
<td><img src="images/img_33.png" width="100%"></td>
</tr></table>

### 第 3 步：配置 mykey.py

拿到 App ID 和 App Secret 后，需要填入 `mykey.py`。

![获取凭证](images/img_36.png)

<details>
<summary>💡 不会编辑 mykey.py？</summary>

用记事本打开 `mykey.py`，按 `Ctrl+F` 搜索 `fs_app_id`：

![搜索 fs_app_id](images/img_37.png)

找到后如图所示：

![定位到配置行](images/img_38.png)

去掉这几行前面的 `#` 和空格（取消注释）：

![取消注释](images/img_39.png)

然后填入你的 App ID 和 App Secret，`fs_allowed_users` 可以暂时留空：

![填写凭证](images/img_40.png)

</details>

### 第 4 步：启动飞书前端

```bash
cd /path/to/GenericAgent/frontends
python fsapp.py
```

看到「等待消息…」表示服务已就绪：

![服务就绪](images/img_41.png)

### 第 5 步：配置事件订阅并发布

回到飞书开放平台，配置长连接事件订阅，添加 `im.message.receive_v1` 事件，然后发布应用。

<table><tr>
<td><img src="images/img_42.png" width="100%"></td>
<td><img src="images/img_43.png" width="100%"></td>
<td><img src="images/img_44.png" width="100%"></td>
</tr></table>

<table><tr>
<td><img src="images/img_45.png" width="100%"></td>
<td><img src="images/img_46.png" width="100%"></td>
<td><img src="images/img_47.png" width="100%"></td>
</tr></table>

### 第 6 步：开始对话

下载 [飞书客户端](https://www.feishu.cn/download)，登录后搜索你的 Agent 名称，即可开始对话：

<table><tr>
<td><img src="images/img_48.png" width="100%"><br><em>搜索框搜索你建立的个人助手名字</em></td>
<td><img src="images/img_49.png" width="100%"><br><em>在对话框中输入问题，手机版飞书也能继续对话</em></td>
</tr></table>

> 第 5 章还涵盖了微信、钉钉、QQ、Telegram、企业微信等平台的接入方式，有需要可以继续阅读。

---

## 3.4 Everything CLI（Windows 文件搜索）

> Everything CLI 是 Windows 平台的极速文件搜索工具，能在毫秒级时间内搜索整个硬盘。配置后，GA 可以快速定位任何文件，无需遍历目录。

### 为什么需要它？

当我们让 GA 读取一个文件时，如果不提供完整路径，GA 可能无法定位：

![GA 找不到文件](images/img_50.png)

### 安装配置

向 GA 发送：

> 安装并配置 Everything 命令行工具进 PATH

![GA 安装 Everything](images/img_52.png)

> ⚠️ 安装过程中，系统可能弹出管理员权限请求窗口（有时藏在任务栏后台），请注意查看并授权。

安装完成后，GA 会确认配置成功：

![安装成功](images/img_56.png)

<details>
<summary>💡 安装后的建议</summary>

- 如果电脑管家提示 Everything 想要开机自启，请允许（不会明显拖慢启动）
- 建议重启电脑以确保 Everything 服务生效
- 可以让 GA 把这个习惯写入记忆：「写入 L1，任何大规模文件查询直接使用 es」

</details>

### 效果验证

再次让 GA 查找文件，这次指定使用 es 搜索：

> 请读取 to_be_ocred.png 里面的文字，使用 es 全盘搜索这个文件

![Everything 搜索成功](images/img_58.png)

GA 仅用 1 轮就定位到了文件路径，效率显著提升。

---

## 3.5 多轮对话与上下文

GA 不是一次性工具，而是可以连续对话的助手。它会记住之前说过的话，理解上下文。

### 什么时候需要开新会话？

| 情况 | 说明 |
|---|---|
| 切换完全不同的任务 | 从数据分析切换到浏览器操作 → 建议开新会话 |
| 对话太长导致"忘事" | 上下文窗口满了，GA 开始遗忘早期内容 → 开新会话 |
| 想要重新开始 | 之前尝试失败想换思路 → 开新会话 |

> 💡 **开新会话方法**：关闭窗口重新启动 GA 即可。

### 上下文管理技巧

1. **明确指代**：说"帮我处理刚才那个 Excel 文件"比"帮我处理一下"好得多
2. **分步确认**：复杂任务分成多步，每步确认后再继续
3. **利用记忆**：常用偏好可以让 GA 记住——"以后处理销售数据时，都用这个格式"，GA 会写入记忆系统，下次自动应用

---

## 3.6 常见问题

<details>
<summary><strong>Q1: GA 说"文件不存在"，但文件明明在？</strong></summary>

**可能原因**：GA 错误识别了目录，或当前工作目录与我们的认知不一致。

**解决方法**：
1. 使用绝对路径（包含盘符的完整路径）
2. 右键文件 →「复制为路径」→ 粘贴到输入框
3. 先告诉 GA 文件位置，再让它操作

</details>

<details>
<summary><strong>Q2: 代码运行结果不对？</strong></summary>

**解决方法**：直接告诉 GA「结果不对，我预期的是 XXX」。GA 会自己 debug 并修复。

**示例**：
- 👧：帮我统计每个月的销售额
- 🤖：[返回结果]
- 👧：不对，应该是按自然月统计，不是按 30 天
- 🤖：[重新计算，返回正确结果]

</details>

<details>
<summary><strong>Q3: 对话太长，GA 开始"忘事"？</strong></summary>

**解决方法**：
1. 让 GA 把关键信息写入工作记忆：「请将 xxx 写入 working_checkpoint」
2. 让 GA 把重要偏好写入长期记忆：「记住这个设置，以后都用」
3. 开新会话后让 GA 读取历史：「我们上次聊了什么？」

</details>

<details>
<summary><strong>Q4: GA 安装依赖失败？</strong></summary>

**解决方法**：
1. 检查网络连接，确保可以访问外网
2. 如果有代理，确保代理设置正确
3. 尝试手动安装：`pip install 包名`
4. 告诉 GA：「继续安装剩余的依赖」

</details>

<details>
<summary><strong>Q5: 浏览器控制不生效？</strong></summary>

**解决方法**：
1. 重新执行：「执行 web setup sop 解锁 web 工具」
2. 检查浏览器扩展管理页面，确认插件已启用
3. 确保浏览器是最新版本

</details>

<details>
<summary><strong>Q6: GA 响应很慢？</strong></summary>

**解决方法**：
1. 切换到更快的模型（如果配置了多个）
2. 把复杂任务拆分成多个简单任务
3. 检查网络连接质量

</details>

<details>
<summary><strong>Q7: 如何让 GA 记住我的偏好？</strong></summary>

明确告诉 GA「记住这个设置」或「以后都这样做」，GA 会把偏好写入记忆系统，下次自动应用。

**示例**：
- 👧：以后分析数据时，都用柱状图，不要用折线图
- 🤖：好的，我会记住这个偏好

</details>

---

## 📂 相关文件速查

| 内容 | 路径 |
|---|---|
| GA 启动入口 | `launch.pyw` |
| API 密钥配置 | `mykey.py` |
| GA 记忆存储目录 | `memory/` |

---

## 📝 小结

- **界面简洁**：GA 是自然语言交互助手，输入框打字即可使用
- **扩展能力**：OCR → 视觉 → 飞书 CLI → Everything CLI，每个都是一句话配置，永久提升 GA 表现
- **对话技巧**：明确指代、分步确认、善用记忆，让 GA 越用越懂你

---

[上一章：第 2 章 浏览器能力解锁](../chapter2/) | [下一章：第 4 章 记忆与 Skill 系统](../chapter4/)
