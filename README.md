```mermaid
graph LR
    subgraph Instruments [儀器層 (Instrument Layer)]
        VNA[VNA 向量網路分析儀<br/>Port 1 / Port 2]
        SA[SA 頻譜分析儀<br/>Rx Only]
        SMIQ[SMIQ03B 訊號產生器<br/>Tx Only]
        SG[SG 一般訊號產生器<br/>Tx / LO Source]
        CMW[CMW270 綜合測試儀<br/>TRx]
    end

    subgraph Matrix [核心切換與訊號處理矩陣 (Switch & Processing Matrix)]
        SW_IN{NxM 無阻塞<br/>RF 開關矩陣<br/>SP6T / 矩陣開關}
        Mixer((混波器 Mixer<br/>Up/Down Conversion))
        SW_OUT{DUT 端<br/>路徑切換開關}
    end

    subgraph DUT_Interface [待測物端 (DUT Fixed Interface)]
        DUT_P1((固定接頭 Port 1<br/>Main / Ant 1))
        DUT_P2((固定接頭 Port 2<br/>Aux / Ant 2))
    end

    VNA <-->|雙向 S 參數| SW_IN
    SA <---|接收路徑| SW_IN
    SMIQ --->|發射路徑| SW_IN
    SG --->|發射路徑 / LO| SW_IN
    CMW <-->|通訊協定收發| SW_IN

    SW_IN <-->|Direct RF Bypass (直通測試)| SW_OUT
    SW_IN --->|IF/RF 訊號| Mixer
    SG -.->|提供 LO 給 Mixer| Mixer
    Mixer --->|升/降頻後訊號| SW_OUT

    SW_OUT <--> DUT_P1
    SW_OUT <--> DUT_P2

    classDef inst fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef matrix fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef dut fill:#e8f5e9,stroke:#388e3c,stroke-width:2px;
    
    class Instruments inst;
    class Matrix matrix;
    class DUT_Interface dut;
