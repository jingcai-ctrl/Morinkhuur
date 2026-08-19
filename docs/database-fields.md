# 马头琴文化资源数据库字段表与著录模板

- 版本：1.0
- 制定日期：2026-08-19
- 对应路线：第4步“制定数据库字段表”
- 适用分支：`database-rebuild`

## 1. 字段设计原则

- 数据采用 UTF-8；字段键使用小写英文 `snake_case`，中文标签用于界面显示。
- 未知单值使用 `null`，未知多值使用空数组 `[]`；不得用推测内容填补空值。
- “待核实”“版权待确认”等状态写入专门字段，不混入事实字段。
- 日期采用 ISO 8601；无法精确到日时只填写已知精度，并保存原始年代文本。
- 人物、地点、来源、媒体和相关资源优先使用编号关联，避免反复复制文字。
- 公开记录必须保留来源、版权、审核状态和更新时间。

字段要求级别：`C` 为创建记录时必填，`P` 为公开发布前必填，`O` 为选填或条件必填。

## 2. 公共字段

| 字段键 | 中文名称 | 类型 | 级别 | 说明与空值规则 |
| --- | --- | --- | --- | --- |
| `record_id` | 唯一编号 | string | C | 按第5步编号规范生成，不得重复或回收 |
| `resource_type` | 资源类型 | enum | C | 九类资源之一 |
| `resource_subtype` | 二级分类 | string | P | 采用第2步目录；无法判断时使用相应“待分类”项 |
| `working_title` | 工作名称 | string | C | 仅用于内部辨识，不得伪装成已确认名称 |
| `title_zh` | 中文名称 | string/null | P | 未确认时为 `null` |
| `title_mn_traditional` | 传统蒙古文名称 | string/null | O | 仅录入经核实文本 |
| `title_mn_cyrillic` | 西里尔蒙古文名称 | string/null | O | 仅录入经核实文本 |
| `title_en` | 英文名称 | string/null | O | 保存核实后的名称或译名 |
| `alternate_titles` | 其他名称 | array | O | 每项注明语言、名称、来源编号和使用说明 |
| `summary_zh` | 中文摘要 | string/null | P | 区分事实、引文和解释 |
| `description_zh` | 详细说明 | string/null | O | 不复制无来源的断言 |
| `keywords` | 关键词 | array | O | 使用规范词，避免同义词无控制扩张 |
| `region_refs` | 地区关联 | array | O | 只关联有依据的地区；推测地点不得填写 |
| `place_note` | 地点原始表述 | string/null | O | 保存来源中的原始地点文字 |
| `date_label` | 年代原始表述 | string/null | O | 如“20世纪初”；不得自行精确化 |
| `date_start` | 起始日期 | string/null | O | 仅在来源支持时填写 |
| `date_end` | 结束日期 | string/null | O | 仅在来源支持时填写 |
| `date_precision` | 日期精度 | enum/null | O | `day/year/decade/century/period/unknown` |
| `date_note` | 年代说明 | string/null | O | 记录争议、推定依据或待核实原因 |
| `source_refs` | 来源编号 | array | P | 公开记录至少关联一项可追溯来源 |
| `media_refs` | 媒体编号 | array | O | 图片、音频、视频、模型等 |
| `related_records` | 相关记录 | array | O | 包含目标编号、关系类型和来源编号 |
| `rights_status` | 版权状态 | enum | C | 值域见第6步规范 |
| `access_level` | 公开级别 | enum | C | 值域见第6步规范 |
| `review_status` | 审核状态 | enum | C | 值域见第6步规范 |
| `credibility_level` | 可信度等级 | enum/null | P | 依据第6步来源等级判定 |
| `uncertainty_note` | 不确定性说明 | string/null | O | 明确哪些信息待核实 |
| `legacy_location` | 原网页位置 | string/null | O | 保存原文件名、章节锚点或旧链接 |
| `internal_note` | 内部备注 | string/null | O | 不在公开页面展示 |
| `created_at` | 创建日期 | date | C | ISO 日期 |
| `updated_at` | 更新日期 | date | C | 每次实质修改时更新 |
| `record_version` | 记录版本 | integer | C | 从 `1` 开始递增，不写入编号 |
| `published_at` | 首次发布日期 | date/null | O | 未公开时为 `null` |

## 3. 九类资源专属字段

### 3.1 器物

| 字段键 | 中文名称 | 类型 | 级别 |
| --- | --- | --- | --- |
| `object_form` | 器型或形制 | string/null | O |
| `materials` | 材料关联 | array | O |
| `dimensions` | 尺寸 | array | O |
| `maker_refs` | 制作者关联 | array | O |
| `component_refs` | 部件关联 | array | O |
| `collection_holder` | 收藏或保管机构 | string/null | O |
| `collection_number` | 馆藏编号 | string/null | O |
| `condition_note` | 保存状况 | string/null | O |
| `provenance_note` | 流传或收藏经过 | string/null | O |

尺寸每项保存 `dimension_type`、`value`、`unit`、`source_ref`，不得从图片估算。

### 3.2 部件

| 字段键 | 中文名称 | 类型 | 级别 |
| --- | --- | --- | --- |
| `component_kind` | 部件类型 | string | P |
| `parent_instrument_refs` | 所属器物 | array | O |
| `position_note` | 所在位置 | string/null | O |
| `function_note` | 功能说明 | string/null | O |
| `materials` | 材料关联 | array | O |
| `dimensions` | 尺寸 | array | O |
| `construction_note` | 构造说明 | string/null | O |

### 3.3 制作技艺

| 字段键 | 中文名称 | 类型 | 级别 |
| --- | --- | --- | --- |
| `process_kind` | 工艺类型 | string | P |
| `sequence_label` | 工序顺序原始表述 | string/null | O |
| `input_material_refs` | 使用材料 | array | O |
| `tool_refs` | 使用工具 | array | O |
| `steps` | 操作步骤 | array | O |
| `output_refs` | 产出器物或部件 | array | O |
| `practitioner_refs` | 实践者关联 | array | O |
| `variation_note` | 地区或个体差异 | string/null | O |
| `safety_note` | 安全说明 | string/null | O |

### 3.4 制作材料与工具

| 字段键 | 中文名称 | 类型 | 级别 |
| --- | --- | --- | --- |
| `item_kind` | 材料或工具 | enum | P |
| `material_kind` | 材料类别 | string/null | O |
| `tool_kind` | 工具类别 | string/null | O |
| `scientific_name` | 材料学名 | string/null | O |
| `properties_note` | 性质说明 | string/null | O |
| `use_process_refs` | 相关工艺 | array | O |
| `specification` | 规格 | string/null | O |

材料学名、物种和产地必须有明确来源。

### 3.5 传承人物

| 字段键 | 中文名称 | 类型 | 级别 |
| --- | --- | --- | --- |
| `preferred_name` | 规范姓名 | string/null | P |
| `name_variants` | 姓名异体 | array | O |
| `birth_date_label` | 出生年代原始表述 | string/null | O |
| `death_date_label` | 卒年原始表述 | string/null | O |
| `role_terms` | 角色 | array | O |
| `affiliations` | 机构关联 | array | O |
| `lineage_relations` | 师承或传承关系 | array | O |
| `biography` | 生平简介 | string/null | O |
| `person_rights_note` | 人物信息授权或隐私说明 | string/null | O |

称号、名录级别、师承和生卒信息均须关联来源。

### 3.6 音乐与曲目

| 字段键 | 中文名称 | 类型 | 级别 |
| --- | --- | --- | --- |
| `music_resource_kind` | 曲目、版本、乐谱、音频或示范 | enum | P |
| `genre_label` | 体裁原始表述 | string/null | O |
| `creator_refs` | 作曲、编曲或创作者 | array | O |
| `performer_refs` | 演奏或演唱者 | array | O |
| `collector_refs` | 采录或整理者 | array | O |
| `duration_seconds` | 时长 | number/null | O |
| `notation_note` | 记谱说明 | string/null | O |
| `version_note` | 版本说明 | string/null | O |
| `audio_media_refs` | 音频媒体 | array | O |
| `score_media_refs` | 乐谱媒体 | array | O |

曲目主体、演奏版本和媒体文件分别著录并关联。

### 3.7 装饰纹样

| 字段键 | 中文名称 | 类型 | 级别 |
| --- | --- | --- | --- |
| `pattern_kind` | 纹样类型 | string | P |
| `motif_elements` | 构成元素 | array | O |
| `application_locations` | 应用部位 | array | O |
| `technique_note` | 制作技法 | string/null | O |
| `color_note` | 色彩说明 | string/null | O |
| `meaning_statement` | 寓意或象征表述 | string/null | O |
| `example_object_refs` | 应用器物实例 | array | O |

纹样名称、族属和寓意不得仅凭外观判断。

### 3.8 历史事件

| 字段键 | 中文名称 | 类型 | 级别 |
| --- | --- | --- | --- |
| `event_kind` | 事件类型 | string | P |
| `event_date_label` | 时间原始表述 | string/null | O |
| `event_place_refs` | 地点关联 | array | O |
| `participant_refs` | 人物或机构 | array | O |
| `event_description` | 事件经过 | string/null | P |
| `historical_context` | 历史背景 | string/null | O |
| `interpretation_note` | 观点与争议 | string/null | O |

事实、来源原文和研究者解释必须分开著录。

### 3.9 文化场景

| 字段键 | 中文名称 | 类型 | 级别 |
| --- | --- | --- | --- |
| `scene_kind` | 场景类型 | string | P |
| `activity_note` | 活动内容 | string/null | O |
| `participant_refs` | 参与者 | array | O |
| `place_refs` | 地点 | array | O |
| `context_note` | 社会文化语境 | string/null | O |
| `documentation_method` | 记录方式 | string/null | O |
| `event_refs` | 相关事件 | array | O |

地点、人物、活动名称和拍摄时间缺失时保持空值。

## 4. 辅助记录模板

### 4.1 来源记录

至少包含：`source_id`、`source_type`、`title`、`creator`、`publisher_or_holder`、`publication_date`、`identifier`、`url`、`accessed_at`、`citation_text`、`credibility_level`、`verification_note`。

### 4.2 媒体记录

至少包含：`media_id`、`parent_record_refs`、`media_type`、`file_path`、`original_filename`、`caption`、`creator`、`created_date`、`created_place`、`provider`、`rights_status`、`license`、`access_level`、`source_ref`、`checksum`、`technical_metadata`。

## 5. JSON 基础模板

```json
{
  "record_id": null,
  "resource_type": null,
  "resource_subtype": null,
  "working_title": null,
  "title_zh": null,
  "title_mn_traditional": null,
  "title_mn_cyrillic": null,
  "title_en": null,
  "alternate_titles": [],
  "summary_zh": null,
  "description_zh": null,
  "keywords": [],
  "region_refs": [],
  "date_label": null,
  "source_refs": [],
  "media_refs": [],
  "related_records": [],
  "rights_status": "版权待确认",
  "access_level": "暂不公开",
  "review_status": "待整理",
  "credibility_level": null,
  "uncertainty_note": null,
  "legacy_location": null,
  "created_at": null,
  "updated_at": null,
  "record_version": 1,
  "type_specific": {}
}
```

模板只规定结构，不代表已有任何具体事实。第9步转换现有内容时再建立实际 JSON 记录。

## 6. 发布前最低完整性要求

- 唯一编号、资源类型、二级分类和中文名称已确认；
- 中文摘要能说明记录对象，不含无来源断言；
- 至少关联一项可追溯来源；
- 来源可信度、版权、公开级别和审核状态已填写；
- 涉及媒体时，媒体自身的来源和版权状态已填写；
- 待核实内容有明确说明，没有把空值改写成推测；
- 创建日期、更新日期和记录版本有效。
