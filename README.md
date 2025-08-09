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
