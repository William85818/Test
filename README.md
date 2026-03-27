graph LR
    subgraph ClientLayer ["用戶端 (Client)"]
        WebUI["測試控管網頁端"]
        AutoScript["自動化 Python 腳本"]
    end

    subgraph CoreProcessing ["核心處理區 (Core Processing)"]
        TestEngine["測試執行引擎"]
        DataParser["數據解析模組"]
        PassFail{"判定邏輯<br>Pass / Fail"}
        ResultDB[("驗證結果資料庫")]
    end

    subgraph ExternalAPI ["外部 API (External API)"]
        IssueTracker["缺陷追蹤 API<br>(例如 Jira)"]
        InstrumentCtrl["儀器控制 API<br>(SCPI over TCP/IP)"]
    end

    %% 連接線與文字說明
    WebUI -->|"REST API 觸發測試"| TestEngine
    AutoScript -->|"JSON over REST"| TestEngine
    
    TestEngine -->|"發送控制指令"| InstrumentCtrl
    InstrumentCtrl -->|"回傳測試 Raw Data"| DataParser
    
    DataParser -->|"提取關鍵參數"| PassFail
    
    PassFail -->|"測試通過 (寫入紀錄)"| ResultDB
    PassFail -->|"測試失敗 (寫入紀錄)"| ResultDB
    PassFail -->|"Webhook 建立 Bug Ticket"| IssueTracker

    %% 莫蘭迪色系自定義樣式 (classDef)
    %% blueNode: 淺藍色背景、深藍邊框
    classDef blueNode fill:#D6E4F0,stroke:#4B6584,stroke-width:2px,color:#2C3E50;
    %% orangeNode: 粉橘色背景、橘色邊框
    classDef orangeNode fill:#FDEBD0,stroke:#E67E22,stroke-width:2px,color:#873600;
    %% 資料庫專用
    classDef dbNode fill:#E5E7E9,stroke:#7F8C8D,stroke-width:2px,color:#2C3E50;
    %% 判斷邏輯專用
    classDef decisionNode fill:#E8DAEF,stroke:#8E44AD,stroke-width:2px,color:#4A235A;

    %% 指派樣式給對應節點
    class WebUI,AutoScript,TestEngine,DataParser blueNode;
    class IssueTracker,InstrumentCtrl orangeNode;
    class ResultDB dbNode;
    class PassFail decisionNode;
