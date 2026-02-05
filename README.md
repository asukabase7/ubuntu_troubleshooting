# Ubuntu Kernel Panic Recovery Log (Dell XPS 13 9310)

## 1. 発生した問題 (Symptom)
Ubuntu 24.04において、起動時に以下のエラーが発生し、システムが完全に停止。
- **Error:** `KERNEL PANIC! Attempted to kill init! exitcode=0x00000007`

## 2. 実行環境 (Environment)
- **OS:** Ubuntu 24.04 LTS
- **Hardware:** Dell XPS 13 9310
- **Problematic Kernel:** 6.14.0-37-generic

## 3. 原因特定 (Root Cause Analysis)
最新のカーネルバージョンのアップデート不備、あるいはハードウェアとの相性問題により、初期プロセス（init）の起動に失敗。

## 4. 解決手順 (Solution)
1. GRUBメニューの `Advanced options` から旧カーネル `6.14.0-27-generic` で起動。
2. `/etc/default/grub` を編集し、`GRUB_DEFAULT=saved` と `GRUB_SAVEDEFAULT=true` を設定。
3. `sudo update-grub` で設定を反映。

## 5. 学び (Key Takeaways)
- 旧カーネルを残しておくことで、パニック時も即座に復旧が可能。
- GRUB設定により、安定した環境をデフォルトに固定する重要性を学んだ。
