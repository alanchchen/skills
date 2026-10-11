# Plan store 與 Issue tracker 分開，由 Setup 寫入設定

每個 repo 由 Setup 分別設定 Plan store（OpenSpec、GitHub、GitLab）與 Issue tracker（GitHub、GitLab），寫入 `.agents/`。其他 skills 只讀設定、只描述動作；缺設定時停下，請使用者先跑 Setup。之所以分開，是因為 openspec 適合存放 Plan，卻不適合追蹤 bug，一個 repo 常常是 Plan 用 openspec、bug 用 GitHub issues。

## Considered Options

- **依 `openspec/` 目錄自動偵測**：省去 Setup，但偵測邏輯會散落在每個 skill，行為難以預測。偵測只保留在 Setup，當作建議的預設值。
- **照 mattpocock 只設定單一 issue tracker**：設定比較簡單，但會把 Plan 和 bug 綁在同一個地方。
- **plan mode 或本地 markdown 檔作為 Plan store**：plan mode 只有部分 agent 支援，產出的檔案也不在 repo 內。原本由 plan mode 承擔的「先規劃再動手」改由 grilling 負責。
