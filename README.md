# memo
## run 部分の出力
```bash
echo "Hello from $GITHUB_REF on ${{ runner.os }}"
echo "Actor: $GITHUB_ACTOR"
pwd && ls -la

# 出力結果
Hello from refs/heads/main on Linux
Actor: naon708
/home/runner/work/lean_github_actions/lean_github_actions
total 20
drwxr-xr-x 4 runner docker 4096 Aug  9 01:29 .
drwxr-xr-x 3 runner docker 4096 Aug  9 01:29 ..
drwxr-xr-x 7 runner docker 4096 Aug  9 01:29 .git
drwxr-xr-x 3 runner docker 4096 Aug  9 01:29 .github
-rw-r--r-- 1 runner docker   71 Aug  9 01:29 README.md
```

## Artifact の保存期間
> パブリックリポジトリの場合: この保持時間を1日から90日の間で変更できます。
> 
> プライベートのリポジトリの場合: この保持期間を 1 から 400 日の間で変更できます。

日次実行だったら余裕。

## スケジュールjobの出力
```shell
Run at (UTC): 2025-08-09T02:23:42Z
Run at (JST): 2025-08-09 11:23:42 JST
Commit: 41770804bf8de5f59b0e2bce9f68d455d204dfb8
Branch/Ref: refs/heads/main
Runner: Linux
```

## 課題

### 事象
* `schedule`（cron）で **10分おき指定でも実行時刻にバラつき**（数分〜十数分の遅延）。

### 原因

* GitHub Actions のスケジューラは **厳密な時刻保証なし**。負荷や混雑（特に毎時00分付近）で遅延や取りこぼしが起き得る。
* そもそもの最短間隔は **5分**（これ未満は不可）。

### 対処

* **混雑回避のオフセット**：0分・5の倍数を避けてズラす。
  * `*/10` は「毎時 0,10,20,30,40,50 分に実行」という意味。
  * `3,13,23,33,43,53` のように10分おきだけど開始を3分にズラすことで、世界中の `*/10（0,10,20…）`に集中するタイミングを避ける。
  ```yaml
  on:
    schedule:
      - cron: '3,13,23,33,43,53 * * * *'  # 10分おき（3分オフセット）
  ```
* **厳密さが必須なら**：外部スケジューラ（EventBridge / Cloud Scheduler / 自前cron 等）から
  **workflow\_dispatch API** を叩いて起動する運用に切り替える。
