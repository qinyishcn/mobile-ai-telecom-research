# 原始来源与论文证据

检索截至 2026-11-16（北京时间）。先按企业与运营商检索官网，再以 AI traffic、split inference、L4S、SCReAM、cross-layer QUIC、ATSSS、TCP offload 等组合词检索论文、标准和互联网草案。以作者单位或企业研究部门的出版记录核对归属。本文使用公开的一手材料，记录了可核实的机制、结果和适用条件。预印本、技术报告、标准论文和同行评议研究分别标注。仅公开题录或摘要的条目将结论限定在公开描述内。

## 论文与作者技术报告

| ID | 作者单位或企业出处 | 发表形态与原文 | 机制、结果与证据边界 |
|---|---|---|---|
| P01 | Qualcomm Technologies | 2026 预印本，[Characterizing AI Service Traffic for 6G](https://arxiv.org/html/2611.07250v1) | §3–6 开源测试平台将应用记录与抓包对齐，覆盖云服务和自托管模型。SA4 TR 26.870 草案 v0.6.2 中测得的 UL/DL 字节比例跨逾六个数量级。个人助理本机控制接口 0.007，广域出口 0.50。带抖动或相关丢包的配置需校验仿真器实际行为。 |
| P02 | Qualcomm AI Research | 2022 作者技术报告，[Neural Network Quantization with AIMET](https://arxiv.org/pdf/2201.08442) | §3–5 提供校准、训练后量化、量化感知训练与量化参数导出。支撑终端推理压缩工程。该文的量化结果局限于所测模型。 |
| P03 | Samsung AI Center Cambridge、剑桥大学 | ACM MobiCom 2020，[SPINN](https://arxiv.org/pdf/2008.06402) | §3–5 运行时联合选择 CNN 切分层与提前退出点。本地保留渐进推理能力，连接恶化时仍可产出结果。变动网络中相对论文协同推理基线吞吐最高 2×。测试任务为 CNN。 |
| P04 | Samsung AI Center Cambridge 等 | Findings of EMNLP 2024，[MobileQuant](https://aclanthology.org/2024.findings-emnlp.570/) | 联合优化权重等价变换与激活量化范围，使更多算子使用移动端整数运算。相对所测端侧量化方案，作者报告时延和能耗降低 20%–50%。参看[作者全文 §3–5](https://arxiv.org/html/2408.13933)。 |
| P05 | Samsung Research 收录 | IEEE WCNC 2020，[CQUIC](https://research.samsung.com/research-papers/CQUIC-Cross-Layer-QUIC-for-Next-Generation-Mobile-Networks) | Galaxy S10 现网试验。预计 SINR、QUIC 吞吐和 RTT 生成路径评分，主动在 Wi-Fi 和蜂窝间迁移。论文报告吞吐相对 GQUIC / TCP / MPTCP Backup 高 20% / 36% / 17%。年代与基线限定了对当前 IETF QUIC 的外推。 |
| P06 | OPPO AI Center | 2025 预印本，[AndesVL](https://arxiv.org/html/2510.11496v2) | §4、§6 量化感知 LoRA、KV 缓存淘汰、稀疏化、硬件感知压缩及推测解码。天玑 9500 上 AndesVL-4B 的组合方案相对论文 PTQ 基线峰值解码加速 6.7×，内存最多降低 30.9%。 |
| P07 | OPPO AI Center | 2025 预印本，[DaMo](https://arxiv.org/html/2510.19336) | 训练预测器优化多任务微调的数据配比。作者报告 PhoneAgentBench 得分相对替代方法提高 3.38%。指标是手机智能体模型能力。 |
| P08 | OPPO AI Center | 作者技术报告，[X-OmniClaw](https://eggplant95.github.io/X-OmniClaw-Page/) | 多模态理解与手机操作统一建模。作为移动应用工作负载参考，其页面未提供无线链路对照。 |
| P09 | LG AI Research | 2025 作者技术报告，[EXAONE 4.0](https://www.lgresearch.ai/data/cdn/upload/EXAONE_4_0.pdf) | 1.2B 版本面向端侧部署，模型支持推理与非推理模式。报告主要给模型基准，缺少移动网络和手机热稳态对照。 |
| P10 | Nokia Bell Labs、中国移动研究院 | 2025 预印本，[Transmission With Machine Language Tokens](https://arxiv.org/html/2507.21454v1) | §III–IV 用任务相关嵌入和联合 Token/信道编码，结合 OFDM 上的模拟传输。在 CLEVR/GQA 设置及所示 0、15 dB 下，约 5 个学习 Token 已接近该实验的准确率平台。任务和收发模型需匹配。 |
| P11 | Nokia Bell Labs 研究者参与 | 论文，[Towards Inference Delivery Networks](https://www.nokia.com/bell-labs/publications-and-media/publications/towards-inference-delivery-networks-distributing-machine-learning-with-optimality-guarantees/) | 按近期请求和邻节点状态调整接入、边缘、区域、云侧模型放置，优化时延和准确率权衡。论文提供理论保证及启发式对照。 |
| P12 | Nokia Bell Labs 出版记录 | IEEE Communications Standards Magazine 2023，[Multipath QUIC for ATSSS](https://www.nokia.com/bell-labs/publications-and-media/publications/multipath-quic-for-access-traffic-steering-switching-and-splitting-in-5g-advanced/) | 面向 Rel-18 的 ATSSS 用户面与代理机制。讲解如何利用多路径 QUIC 在 Wi-Fi / NR 分流并代理 UDP。公开摘要未给出 AI 业务对照。 |
| P13 | Nokia Bell Labs 出版记录 | 2016 论文，[5G Access Slices with Native TCP](https://www.nokia.com/bell-labs/publications-and-media/publications/5g-access-slices-with-native-tcp/) | 提案将 TCP 功能放到靠近无线链路的边缘，通过接入切片按需串接本地控制功能。该方案是早期研究设计。 |
| P14 | Nokia Bell Labs 出版记录 | 2022 论文，[TCP-Based Distributed Offloading](https://www.nokia.com/bell-labs/publications-and-media/publications/tcp-based-distributed-offloading-architecture-for-the-future-of-untethered-immersive-experiences-in-wireless-networks/) | 用 TCP 卸载沉浸媒体任务，在 Wi-Fi、5G 毫米波和 RAN 仿真器测试，并考察共享资源时用户优先级。属于邻近场景。 |
| P15 | Ericsson AB 等 | Wireless Communications and Mobile Computing 2018，[Adaptive Video with SCReAM over LTE](https://onlinelibrary.wiley.com/doi/10.1155/2018/3142496) | LTE 远程机械操控。SCReAM 对多路视频分配优先级并控制编码码率，提供实地测试。关键指标是传输和遥操作体验。 |
| P16 | Ericsson Research、Luleå University of Technology | 2025 预印本，[Real-time Point Cloud Data Transmission via L4S](https://arxiv.org/html/2511.15677) | L4S/SCReAM v2 感知拥塞，动态控制 Draco 点云压缩，在机器人任务精度约束下调节码率。评价对象为 5G 边缘辅助机器人输入。 |
| P17 | 上海大学、Huawei Wireless X-Labs | IEEE Network，[Optimizing 5G-Advanced Networks for Time-Critical Applications](https://ieeexplore.ieee.org/document/10648595/) | RTC 原型以 L4S-GCC 对比 GCC，原文报告卡顿率降低 1.51%–2.80%、带宽利用率提升 11.4%–31.4%。需要按[作者全文 §IV](https://arxiv.org/html/2407.20852v1)的配置理解百分比。 |
| P18 | NTT Network Service Systems Labs | 2026 预印本，[Scheduler-Agnostic Adaptive-FEC for MPQUIC](https://arxiv.org/html/2607.14482v1) | 车辆移动中使用三条商用 LTE/5G 路径。4 Mbps 主设置下相对无 FEC，单向均值 103.0→70.8 ms，P95 281.2→142.3 ms，丢包 1.7%→0.8%，平均编码率 0.94。限 QUIC Datagram 及所测路线与负载。 |
| P19 | Huawei Cloud | 2021 作者预印本，[Auto-Split](https://arxiv.org/pdf/2108.13041) | 搜索模型切分点和边端量化位宽，在带宽、内存及准确率条件下选择部署方案，并验证协同推理原型。相关性主要是 DNN 图像推理方法。 |
| P20 | 中国移动研究院 | FITEE，[AI-agent communication network for 6G](https://jzus.zju.edu.cn/iparticle.php?doi=10.1631%2FFITEE.2500582) | 智能体通信网设计，包含多模态流传输和智能体网络架构。属于方案型研究，缺少同条件跨网业务验证。 |
| P21 | IBM Research | 2026 作者稿，[Simulate Before You Scale](https://arxiv.org/html/2610.14255) | BLIS 仿真建模后实机验证服务端策略。36 个实验端到端预测中位误差 6.7%。所测负载下准入策略最多降低关键等级 P90 TTFT 82%。这是服务端排队参考。 |
| R01 | SUTD | 2025 预印本，[Adaptive AI Model Partitioning over 5G Networks](https://arxiv.org/html/2509.01906v1) | NVIDIA Aerial 试验台预测可用吞吐，据此切分隐私敏感图像推理。作者报告相对基线至多改善 65% E2E 时延。作者单位为高校。 |
| R02 | SUTD | IEEE INFOCOM Workshops 2026，[SLA-Aware Distributed LLM Inference Across Device-RAN-Cloud](https://arxiv.org/html/2602.23722v1) | 5G SA 台架，dUPF 本地分流、固定 GPU MIG 隔离、设备/RAN 边缘/云分层。所测 WAN 云侧 0.5 秒达标率最高 32.9%，1 秒为 100%。三层使用不同硬件，该结果为部署组合对照。 |
| R03 | 普林斯顿大学 | ACM SIGCOMM 2020，[PBE-CC](https://arxiv.org/html/2002.03475) | 终端依据蜂窝物理层带宽估计调节发送速率。相对论文 BBR 基线平均吞吐高 6.3%，P95 时延约为其 1/1.8。需要终端取得网络测量，文中平台为原型。 |
| R04 | UC Santa Barbara | 2026 预印本，[Fake It No More](https://arxiv.org/html/2607.23767v1) | SCReAM 加实际视频编码器，移动轨迹仿真基线中每次实验 P95 队列时延的中位数降 35%，发送吞吐降 42%。说明队列时延、码率和感知质量需一起评估。 |

P01–P21 中的 P07、P08 和 P21 是业务负载或服务端侧参考。R01–R04 为学术对照，使用企业软件、芯片、或跨层算法并不会改变论文归属。

## 官网、新闻、白皮书

| ID | 原始来源 | 取证内容 |
|---|---|---|
| I01 | [Qualcomm X105 产品页](https://www.qualcomm.com/modems/products/qualcomm-x105-5g-modem-rf) | 支持 chatbot 等 OTT 流量检测；列出上行切换、补充上行等调制解调器能力。 |
| I02 | [Qualcomm MWC 2026 原型](https://www.qualcomm.com/news/onq/2026/02/mwc-barcelona-2026-prototyping-ai-native-6g-services) | 联通 AI/AR 设备、端与网络边缘的分布式推理演示。 |
| I03 | [Qualcomm 6G Foundry 白皮书](https://www.qualcomm.com/content/dam/qcomm-martech/dm-assets/documents/6G-Foundry-10-Rewriting-the-mobile-playbook.pdf) | 端侧流分类、上下文感知通信及端边云分工的研究路线。 |
| I04 | [Ericsson / 中国移动 / OPPO 德州现网试验](https://www.ericsson.com/en/press-releases/2/2026/china-mobile-oppo-complete-5g-sa-network-slicing) | 5G SA 商用网测试应用级和终端级切片。眼镜识物业务的空口平均 RTT 至少降 30%。 |
| I05 | [Ericsson《The Network for AI Experiences》](https://www.ericsson.com/en/reports-and-papers/white-papers/the-network-for-ai-experiences) | 针对 AI 会话提供切片、URSP、QoD、先进调度及边缘能力。 |
| I06 | [Ericsson Technology Review：生成式 AI 上行](https://www.ericsson.com/en/reports-and-papers/ericsson-technology-review/articles/network-performance-impact-of-generative-ai-services) | 三处现网忙时基线和面向交互 GenAI 的上行改造模型，含上行感知小区选择。 |
| I07 | [Verizon / Ericsson / Qualcomm 上行试验](https://www.ericsson.com/en/press-releases/6/2025/verizon-ericsson-qualcomm) | 双 TDD C-band 载波聚合加上行 MIMO，试验峰值 480 Mbps。 |
| I08 | [AT&T / Ericsson / MediaTek 低时延移动性](https://www.ericsson.com/en/press-releases/6/2026/ericsson-att-mediatek) | 实地 L1/L2 触发移动性，正文报告小区切换的数据中断相对 L3 最多降低 25%。 |
| I09 | [Nokia 白皮书《The impact of AI-apps on mobile networks》](https://www.nokia.com/asset/f/215147/) | 测 50 余 AI 应用和服务、20 余场景、商业 iOS/Android 终端。见 pp. 3–8 测试方法与延迟注入。 |
| I10 | [NTT / DOCOMO ISAP 联合控制验证](https://www.docomo.ne.jp/english/info/media_center/pr/2025/0303_02.html) | 服务属性经 API 同步触发计算与移动网策略，所测视频分析准确率由 57% 到 90%。 |
| I11 | [NTT / DOCOMO INC Edge 远程 GPU 联试](https://www.docomo.ne.jp/english/info/media_center/pr/2026/0302_02.html) | 推理预处理/执行拆分、IOWN APN 和商用 5G 核心网优先控制。所测总延迟符合指定实验阈值，新闻未给绝对时延。 |
| I12 | [NTT Inclusive Core 白皮书](https://www.rd.ntt/e/ns/inclusivecore/whitepaper_ver1.html) · [NTT ISAP 技术文章](https://www.rd.ntt/e/research/JN202410_29828.html) | 计算功能按需部署，与终端连接状态和网络路径联动。 |
| I13 | [T-Mobile / NVIDIA / Nokia 边缘 Physical AI](https://www.t-mobile.com/news/network/nvidia-t-mobile-and-partners-integrate-physical-ai-applications-on-ai-ran-ready-infrastructure) | anyRAN 配合运营商分布式 GPU 基础设施试点，运行视觉 AI 视频检索及摘要应用。 |
| I14 | [Deutsche Telekom 5G+ Ultra](https://www.telekom.com/en/newsroom/latest-updates/media-information/2026/4/5g-ultra-for-video-calls) | 5G SA 切片加 L4S，公布 FaceTime 支持。公开的 20% 时延改善来自 GeForce NOW 对照。 |
| I15 | [Ericsson / Deutsche Telekom L4S 联试](https://www.telekom.com/en/newsroom/latest-updates/news/2021/10/5g-low-latency-feature-for-time-critical-applications) · [Ericsson L4S 技术页](https://www.ericsson.com/en/differentiated-connectivity/technology/l4s) | 拥塞反馈到应用码率的 5G 实现，用云游戏演示对照。 |
| I16 | [LG U+ / 延世大学跨网试验](https://news.lguplus.com/22640) | PM95 终端在商用 5G 和自建 GPU AI-RAN 测试网间选择网络，验证连接连续性。 |
| I17 | [LG Electronics CLOiD](https://www.lg.com/global/newsroom/news/home-appliance-solution/lg-electronics-presents-lg-cloid-home-robot-to-demonstrate-zero-labor-home-at-ces-2026/) | 视觉语言模型与视觉语言动作模型驱动家务机器人。 |
| I18 | [OPPO / MediaTek MWC 2026](https://www.oppo.com/en/newsroom/press/oppo-and-mediatek-mwc2026/) · [OPPO MWC 2025](https://www.oppo.com/en/newsroom/press/oppo-ai-tech-summit-mwc-25/) | Omni 端侧语音、视频和文本交互原型；Private Computing Cloud 用于云端处理。 |
| I19 | [Samsung 端侧运行时](https://news.samsung.com/global/interview-the-technologies-bringing-cloud-level-intelligence-to-on-device-ai) · [Samsung 智能眼镜](https://news.samsung.com/global/interview-galaxy-unpacked-july-2026-intelligent-eyewear-the-first-step-toward-the-next-mobile-ai-interface) | 量化校准与内存/计算运行时优化；眼镜作为移动 AI 终端。 |
| I20 | [Samsung Research SAC-JSCM](https://research.samsung.com/blog/6G-AI-ML-for-Physical-layer-Part-II-Audio-Traffic-with-JSCM) | 语音源编码、信道编码和调制联合训练。链路仿真与蓝牙频段 USRP 原型，厂商报告原型相对其基线超过 10 dB 增益。 |
| I21 | [中国移动湖北 / 华为 UCM](https://www.huawei.com/en/news/2026/6/mwcs-cmcc-token-economy) | 推理 KV 缓存分层管理，MiniMax M2.5 所测 TTFT 改善 26%–62%。 |
| I22 | [华为 Agent-Oriented Mobile Networks](https://www.huawei.com/en/huaweitech/publication/202601/agent-oriented-mobile-networks-in-agentverse-era) | Token block pipe、分层传输及与中国移动合作的 A2A-T 意图接口。属于企业公开方案。 |
| I23 | [Verizon AI Sports Companion](https://www.verizon.com/about/news/verizon-leads-6g-momentum-expansion-isac-trials) | AI 可穿戴设备借助边缘网络查询实时比赛数据，原型未公开可比体验时延。 |

## 规范与草案

| ID | 原文 | 相关能力 |
|---|---|---|
| S01 | [3GPP TS 23.501 Rel-18](https://www.etsi.org/deliver/etsi_ts/123500_123599/123501/18.11.00_60/ts_123501v181100p.pdf) | §5.7 QoS。§5.37.3 分述 NG-RAN 和 PSA UPF 的 L4S ECN 标记，§5.37.5 定义 PDU Set 处理，§5.37.6 给出基于 RTT 要求的上下行策略管理。 |
| S02 | [3GPP TS 24.193 Rel-18](https://www.etsi.org/deliver/etsi_ts/124100_124199/124193/18.06.00_60/ts_124193v180600p.pdf) | ATSSS 与 MPQUIC steering functionality 的终端与网络流程。 |
| S03 | [RFC 9330 架构](https://www.rfc-editor.org/rfc/rfc9330) · [RFC 9331 ECN](https://www.rfc-editor.org/rfc/rfc9331) · [RFC 9332 DualQ](https://www.rfc-editor.org/rfc/rfc9332) | L4S 架构、ECT(1)/CE 语义及 DualQ 耦合队列管理。分别查看 RFC 类别。 |
| S04 | [RFC 9000 QUIC](https://www.rfc-editor.org/rfc/rfc9000) · [RFC 9221 Datagram](https://www.rfc-editor.org/rfc/rfc9221) · [RFC 10069 Multipath](https://datatracker.ietf.org/doc/rfc10069/) | 单连接流复用和迁移、非可靠数据报与同时管理多条路径。RFC 10069 的发布记录为 2026 年 11 月。 |
| S05 | [RFC 8298 SCReAM](https://datatracker.ietf.org/doc/html/rfc8298) · [SCReAM v2 工作组草案](https://datatracker.ietf.org/doc/draft-ietf-ccwg-rfc8298bis-screamv2/) · [EricssonResearch 代码](https://github.com/EricssonResearch/scream) | 实验类 RFC、演进草案与工程实现。 |
| S06 | [CAMARA Quality on Demand](https://camaraproject.org/quality-on-demand/) | 应用申请、查询和结束指定流的 QoS 会话。 |
| S07 | [中国移动作者的 Token Service Flow Awareness 个人草案](https://datatracker.ietf.org/doc/html/draft-wang-token-aware-usecases-requirements-00) | 交互、批处理和跨域智能体业务的流识别与指标需求。属于 Internet-Draft。 |
