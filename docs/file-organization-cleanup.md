# 第8步：网站文件清理与重新组织记录

- 执行日期：2026-08-19
- 执行分支：database-rebuild
- 范围：仅清理和重组现有网站文件；未创建数据库记录，未进入第9步
- 原则：不改变媒体二进制内容，不推测年代、来源、版权或蒙古文信息

## 1. 检查结论

| 检查项 | 结果 | 处理 |
| --- | --- | --- |
| SVG 版本 | 仓库中未发现 SVG 文件 | 无需处理 |
| .bak 文件 | 未发现 | 无需处理 |
| CSS | style.appfix.css 为当前页面加载版本；style.css 未被页面加载 | 后者归档到 archive/legacy/style.css |
| JavaScript | js/main.appfix.js 为当前页面加载版本；js/main.js 未被页面加载 | 后者归档到 archive/legacy/js/main.js |
| 未使用图片 | 结合 HTML、CSS 和 JavaScript 静态引用检查，未发现可安全判定为未使用的独立图片 | 全部保留并迁移 |
| 重复图片 | 发现两组内容完全相同的历史图片 | 统一引用后移除两个重复路径 |
| FBX / GLB | 未发现 | 建立空目录占位，等待后续有真实文件时使用 |
| 文件名 | 存在中文、数字序号及多套目录混用 | 改为小写英文、数字和连字符 |
| 原始文件与网页文件 | 当前仓库只有网页在用媒体，不能可靠判定保存级原件 | 不标记 master；仅按网页资源重组 |

## 2. 清理与归档清单

### 归档但不删除内容

- style.css → archive/legacy/style.css
- js/main.js → archive/legacy/js/main.js

这两个文件均未被现有 HTML 页面加载；归档保留历史内容，不影响当前展示。

### 删除重复路径

- assets/history/history-ppt-1.png：与 assets/history/history-mawei.jpeg 内容完全相同；页面引用统一到迁移后的单一文件。
- assets/history/history-ppt-2.png：与 assets/history/history-modern.jpeg 内容完全相同；页面引用统一到迁移后的单一文件。

删除的是重复路径，不删除唯一媒体内容。

## 3. 新目录结构

    assets/
    ├── images/
    │   ├── instruments/
    │   ├── components/
    │   ├── crafts/
    │   ├── materials-tools/
    │   ├── people/
    │   ├── music/
    │   ├── patterns/
    │   ├── history/
    │   ├── scenes/
    │   ├── performance/
    │   └── interface/navigation/
    ├── audio/performance/
    ├── video/
    ├── models/
    └── documents/
    archive/
    └── legacy/js/

空目录通过 .gitkeep 保留；它们不代表已有资源。

## 4. 媒体映射

完整逐文件映射见 docs/media-file-map.csv。其中：

- S8-MEDIA-* 只是本次迁移序号，不是数据库资源编号或媒体编号；
- database_record_status 全部为 pending_step_9；
- original_filename 保留迁移前文件名；
- 本步骤没有分配 MK-* 编号，也没有填写来源和版权结论。

## 5. 验证标准

- 活跃 HTML、CSS、JavaScript 中不再引用旧媒体路径；
- 所有活跃本地资源引用均能在重组后的目录树中解析；
- 图片与音频的 Git blob SHA 在迁移前后保持一致；
- 现有图片数量由 73 个路径变为 71 个唯一图片路径，差额为两条已证实的重复路径；
- 5 个音频全部保留；
- 活跃 JavaScript 通过语法检查；
- 页面标题、正文、交互结构和媒体内容均未改写。
