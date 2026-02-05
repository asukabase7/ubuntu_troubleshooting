# Ubuntu Kernel Panic Recovery & Hardening Log (Dell XPS 13 9310)

## 1. 発生した問題 (Symptoms)
Ubuntu 24.04にて、カーネルアップデート後に以下の多層的な障害が発生。

### Phase 1: 起動時の KERNEL PANIC
- **症状:** `KERNEL PANIC! Attempted to kill init! exitcode=0x00000007`
![Kernel Panic Init](images/panic_init.png)

### Phase 2: 描画異常とシステムハング
- **症状:** マウスカーソル矩形化および極度の低速化
![Square Cursor Bug](images/cursor_bug.png)

### Phase 3: 起動イメージ破損
- **症状:** `VFS: Unable to mount root fs on unknown-block(0,0)`
![Kernel Panic VFS](images/panic_vfs.png)

## 2. 実行環境 (Environment)
- **OS:** Ubuntu 24.04 LTS
- **Hardware:** Dell XPS 13 9310
- **Kernel Versions:** 6.14.0-37 (Problematic) / 6.14.0-27 (Stable)

## 3. 解決手順 (Full Solutions)

### A. ブートローダーの暫定復旧と永続化
1. GRUBメニューの `Advanced options` から旧カーネル（-27）を選択し緊急起動。
2. `/etc/default/grub` を編集し、安定環境を固定：
   - `GRUB_DEFAULT=saved`
   - `GRUB_SAVEDEFAULT=true`
   - `GRUB_TIMEOUT=5`（リカバリ猶予の確保）
3. `sudo update-grub` を実行し反映。

### B. 破損したブートイメージの修復
強制終了に伴うファイルシステム不整合を解消するため、initramfsを再生成。
\`\`\`bash
sudo update-initramfs -u -k all
sudo update-grub
\`\`\`

## 4. 学び (Key Takeaways)
- **可用性の設計:** `TIMEOUT=0` 設定のリスクと、リカバリ猶予を設ける重要性を痛感した。
- **多角的な復旧:** カーネルパニックだけでなく、描画異常やファイルシステム破損が連鎖する際の切り分け手法を習得した。
- **インフラ管理:** 常に「動く状態」をシステムに記憶させる設定の有用性を学んだ。
