# GPU architecture capability table

| Item | Definition |
| --- | --- |
| Reviewed | 19 November 2026. Hardware reference, not an implemented support promise. |
| Coverage | Common desktop, laptop, workstation and compute GPU families used during 2016–2026, including older architectures still common at the start of that period. Mobile/embedded families are listed separately. Representative models identify families; exceptions identify meaningful capability differences rather than every clock, memory-capacity or board variant. |
| Project requirement | **Use the highest hardware-accelerated floating point method supported by the detected GPU.** See [scope](../scope.md). A slow native FP64 unit remains hardware acceleration; this table does not silently substitute a faster, lower precision. |
| Arithmetic versus storage | Native arithmetic, matrix instructions, storage formats and compression are separate capabilities. FP16/FP8 storage does not prove native arithmetic. FP64 add/multiply does not prove native FP64 atomics or every mathematical function. |
| Matrix column | Selected floating-point input formats for tensor/matrix instructions, not a general-purpose arithmetic menu. Accumulator precision and supported shapes must be checked per instruction. BF16 and TF32 are not higher precision than FP32; FP4/6/8 are not replacements for arbitrary solver arithmetic. |
| Compression notation | **G:** transparent graphics/surface compression; **C:** documented compute-buffer compression; **D:** dedicated general-data decompression; **S:** programmable software codecs. See the mechanism table below. “Unconfirmed” means no general-data capability established by the cited sources, not proof of absence. |
| Backend notation | CUDA, HIP, SYCL, Vulkan compute, OpenCL and Metal are candidate companion-executable backends. Availability depends on the particular device, OS, compiler and driver. Blender rendering-backend support does not establish companion-solver support. |
| Recent architectures | Newly published 2026 products are included for completeness; their presence is not evidence that they are already common or locally testable. Announced/sampling hardware is separated below. |

## NVIDIA

| Architecture / period | Representative models | Native general FP arithmetic | Matrix/tensor FP features | Memory, compression and project implications | Sources |
| --- | --- | --- | --- | --- | --- |
| Kepler, legacy in 2016 | GTX 700; Tesla K40/K80 | FP32, FP64; FP64 rate differs greatly between gaming and GK110/GK210 compute parts. No general native FP16 arithmetic baseline. | None. | Discrete VRAM; S requires a compatible legacy toolchain. Historical compatibility reference, not a new backend baseline. | N1, N2 |
| Maxwell, 2014–2016 | GTX 900; Quadro M; Tesla M40 | FP32, low-rate FP64. FP16 storage/conversion is not native FP16 arithmetic on ordinary desktop Maxwell. | None. | G; S through kernels. Retain dirty-block transfers; graphics compression is not a particle-cache codec. | N1, N2, N3 |
| Pascal, 2016 | GTX 10; Quadro P; Tesla P4/P40 | FP32, low-rate FP64 and FP16 on GP10x gaming/inference parts. P100 is a major exception below. | None. | G; GDDR5/GDDR5X or HBM2 depending on model. No assumed D. | N1, N2, N4 |
| Volta, 2017 | V100; TITAN V | FP16, FP32, strong FP64 on GV100. | FP16 multiplication with FP32 accumulation. | HBM2 on GV100; S. Separate bandwidth benefit from compression. | N1, N2, N5 |
| Turing, 2018 | RTX 20; Quadro RTX; T4 | FP16, FP32, low-rate FP64. | RTX/T4: FP16 Tensor Cores; GTX 16 and small T-series exceptions below. | Improved G; S. RTX branding matters more than CC alone for tensor availability. | N1, N2, N6 |
| Ampere compute, 2020 | A100, A30; CC 8.0 | FP16, BF16, FP32, strong FP64. | FP16/BF16, TF32 and FP64 matrix paths. | HBM2/HBM2e; A100 C is documented, with allocation requirements. No dedicated D established here. | N1, N2, N7, N8 |
| Ampere graphics, 2020 | RTX 30; RTX A6000; A10/A40; CC 8.6 | FP16, BF16, FP32, low-rate FP64. | FP16/BF16, TF32; do not inherit A100 FP64 Tensor Cores. | GDDR6/GDDR6X, G and S. Query C instead of inheriting A100 capabilities. | N1, N2, N7, N8 |
| Ada Lovelace, 2022 | RTX 40; RTX 6000 Ada; L4/L40/L40S; CC 8.9 | FP16, BF16, FP32, low-rate FP64. | FP16/BF16, TF32, FP8. | Larger cache and G can reduce VRAM traffic; neither proves compressed PCIe transfers. S; no assumed D. | N1, N2, N9 |
| Hopper, 2022 | H100/H200; GH200; CC 9.0 | FP16, BF16, FP32, strong FP64. | FP16/BF16, TF32, FP64, FP8. | HBM3/HBM3e; asynchronous transfers and tensor-memory movement. Query C; use S for application records. GH200 topology differs below. | N1, N2, N10 |
| Blackwell compute, 2024–2025 | B200/GB200; B300/GB300; CC 10.0/10.3 | FP16/BF16, FP32, FP64; measure FP64 rather than inheriting Hopper rates. | FP16/BF16, TF32, FP8 and newer narrow/block-scaled formats including FP4; exact instruction set differs by CC. | HBM3e; **D confirmed on B200, B300, GB200, GB300**. Compatible compressed input can cross PCIe/C2C before decompression. Compression itself still needs a software producer. | N1, N2, N11, N12 |
| Blackwell graphics, 2025 | RTX 50; RTX PRO Blackwell; CC 12.0 | FP16/BF16, FP32, low-rate FP64. | FP16/BF16, TF32, FP8 and FP4 paths; not identical to server Blackwell. | GDDR7, G and S. The server D feature must not be inferred from the Blackwell name. | N1, N2, N11, N12 |
| Rubin, published 2026 | Rubin / Vera Rubin systems | Native FP64 and FP32 documented; validate the deployed compiler's complete instruction support. | New Tensor Cores; FP4 and FP8/FP6 advertised for matrix workloads. | HBM4 and NVLink 6; server reference. Do not inherit a specific D or C feature without product/API confirmation. | N13 |

## AMD

| Architecture / period | Representative models | Native general FP arithmetic | Matrix/packed FP features | Memory, compression and project implications | Sources |
| --- | --- | --- | --- | --- | --- |
| GCN 1/2, legacy in 2016 | HD 7000; R9 280/290/390 | FP32, FP64 with substantial chip-specific rate differences. | No modern matrix-core baseline. | Discrete GDDR5; legacy driver/toolchain constraints. Do not infer later DCC behaviour. | A1 |
| GCN 3, 2014–2016 | R9 285/380; Fury/Fury X | FP32, FP64; reduced-precision instructions are not Vega-style packed throughput. | No dedicated matrix cores. | G/DCC on eligible surfaces; Fury uses HBM1. S for particle buffers. | A1, A2 |
| GCN 4 / Polaris, 2016 | RX 460–480, RX 550–590; Radeon Pro WX | FP32, FP64; distinguish FP16 instruction support from packed FP16 throughput. | No Vega Rapid Packed Math baseline. | G/DCC; GDDR5; S. Modern HIP support cannot be assumed from shader capability. | A1, A2 |
| GCN 5 / Vega 10, 2017 | RX Vega 56/64; MI25; Vega iGPUs | FP16, FP32, FP64; MI25 FP64 is 1/16 of FP32. | Packed FP16 (“Rapid Packed Math”). No CDNA matrix-core baseline. | G/DCC; HBM2 on discrete Vega, shared RAM on APUs. | A1, A2, A3 |
| Vega 20 / 7 nm, 2018–2019 | MI50/MI60; Radeon VII | FP16, FP32, FP64; MI50/MI60 FP64 is 1/2 FP32. | Packed FP16; not CDNA. | HBM2; model-specific ECC and interconnect. Radeon VII must not inherit all Instinct features. | A1, A3 |
| RDNA 1, 2019 | RX 5000; Pro W5500/W5700 | FP16, FP32, low-rate FP64. | Packed FP16; no RDNA 3 WMMA baseline. | G/DCC, GDDR6, wave32/wave64. S for arbitrary records. | A1, A2, A4 |
| RDNA 2, 2020 | RX 6000; Pro W6000; 680M; Steam Deck APU | FP16, FP32, low-rate FP64; instruction-specific BF16/dot support needs checking. | Packed/dot operations; do not equate these with RDNA 3 WMMA or CDNA MFMA. | G/DCC. Infinity Cache on selected discrete chips; UMA on APUs. Neither is a general cache-file codec. | A1, A2, A4, A5 |
| RDNA 3, 2022 | RX 7000; Pro W7000; 780M | FP16, FP32, low-rate FP64; BF16 support is operation-specific. | FP16/BF16 WMMA with supported accumulation modes. | G/DCC; chiplets on some discrete models, monolithic chips on others; UMA on APUs. | A1, A4, A5 |
| RDNA 3.5, 2024 | Radeon 880M/890M; 8050S/8060S | FP16, FP32, low-rate FP64; verify exact gfx target. | RDNA-family matrix/packed paths, with instruction-level differences. | Shared system RAM; larger APUs can hold more resident state, but CPU and GPU share bandwidth. | A1, A4 |
| RDNA 4, 2025 | RX 9070/9070 XT, RX 9060 XT; Radeon AI PRO R9700 | FP16, FP32, low-rate FP64; BF16 as supported by the instruction. | FP16/BF16 and FP8 matrix acceleration. FP8 is not general native vector arithmetic. | G/DCC; GDDR6 and cache. No general-purpose D established by these sources. | A1, A4, A5 |
| CDNA 1, 2020 | MI100 | FP16/BF16, FP32, FP64. | FP16/BF16 and FP32 MFMA; no FP64 matrix path. | HBM2, ECC; compute-oriented S. Do not apply Radeon DCC assumptions to arbitrary HIP allocations. | A1, A5 |
| CDNA 2, 2021 | MI210, MI250/MI250X | FP16/BF16, FP32, strong FP64. | Adds FP64 matrix operations. | HBM2e, ECC, Infinity Fabric; MI250 multi-die scheduling exception below. | A1, A5 |
| CDNA 3, 2023 | MI300A/MI300X, MI325X | FP16/BF16, FP32, strong FP64. | FP16/BF16, FP32/FP64, TF32 and FP8 paths. | HBM3/HBM3e, ECC; MI300A has shared CPU/GPU HBM. No assumed D. | A1, A5, A6 |
| CDNA 4, 2025 | MI350X/MI355X, MI350P | FP16/BF16, FP32, FP64. | Adds FP4/FP6 and block-scaled formats; FP8 encoding differs from CDNA 3. Verify TF32 per instruction/library rather than assuming identical support. | HBM3e; no general-data D established here. S competes with solver compute. | A5, A7 |
| CDNA 5, published 2026 | MI455X | FP32 and FP64 advertised; qualify exact vector formats on the deployed backend. | FP16/BF16, FP8 and microscaled FP8/FP6/FP4 paths. | HBM4, ECC, UALink; FP64 throughput is a model-specific check, not inherited from MI355X. New server platform reference. | A8 |

## Intel

| Architecture / period | Representative models | Native general FP arithmetic | Matrix FP features | Memory, compression and project implications | Sources |
| --- | --- | --- | --- | --- | --- |
| Gen9 / Gen9.5, 2015–2017 | HD 530; HD/UHD 620/630; Iris Plus 640/650 | FP16, FP32; FP64 available on relevant implementations, verify device exposure. | No XMX. | Shared RAM; graphics compression varies by resource/driver. S requires compatible OpenCL/Vulkan implementation. | I1 |
| Gen11, 2019 | Ice Lake Iris Plus | FP16/FP32; do not assume the preceding generation's FP64 capability. | No XMX. | Shared RAM. Treat general-buffer hardware compression as unconfirmed. | I2 |
| Xe-LP, 2020 | Iris Xe; UHD 730/770; DG1/Iris Xe MAX | FP16, FP32; no native FP64 baseline. | No XMX; integer DP4a is not floating-point matrix arithmetic. | Usually UMA; DG1 has dedicated memory. Driver-dependent compression; S. | I3 |
| Xe-HPG / Alchemist, 2022 | Arc A310–A770; Arc Pro A; Flex 140/170 | FP16, FP32; no native FP64 baseline for Arc A. Flex needs product-specific validation. | XMX FP16/BF16. | Documented lossless compression extends to compute resources, subject to driver allocation. Dedicated GDDR6; no assumed compressed host transfer. | I3, I4, I5 |
| Xe-HPC / Ponte Vecchio, 2023 | Data Center GPU Max 1100/1350/1550 | FP16, FP32, strong native FP64. | XMX FP16/BF16; check operation-specific precision. | HBM2e, ECC and multiple tiles. Query topology; S remains an explicit application operation. | I3, I6 |
| Xe-LPG / Meteor Lake, 2023 | Core Ultra 100H/U graphics | FP16, FP32; FP64 status is inconsistent across generic documentation, so unqualified here. | **No XMX**, per Intel's product-specific AI-engine table. | UMA; do not inherit discrete Arc A features from the Arc name. | I3, I5, I6 |
| Xe-LPG+ / Arrow Lake-H, 2025 | Arc 130T/140T | FP16, FP32; qualify FP64 separately. | XMX/BF16 support on this branch. Desktop Arrow Lake is not this GPU. | UMA; identify graphics IP, not CPU series number alone. | I6, I7 |
| Xe2-LPG / Lunar Lake, 2024 | Arc 130V/140V | FP16, FP32, native FP64 listed; benchmark the actual rate. | XMX FP16/BF16. | UMA; CPU/GPU contend for system-memory bandwidth. No assumed general-data D. | I3, I6, I8 |
| Xe2-HPG / Battlemage, 2024 | Arc B570/B580; Pro B50/B60/B65/B70 | FP16, FP32, native FP64 listed; not necessarily HPC-class throughput. | XMX FP16/BF16. | GDDR6; buffer-compression behaviour must be checked in the chosen driver. | I3, I6, I8 |
| Xe3-LPG / Panther Lake, 2026 | Arc B370/B390 integrated graphics | FP16, FP32, native FP64 listed in oneDNN's hardware table. | XMX FP16/BF16. | UMA; do not confuse integrated B390 with discrete Battlemage B580. | I6, I8 |

## Apple and Qualcomm computer GPUs

| Architecture / period | Representative models | Native general FP arithmetic | Matrix/AI features | Memory, compression and project implications | Sources |
| --- | --- | --- | --- | --- | --- |
| Apple GPU family 7, 2020 | M1, M1 Pro/Max/Ultra | FP16, FP32; no Metal `double` arithmetic. | SIMD-group matrix operations; supported inputs/accumulators depend on Metal version. | UMA; shared buffers can avoid separate CPU/GPU copies, but synchronization and bandwidth still matter. Texture compression is not a generic buffer codec. | P1, P2 |
| Apple GPU family 8, 2022 | M2, M2 Pro/Max/Ultra | FP16, FP32; no Metal FP64. | SIMD-group matrix paths. | UMA; S on Metal or CPU. No exposed general-data D established here. | P1, P2 |
| Apple GPU family 9, 2023–2024 | M3 and M4 families; M3 Ultra | FP16, FP32; no Metal FP64. | SIMD-group matrix paths; BF16 availability is language/OS dependent. | UMA; Dynamic Caching concerns allocation/utilization, not a portable lossless particle codec. | P1, P2, P3 |
| Apple GPU family 10, 2025–2026 | M5, M5 Pro/Max/Ultra | FP16, FP32; no Metal FP64. | GPU Neural Accelerators; distinguish these from the separate Neural Engine and from arbitrary shader arithmetic. | UMA; qualify acceleration through supported Metal APIs. No general-data D assumed. | P1, P2, P4 |
| Apple GPU family 11, 2026 | M6 | FP16, FP32 via Metal; no Metal FP64 baseline. | New family; qualify matrix/tensor kernels rather than inferring exact formats from marketing. | UMA; Dynamic Caching and graphics features do not establish CPU–GPU transfer compression. | P1, P2, P5 |
| Qualcomm Adreno X1, 2024 | Snapdragon X Elite/Plus; X1-85/X1-45 | FP32; native FP16 through supported Adreno compute paths. No FP64 baseline established. | Hexagon NPU capabilities must not be attributed to the GPU. | UMA; image-oriented UBWC is not general particle-buffer compression. Validate Windows-on-Arm backend and driver first. | Q1, Q2 |
| Qualcomm Adreno X2, 2025–2026 | Snapdragon X2 Elite; X2-90/X2-85 | FP32; qualify FP16 and exact extensions on device. No FP64 baseline established. | Do not inherit NPU formats or assume tensor operations from GPU branding. | UMA; advertised Vulkan/OpenCL APIs provide candidate compute routes, not proof of every optional feature. | Q3 |

## Individual models and variants that need overrides

| Model / variant | Difference from family shorthand | Required project handling | Sources |
| --- | --- | --- | --- |
| Tesla P100 / Quadro GP100 versus GTX 1080, Titan X Pascal, P4/P40 | GP100 has FP64 at 1/2 FP32 and FP16 at 2x FP32; GP104 FP16 is only 1/64 FP32. | Identify chip/CC and precision throughput; “Pascal supports FP16” is insufficient. | N4 |
| Jetson TX2, Xavier, Orin; Thor T5000/T4000 | Embedded Pascal/Volta/Ampere/Blackwell variants have distinct CC and shared-memory topology. | Probe capabilities separately; desktop model assumptions and discrete-PCIe cost models do not apply. | N1 |
| GTX 1650/1660 family; T400/T600/T1000 | Turing-generation products without the RTX/T4 Tensor Core feature set. | General compute remains possible; disable tensor-specific kernels unless positively confirmed. | N1, N6 |
| A100/A30 versus RTX 3090 / RTX A6000 | Same broad generation, different FP64 and tensor capability. A100 C does not prove RTX C. | Separate CC 8.0 and 8.6; query compression support and actual allocation properties. | N1, N7, N8 |
| RTX A6000 versus RTX 6000 Ada versus RTX PRO 6000 Blackwell | Similar names span three architectures. | Match PCI/device IDs and API capability, not a substring such as “6000”. | N1 |
| B200/B300/GB200/GB300 versus RTX 5090 | Dedicated DE is documented for these server parts, not all Blackwell products. | Gate DE by runtime/library support; keep software decompression fallback. | N12 |
| GB10 DGX Spark / RTX Spark N1X | CC 12.1, distinct from B200 CC 10.0 and RTX CC 12.0; shared-memory platform. | Separate capability record; do not assume server FP64, DE or HBM characteristics. | N1 |
| GH200 versus standalone H100/H200 | Grace CPU plus coherent CPU–GPU interconnect changes access topology. | Benchmark shared/coherent accesses as well as explicit copies; remote memory still has a cost. | N10 |
| Radeon VII versus MI50/MI60; MI25 versus MI50 | Vega naming spans chips and SKUs with different FP64 rates, ECC and links. | Record actual SKU specifications; no universal “Vega FP64 rate”. | A1, A3 |
| RX 6500 XT / RX 6400 | RDNA 2 cards with restricted PCIe link width and small VRAM compared with larger RX 6000 cards. | Resident working-set sizing and sparse transfers are particularly important; detect negotiated link, not just GPU generation. | A9 |
| Radeon 680M / 780M / 890M / 8060S | RDNA 2 / 3 / 3.5 respectively, using system RAM rather than ordinary discrete VRAM. | Identify graphics IP independently of Ryzen CPU branding; account for shared bandwidth. | A1 |
| MI250/MI250X | Multiple graphics compute dies are exposed as separate GPU agents. | Explicitly partition state and exchange halos; do not assume one contiguous local-memory device. | A6 |
| MI300A versus MI300X/MI325X | MI300A combines CPU/GPU with shared HBM; X variants are discrete accelerators. | Separate ownership/synchronization and transfer policies for the APU. | A6 |
| Iris Xe MAX / DG1 versus Iris Xe integrated | Dedicated GPU memory versus UMA despite Xe-LP branding. | Detect topology; do not select zero-copy policy from “Iris Xe”. | I3 |
| Meteor Lake Ultra 155H versus Arrow Lake-H Arc 140T | Xe-LPG lacks XMX; Xe-LPG+ adds relevant matrix capability. | Do not apply a blanket “Core Ultra has XMX” rule. | I5, I6 |
| Arc B390 versus Arc B580 | Integrated Xe3 versus discrete Xe2; model number is not architecture order. | Query architecture and UMA before selecting kernels or transfer policy. | I8 |
| Apple Pro / Max / Ultra variants | Different memory bandwidth, capacities and GPU counts; Ultra is a multi-die package. Metal GPU family can remain shared. | Size tiles from actual device limits; do not promise linear scaling or infer new FP formats from “Ultra”. | P1, P4, P5 |

## Mobile and embedded coverage

| Architecture families used in 2016–2026 | Representative GPUs | Relevant FP / memory capabilities | Project status / exceptions | Sources |
| --- | --- | --- | --- | --- |
| Arm Midgard | Mali-T760/T880 | FP16/FP32 compute paths; shared RAM; graphics/framebuffer compression is resource-specific. | Historical mobile reference; no FP64 baseline or desktop Blender integration assumed. | M1, M2 |
| Arm Bifrost | Mali-G71/G72/G76 | FP16/FP32; shared RAM; AFBC for compatible surfaces. | Backend/extensions vary by SoC and OS. | M1, M2 |
| Arm Valhall | Mali-G77/G78/G710; Immortalis-G715 | FP16/FP32; shared RAM; image compression. | Immortalis ray tracing does not provide particle solving or general-data compression. | M1, M2 |
| Arm 5th Gen | Immortalis-G720/G925; Mali-G725/G625; Mali G1 family | FP16/FP32 shader workloads; improved graphics/memory handling, not a promised general-buffer codec. | Mobile/embedded portability reference; qualify compute separately. | M2, M3 |
| Qualcomm Adreno 5xx / 6xx / 7xx / newer mobile generations | Adreno 530, 630, 740; Snapdragon 8 Elite GPU family | FP16/FP32 compute, shared RAM and UBWC on eligible image paths. | Generation labels do not guarantee all OpenCL/Vulkan options. Separate GPU support from Hexagon NPU support. | Q2 |
| PowerVR Rogue / Furian; later IMG A/B/C/D families | PowerVR GT/GE, Series8XT; IMG B-Series, CXT/DXT/DXTP | FP16/FP32 compute configurations; texture/framebuffer/parameter compression. | IP configuration and vendor driver determine capability. Do not treat graphics compression or ray-tracing hardware as a general-data codec. | M4, M5 |
| Apple mobile families 3–6, then 7–11 | A9/A10, A11, A12, A13; later A-series | FP16/FP32 Metal paths; shared RAM. Family 7+ overlaps desktop feature families, not identical device limits. | Mobile reference only; an iPhone GPU feature does not establish a supported Blender host. | P1, P2 |

## Announced or sampling architectures to track

| Architecture | Published status / relevance | Treatment | Sources |
| --- | --- | --- | --- |
| Intel Xe3P / Crescent Island | Announced inference-oriented data-center architecture; 2026 disclosure and sampling plans. XMX formats expand beyond Xe3. | Keep separate from common Arc desktop support. Confirm production SKU, driver, precision and memory-compression features before enabling. | I6, I9 |
| Future successors not verified here | A roadmap name or leaked model does not establish hardware/API support. | Add rows only with primary documentation; do not extrapolate from preceding architecture. | Project policy |

## What compression and memory features actually provide

| Mechanism | Architectures / examples | Benefit for OpenGrainSim | Limitation / implementation rule |
| --- | --- | --- | --- |
| G: transparent surface compression | NVIDIA graphics compression, AMD DCC, Intel surface compression, Arm AFBC, Qualcomm UBWC, PowerVR image compression | Lower traffic for eligible graphics/image resources. | No portable compressed byte stream; no guaranteed benefit for arbitrary particle buffers, RAM caches or PCIe transfers. |
| C: compute-buffer compression | A100 documented; Intel Xe-HPG compute-resource compression; later devices require confirmation | Potentially reduces physical memory traffic for compressible allocations. | Driver/allocation dependent; cannot count advertised compression as extra allocatable VRAM. Not automatically a host-transfer format. |
| D: dedicated decompression | Confirmed Blackwell B200/B300/GB200/GB300 | Decode compatible LZ4/Snappy/Deflate/Gzip streams without consuming solver SM resources; can reduce compressed input traffic. | Requires compatible buffers, codec and API. Does not perform compression, dirty tracking or automatically compress GPU-to-CPU output. |
| S: software compression | Any supported programmable compute backend with a suitable codec implementation | Explicit packed IDs, changed blocks, numerical codecs and transfer/cache records. | Kernel time and scratch buffers compete with simulation. Benchmark total encode + transfer + decode cost. See [compression methods](compression_methods.md). |
| UMA / coherent memory | Apple M; Intel/AMD/Qualcomm iGPUs; MI300A; Grace-linked systems have their own topology | Can avoid duplicate host/device allocations and copies. | Synchronization, ownership, cache coherence and shared bandwidth remain costs; coherent remote memory is not necessarily local bandwidth. |
| Large cache / HBM / interconnect | Infinity Cache, L2/L3, HBM, NVLink, Infinity Fabric, UALink | Less external traffic, more bandwidth or faster peer access. | These are not compression formats. Do not count them as reducing disk-cache size. |
| Sparse/virtual allocations | API- and device-dependent across vendors | Allocate spatial blocks on demand and retain inactive blocks efficiently. | Sparse allocation does not discover changed particles or eliminate solver dependencies. |
| Atomics, subgroup operations, asynchronous copies | Vary within every vendor family | Active lists, reductions, contact accumulation, sorting and overlap of transfer/compute. | Probe types, scope, subgroup size and operation support separately from FP arithmetic; a CAS loop is emulation. |
| RT cores / matrix cores / video engines | Selected modern GPUs | RT may assist suitable geometry queries; matrix units may assist reformulated linear algebra. | None is a general-purpose replacement for particle/contact kernels. Video codecs are unsuitable for exact restart state. |

## Runtime selection and validation

| Check | Required record / action |
| --- | --- |
| Device identity | Vendor/device ID, chip architecture, driver/compiler version, API capability, memory capacity and UMA/discrete topology. Use this table as context, not a replacement for detection. |
| Arithmetic | Verify native instructions for the requested FP method, accumulator precision, denormal behaviour, atomics and reductions. Honor the scope's highest-hardware-accelerated-method requirement. Report missing capabilities explicitly. |
| Compression | CUDA: query generic compression support and actual allocation properties; DE: verify device, pointer and codec eligibility. Other APIs: enable only documented supported resource paths. |
| Kernel execution | Compile and execute representative contact, neighbour-search, reduction and sparse-update kernels. Record native versus emulated paths and performance separately. |
| Transfers and caching | Measure changed-byte volume, encode/decode latency, transfer time, scratch-memory peak and random-seek latency. Keep an uncompressed fallback for small/incompressible records. |
| Precision validation | Compare hybrid and GPU-only preview separately against the chosen reference. Matrix-format availability alone does not justify changing solver precision or cache accuracy. |
| Conflicting documentation | Intel's generic Xe table disagrees with its product-specific XMX table for Meteor Lake and labels some products inconsistently. Preserve this uncertainty; use chip-specific ISA/runtime evidence before implementation. Likewise, validate AMD TF32 by exact operation rather than a broad library type table. |

## Primary references

| ID | Source | Used for |
| --- | --- | --- |
| N1 | [NVIDIA current GPU capabilities](https://developer.nvidia.com/cuda/gpus), [legacy GPUs](https://developer.nvidia.com/cuda/legacy-gpus) | Product/CC mapping and embedded exceptions. |
| N2 | [CUDA Programming Guide 12.8.1](https://docs.nvidia.com/cuda/archive/12.8.1/cuda-c-programming-guide/index.html), [current Best Practices Guide](https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/) | Arithmetic types and instruction throughput; distinguish native arithmetic from storage. |
| N3 | [NVIDIA legacy GeForce support plan](https://nvidia.custhelp.com/app/answers/detail/a_id/5676/) | Maxwell/Pascal/Volta driver lifecycle. |
| N4 | [Pascal Tuning Guide](https://docs.nvidia.com/cuda/pascal-tuning-guide/) | GP100/GP104 FP16 and FP64 differences. |
| N5 | [Volta Tuning Guide](https://docs.nvidia.com/cuda/volta-tuning-guide/) | Volta precision and execution features. |
| N6 | [Turing architecture](https://developer.nvidia.com/blog/nvidia-turing-architecture-in-depth/), [GeForce comparison](https://www.nvidia.com/en-us/geforce/graphics-cards/compare/) | Tensor features, GTX exceptions and memory compression. |
| N7 | [Ampere architecture](https://developer.nvidia.com/blog/nvidia-ampere-architecture-in-depth/), [Ampere Tuning Guide](https://docs.nvidia.com/cuda/ampere-tuning-guide/) | A100 precision, compute data compression and graphics/compute differences. |
| N8 | [CUDA virtual memory management](https://docs.nvidia.com/cuda/cuda-programming-guide/04-special-topics/virtual-memory-management.html) | Compressible allocations and actual-support checks. |
| N9 | [Ada Tuning Guide](https://docs.nvidia.com/cuda/ada-tuning-guide/) | Ada tensor and execution capabilities. |
| N10 | [Hopper Tuning Guide](https://docs.nvidia.com/cuda/hopper-tuning-guide/), [Grace Hopper](https://www.nvidia.com/en-us/data-center/grace-hopper-superchip/) | Hopper and CPU/GPU topology. |
| N11 | [Blackwell Tuning Guide](https://docs.nvidia.com/cuda/blackwell-tuning-guide/) | Compute capability differences. |
| N12 | [nvCOMP Decompression Engine FAQ](https://docs.nvidia.com/cuda/archive/13.4.3/nvcomp/decompression_engine_faq.html) | Explicit supported devices, codecs, allocation requirements and software fallback. |
| N13 | [Rubin architecture](https://developer.nvidia.com/blog/inside-nvidia-rubin-gpu-architecture-powering-the-era-of-agentic-ai/), [native FP64 announcement](https://nvidianews.nvidia.com/news/nvidia-vera-rubin-delivers-world-class-supercomputers-for-science) | Published 2026 compute and memory features. |
| A1 | [AMD architecture documentation](https://rocmdocs.amd.com/en/latest/reference/gpu-arch/index.html), [GPU specifications](https://rocm.docs.amd.com/en/latest/reference/gpu-specs.html) | GCN/Vega/RDNA/CDNA identity, ISA and model mapping. |
| A2 | [AMD DCC overview](https://gpuopen.com/learn/dcc-overview/), [RDNA performance guide](https://gpuopen.com/learn/rdna-performance-guide/) | Surface compression boundaries. |
| A3 | [AMD MI50/MI60 announcement](https://ir.amd.com/news-events/press-releases/detail/859/amd-unveils-worlds-first-7nm-datacenter-gpus----powering-the-next-era-of-artificial-intelligence-cloud-computing-and-high-performance-computing-hpc) | Vega 10 versus Vega 20 FP64 rates. |
| A4 | [AMD GPU architecture specifications](https://rocm.docs.amd.com/en/latest/reference/gpu-specs.html) | RDNA/APU model distinctions and execution resources. |
| A5 | [ROCm precision support](https://rocm.docs.amd.com/en/latest/reference/precision-support.html) | Separate compute, matrix, atomic and library support. |
| A6 | [AMD CDNA architecture](https://www.amd.com/en/technologies/cdna.html), [MI250 topology](https://instinct.docs.amd.com/projects/system-acceptance/en/latest/gpus/mi250.html) | CDNA generations; linked architecture whitepapers for multi-die and unified-memory design. |
| A7 | [CDNA 4 whitepaper](https://www.amd.com/content/dam/amd/en/documents/instinct-tech-docs/white-papers/amd-cdna-4-architecture-whitepaper.pdf) | MI350 precision and memory. |
| A8 | [MI455X specifications](https://www.amd.com/en/products/accelerators/instinct/mi400/mi455x.html) | Published CDNA 5 features. |
| A9 | [AMD graphics specifications](https://www.amd.com/en/products/specifications/graphics.html), [MSI RX 6500 XT datasheet](https://storage-asset.msi.com/datasheet/vga/global/Radeon-RX-6500-XT-MECH-2X-4G-OC.pdf) | Per-SKU memory and PCIe exceptions. |
| I1 | [Intel Gen9 compute architecture](https://cdrdv2-public.intel.com/774710/the-compute-architecture-of-intel-processor-graphics-gen9-v1d0-166010.pdf) | Legacy integrated compute. |
| I2 | [Intel Gen11 FP64 support discussion](https://community.intel.com/t5/Graphics/What-processors-have-an-internal-Gen11-GPU-with-FP64-double/m-p/691214) | Why Gen9 FP64 cannot be inherited blindly. |
| I3 | [Intel Xe architecture guide](https://www.intel.com/content/www/us/en/docs/oneapi/optimization-guide-gpu/2025-2/intel-xe-gpu-architecture.html) | Architecture/topology overview; conflicts explicitly flagged above. |
| I4 | [Intel Xe-HPG introduction](https://www.intel.com/content/www/us/en/developer/articles/technical/introduction-to-the-xe-hpg-architecture.html) | Vector/XMX formats and compute-resource compression. |
| I5 | [Intel AI engines and datatypes](https://www.intel.com/content/www/us/en/support/articles/000098346/graphics.html) | Product-specific Xe-LPG versus Xe-HPG XMX distinction. |
| I6 | [oneDNN datatype support](https://uxlfoundation.github.io/oneDNN/dev_guide_data_types.html) | Native versus converted types by Intel architecture; library scope applies. |
| I7 | [Intel Core Ultra Series 2 resources](https://www.intel.com/content/www/us/en/newsroom/resources/press-kit-core-ultra-series-2.html) | Distinct Arrow Lake and Lunar Lake products. |
| I8 | [Intel Xe driver GPU list](https://dgpu-docs.intel.com/overview/supported-hardware/xe-driver-gpus.html) | Xe2/Xe3 product IDs and driver dependencies. |
| I9 | [Crescent Island announcement](https://www.intel.com/content/www/us/en/newsroom/news/artificial-intelligence/intel-to-expand-ai-accelerator-portfolio-with-new-gpu.html), [Hot Chips 2026](https://www.intel.com/content/www/us/en/newsroom/news/client-computing/intel-outlines-architectures-for-agentic-ai-at-hot-chips-2026.html) | Announced Xe3P architecture; separate from mainstream qualification. |
| P1 | [Metal feature tables, October 2026](https://developer.apple.com/metal/Metal-Feature-Set-Tables.pdf) | Apple family mapping and per-family features. |
| P2 | [Metal Shading Language specification](https://developer.apple.com/metal/Metal-Shading-Language-Specification.pdf) | Arithmetic types and language/API constraints. |
| P3 | [Apple M3 announcement](https://www.apple.com/newsroom/2023/10/apple-unveils-m3-m3-pro-and-m3-max-the-most-advanced-chips-for-a-personal-computer/) | Dynamic Caching and graphics changes. |
| P4 | [Apple M5](https://www.apple.com/newsroom/2025/10/apple-unleashes-m5-the-next-big-leap-in-ai-performance-for-apple-silicon/), [M5 Pro/Max](https://www.apple.com/newsroom/2026/03/apple-debuts-m5-pro-and-m5-max-to-supercharge-the-most-demanding-pro-workflows/) | GPU Neural Accelerators and variants. |
| P5 | [Apple M6 and M5 Ultra](https://www.apple.com/newsroom/2026/08/apple-introduces-m6-and-m5-ultra-for-a-big-leap-in-performance-and-ai-compute/) | Published 2026 models. |
| Q1 | [Snapdragon X Elite product brief](https://docs.qualcomm.com/doc/87-71417-1/87-71417-1_REV_C_Snapdragon_X_Elite_Product_Brief.pdf) | Computer GPU and memory platform. |
| Q2 | [Qualcomm OpenCL programming guide](https://developer.qualcomm.com/qfile/33472/80-nb295-11_c.pdf) | Adreno arithmetic and memory optimization. |
| Q3 | [Snapdragon X2 Elite brief](https://www.qualcomm.com/content/dam/qcomm-martech/dm-assets/documents/Snapdragon-X2-Elite-Product-Brief.pdf) | X2 GPU variants and advertised APIs. |
| M1 | [Mesa Panfrost documentation](https://docs.mesa3d.org/drivers/panfrost.html) | Mobile architecture mapping and actual driver boundaries. |
| M2 | [Arm Mali Offline Compiler](https://developer.arm.com/Tools%20and%20Software/Mali%20Offline%20Compiler) | Architecture-specific shader qualification. |
| M3 | [Arm Mali-G725](https://www.arm.com/products/silicon-ip-multimedia/gpu/mali-g725), [Mali G1-Ultra](https://www.arm.com/products/silicon-ip-multimedia/gpu/mali-g1-ultra) | 5th Gen family identification. |
| M4 | [PowerVR Rogue](https://www.imaginationtech.com/products/gpu/graphics-architecture/powervr-rogue/), [Furian](https://www.imaginationtech.com/products/gpu/graphics-architecture/powervr-furian/) | Earlier mobile compute/graphics architectures. |
| M5 | [IMG B-Series](https://www.imaginationtech.com/products/gpu/img-b-series-gpu/), [DXTP](https://www.imaginationtech.com/products/gpu/dxtp-gpu/) | Later embedded GPU configurations and image compression. |
