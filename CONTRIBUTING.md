# Contributing

Issues and pull requests are welcome in English or Chinese.

Keep the app easy to download and run: one HTML file, no build step, and no external dependencies. Preserve local-only photo handling; avoid adding analytics, uploads, or external assets.

For a bug report, include the device, OS/browser version, steps to reproduce, and expected versus actual behavior. Use synthetic or freely shareable images rather than personal photos.

## Run locally

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000/` on your computer or `http://<computer-local-IP>:8000/` in Safari on an iPad on the same local network.

## Manual smoke test

1. Select several JPEG or PNG images. Verify the first photo, filename, and counter.
2. Exercise previous/next, pause/resume, and sequential/random playback.
3. Set a custom interval such as 5 seconds. Wait for controls to hide, then verify that automatic advancement does not reveal them. Tap the image to reveal them again.
4. Switch between fit and fill with portrait and landscape images.
5. Replace the photo selection, then try a single image and an empty picker cancellation.
6. Check fullscreen success or the graceful fallback message on your browser.
7. Switch to another tab and back; verify playback resumes. Reload and confirm that photos must be selected again.
8. After loading the page and photos, stop the server and check that the live page continues playing.
9. Inspect network activity: selecting and playing photos should not upload them or request external assets.

For UI or browser behavior changes, report the devices you actually tested. Desktop testing alone does not establish iPad Safari compatibility.

By submitting a contribution, you agree that your contribution is licensed under this repository's MIT License.

## 中文

欢迎用中文或英文提交 Issue / PR。请保持单文件、零依赖、本地读取照片的设计。提交问题时说明设备、系统、浏览器版本和复现步骤；请勿附带不适合公开的私人照片。修改播放逻辑或界面后，按上面的清单检查，并注明实际测试过的设备。
