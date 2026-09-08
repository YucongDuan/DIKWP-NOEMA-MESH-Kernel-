[English project overview](README.md)

# DIKWP NOEMA-MESH Ω Kernel 交付说明

Created by Yucong Duan (段玉聪).

## 1. 系统定位

DIKWP NOEMA-MESH Ω Kernel 是面向 NOEMA 下一代的可运行内核，并以 DIKWP-MESH 4.0 SemanticClosure 为主线闭包机制。版本为 `0.2.0-alpha`。

系统不再把自然语言、提示词或单个交换包视为运行时中心，而是把一个持续演化的 DIKWP-R 六层 MeshCell 作为一等状态：

- D：数据场；
- I：关系场；
- K：可复用稳定场；
- W：权衡与行动场；
- P：目的与授权场；
- R：未闭合、冲突、损失与未知场。

ALOGOS 的振幅、复相位、显著性、拓扑、Residual 与不确定性成为每一层的数值动力学底座；NOEMA 的结构化机内语义成为 NMX/2.0 边界交换协议；Mesh 4.0 则成为每次状态跃迁的闭包准则。

## 2. 相对上一代的关键提升

1. 六层同维状态内核：由单一 FieldState 升级为 D/I/K/W/P/R 六层 MeshCell。
2. 语义代谢：实现 D→I→K→W→P 正向循环、P→D 反向循环与跨层补偿；无法恢复的部分进入 R。
3. Mesh 4.0 原生运行时：Three-No、Anchor、Unit、Denominator/Scope、Reverse、Invariant、Residual、Kill/Recovery 进入闭包对象。
4. 事件驱动调度：优先队列执行 METABOLIZE、SETTLE、CLOSE、EMIT，形成哈希链接事件账本。
5. NMX/2.0：交换完整六层状态、Purpose、锚点、闭包和 Ω 摘要。
6. MKO/2.0：12 字节固定宽度机器操作码，可保存、重放和检查。
7. 检查点与回放：Cell、Closure、OMEGA-UCI、事件链和清单共同冻结。
8. 兼容层：支持 NIP/1.0、AFP/1.0 与本地 NPY 激活快照。
9. Ω Horizon：接收数值数组、FITS 风格元数据、VOEvent XML、SPICE 派生状态、仿真和未来仪器插件。
10. 只读检查 API：提供运行指标、闭包、观测帧、UCI 地平线和检查点摘要。

## 3. “宇宙终极”的工程接口：OMEGA-UCI/0.1

本系统把“宇宙终极”实现为持续可扩展的开放地平线，而不是预设一个已完成的最终本体。

每个多尺度节点明确区分：

- `observed`：仪器、档案或直接数据源提供；
- `modelled`：物理模型、几何系统或仿真产生；
- `inferred`：由锚点、映射和跨尺度关系推导；
- `residual`：尚未观测、冲突、不可恢复或当前无合适表示。

`ScaleCoordinate` 可记录空间、时间、能量、质量、红移、参考系与时间尺度的对数坐标。跨尺度 `HorizonLink` 保留关系码、置信度、Residual 和变换摘要。`open_horizon_ratio` 表示当前连接图仍有多少未知与未闭合部分；它不是“完成度”或“终极真理分数”。

未来插件方向包括：多信使天文学、空间任务遥测、引力波、中微子、粒子事件、气候和地球系统、复杂系统仿真、量子与场论数值输出，以及真实开放权重模型内部表征。

## 4. 固定参考运行

参考运行在 128 维生成：

- 1 个多通道合成 ΩFrame；
- 1 个六层 MeshCell；
- 2 轮语义代谢；
- 1 次 K 层吸引子收敛；
- 3 项不变量检查；
- 1 个 S4 参考闭包；
- 1 个 NMX/2.0 包；
- 1 个四指令 MKO/2.0 程序；
- 1 个内核检查点；
- 10 个哈希链接事件；
- 1 个 128→96 表示桥；
- 4 个 OMEGA-UCI 节点和 3 条链接。

固定指标：

- 闭包：`S4-stable`；
- 闭包分数：`0.96882353`；
- 反向一致性：`0.9556102482`；
- 不变量均分：`0.9992589002`；
- 桥接保留集平均余弦：`0.9773529081`；
- 桥接反向平均余弦：`0.6761743420`；
- 开放地平线比例：`0.3935416667`。

以上数据是固定软件夹具的结果，只用于验证内核路径与工件闭合。

## 5. 快速运行

```bash
unzip DIKWP-NOEMA-MESH-OMEGA-KERNEL_Source_0.2.0-alpha.zip
cd DIKWP-NOEMA-MESH-OMEGA-KERNEL

python -m venv .venv
. .venv/bin/activate
python -m pip install -e .

python -m dikwp_omega demo --output artifacts/demo
python -m dikwp_omega verify --artifacts artifacts/demo
python -m unittest discover -s tests -v
```

检查协议对象：

```bash
python -m dikwp_omega packet-info artifacts/demo/packets/reference.nmx
python -m dikwp_omega program-info artifacts/demo/program/reference.mko
python -m dikwp_omega checkpoint-verify artifacts/demo/kernel/checkpoint
```

可选只读 API：

```bash
python -m pip install -e '.[api]'
uvicorn 'dikwp_omega.api:create_app("artifacts/demo")' --factory
```

端点：

```text
GET /v1/health
GET /v1/metrics
GET /v1/closure
GET /v1/omega-frame
GET /v1/horizon
GET /v1/checkpoint
```

## 6. 工程规模与验证

- 23 个核心 Python 模块；
- 约 3,297 行核心 Python；
- 9 个测试文件、59 项自动化测试；
- 8 类 JSON Schema；
- 13 份专项文档；
- 22 项固定参考工件；
- 只读 API 6 个 GET 端点；
- DOCX/PDF 白皮书 24 页；
- DOCX 无障碍审计高风险 0、中风险 0、低风险 0。

最终源码 ZIP 已在新目录解压并完成：可编辑安装、59 项测试、全新演示、NMX/Cell/检查点/账本验证、8 类 Schema 验证、`compileall` 和只读 API 冒烟测试。

## 7. 证据门

不采用固定日历推进，采用证据门：

- G1：可运行参考实现——本次已达到；
- G2：两个无实现依赖团队独立复现；
- G3：真实开放权重模型及授权激活接入；
- G4：公开 FITS/VOTable/ObsCore 数据、单位与时间坐标闭合；
- G5：SPICE 几何、VOEvent 事件和仿真回放；
- G6：光、电磁、引力波、粒子等多信使联合；
- G7：跨机构协议治理、版本兼容和一致性套件；
- GΩ：开放地平线持续扩展，所有未知保持可见、可连接、可重新闭包。

## 8. 正式并入主线前

正式以段玉聪名义发布或并入其 GitHub/DIKWP-MESH 主线前，应确认项目名称、作者与贡献归属、许可证、维护者、安全联系人、协议治理、真实数据许可和研究主张。当前交付为供其审阅的独立候选实现。
