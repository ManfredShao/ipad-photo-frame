# iPad Photo Frame

A tiny, local-first photo frame for iPad Safari. One HTML file. No dependencies. Your photos stay on your device.

[中文说明](#中文说明) · [Getting started](#getting-started) · [Privacy](#privacy) · [Contributing](CONTRIBUTING.md)

## Why this exists

Sometimes you just want a photo to stay on screen for five minutes. This project grew out of that simple wish: turn an iPad into a quiet photo frame, with an exact slideshow interval instead of vague “short / medium / long” choices.

Choose some photos, set the pace, and let them play. No account, cloud album, subscription, or build step.

What started as a five-minute slideshow interval somehow turned into local HTTP serving, Safari debugging, and an open-source project.

Thanks, Apple.

## Features

- **Custom timing:** 30 seconds, 1 minute, 5 minutes, 10 minutes, or a custom interval from 1 to 86,400 seconds.
- **Sequential or random playback**, with previous/next and play/pause controls. Sequential order follows the files returned by the picker.
- **Fit or fill:** show the whole photo with space around it, or fill the screen by cropping its edges.
- **Quiet controls:** controls hide after 3.5 seconds during playback. Automatic photo changes keep them hidden; tap the photo to reveal or hide them.
- **Local photos:** choose multiple images using the device's file/photo picker. The app displays them through local browser object URLs.
- **Single-file app:** HTML, CSS, and JavaScript together, with no external libraries, fonts, or services.
- **Fullscreen when supported**, safe-area-aware layout, and reduced-motion support.

The interface is currently in Simplified Chinese; the control guide below provides English translations.

## Getting started

### 1. Download and serve the app

Clone this repository, or use GitHub's **Code → Download ZIP** and extract it. On your Mac or another computer with Python 3, open a terminal in the project folder:

```bash
git clone https://github.com/ManfredShao/ipad-photo-frame.git
cd ipad-photo-frame
python3 -m http.server 8000
```

Open `http://localhost:8000/` on that computer to check the page. Run the server from the project folder, since it serves the files in that directory.

### 2. Open it on your iPad

1. Connect the computer and iPad to the same local network.
2. Find the computer's local IP address in its network settings.
3. In **iPad Safari**, enter `http://<computer-local-IP>:8000/`, replacing the placeholder with the actual address. For example: `http://192.168.1.20:8000/`.
4. Tap **选择照片** (Choose photos), select images, and set your preferred interval.

Use the complete **`http://`** address: this Python command serves HTTP, not HTTPS. Keep the computer awake and the server running while loading or reloading the page. Stop the server with **Ctrl+C** when finished.

### 3. Make it comfortable

Tap **全屏** (Fullscreen) if your browser supports it. You can also try Safari's **Share → Add to Home Screen** for a more app-like launch. This does not install an offline cache; loading the app again may still require the server. Browser and system bars may remain visible depending on iPadOS and the launch mode.

For extended viewing, adjust your iPad's Auto-Lock setting if needed. The app does not request a screen wake lock or prevent the device from sleeping.

## Controls

| Label | Meaning |
| --- | --- |
| 选择照片 / 换照片 | Choose / replace photos |
| ‹ / › | Previous / next photo |
| Ⅱ / ▶ | Pause / play |
| 自定义 | Custom interval, in seconds |
| 顺序 / 随机 | Sequential / random playback |
| 适应 / 填满 | Fit whole image / fill with cropping |
| 全屏 / 退出全屏 | Enter / exit fullscreen |

Replacing the selection releases the previous photos' object URLs. Reloading or closing the page clears the selection and settings; choose your photos again next time.

## Privacy

**The app does not upload your selected photos.** It uses the browser's file picker and `URL.createObjectURL()` to display images locally. There are no accounts, analytics, trackers, upload endpoints, or external assets. Its Content Security Policy blocks network connections initiated by scripts (`connect-src 'none'`).

The HTTP server delivers the app's HTML; it does not receive the photos you choose on your iPad. The page does not save your selection in local storage or IndexedDB. Your operating system's picker may separately download an image from iCloud if it is not already stored on the device.

Once the page and selected photos are loaded, playback can continue without the server while that page remains alive. This is **not guaranteed offline installation**: a refresh, browser eviction, or closing the tab requires loading the app and selecting photos again.

## Why HTTP instead of opening the HTML in Files?

Opening a local HTML file from the iPad Files app may lead to a preview rather than the normal Safari page experience. File picking and script behavior can differ in that context. Serving the same file over HTTP gives Safari a regular page URL and is the intended workflow for this project.

## Troubleshooting and limitations

- **Safari cannot establish a secure connection:** check that the address starts with `http://`, not `https://`.
- **The computer can open the page but the iPad cannot:** check the local IP, computer firewall, and whether the Wi-Fi permits devices to communicate. Campus or guest networks may isolate clients. A shared personal hotspot can provide another local network.
- **You changed Wi-Fi networks:** the computer's IP may have changed. Recheck its address before reloading the iPad page.
- **A photo cannot be displayed:** format support depends on the browser and OS. Try JPEG or PNG; accepting a file in the picker does not guarantee Safari can decode it. An image error stops automatic advancement until you move to another photo or resume playback.
- **Very large albums:** selected files and decoded images consume device memory. Start with a modest selection, especially on older iPads.
- **Background tabs or a locked screen:** browsers may suspend playback. This app pauses its slideshow timer while the document is hidden.
- **No persistent albums or preferences:** there is no backend, service worker, or saved photo library.

## Development

Edit `index.html` and reload the page served by the command above. There is no package manager or build pipeline. See [CONTRIBUTING.md](CONTRIBUTING.md) for a manual smoke-test checklist and contribution guidance.

## 中文说明

**把 iPad 变成一个安静的电子相框。** 项目起因很简单：只想让一张照片停留五分钟，并且能准确设置轮播时间。

- 单个 HTML 文件，零依赖，无需注册。
- 支持 30 秒、1 分钟、5 分钟、10 分钟，以及 1～86400 秒的自定义间隔。
- 支持顺序 / 随机播放、上一张 / 下一张、播放 / 暂停。
- 支持适应 / 填满：适应保留完整画面；填满可能裁切边缘。
- 播放时控制栏在 3.5 秒后自动隐藏；自动换图不会弹出控制栏，轻点照片可切换显示。
- 照片由 iPad 本地选择和显示，**不会被本应用上传到服务器**。关闭或刷新页面后，需要重新选照片。

### 使用方法

下载仓库，在项目文件夹中运行：

```bash
python3 -m http.server 8000
```

让 Mac 和 iPad 连接同一局域网，在 iPad Safari 中访问 `http://你的电脑局域网IP:8000/`，点击“选择照片”即可开始。请完整输入 `http://`，这个命令不提供 HTTPS。

建议通过 HTTP 服务访问，因为 iPad“文件”App 打开 HTML 时可能进入预览，而不是完整的 Safari 网页环境。校园网或访客 Wi-Fi 如果隔离设备，可能需要换到允许设备互访的网络。

页面和照片加载完成后，只要当前页面仍然存活，停止电脑上的服务通常不会影响播放；刷新、关闭或被系统回收后，需要重新打开服务并选择照片。添加到主屏幕不等于安装离线版。全屏效果与状态栏是否保留取决于系统和浏览器；长时间展示时请按需调整 iPad 自动锁定设置。

应用界面目前为简体中文，欢迎贡献其他语言支持。图片格式兼容性取决于 Safari / iPadOS；遇到无法显示的图片可尝试 JPEG 或 PNG。

## License

[MIT](LICENSE) © 2026 Haolin Shao (ManfredShao).

## Acknowledgements

Built with help from ChatGPT.
