# StreamScope 官方插件库

为 [StreamScope](https://github.com/ZeruneLab/StreamScope) 提供独立发布的签名功能插件及共享运行时。面向 Windows x64，用户下载和安装不需要 GitHub 登录。

## 最新媒体插件：v0.2.2

批次 [`plugins-2026.10.23.1`](https://github.com/ZeruneLab/StreamScope-Plugins/releases/tag/plugins-2026.10.23.1) 更新音频、H.264、H.265、PCAP、RTSP 和视频深度分析六个插件，建议配合核心宿主 [v0.2.5](https://github.com/ZeruneLab/StreamScope/releases/tag/v0.2.5) 使用。

- 修正诊断证据分类、帧率来源、长 GOP / HRD / 近静音的判断与展示，保留真实中途解码异常。
- RTSP 插件修复 H.264 限时停止时尾部孤立 SEI 引起的人为解码错误，原始证据不变；解码副本记录排除范围。
- 六个媒体插件统一从本地测试版升级至 v0.2.2，不覆盖同版本包。发布者密钥和权限不变；SIP、ONVIF、示例和 FFmpeg 不重发。
- 先更新主程序，再在插件管理页批量更新媒体插件。核心安装包不包含或自动替换独立业务插件，旧报告也不会自动改变结论。

签名目录保留旧公开版本，并把新版本放在同一插件的旧版本之前；旧 Release 和资产不删除。新批次 Release 同时提供签名目录快照与 SHA-256 文件，便于离线核验。

## 安装

安装 StreamScope v0.2.0 或更高版本，在“插件管理”点击“检查官方插件目录”，按需选择功能并确认发布者、权限和原生代码风险。媒体功能会安装共享 FFmpeg 运行时，无需单独配置系统 FFmpeg。

离线用户可从本仓库 Releases 下载 `.ssp`，先导入依赖再导入功能包。插件按 Windows 用户安装，报告在卸载插件时保留。

## 目录结构与信任

- `catalog.json`：所有已发布兼容版本的目录，包 URL 指向本仓库具体 Release 标签，不能使用 `releases/latest` 作为包地址。
- `catalog.json.sig`：目录原始字节的 Ed25519 签名，Base64 格式。
- `catalog.pub`：目录公钥，仅供核对；软件不会动态相信仓库内的新公钥，信任根来自已安装主程序。
- `publisher.pub`：官方插件发布者公钥，仅供核对。
- `releases/download/<批次>/<插件ID>-<版本>-windows-x86_64.ssp`：签名插件包；同一版本不覆盖，更新须发布新版本。

固定目录地址：https://raw.githubusercontent.com/ZeruneLab/StreamScope-Plugins/main/catalog.json

正式插件发布者指纹：`sha256:8f1e1ccefc22442539ee9084cd16835ae4e211008a99a130f4dd1263b465bbbf`。

签名证明来源和完整性，不保证原生代码安全。插件是独立 EXE，以当前 Windows 用户权限运行，没有系统级沙箱；请只安装可信发布者的插件。

## 从本地预览迁移

0.1.10/0.1.11 本地预览插件由临时密钥签名，不自动换成正式签名。先备份 `%LOCALAPPDATA%\StreamScope\plugins`，在无任务运行时先卸载测试功能插件，再卸载被它们依赖的测试运行时包，随后通过官方目录重新确认安装。不要手动改注册表或绕过验签；报告不会随插件卸载删除。

## 维护者发布流程

1. 插件源代码、Schema 和开发文档在主程序仓库维护；本仓库只存公开目录、公钥及发布说明。私钥不进任何仓库或 Release。
2. 使用固定发布者密钥构建 `.ssp`，每个插件分别递增 SemVer；运行隔离安装、目录验签、依赖和工作流回归。
3. 创建新批次的草稿 Release，上传完整包并验证，之后发布。永远不覆盖已经被目录引用的同版本包。
4. 用固定目录密钥签署新目录，在单个提交中更新 `catalog.json` 和 `.sig`。保留仍受支持的旧版本条目；不要覆盖在用平台或删除旧包。
5. 验证公开目录及全部资产无需身份认证即可访问，哈希、签名、Host API 和依赖一致；主程序 Release 不受插件发布影响。

主程序更新清单仅在 `ZeruneLab/StreamScope` 发布。本仓库发布任何新插件，都不会改变主程序的最新版本。
