# 移动 AI 业务增强技术调研

**公开资料截至 2026 年 11 月 16 日，重点关注面向业务的网络、传输与推理协同。** 覆盖高通、爱立信、三星、诺基亚、LG Electronics、LG AI Research、OPPO、华为，以及中国移动、NTT DOCOMO、LG U+、T-Mobile、Verizon、AT&T、Deutsche Telekom。运营商案例以实际承担网络、试验或产品角色为纳入依据。

[论文与原始来源索引](SOURCES.md)收录 25 项论文与作者技术报告，其中 21 项有企业作者或企业出版记录，另有 23 组企业一手资料和 7 组规范与草案。下文将消费级手机、眼镜和联网机器人纳入移动 AI，把 RTC、XR、云游戏用作可迁移的时延与传输对照。

## 结论

1. **已有专门面向 AI 业务的技术和现网验证。** 中国移动、爱立信和 OPPO 在商用 5G SA 网为 AI 眼镜识物配置终端级切片和时延优先调度，报告空口平均 RTT 至少下降 30%。高通 X105 将 chatbot 等 OTT 流量检测列为产品功能，高通还演示了按链路和功耗在端与边缘之间移动视觉记忆任务。检测、应用选路和资源保障需要各自落实。 [AI 眼镜现网验证](https://www.ericsson.com/en/press-releases/2/2026/china-mobile-oppo-complete-5g-sa-network-slicing) · [X105](https://www.qualcomm.com/modems/products/qualcomm-x105-5g-modem-rf) · [MWC 原型](https://www.qualcomm.com/news/onq/2026/02/mwc-barcelona-2026-prototyping-ai-native-6g-services)
2. **紧时延要按瓶颈选机制。** 弱覆盖下，保障上行比单纯增加下行峰值更贴近多模态感知；拥塞下，L4S 和发送端码率适配处理队列；移动时，L1/L2 触发切换和多接入处理链路中断；首个有效响应还受预处理、GPU 排队和推理影响。AT&T 联试正文的切换中断降幅最多 25%，适用范围是该试验与 L3 切换的对照。 [交互 GenAI 上行](https://www.ericsson.com/en/reports-and-papers/ericsson-technology-review/articles/network-performance-impact-of-generative-ai-services) · [移动性联试](https://www.ericsson.com/en/press-releases/6/2026/ericsson-att-mediatek) · [推理分层台架](https://arxiv.org/html/2602.23722v1)
3. **有证据的端到端联合打通涉及网络和计算两组动作。** NTT DOCOMO 的 ISAP 用一次启动请求联动计算实例、专用路径和连接控制。后续 INC Edge 将视频预处理与远端推理拆开，再通过优先级控制的 5G 核心网与 IOWN 光网连接远程 GPU。三星 SPINN 提供另一个落点，在设备与云间实时调整切分点和提前退出策略。 [ISAP 联试](https://www.docomo.ne.jp/english/info/media_center/pr/2025/0303_02.html) · [INC Edge 联试](https://www.docomo.ne.jp/english/info/media_center/pr/2026/0302_02.html) · [SPINN](https://arxiv.org/pdf/2008.06402)
4. **传输层有双向适配。** 网络向端点提供 ECN 拥塞信号或多接入策略，端点据此限速、改码率或改路径。实时媒体可用 RTP/SCReAM 或 QUIC Datagram 加纠错，文本响应和 agent 工具控制通常需要可靠有序语义。移动 AI 公开资料仍缺少同一模型、同一负载下 TCP、QUIC、RTC 三路对照，现有结果适合按载荷和失效条件采用。 [L4S 架构](https://www.rfc-editor.org/rfc/rfc9330) · [SCReAM](https://datatracker.ietf.org/doc/html/rfc8298) · [NTT MPQUIC 试验](https://arxiv.org/html/2607.14482v1)

## 1. 哪些技术明确服务移动 AI

| 公司 | 可核对的技术 | 结果和成熟度 | 来源 |
|---|---|---|---|
| 高通 | X105 的 OTT 流量检测，配套上行切换与补充上行能力。另演示按无线链路和能耗条件将视觉记忆的生成或检索放在端或网络边缘 | 产品规格与展会原型。公开资料没有给检测准确率、触发何种承载策略或业务 TTFT 对照 | [I01](https://www.qualcomm.com/modems/products/qualcomm-x105-5g-modem-rf) · [I02](https://www.qualcomm.com/news/onq/2026/02/mwc-barcelona-2026-prototyping-ai-native-6g-services) |
| 爱立信、中国移动、OPPO | 山东德州 5G SA 商用网验证。AI 眼镜端侧采图上传、云 Agent 返回语音，采用终端级切片及低时延优先调度。另有 OPPO 检测直播卡顿并经用户选择切换应用切片，应用级选路基于 URSP | 眼镜空口平均 RTT 至少降 30%。应用级直播切片与眼镜终端级切片是同一新闻稿中的不同验证，直播结果不能归入 AI 眼镜 | [I04](https://www.ericsson.com/en/press-releases/2/2026/china-mobile-oppo-complete-5g-sa-network-slicing) |
| 诺基亚 Bell Labs、中国移动研究院 | 为 agent 间视觉问答学习紧凑的机器语言 Token embedding，联合表示和信道编码，映射到 OFDM 资源 | CLEVR、GQA 仿真中，所示 0 和 15 dB 条件约 5 个学习 Token 即接近准确率平台。需要任务、收发端模型和信道条件匹配 | [P10](https://arxiv.org/html/2507.21454v1) |
| 三星 | MobileQuant 用权重/激活联合优化压低移动 LLM 计算开销。SAC-JSCM 联合训练语音压缩、信道编码和调制，优化恶劣链路下音频质量 | 前者论文相对所测端侧量化方案，时延和能耗降 20%–50%。后者使用链路仿真和蓝牙频段 USRP 原型，证据到物理链路 | [P04](https://aclanthology.org/2024.findings-emnlp.570/) · [I20](https://research.samsung.com/blog/6G-AI-ML-for-Physical-layer-Part-II-Audio-Traffic-with-JSCM) |
| OPPO、MediaTek | AndesVL 的移动推理由量化感知 LoRA、KV 缓存淘汰、稀疏化、硬件感知压缩与推测解码组成。另演示 Omni 手机上的实时语音/视频/文本交互，云处理产品路线含 Private Computing Cloud | AndesVL-4B 在天玑 9500 上报告对论文 PTQ 基线峰值解码加速 6.7×，内存最多降 30.9%。这些是终端解码或设备功能证据 | [P06](https://arxiv.org/html/2510.11496v2) · [I18](https://www.oppo.com/en/newsroom/press/oppo-and-mediatek-mwc2026/) · [云处理](https://www.oppo.com/en/newsroom/press/oppo-ai-tech-summit-mwc-25/) |
| LG Electronics、LG AI Research、LG U+ | LG Electronics 的 CLOiD 用 VLM 识别场景、VLA 执行动作。LG AI Research 的 EXAONE 4.0 提供面向设备侧的 1.2B 模型。运营商 LG U+ 在商用 5G 与延世大学 GPU AI-RAN 测试网间验证终端选网与连续连接 | 三项是不同主体的独立证据。LG U+ 稿件未给中断分位数或具体传输协议。CLOiD 和 EXAONE 资料未给蜂窝对照 | [I17](https://www.lg.com/global/newsroom/news/home-appliance-solution/lg-electronics-presents-lg-cloid-home-robot-to-demonstrate-zero-labor-home-at-ces-2026/) · [P09](https://www.lgresearch.ai/data/cdn/upload/EXAONE_4_0.pdf) · [I16](https://news.lguplus.com/22640) |
| 华为、中国移动 | 华为提出 Token block pipe 为有突发和时限的数据块提供分级传输，并联合探索 A2A-T 意图接口。湖北移动另验证 UCM，按层管理推理 KV 缓存 | block pipe/A2A-T 为公开技术方向。UCM 现网验证报告 MiniMax M2.5 的 TTFT 改善 26%–62%，改动点位于推理基础设施 | [I22](https://www.huawei.com/en/huaweitech/publication/202601/agent-oriented-mobile-networks-in-agentverse-era) · [I21](https://www.huawei.com/en/news/2026/6/mwcs-cmcc-token-economy) |
| T-Mobile、Nokia、NVIDIA / Verizon | 前者试点 Nokia anyRAN 和运营商分布式 GPU 基础设施，在网络边缘运行视频检索、摘要和视觉 AI agent。Verizon 单独展示穿戴设备接入边缘的 AI Sports Companion | 试点/业务原型。所引材料缺少可归因的移动端任务 P95 | [I13](https://www.t-mobile.com/news/network/nvidia-t-mobile-and-partners-integrate-physical-ai-applications-on-ai-ran-ready-infrastructure) · [I23](https://www.verizon.com/about/news/verizon-leads-6g-momentum-expansion-isac-trials) |

### AI 流量需要按业务建模

诺基亚在芬兰用商用手机实测 50 余应用和服务。白皮书给出的聊天、文档和图像生成类别中位流量比大致为下行:上行 3:1–5:1，个别图像/视频输入应用则明显上行占优。测试为观察端点能力移除了无线带宽瓶颈，并以额外时延做敏感性试验。高通 P01 基于 3GPP SA4 研究测试台，给出同一智能体本机控制口与广域出口分别为 0.007 和 0.50 的上/下行字节比。比值需要附测点、交互方式、输入模态、会话阶段及模型位置。 [Nokia 白皮书 pp. 3–8](https://www.nokia.com/asset/f/215147/) · [Qualcomm 论文 §4–6](https://arxiv.org/html/2611.07250v1)

从机制看，文本流式回复通常字节少，体验受首响应与长停顿支配。语音/视频助手的上行更连续，弱覆盖时会积压或强制降质。眼镜抓拍识物是小请求与较大的上行突发叠加。自主 agent 可有多轮工具调用，也可能容忍后台调度。优化单位应按业务选择会话、请求、帧、任务或数据块。3GPP 文档的 PDU Set 是一组应用数据单元关联包的 QoS 抽象。模型词元、应用发出的流式事件、PDU Set 和 IP 包之间没有固定的一一映射。 [SA4 测量](https://arxiv.org/html/2611.07250v1) · [3GPP TS 23.501](https://www.etsi.org/deliver/etsi_ts/123500_123599/123501/18.11.00_60/ts_123501v181100p.pdf) · [Token 流识别个人草案](https://datatracker.ietf.org/doc/html/draft-wang-token-aware-usecases-requirements-00)

## 2. 紧时延增强技术

| 瓶颈 | 技术动作 | 公开证据与适用边界 |
|---|---|---|
| 弱覆盖，上行突发排队 | 按上行质量选小区或低频 FDD 承载；载波聚合、上行 MIMO、切换上行 | 爱立信列出三处部署忙时小区边缘上行 0.8、0.3、0.25 Mbps，用 1.5 Mbps 作交互 GenAI 的规划参考，并建模上行感知多层小区选择。Verizon + Ericsson + Qualcomm 的双 TDD C-band 聚合及 UL MIMO 实测峰值 480 Mbps，属于另一试验。 [I06](https://www.ericsson.com/en/reports-and-papers/ericsson-technology-review/articles/network-performance-impact-of-generative-ai-services) · [I07](https://www.ericsson.com/en/press-releases/6/2025/verizon-ericsson-qualcomm) |
| 网络有负载时，关键包与普通流争用 | 切片保留或隔离资源，业务映射到 QoS 流，RAN 优先调度。对支持的实时媒体用 PDU Set 表示帧级预算和丢失目标 | 中国移动德州眼镜现网是直接业务证据。TS 23.501 描述标准能力；上线需要端、核心网与 RAN 一致配置，并检查高负载时的资源兑现。 [I04](https://www.ericsson.com/en/press-releases/2/2026/china-mobile-oppo-complete-5g-sa-network-slicing) · [S01](https://www.etsi.org/deliver/etsi_ts/123500_123599/123501/18.11.00_60/ts_123501v181100p.pdf) |
| 拥塞引发的队列长尾 | L4S 的浅队列标记、发送端可扩展拥塞控制和编码器联动 | Deutsche Telekom 5G+ Ultra 已公开支持 FaceTime，采用切片与 L4S；公开 20% 时延改善取自 GeForce NOW 测试。华为合作 RTC 论文用 L4S-GCC 对比 GCC，报告带宽利用率提升 11.4%–31.4%。 [I14](https://www.telekom.com/en/newsroom/latest-updates/media-information/2026/4/5g-ultra-for-video-calls) · [P17 §IV](https://arxiv.org/html/2407.20852v1) |
| 小区或接入网切换时停顿 | L1/L2 触发移动性用于蜂窝小区切换；Wi-Fi/蜂窝之间用连接迁移、ATSSS 或应用重连恢复 | AT&T 联试正文为相对 L3 切换最多降低 25% 数据中断。三星 CQUIC 和 LG U+ 分别提供跨接入路径和跨网的试验证据。三项不能合并成同一移动连续性结果。 [I08](https://www.ericsson.com/en/press-releases/6/2026/ericsson-att-mediatek) · [P05](https://research.samsung.com/research-papers/CQUIC-Cross-Layer-QUIC-for-Next-Generation-Mobile-Networks) · [I16](https://news.lguplus.com/22640) |
| 远端路径、GPU 排队和首轮推理 | 近端 UPF 分流，按负载选推理节点，按 deadline 准入，缓存对齐、模型分层、端侧早退 | NTT DOCOMO 给出通算联试；SUTD 的 5G SA 台架显示 0.5 秒预算对量化变体与路径长尾敏感；中国移动湖北用 KV 缓存体系缩短所测模型 TTFT。 [I11](https://www.docomo.ne.jp/english/info/media_center/pr/2026/0302_02.html) · [R02](https://arxiv.org/html/2602.23722v1) · [I21](https://www.huawei.com/en/news/2026/6/mwcs-cmcc-token-economy) |

## 3. 端、网、算怎么联合打通

**方案一：应用启动时同步建立路径和算力。** NTT / DOCOMO / Nokia 的 2025 ISAP PoC 由 Nokia Network as Code 承接计算服务启动请求，向符合 3GPP 规范的核心网下发通往计算平台的路由，同时启动配有 GPU/DPU 的容器功能。视频分析准确率在该对照里由 57% 到 90%，企业将变化归因于传输时延和抖动改善。没有公布每阶段毫秒值或绝对 SLA。2026 联试新增 INC Edge 和 IOWN APN，将预处理后数据交给远程 GPU，并在 AWS 上的商用 5G 核心网做优先控制。其结论为通信加视频分析满足设定场景的时限，通稿没有给绝对时延。 [2025 PoC](https://www.docomo.ne.jp/english/info/media_center/pr/2025/0303_02.html) · [2026 联试](https://www.docomo.ne.jp/english/info/media_center/pr/2026/0302_02.html) · [Inclusive Core 白皮书](https://www.rd.ntt/e/ns/inclusivecore/whitepaper_ver1.html)

**方案二：按运行状态在端、边缘与云间选模型或切分点。** 三星 SPINN 在 CNN 上把早退阈值与切分层一起调，考虑设备负载与无线链路。华为 Auto-Split 则将切分点和量化位宽作为部署搜索变量。高通 MWC 用视觉记忆展示端或边缘放置。诺基亚的 IDN 研究进一步在接入、边缘、区域、云节点间按近期请求动态放置模型。当中间激活比原始压缩图像更大、边缘未预热或网络切换频繁时，需要比较端推理、上传媒体和上传激活三种成本；任务准确率和能耗也进入决策。 [SPINN](https://arxiv.org/pdf/2008.06402) · [Auto-Split](https://arxiv.org/pdf/2108.13041) · [高通演示](https://www.qualcomm.com/news/onq/2026/02/mwc-barcelona-2026-prototyping-ai-native-6g-services) · [Nokia IDN](https://www.nokia.com/bell-labs/publications-and-media/publications/towards-inference-delivery-networks-distributing-machine-learning-with-optimality-guarantees/)

**方案三：把应用截止时间映射成网络和计算动作。** 爱立信白皮书列出 URSP、切片、优先调度和 QoD。URSP 在可匹配规则下决定应用流量进入哪个 PDU 会话，CAMARA QoD 可为指定流申请有时限的 QoS 会话。用于 AI 时，可以按业务把关键请求、过时即丢的媒体、后台同步拆到不同通道，同时让计算侧按请求截止时间选模型和实例。后半部分是由公开机制推导的集成设计。QoD 对网络承载生效，GPU 队列和模型 SLA 需要服务方另行控制。 [Ericsson AI 体验白皮书](https://www.ericsson.com/en/reports-and-papers/white-papers/the-network-for-ai-experiences) · [CAMARA QoD](https://camaraproject.org/quality-on-demand/) · [固定策略通算实测](https://arxiv.org/html/2602.23722v1)

## 4. QUIC、RTC、TCP 怎样与移动网络互相适配

| 路线 | 信号与动作 | AI 业务适用性和约束 |
|---|---|---|
| **网络→端，L4S/ECN** | 支持 L4S 的端点发送 ECT(1)，瓶颈队列遇拥塞设 CE，接收端反馈后发送端细粒度降速。DualQ 是一种在经典流共存时维持浅队列的实现。QUIC 可反馈 ECN 计数，是否具备 L4S 取决于拥塞控制及路径 | 适合网络拥塞导致实时语音/视频、上传帧或流式输出排队的情况。需要端点响应与瓶颈支持。对无线覆盖损失、云排队、模型算时的作用要单独测量。 [S03](https://www.rfc-editor.org/rfc/rfc9330) · [RFC 9000](https://www.rfc-editor.org/rfc/rfc9000) · [I15](https://www.ericsson.com/en/differentiated-connectivity/technology/l4s) |
| **端适配网络，RTC/SCReAM 或 GCC** | WebRTC/RTC 实时媒体通常用 RTP/RTCP 形成反馈环。SCReAM 和 L4S-GCC 按交付/拥塞反馈降低发送速率。Ericsson 机器人论文将反馈进一步送给 Draco，动态改变点云压缩率 | 适合语音、视频、点云帧等软实时输入。网络与编码器共同决定质量。过低码率可能降低模型识别率。针对同一任务应一并测捕获到可用帧的尾时延、丢帧和任务准确率。 [P15](https://onlinelibrary.wiley.com/doi/10.1155/2018/3142496) · [P16](https://arxiv.org/html/2511.15677) · [P17](https://arxiv.org/html/2407.20852v1) · [R04](https://arxiv.org/html/2607.23767v1) |
| **端适配网络，跨层 QUIC 迁移** | 三星 CQUIC 将预测 SINR、可用吞吐、RTT 送入路径评分，提前从劣化接入迁移。IETF QUIC 单路径连接迁移可跨地址保留连接状态；实际迁移依赖路径验证和实现 | 适合眼镜/手机在 Wi-Fi 与蜂窝间切换时持续交互。Galaxy S10 论文报告对早期 GQUIC 吞吐高 20%。需要在当前 QUIC 栈复测迁移停顿、请求重试和能耗。 [P05](https://research.samsung.com/research-papers/CQUIC-Cross-Layer-QUIC-for-Next-Generation-Mobile-Networks) · [S04](https://www.rfc-editor.org/rfc/rfc9000) |
| **网络+端，ATSSS MPQUIC** | 5G 核心网与 UE 共享接入策略，使用多接入 PDU 会话，MPQUIC steering 功能可在 Wi-Fi/NR 链路间选择、切换或分流。诺基亚论文解释 UDP 代理 | 适用于可用双接入时的业务连续性。选路需兼顾交付期限、乱序、蜂窝费用和能耗。3GPP 代理式用户面与应用直接开通原生多路径 QUIC 具有不同端点和配置。 [P12](https://www.nokia.com/bell-labs/publications-and-media/publications/multipath-quic-for-access-traffic-steering-switching-and-splitting-in-5g-advanced/) · [S02](https://www.etsi.org/deliver/etsi_ts/124100_124199/124193/18.06.00_60/ts_124193v180600p.pdf) · [RFC 10069](https://datatracker.ietf.org/doc/rfc10069/) |
| **端适配网络，QUIC Datagram + 自适应 FEC** | NTT 复用 QUIC 各路径丢失探测，按平滑丢失率计算 Reed–Solomon 冗余。报文逾期后的收益需由应用定义 | 东京武藏野市固定路线，三条商用 LTE/5G 路径、4 Mbps UDP 上行，汇总七圈后相对无 FEC，P95 单向时延 281.2→142.3 ms，丢包 1.7%→0.8%。适合有时效、可接受少量丢失的媒体或传感数据，需计算 FEC 带宽及成块等待。 [P18](https://arxiv.org/html/2607.14482v1) · [RFC 9221](https://www.rfc-editor.org/rfc/rfc9221) |
| **TCP，保留可靠服务语义或缩短反馈环** | 文本流式返回或工具 RPC 可继续使用 HTTP/2/TCP。TCP 连接上的恢复可能推迟后续字节；HTTP/3/QUIC 流将有序交付阻塞隔离到相应流，连接仍共享拥塞预算。诺基亚研究在边缘切片串接 Native TCP，也在 Wi-Fi/5G mmWave 测过基于 TCP 的计算卸载 | 适合控制、文本和要求完整交付的负载。可测试 TCP+连接复用/边缘接入与 QUIC 的对照，观察首字、重传停顿和切换。若部署代理，应用连接端点、TLS 与重试语义取决于代理架构。 [P13](https://www.nokia.com/bell-labs/publications-and-media/publications/5g-access-slices-with-native-tcp/) · [P14](https://www.nokia.com/bell-labs/publications-and-media/publications/tcp-based-distributed-offloading-architecture-for-the-future-of-untethered-immersive-experiences-in-wireless-networks/) · [RFC 9000](https://www.rfc-editor.org/rfc/rfc9000) |

### 混合业务如何选传输

- **文本聊天或 agent 控制。** 同一 SSE/RPC 语义对照热连接的 HTTP/2 与 HTTP/3。用短请求、并发工具调用、连接冷启动和跨接入移动分别报告结果。单请求已有热连接时，握手项可接近零，此时主要观察队列与首轮推理。
- **实时视频或点云问答。** 输入媒体可走 RTC 或 QUIC Datagram，携带时间戳与应用帧截止时刻。指令、任务取消与最终结果留在可靠通道。对照媒体自适应、部分丢失和 FEC 时，观察可用画面和回答正确率随码率变化。
- **弱网辅助与连续性。** 先区分同一蜂窝接入内切换和 Wi-Fi/蜂窝跨接入。前者测 RAN 中断，后者测迁移/ATSSS 恢复及应用会话连续性。多路径对冲在拥塞或共享瓶颈下可能增加负载，移除路径前应观测已在途的数据及 deadline。

上述为基于规范和论文机制的设计判断。资料没有提供可直接复用的三协议 AI 基准。 [QUIC](https://www.rfc-editor.org/rfc/rfc9000) · [SCReAM](https://datatracker.ietf.org/doc/html/rfc8298) · [NTT MPQUIC](https://arxiv.org/html/2607.14482v1)

## 5. 如何验证收益是否到达用户

建议用同一终端、模型版本、输入序列和输出质量门槛建立四组对照。以 AI 眼镜问答和实时语音/视频助手为共用场景，记录采集→预处理→上行排队与交付→计算排队→prefill/首输出→语音播放或动作完成。对 agent 工具链再记录每次工具请求及重试。TTFT 使用客户端提交请求至收到首个模型 Token 的时间，首个有效响应另测其实际显示或播放时间。分阶段时间可能重叠，端到端值由统一时钟或明确定义的客户端观测计算。

| 对照组 | 控制变量 | 核心指标 |
|---|---|---|
| 网络策略 | 同一 AI 服务与协议，普通承载对照应用策略/切片；逐步提高背景负载并覆盖小区边缘 | 空口 RTT、上行交付率、应用 P50/P95/P99、同 deadline 的任务完成率 |
| 推理位置 | 同一模型和相近计算配置比较本地、同城边缘和远端；分开统计模型热冷、GPU 并发及连接热冷 | 动作或请求发起至首个有效响应、GPU 队列、TTFT、任务准确率、热稳态终端功耗 |
| 协议和拥塞控制 | 同业务语义比较 H2/TCP 与 H3/QUIC，实时媒体另比较 RTC 和 QUIC Datagram。做 L4S 和 FEC 开关对照 | P95/P99 长停顿、码率/损失/重传、切换中断、媒体可用率与任务准确率 |
| 联合控制 | 保留各单项控制组，再比较应用触发 QoS + 本地分流 + 按 deadline 选模型 | 截止期达标率与单位资源完成任务数；检查失败后的本地降级和会话恢复 |

公开证据里，空口收益、媒体收益、终端模型加速和服务端 TTFT 分别来自不同负载和测量点。验证时用同一业务流水线补齐这些测点，才能判断某项技术改善的是哪一段，以及改善是否传到用户。 [SA4 实验方法](https://arxiv.org/html/2611.07250v1) · [SLA 分层试验](https://arxiv.org/html/2602.23722v1) · [交互流量测试法](https://www.nokia.com/asset/f/215147/)
