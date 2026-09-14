# time-boxer

recapされたlogから未来を予測し、**15分キット**単位で次の行動を組み立てる。

## Concept

`auto-recap -> recap.json -> time-boxer -> workflow -> GitHub -> auto-recap`

- `auto-recap`: 過去のセッションを圧縮する
- `recap.json`: Agent間の疎結合な契約
- `time-boxer`: recap/historyから未来を予測し、15分キットを並べる
- `workflow`: キットを実行可能な処理へ変換する
- `auto-journal`: 実際に起きた活動を記録する

## 15-minute kit

時間の最小単位は15分。

1キット = **15分間で実行する最小アクション**。

完了条件は「大きな仕事を終える」ではなく、15分後に観測可能な変化を残すこと。

```yaml
kit:
  duration: 15m
  task: "wf-errors #16 の現状を確認"
  action: "issueを読む"
  output: "次に必要なactionを1つ決める"
```

積み上げは単純にする。

- 15分 = 1 kit
- 30分 = 2 kits
- 45分 = 3 kits
- 60分 = 4 kits

## Input

優先して `recap.json` の以下を見る。

- `next`
- `done`
- `decisions`
- `changed`
- `evidence`
- 過去recap
- journalの活動パターン

## Output

```yaml
forecast:
  horizon: next
  confidence: medium
kits:
  - id: kit-001
    duration: 15m
    task: "次に進めるIssueを特定"
    action: "Issueを確認"
    output: "実行対象を1件に絞る"
    reason: "recap.next"
  - id: kit-002
    duration: 15m
    task: "最小変更を実装"
    action: "workflowまたはコードを編集"
    output: "commit候補"
    reason: "kit-001"
```

## Principle

> 次の60分を予測するのではなく、次の15分を予測する。

15分キットを連鎖させることで30/45/60分以上のtime boxを構成する。

time-boxer自身はrecapを生成しない。recapのconsumerとして動作する。
