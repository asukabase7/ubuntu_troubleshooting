# Ubuntu Kernel Panic Recovery & Hardening Log (Dell XPS 13 9310)

## 1. 発生した問題 (Symptoms)

### Phase 1: 起動時の KERNEL PANIC
- **症状:** `KERNEL PANIC! Attempted to kill init! exitcode=0x00000007`
*(※この段階の画像は現在捜索中)*

### Phase 2: 描画異常とシステムハング
- **症状:** マウスカーソル矩形化および極度の低速化。Wayland環境でのドライバ不整合が疑われる。
![Square Cursor Bug](images/cursor_bug.png)

### Phase 3: 起動イメージ破損
- **症状:** 強制終了により `initramfs` が破損し、`VFS: Unable to mount root fs` が発生。
![Kernel Panic VFS](images/panic_vfs.png)

---

## 2. 復旧プロセス (Recovery Process)

トラブルシューティング中に使用した特殊なブート環境の記録です。

| Dell XPS 13 9310 Boot Menu | GRUB Rescue Shell |
| :---: | :---: |
| ![Dell Menu](images/dell_boot_menu.png) | ![GRUB Shell](images/grub_shell.png) |

---

## 3. 解決手順 (Final Solutions)

1. **安定カーネルの選択:** GRUBの `Advanced options` から旧バージョン（-27）で起動。
2. **ブート設定の永続化:** `/etc/default/grub` を編集し、`GRUB_DEFAULT=saved` を適用。
3. **イメージの再生成:** `sudo update-initramfs -u -k all` で全カーネルの起動ファイルを修復。

## 4. 復旧後の確認
日本語入力環境（Fcitx5）のWayland最適化診断を確認し、正常動作を担保。
![Wayland Diagnosis](images/wayland_diagnosis.png)
