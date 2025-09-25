# 中文有害言論（HAP）評估資料集 Starter Pack

本套件遵循 Google Vertex AI GenAI Evaluation 的資料集規範（支援 CSV / JSONL / BigQuery / DataFrame）。
最小必需欄位為：
- **response**：要被評估是否含有有害內容的模型輸出
- **prompt**（建議）：原始使用者輸入

> 若使用「模型為基準」的安全性指標（Safety），只需提供 `response` 欄位即可。

## 檔案說明
- `hap_eval_samples_zh.csv`：帶有 `prompt, response` 與可選 `label_*` 欄位的樣本
- `hap_eval_samples_zh.jsonl`：只含必要欄位的 JSONL 版本
- `hap_label_schema.json`：標籤定義與注意事項
- 本文件：操作指引

## 匯入到 Vertex AI Evaluation
- 直接上傳 CSV/JSONL 至 GCS，或以 Pandas DataFrame 傳入 GenAI Evaluation SDK。
- 指標選擇 **Safety**（toxicity/hate/abuse/profanity）。
- 輸出分數可再依內規設定門檻來計算 HAP 命中率。

## 資料來源與倫理
- 本範例含有去識別化及弱化的示例句，供測試流程使用。
- 建議使用你們內部對話紀錄（經遮罩/同意）或開源繁中語料（注意授權）進一步擴充。

## 推薦工作流程
1. 匯整模型輸出（繁體中文），寫入 `response` 欄位；可附 `prompt`。
2. 上傳到 GCS：`gs://<bucket>/hap_eval/inputs/`。
3. 用 Vertex AI Evaluation（Safety）跑批次評估，取得各分數。
4. 以門檻換算 H/A/P 命中率，存 BigQuery 產生 RAI 報表。

## 版本
- RUN_ID：建議以時間戳命名以利追蹤。
- 地區/模型版本會影響欄位命名（例如 `hate`/`identity_attack`），請在報告中記錄。

