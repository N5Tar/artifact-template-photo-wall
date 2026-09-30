# 纸质处理层：把照片做成「砖」

每张印刷照片 = **生活照内容 + 单独分配的纸边与内部印刷效果**。可在一次场景生成中直接实现，也可先生成独立素材再融合。本文材质段适用于所有路线；“平放、无场景”的素材契约只适用于独立处理图。输入位与分批规则统一遵循主文，不强制逐张调用。
不是海报或微缩场景，不添加背景故事。默认采用纯照片样式；仅用户选择时使用下述其他版式，不主动新增文字。默认照片内部也要有温和的印刷处理，不只添加纸边；用户明确要求保持原始外观时，遵循下文的例外分支。

## 独立素材要求（单次场景生成不套用）

仅在制作独立处理图时，产物须满足：

- 正面平放，无透视、无倾斜、无场景投影
- 工具支持时使用真实透明背景；否则放在干净的中性纯色工作背景上，四周留边，供后续图像模型识别纸张主体
- 保持照片内容窗口的原始长宽比，卡片外形可以不同；不得拉伸或裁掉受保护内容
- 不含新增的墙、桌、夹、绳或道具；保留原照片已有内容与文字，不从参考图复制文字、水印。仅按用户确认的确切内容在指定位置新增文字
- 照片内容可辨认——脸、宠物、地方不能变

工作背景不是照片或纸张的一部分。最终将处理图连同场景背景实际传给图像模型，由模型融合纸张并排除工作底色；不使用脚本阈值分割、抠像或拼接。透明能力不可用时不得声称输出透明，最终需检查工作背景残留。

## 砖的样式：可混搭的版式

一面真实的墙不会只贴一种东西——票根、明信片、拍立得、普通照片混着贴才有人味。所以砖有两个维度：

- **样式**（版式）：可混搭，一面墙最多用 2-3 种。太多了变杂货铺。
- **材质**：默认按内容、比例、情绪和构图逐张选 2–3 种，有主有次，不平均轮换。四种材质都可共同出现，用户指定统一材质时遵从。统一场景光线与整体色调，保留纸边、颗粒、褪色强度的差异；哑光仅为可选项，不是首选。照片少时不硬凑种类，不复制照片。

| 样式 ID | 版式要点 | 借用来源 |
|---|---|---|
| `photo` | 纯照片 + 纸质边，无版式无文字 | 本文件四种材质 |
| `ticket` | 圆角照片窗、齿孔、撕票虚线、编号、装饰条形码 | `travel-ticket` 的 travel-ticket 段 |
| `boarding-pass` | 顶部深色标题带 + 字段网格（DESTINATION / FLIGHT / DATE / GATE / SEAT）+ 虚线齿孔 + 条形码 | `travel-ticket` 的 boarding-pass 段 |
| `postcard` | 照片满版 + 细白边 + 右上角邮票与圆形邮戳 + 地名衬线大写 | `travel-ticket` 的 postcard 段 |
| `instant` | 厚白框，下巴明显更宽 | `travel-ticket` 的 instant-film 段 |
| `film-frame` | 横版，近黑片基 + 上下齿孔 + 中间一帧照片 | `travel-ticket` 的 film-frame 段 |

### 样式提示词要点

独立素材使用下文独立前缀，按工具能力选择透明或中性工作底，正面平放、无场景投影。单次或分批场景生成只将所选样式和材质段绑定到相应 ID，由场景提示决定透视、摆放和投影，不使用独立前缀。以下含文字的版式元素仅在用户明确要求并提供确切内容时启用，否则省略标题、编号、字段、条形码和邮戳；保留照片中已有的文字。

**ticket**：`Warm ivory ticket stub with a photo window preserving the complete photograph and its aspect ratio, punched edge perforations and a tear-off dashed line outside the photo. Leave the remaining cardstock blank. Preserve existing source text; add no lettering, numbers, barcode or watermark.`

**boarding-pass**：`Tall white cardstock with an unlettered muted header band, a photo window sized to show the complete photograph at its original aspect ratio, blank ruled sections below, and a perforated tear-off line outside the photo. Preserve existing source text; add no labels, values, numbers or barcode.`

**postcard**：`The complete photo at its original aspect ratio with a slim off-white cardstock margin. Use the selected print tone and texture. Preserve existing source text; add no stamp, postmark, place name, caption or watermark.`

**instant**：`Thick off-white frame with even side margins and a distinctly wide bottom chin, photo inset with softly rounded corners and gentle chemical fade. Preserve source handwriting and dates; add none.`

**film-frame**：`A horizontal near-black film card with sprocket perforations along top and bottom. Fit the complete photograph within it at its original aspect ratio and orientation. For a portrait photo, use wider blank side margins; do not crop, stretch or rotate the photo to make it landscape. Use only non-textual edge markings; add no frame numbers or branding.`

### 混搭规则

- 一面墙最多 2-3 种样式，且必须有主有次（以一种为主，其余点缀）。
- 各照片的材质保持差异：撕边带纤维与印刷颗粒，拍立得带宽下边与化学褪色，黑胶片边带胶片颗粒，哑光带纸感与轻柔化。整体色调和场景照明协调即可，不将全部照片套同一滤镜。
- 逐图记录选择理由、材质、处理图（直接生成时无独立文件）、批次与最终位置。内容与比例是判断依据，不机械将竖图全配拍立得或横图全配胶片。
- 不编造日期、地点、编号、邮戳或装饰文字。用户要求新增时记录确切内容和位置，不暗示装饰条码可扫描或真实。
- 保留源图文字；新增文字必须符合确认的内容。出现乱码按主流程重试上限修正，不能擅自换成更短字符串。

## 四种纸质语言（材质）

每张分别选材质，默认混搭而非全组统一。用户指定统一材质时才按该要求执行。默认 `photo` 使用对应完整材质模板，同时呈现边缘与内部效果。其他样式选兼容材质：`instant` 配同名材质只产生一个白框，`film-frame` 配 `film-border` 保留黑片边。票根等信息型样式需用户选择；边缘冲突时调整计划或澄清，不叠框，也不把丢掉手撕／黑片边的结果算作该材质已实现。

照片比例指内部图像窗口，非整张卡片。竖照可完整置入横向胶片卡，留出侧边片基；若主体因此过小，提议改用随原图方向的 `film-border`，不要擅自换样式。

| ID | 材质 | 边缘 | 借用来源 |
|---|---|---|---|
| `matte-print` | 暖调哑光相纸，细颗粒，轻微柔化，高光压一点 | 整齐裁切，窄白边 | `travel-ticket` 的 postcard 材质段 |
| `torn-edge` | 暖米色纸，印刷颗粒，边缘轻微褪色 | 手撕纤维边，露纸毛 | `zine` 的 Torn-paper handoff 全节 |
| `instant` | 即时成像，轻微化学褪色，色彩偏成熟 | 厚白框，下巴明显更宽 | `travel-ticket/assets/styles/instant-film.png` |
| `film-border` | 细颗粒，柔和色彩，轻微年代感 | 细黑片边，可带无字片边标记 | `travel-ticket/assets/styles/film-frame.png` |

## 提示词模板

将当前原图实际传入工具，照片内容只由它提供；必要时追加一句已核实的受保护细节，用于防止漂移，不能替代图像输入。可选材质参考仅指导纸质外观，不能带入其主体或文字。独立素材使用下方前缀及该图材质模板；场景生成使用主文场景骨架及逐 ID 材质段，不追加独立素材前缀。若需两阶段，已确认素材在后续融合时保留效果、不二次加框。

用户要求保持原始外观时，将下面前缀中的内部印刷处理指令替换为 `Preserve the photograph's original colors, contrast, sharpness and texture; apply the agreed treatment only to the surrounding paper.`，并移除所选样式/材质段中内部调色、颗粒、柔化、暗角、褪色等指令，不只在末尾追加相反要求。

默认模板禁止新增文字。仅在用户明确要求时，将对应禁新增文字条款改为只允许已确认的确切字符串和位置，保留其他禁项；不得删除原图文字，不把格式示例当用户内容。邮戳、条码等装饰也须单独确认，不自动生成。

### 独立素材前缀（仅两阶段的单图处理使用）

```
Render the supplied photograph as a single small paper print, lying flat and
facing the viewer, with margins on all sides. Use a truly transparent exterior
if the tool supports it; otherwise use a plain neutral working background.
Keep the photo's original aspect ratio. Preserve the same person/pet/place,
pose, scene and existing readable text. Apply the selected print tone and
texture within the photograph as well as its paper edge; do not redraw or
invent subjects. Flat scan lighting, no perspective, no tilt, no cast shadow,
no added props, no wall, no added text or watermark.
```

### matte-print

```
Paper: warm-toned matte photo paper with fine print grain, slight softening
and gently pulled-down highlights. Clean straight-cut edge with a narrow white
margin. Colors stay true with a quiet warm shift.
```

### torn-edge

```
Paper: warm cream matte stock with fine print grain and gentle fading toward
the edges. All four sides are hand-torn: an irregular contour with a narrow
feathered fringe of exposed paper fibers and warm paper tone interrupting the
photographic edge, slight local thinning and dry pigment loss near a few
segments, natural asymmetry — some stretches nearly straight, some softly
ragged, one or two stronger torn pressure points. Keep the active fibrous band
narrow. No lifted-paper depth.
```

### instant

```
Paper: classic instant print — thick off-white frame with even side margins and
a distinctly wide bottom chin, photo inset with softly rounded corners and a
gentle chemical fade. Frame slightly warm-aged. Preserve source handwriting and dates; add none.
```

### film-border

```
Paper: a single film print with a thin black border, fine film grain and
gentle color rendering. Edge markings, if any, must be non-textual. No frame
numbers, no brand names.
```

## 参考选择与可选外部技能

先按主文优先级使用用户本轮参考或本地已认可的 `assets/frames/` 对应场景图。逐张处理时，在预算允许的情况下将其与原图一同实际传入，明确只借纸张、色调、颗粒和边缘，不借场景、人物或文字、水印。照片主体只来自当前原图。素材未去水印时明确禁止复制水印，并检查处理图。参考输入不足时按主文披露，不假装已传入。

只有这些参考不足以指导材质时才考虑以下可选来源，不自动替换已确认参考。按需检查目录：

- `../artifact-template-travel-ticket/` — 有则借它的参考图
- `../scenes-gathered-zine-v1-3/SKILL.md` — 有则读它的 Torn-paper handoff 章节

**借参考图**（比文字有效得多）：

| 砖的处理 | 可借的参考图 |
|---|---|
| `instant` | `artifact-template-travel-ticket/assets/styles/instant-film.png` |
| `film-border` | `artifact-template-travel-ticket/assets/styles/film-frame.png` |
| `matte-print` | `artifact-template-travel-ticket/assets/styles/postcard.png`（取其材质与颗粒，忽略版式） |

**借规则**：使用外部技能前读取其指令；`zine` 的 Torn-paper handoff 可补充纤维边、纸毛、磨损和自然不对称细节，但不得覆盖用户已确认的参考、内容保护、文字限制或本 Skill 的无脚本流程。

**借不到的兜底**：目录不存在就用上面的文字模板，不报错、不阻塞、不要求用户去装。装了体验更好，装一个也能跑。

参考图提供材质，**绝不提供照片主体、文字或水印**。

## 产出与验收

- 单次路线不强制单张样片；两阶段有精修需求时可先确认代表样片，逐张按各自材质处理，不能用一种材质的通过代替其他材质验收。所有路线均逐照片对照人物、宠物、姿态、场景、文字与受保护细节。
- 默认处理效果必须在照片内部可见，同时不过度模糊或遮挡内容。用户明确要求保持原始外观时，记录受影响源图并豁免内部风格强度检查，改查外观保持、约定纸边和内容；该例外不豁免其他验收。
- 保存工具实际输出，记录为 `photo-01_<paper>` 等稳定标识及对应文件；扩展名遵循真实格式，不把改后缀当格式转换。
- 核实纸边完整、工作背景形式如实记录；工具不支持透明时不强制脚本抠图。
- 只要成图且输入足够时，无需生成独立素材文件；在场景中完成各图纸边和内部处理即可。要独立素材时才按此契约制作文件，后续融合仍需检查实际容量。
- 漂移按主流程上限进行定向重试；无法视觉检查时遵循人工验收门槛。

## 交给图像模型做最终融合

按主文核实当前接口上限 K，再列本轮原图／处理图、背景、风格参考、上一轮场景等实际输入；全部占位。输入足够可一次按逐图材质表完成处理和融合。两阶段使用已确认的处理图，不擅自替换为裸原图。

超限先移除可选风格参考；仍不足时说明批次、成品数量及风险，确认实质取舍。分批续轮的已有场景占 1 位，新增图最多 K−1 张，附加参考和重传图再扣减；每轮验收全部累计照片，不能只查本轮新增。指定背景需首轮实际传入、后续场景保留，不用文字或参考重生成替代。禁止脚本拼接或低清参考拼图绕过限制。

按最终位置和显示尺寸检查数量、内容保真、各自材质的边缘及内部效果、留白、光照透视和可读性。多图塞不下就建议分组／多成品，不无限缩小。背景为参考重生成时如实说明。任何路线均不保证零漂移，逐轮对照原图、已有场景及可用处理图验收。
