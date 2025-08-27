# ASTRA-Sim Tutorials

## MICRO 2024 Tutorial - 分散式訓練系統模擬完整指南

本教學為 MICRO 2024 會議的完整教學材料，涵蓋 ASTRA-Sim 分散式訓練模擬器和 Chakra 執行追蹤格式的深度學習與示範。適合研究人員、工程師和學生了解現代分散式深度學習系統的效能建模與最佳化。

### 🎯 學習目標

- **系統建模**: 學習如何為大規模分散式訓練系統建立精確的效能模型
- **工作負載分析**: 掌握深度學習工作負載的特性分析和追蹤方法
- **效能最佳化**: 了解網路拓撲、集體通訊演算法對訓練效率的影響
- **實務應用**: 透過真實案例學習系統設計的權衡考量

### 📋 前置需求

- **硬體**: 建議 8GB+ RAM，多核心處理器
- **軟體**: Docker、Python 3.8+、基本 Linux 指令知識
- **背景知識**: 分散式系統基礎、深度學習訓練流程概念

### 🗂️ 目錄結構

```
micro2024/
├── 📁 環境設置腳本
│   ├── clone_repos.sh              # 複製核心程式碼庫和依賴項目
│   ├── install_chakra.sh           # 安裝 Chakra 追蹤工具鏈
│   ├── compile_astra_sim.sh        # 編譯 ASTRA-Sim (analytical backend)
│   └── compile_astra_sim_ns3.sh    # 編譯 ASTRA-Sim (ns-3 詳細網路模擬)
├── 📁 astra-sim-demo/             # ASTRA-Sim 核心模擬示範
│   ├── demo1/ 🔄                  # All-Reduce 基礎集體通訊
│   ├── demo2/ 🚀                  # DGX-H100 高效能系統模擬
│   └── demo3/ ⚖️                   # 1D vs 2D 演算法效能比較
└── 📁 chakra-demo/                # Chakra 追蹤生態系統
    ├── demo1/ 🔧                  # 程式化追蹤生成
    ├── demo2/ 📝                  # 文字描述轉追蹤格式
    ├── demo3/ 📊                  # 追蹤視覺化與分析
    └── demo4/ 🔄                  # PyTorch 生產追蹤轉換
```

### ⚡ 快速開始

#### 步驟 1: 環境準備
```bash
# 使用官方 Docker 映像檔 (推薦)
docker pull astrasim/tutorial-micro2024
docker run -it -v ./tutorials/micro2024:/app/micro2024 astrasim/tutorial-micro2024

# 或手動設置環境
cd tutorials/micro2024/
./clone_repos.sh        # 複製所需程式碼庫
./install_chakra.sh     # 安裝 Chakra 套件
```

#### 步驟 2: 編譯模擬器
```bash
# 快速分析模型 (適合初學者)
./compile_astra_sim.sh

# 詳細網路模擬 (適合進階研究)
./compile_astra_sim_ns3.sh
```

#### 步驟 3: 執行第一個示範
```bash
cd astra-sim-demo/demo1/
./run_demo1-1.sh  # 體驗 8 節點 All-Reduce 模擬
```

## 🧪 ASTRA-Sim 模擬示範

### Demo 1: 分散式 All-Reduce 基礎 🔄
> **學習重點**: 集體通訊基礎、網路拓撲影響、壅塞控制機制

**模擬場景**:
- **系統配置**: 8 節點 Ring 拓撲網路
- **通訊模式**: All-Reduce 集體通訊操作
- **分析角度**: 壅塞感知 vs 非壅塞感知效能差異

**核心概念**:
- Ring All-Reduce 演算法運作原理
- 網路頻寬利用率分析
- 通訊延迟與吞吐量權衡

```bash
cd astra-sim-demo/demo1/
./run_demo1-1.sh  # 壅塞感知模式 - 真實網路行為模擬
./run_demo1-2.sh  # 非壅塞感知模式 - 理想化效能基準
```

**預期學習成果**:
- 理解集體通訊對分散式訓練的重要性
- 掌握網路壅塞對效能的實際影響
- 學會解讀 ASTRA-Sim 輸出結果

---

### Demo 2: DGX-H100 企業級系統模擬 🚀
> **學習重點**: 真實硬體建模、MLP 工作負載特性、高效能互連網路

**模擬場景**:
- **硬體平台**: NVIDIA DGX-H100 (32 GPU 配置)
- **工作負載**: 多層感知器 (MLP) 模型並行訓練
- **網路架構**: NVLink/InfiniBand 高速互連

**技術深度**:
- DGX-H100 系統架構細節
- 模型並行與資料並行混合策略
- GPU 記憶體階層與通訊最佳化

```bash
cd astra-sim-demo/demo2/
./run_demo2-1.sh  # 標準配置效能基準測試
./run_demo2-2.sh  # 最佳化配置效能分析
```

**實務價值**:
- 學習大規模 GPU 叢集的效能預測
- 理解現代 AI 硬體的設計考量
- 掌握工作負載與硬體的匹配原則

---

### Demo 3: 演算法比較與最佳化 ⚖️
> **學習重點**: 演算法選擇策略、擴展性分析、系統設計權衡

**比較維度**:
- **1D All-Reduce**: 傳統線性演算法
- **2D All-Reduce**: 階層式最佳化演算法
- **效能指標**: 延遲、頻寬利用率、擴展性

**分析重點**:
- 不同拓撲下的演算法適用性
- 系統規模對演算法選擇的影響
- 理論最佳與實際效能的差距

```bash
cd astra-sim-demo/demo3/
./run_demo3-1.sh  # 1D All-Reduce 基準測試
./run_demo3-2.sh  # 2D All-Reduce 效能分析
./run_demo3-3.sh  # 比較分析與視覺化
```

**深度洞察**:
- 演算法複雜度與實際效能的關聯
- 大規模系統設計的關鍵決策點
- 效能建模的準確性驗證方法

## 📊 Chakra 追蹤生態系統深度探索

### Demo 1: 程式化追蹤生成 🔧
> **核心技能**: 追蹤格式設計、程式化工作負載建模

**技術亮點**:
- **Protobuf 序列化**: 高效能追蹤格式設計
- **多 NPU 協調**: 8 個處理單元的同步追蹤生成
- **可擴展架構**: 支援任意規模的系統建模

**實作內容**:
```python
# 使用 Chakra API 程式化生成 All-Reduce 追蹤
from chakra.src.third_party.utils.protolib import encodeMessage
from chakra.schema.protobuf.et_def_pb2 import *

# 生成標準 All-Reduce 操作追蹤
python3 generate_all_reduce.py  # 產生 8 個 NPU 的追蹤檔案
```

```bash
cd chakra-demo/demo1/
./run_demo1-1.sh  # 執行追蹤生成和驗證
```

**學習成果**:
- 掌握 Chakra 追蹤格式的內部結構
- 學會自定義工作負載的追蹤生成
- 理解分散式系統的事件同步機制

---

### Demo 2: 工作負載描述語言 📝
> **核心技能**: DSL 設計、自動化追蹤轉換

**創新特色**:
- **文字格式 DSL**: 人類可讀的工作負載描述語言
- **自動轉換器**: Text → Chakra ET 格式無縫轉換
- **多 NPU 支援**: 最高支援 32 個處理單元

**實用範例**:
```bash
cd chakra-demo/demo2/
./run_demo2.sh      # 文字描述轉 Chakra 格式
./visualize.sh      # 工作負載視覺化分析
```

**核心優勢**:
- 大幅降低追蹤生成的技術門檻
- 支援快速原型開發和實驗迭代
- 便於研究人員專注於演算法設計

---

### Demo 3: 進階追蹤分析與視覺化 📊
> **核心技能**: 效能分析、視覺化設計、最佳化識別

**分析維度**:
- **時間序列分析**: 操作執行時間線視覺化
- **資源利用率**: CPU/GPU/記憶體使用模式
- **通訊模式**: 資料流和依賴關係圖譜
- **瓶頸識別**: 效能限制因子自動檢測

```bash
cd chakra-demo/demo3/
./run_demo3.sh  # 啟動互動式分析環境
```

**進階功能**:
- 動態追蹤過濾和聚合
- 多維度效能指標比較
- 自動化最佳化建議生成

---

### Demo 4: 生產環境整合 🔄
> **核心技能**: 真實系統追蹤、格式轉換、生產部署

**企業級功能**:
- **PyTorch 原生支援**: 直接從 PyTorch ET+ 格式轉換
- **大規模追蹤處理**: 支援 TB 級追蹤檔案
- **批次處理管線**: 自動化追蹤處理工作流程

**實務應用**:
```bash
cd chakra-demo/demo4/
./convert.sh    # PyTorch → Chakra 格式轉換
./merge.sh      # 多 GPU 追蹤檔案合併處理
```

**產業價值**:
- 與現有 MLOps 工具鏈無縫整合
- 支援大規模生產環境的效能監控
- 提供端到端的追蹤分析解決方案

## 🌟 核心技術特色

### 🔬 多後端模擬引擎
- **Analytical Backend**: 快速模型原型驗證，適合演算法設計初期
- **NS-3 Backend**: 詳細網路模擬，提供封包級別的精確度分析
- **混合模式**: 根據研究需求靈活選擇模擬精度與速度平衡

### 🏢 真實系統建模精度
- **DGX-H100**: 完整的 NVIDIA 企業級系統建模
- **硬體拓撲**: 精確反映 NVLink、PCIe、InfiniBand 互連架構
- **記憶體階層**: 包含 HBM、GDDR、系統記憶體的完整建模

### 📋 完整追蹤工具鏈
- **多格式支援**: PyTorch、TensorFlow、自定義格式無縫轉換
- **可擴展設計**: 支援從單機到數千節點的大規模系統
- **標準化格式**: Chakra ET 成為業界標準追蹤格式

### 📈 專業級視覺化
- **即時分析**: 追蹤生成過程中的即時效能監控
- **多維度視圖**: 時間線、資源利用率、通訊拓撲多角度分析
- **自動化報告**: 一鍵生成專業級效能分析報告

## 🎓 進階學習路徑

### 初學者路徑 (1-2 週)
1. **環境設置** → 熟悉 Docker 環境和基本工具
2. **Demo 1** → 理解集體通訊基礎概念
3. **Chakra Demo 1** → 學習追蹤格式基礎
4. **文檔閱讀** → 深入理解核心概念

### 中級開發者路徑 (2-4 週)
1. **所有 Demo 完成** → 全面掌握工具功能
2. **自定義工作負載** → 建立專屬的效能測試案例
3. **效能調優** → 學習系統最佳化技巧
4. **論文閱讀** → 研究相關學術文獻

### 進階研究者路徑 (1-3 個月)
1. **原始碼分析** → 深入理解模擬器內部機制
2. **擴展開發** → 開發新的模擬後端或追蹤格式
3. **學術合作** → 參與開源社群和研究協作
4. **論文發表** → 基於研究成果撰寫學術論文

## 🔧 故障排除與最佳實務

### 常見問題解決
```bash
# 記憶體不足問題
export ASTRA_SIM_MEMORY_LIMIT=8G

# 編譯錯誤解決
./clean_build.sh && ./compile_astra_sim.sh

# 權限問題修復
sudo chown -R $USER:$USER ./astra-sim/
```

### 效能最佳化建議
- **小規模測試**: 先用小型配置驗證正確性
- **增量擴展**: 逐步增加系統規模和複雜度
- **結果驗證**: 交叉比對不同後端的模擬結果
- **文檔記錄**: 詳細記錄實驗參數和結果

## 📚 延伸資源

### 🎥 教學影片
- [MICRO 2024 Tutorial 完整講座](https://micro2024.github.io/)
- [ASTRA-Sim 深度技術講解系列](https://astra-sim.github.io/tutorials/)

### 📖 學術文獻
- **ASTRA-Sim 核心論文**: "ASTRA-Sim: Enabling SW/HW Co-Design Exploration for Distributed ML Training Platforms"
- **Chakra 格式規範**: "Chakra: A Comprehensive Execution Trace Format for ML Systems"
- **分散式訓練最佳化**: "Efficient Large-Scale Distributed Training Architectures"

### 🤝 社群參與
- **GitHub Issues**: [回報問題和功能請求](https://github.com/astra-sim/astra-sim/issues)
- **討論區**: [技術討論和經驗分享](https://github.com/astra-sim/astra-sim/discussions)
- **學術會議**: MICRO、ISCA、ASPLOS、MLSys 等頂級會議追蹤

## 🔗 相關連結與資源

### 官方專案
- 🏠 [ASTRA-Sim 官方網站](https://astra-sim.github.io/)
- 📦 [ASTRA-Sim GitHub 倉庫](https://github.com/astra-sim/astra-sim)
- 🧪 [Chakra 追蹤格式專案](https://github.com/facebookresearch/chakra)
- 🔬 [PARAM 效能分析工具](https://github.com/facebookresearch/param)
- 📊 [符號張量圖生成器](https://github.com/astra-sim/symbolic_tensor_graph)

### 技術文檔
- 📋 [完整 API 文檔](https://astra-sim.readthedocs.io/)
- 🛠️ [開發者指南](https://github.com/astra-sim/astra-sim/wiki)
- 📖 [最佳實務手冊](https://astra-sim.github.io/best-practices/)

### 學術資源
- 🎓 [研究論文集](https://astra-sim.github.io/publications/)
- 🏆 [獎項與認可](https://astra-sim.github.io/awards/)
- 📅 [相關會議和研討會](https://astra-sim.github.io/events/)

---

## 🙏 致謝

本教學材料由 ASTRA-Sim 開發團隊和 MICRO 2024 會議組委會共同製作。特別感謝：

- **Georgia Institute of Technology** - 核心開發團隊
- **Meta AI Research** - Chakra 格式設計與實作
- **NVIDIA Research** - DGX 系統建模支援
- **學術社群** - 寶貴的回饋和貢獻

---

## 📞 聯絡與支援

- **技術問題**: [GitHub Issues](https://github.com/astra-sim/astra-sim/issues)
- **功能建議**: [GitHub Discussions](https://github.com/astra-sim/astra-sim/discussions)
- **學術合作**: astra-sim-dev@lists.gatech.edu
- **商業支援**: 請透過官方網站聯絡
