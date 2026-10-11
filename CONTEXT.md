# Skills

一套跨 repo、跨 agent 通用的個人開發 skills，從想法到 commit。參考 mattpocock/skills 的流程設計。

## Language

### 流程

**Planning**:
把想法變成 Plan 的階段，由規劃 skill 負責，第一步是 grilling。
_Avoid_: 事前規劃、spec 階段

**Implement**:
把 Plan 變成已 commit 程式碼的階段，涵蓋 openspec-apply 的 task 迴圈與 mattpocock implement 的 TDD、review、commit。
_Avoid_: apply、build

### 交接

**Plan**:
Planning 交給 Implement 的東西，不論採用哪種後端。
_Avoid_: artifacts、spec、ticket

**Artifact**:
OpenSpec change 裡的單一檔案（proposal、design、specs、tasks）。只用於 OpenSpec backend。

**Plan contract**:
任何 Plan 都必須滿足的最小內容：Seams、Slices、驗收條件。後端只決定這些內容寫在哪裡。
_Avoid_: 格式、template

**Seam**:
測試所在的公開介面，在 Planning 階段與人確認，Implement 只在這裡寫測試。

**Slice**:
Plan 中可獨立驗證的一段垂直工作，附帶自己的驗證方式。
_Avoid_: task group、ticket、step

### 設定

**Setup**:
每個 repo 執行一次，把該 repo 的 Plan store 與 Issue tracker 寫成設定檔。其他 skills 只描述動作（寫入 Plan、讀取 Plan），具體做法從設定檔讀取；缺設定時停下，不自行偵測。

**Plan store**:
Plan 存放的地方：OpenSpec（openspec change，commit 進 repo）、GitHub issues 或 GitLab issues。
_Avoid_: 規劃後端、backend

**Issue tracker**:
bug 與外部需求存放的地方（GitHub issues 或 GitLab issues），與 Plan store 分開設定。
_Avoid_: Plan store
