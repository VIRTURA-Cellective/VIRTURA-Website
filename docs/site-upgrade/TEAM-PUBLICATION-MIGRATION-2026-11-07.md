# VIRTURA 官网与团队内容迁移实施方案

日期：2026-11-07。此文件属于维护者实施文档，不是公开页面文案。本 PR 仅交付方案，不改变部署。

## 1. 已核实范围与权限

- 官网仓库：VIRTURA-Cellective/VIRTURA-Website；默认分支 main；当前 ewanqian 的 CLI 与连接器均为 pull=true、push=false、admin=false。
- 仓库所有者 API 类型为 User，不能把该个人账号误称为已经配置好的 GitHub Organization。
- 内容仓库：ewanqian/VIRTURA-Collective；PR #11 已合并，收录 TIMER 图片、研究条目和官网实施清单。
- 官网 README 当前定义：virtura.space 为业务官网，collective.virtura.space 为成员、艺术作品与公共项目档案，spaceport.virtura.space 为工具、研究、实验与长期档案。此次沿用当前定义，不直接用旧域名规划覆盖现有官网。
- 官网当前数据源 data/projects.csv；静态入口 index.html；项目页 work/*.html；品牌规范 brand-spec.md；图片在 VIRTURA_WEBSITE_ASSETS 与 assets。
- VIRTURA-Cellective 账号当前可见仓库仅 VIRTURA-Website 与 Virtura-web-；后者 contents API 返回空仓库。没有足够材料判定收藏品或商城实现质量。
- 本地已存在的 Collective checkout 有未提交修改；本次不覆盖、不提交这些文件。

参考：
- https://github.com/ewanqian/VIRTURA-Collective/pull/11
- https://github.com/ewanqian/VIRTURA-Collective/blob/main/docs/governance/TIMER-VOLUMETRIC-OFFICIAL-WEBSITE-HANDOFF.md
- https://github.com/VIRTURA-Cellective/VIRTURA-Website/blob/main/README.md
- https://github.com/VIRTURA-Cellective/VIRTURA-Website/blob/main/data/projects.csv

## 2. 个人经验迁移为团队机制

个人站的核心资产不是页面皮肤，而是作品身份、具体角色、版本关系、可用媒体与真实状态。团队迁移需多一层：逐人署名与可核实的团队责任。

| 个人站做法 | 团队迁移 | 官网落点 |
| --- | --- | --- |
| 一件作品有稳定入口 | object_id 跨站统一，页面各有视角 | 项目 CSV + 各站详情 |
| 个人参与说明 | 分开记录项目作者、团队承担范围、个人参与 | 详情页 Credits 与 Scope |
| 时间与媒介标签 | 展演版本、研究版本、商业交付分别建 edition_id | 原作与研究互链 |
| 图片与说明 | 每项媒体关联作者、权利、版本、用途 | 媒体清单，不依赖长期个人站热链 |
| 精选作品排序 | business / artwork / research 分区，编辑优先级独立 | Featured Work 与 Featured Research |
| 档案与文章 | 作品是对象，报道是关联出版物 | Collective / SpacePort / Newsroom |

入口按责任划分：
- 业务官网：客户能委托什么、团队实际交付什么、对应案例与联系。
- Collective：谁创作、谁合作、哪个版本、公开展演记录。
- SpacePort：研究、工具与长期档案；标清可访问性和研究进度。
- 个人站：个人实践与角色；链接团队原作，不取代团队署名。
- Newsroom：在维护者确认该入口当前运行情况后接入文章和媒体包，不在本次假定已部署。

建议导航：业务官网保留 Work / Capabilities / Studio / Contact；Research 为精选入口；页脚连 Collective 与 SpacePort。商城先不进入一级导航。

## 3. 数据与署名契约

保留现有 CSV，先增列再逐步迁移，避免直接重写所有静态页：

`object_id, edition_id, authorship_type, work_type, delivery_status, research_status, publication_status, editorial_priority, canonical_url, credits_url, media_manifest_url, updated_at`

- authorship_type：collective_work / collaboration / member_independent_work / external_collaboration。
- work_type：commercial_case / artwork / research / tool / edition。
- delivery_status：not_applicable / planned / delivered / archived。
- research_status：not_applicable / proposed / ongoing / completed。
- publication_status：internal / ready / published / withdrawn。
- editorial_priority：P1 / P2 / archive。高优先级不代表已经交付。
- 现有 status=confirmed 只作旧字段兼容；不能据此推导商业交付、权利或所有作者都已确认。
- canonical_url 是各对象稳定入口；related_views 连接个人视角、团队视角、研究视角。

每个版本的 Credits 必填：人名或主体、具体角色、对应版本、署名来源、公开使用状态。缺失的人名不由编辑推断；未完成记录的条目留在内部，不用模糊的“VIRTURA 全案”代替。

详情页顺序：作品名称 → 日期/地点/版本 → 作品介绍 → 团队职责 → 图片/视频 → Credits → 关联版本与媒体资料。

公开角色采用具体职责：Creative Direction / Visual Content / Sound / Realtime System / Spatial Capture / Technical Integration / On-site Delivery / Commissioned by / Presented by / Photography。根据材料选择，不把这些角色自动赋给所有参与者。

TIMER 示例：
- `timer-2024`：合作视听原作；保留现有原作所有作者与展演版本署名。
- `timer-spatial-study`：关联原作；研究与空间档案：Ewan Qian；research_status=ongoing，editorial_priority=P1。
- `timer-volumetric-sand`：规划对象；research_status=proposed；不得列为已展出的独立作品。
- 原作 Credits 从 Collective 原档案逐项迁移；本文件不编造缺失姓名。

TITAN 必须专项核对：当前 CSV 把它列为 confirmed 精选案例，但现有内容边界将 Kashiwa / TITAN 放在个人／外部合作。维护者需提供团队承担角色及来源；确认前，不将个人贡献扩大为团队独立交付。保留第三方与合作方署名。

## 4. TIMER 第一批官网修改

文件级实施：
1. 将已归档 assets/timer/featured-timer-visionpro.webp 复制到官网 VIRTURA_WEBSITE_ASSETS/03_UFO_CIRCULAR_SERIES/timer-visionpro-spatial-study.webp；文件 Git blob SHA 应为 b61688ac79f41eebd3856f3ec70a42a3033a3a74。
2. index.html 在现有精选作品后增加独立 Featured Research 区块，不插入现有使用 nth-child 控制尺寸的 .project-grid。单独使用 .research-grid / .research-card；图片保持完整比例，object-fit: contain。
3. 新建 work/timer-spatial-study.html；原 work/ufo-circular-series.html 保留原作与 Drop Flow，增加研究关联入口。
4. data/projects.csv 新增独立研究行：status=research；homepage=yes；detail_page=work/timer-spatial-study.html。新增列后该对象 research_status=ongoing、delivery_status=not_applicable、editorial_priority=P1。
5. 本轮不重排其他商业案例；避免把研究标签转译成商业完成状态。

公开文案：

**FEATURED RESEARCH / SPATIAL COMPUTING**

**控时者 · 空间观看研究 / TIMER — Spatial Study**

从 2024 年《控时者》合作视听作品出发，钱誉文继续探索体积化场景、三维档案与 Apple Vision Pro 空间观看。

Extending the collaborative 2024 audiovisual work TIMER, Ewan Qian explores volumetric scenes, three-dimensional archives and spatial viewing on Apple Vision Pro.

**Volumetric Scene · XR · Ongoing**

CTA：查看空间研究 / Explore Spatial Study。

署名：2024 合作原作：沿用原作完整 Credits；空间档案与 XR 研究：Ewan Qian。图片作者与使用说明从原资产记录核对后补入，不能用文件复制成功代替使用授权。

## 5. 仓库质量分级与视觉规范

不要按仓库数量同步上线。先建仓库清单，字段包含：owner、purpose、canonical_object、production_domain、maintainer、last_verified、build_status、rights_status、publication_tier、replacement_url。

- A / production：入口明确、作者与媒体可核实、导航和构建通过；可进入官网导航。
- B / research：可访问、状态准确、维护者明确；进入研究索引。
- C / archive：保留历史来源和时间；归档页可访问，退出首页精选。
- D / internal：缺权利、断链、责任不清或仅概念；仅内部保留，补齐后重新分级。

先用同一内容契约，再统一基础视觉。官网延续 brand-spec.md：深底 #0b0b0b、浅字 #f3f3ef、灰字 #a3a39d、清晰边界与项目媒体优先。Collective 可强化作品与作者；研究页强化版本、状态与观看入口。各站不必复制同一首页。

维护责任按工作流指定：内容编辑维护事实和版本；项目负责人确认职责与合作者；媒体维护人记录授权；站点维护人验证构建与发布。具体人选由团队分配，不自动指定他人。

## 6. 收藏品、版本与周边商城

当前无可核实商城仓库或在售目录，以下是准入要求，不是现状审计结论。

先建设 Editions 索引；只有真实可履约商品才开放购买：
- 商品与作品 object_id、作者、授权主体关联。
- 明确 SKU、实体/数字类型、规格、材料、尺寸、版本数及编号规则。
- 价格、币种、库存/预售、发货地区、预计时效、运费与退换规则齐全。
- 艺术家署名与收益分配在内部协议落实；页面明确卖方与联系入口。
- 使用真实产品照片；视觉概念图须标明，不以概念图暗示现货。
- 数字藏品注明交付内容与许可范围；不暗示升值或投资收益。
- 支付成功/失败、库存变化、订单通知与售后流程完成验证后再上线购买。

无商品时保留作品版本档案；不做空商城，不显示虚构库存、订单或销量。

## 7. 发布顺序与验收

PR A：本文件，确认维护者、职责、内容模型与实施顺序。
PR B：TIMER 本地媒体、独立研究卡与详情、CSV 对齐。按上述文件路径实施。
PR C：全站角色与 Credits 核对，优先 TIMER / Drop Flow / TITAN；现有商业案例逐项补具体 Scope。
PR D：仓库清单与归档分级；确认商品后再单独做 Editions / Shop。

每批验收：
- 375px / 840px / 1280px 页面无横向溢出，完整图片不被拉伸。
- 中英文切换、键盘焦点、标题层级、图片 alt、CTA 与站间链接有效。
- CSV 与 HTML 的标题、状态、目标路径一致；原始署名完整。
- 图片 SHA、媒体用途、原作与研究关系可追溯。
- 运行现有构建与链接检查；新增首页截图附在对应实现 PR。
- 合并后检查公开 URL 和部署结果，再将 publication_status 标记为 published；本方案合并不能自动标记内容已上线。

## 8. GitHub 与 ChatGPT 访问机制

- 仓库权限、PAT 权限、ChatGPT/Codex GitHub App 授权是三层，不能相互替代。
- 当前读取凭据不能自行给 ewanqian 提权；仓库所有者在 Settings → Collaborators 添加 ewanqian 并授予写权限，随后 ewanqian 接受邀请。
- 再由相应账号管理 GitHub App 可访问仓库；个人账号所有者与 Organization 的设置流程不同。
- 普通 ChatGPT GitHub 连接按官方说明提供读取；代码编辑与推送应使用支持该能力的 Codex 工作流。增加仓库写权限不保证所有 ChatGPT 页面都能提交。
- 本机现有 gh 凭据在系统钥匙串，可继续用于已授权操作。凭据不进入代码库、PR、长期文本记忆；已在聊天暴露的 PAT 应撤销换新。
- 如团队提供独立的管理员凭据，使用操作系统凭据存储与最小必要权限，不将其复制到项目文件。

官方参考：
- https://help.openai.com/en/articles/11145903-connecting-github-to-chatgpt
- https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository
