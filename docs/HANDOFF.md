# 下一轮优化精简交接

## 从这里继续

用户要求把 v0.20 上传 GitHub，整理需求、压缩上下文，准备下一轮优化。需求以 REQUIREMENTS.md 为准。无需重读全部聊天或历史版本。

- 项目：Windows C# .NET 6 WinForms，目录 F:/ChatGpt/JX3，核心源码 app/JX3Monitor。
- 基线：release/v0.20，旧版本保留；不要杀用户正在运行的监控、覆盖其 user-data 或批量清理工作目录。
- 构建必须在 app/JX3Monitor 运行，global.json 固定 SDK 6.0.428。dotnet build -c Release；--no-restore 仅用于本地已有缓存时。
- 新目录构建示例：dotnet build -c Release -o ../../release/v0.21。不要把测试直接跑到用户现用版本。
- 自检：启动 exe --self-test 并等待退出，检查 self-test.txt 是否 ALL PASSED。v0.20 历史记录 209 项通过；部分测试依赖本机临时截图，干净机器不能以相同数量为验收标准。

## 模块地图

- Program.cs：Rule、Tracker、主窗体、编辑器和旧自检；后续适合拆分。
- RealtimeCapture.cs / BatchAnalyzer.cs：分组采集、异步识别、缓存、状态机调用。
- YoloDetector.cs：ONNX Runtime、分块预处理和 NMS；模型在 models/jx3-yolo-v1。
- DenseIconScore.cs / IconPatchMatcher.cs / IconIdentityGuard.cs：候选图标复核、遮挡容错、已确认心法身份排除。
- TargetMonitoring.cs / TargetHearts.cs：三槽目标、信任时效、预设生成；目标有效结果不能被无关技能更新的 epoch 丢弃。
- MonitorList.cs / RuleOrder.cs / OverlayOrdering.cs：列表合并、移动及浮窗顺序。
- CountdownSurface.cs：透明浮窗直接绘制，中央 CD、右下角黑描边充能数字。
- ChargeTracker.cs / ChargeDigitReader.cs / ChargeOptions.cs：逐层回充、PVE 角标识别、同栏同技能选项同步。
- ConfigurationPackage.cs / ConfigurationActions.cs / RegionBinding.cs：配置打包、导入及窗口重新绑定。

## 最近修复与陷阱

- v0.17：窄长 ROI 整体缩小让两个 Buff 合成一个框，全部技能只有约 40%。改重叠分块；雪重花满截图约 89%。
- v0.18：心法图标令无关技能相关性约 50%，增加 YOLO+库图双重复核身份排除。默认关闭静态复用；修复目标结果被无关 epoch 丢弃。不能声称分数已概率校准。
- v0.19：充能及区域恢复、统一浮窗顺序。
- v0.20：用户手动星楼月影配置充能为 true，但实际自动目标配置为 false，浮窗无角标。SyncChargeOptions 按同 GroupId+技能身份同步开关和上限，独立保留各目标计时。启动、导入及新自动预设执行 NormalizeChargeOptions。
- 不要仅改右下角绘制却继续显示错误配置。验证真实自动槽使用后 tile.Charges=1 且 Cooling=true。

## 证据及限制

模型使用合成图标及两张用户心法截图训练，缺少独立实战数据集。数字 1/2 样本也用作字形参考，测试回放不是独立准确率验证。CPU 两线程，默认 8 Hz/预览 2 Hz；DirectML 可选，未做游戏负载下完整对照。

实际桌面探针在游戏遮住测试窗时采集到游戏画面，不能据此声称阳性识别成功。自检和少量截图通过不代表实战全部修复。

GitHub 排除个人配置、完整截图、训练环境、历史 release 和数据库副本；保留源码、运行模型、游戏图标/目录及原生依赖。第三方资产与模型许可见 THIRD_PARTY.md。根目录 README.md 保留历史记录；下一轮优先读本文件、REQUIREMENTS.md 和当前源码。

## 新轮启动提示词

继续优化剑三监控程序。先阅读 docs/HANDOFF.md 和 docs/REQUIREMENTS.md，以 v0.20 为基线。核对当前源码、Git 状态和正在运行版本；按我提供的新问题复现后修复。保留配置与历史版本，用独立构建目录验证；报告测试证据和实战限制。
