# 🔒 Reed

> Reed is a lightweight, air-gapped conduit for sensitive data.

一个用于安全传输密码、私钥、助记词等敏感信息的工具。
通过一端加密生成二维码，另一端扫码解密的方式，实现无需网络、无需中转服务器的点对点安全传输。

---

## 🚀 功能特性

- 文本加密生成二维码
- 扫码解密还原明文
- 支持密码/私钥/助记词
- 支持自定义加密口令
- 解密后自动清空内存
- 离线运行（无网络权限）

---

## ✨ 项目背景

在实际使用中，我们经常需要在两台设备之间传输以下信息：

- 密码/Token
- API Key/Secret
- 区块链私钥/助记词
- 临时凭证或一次性密钥

传统方式（聊天工具、剪贴板、截图、云同步）都存在被记录、被监听或长期留存的风险。
本项目旨在提供一种：

- 完全离线
- 端到端加密
- 短生命周期
- 人为可控的敏感信息传输方案。

---

## Todo List

- 页面
  - 输入框、生成按钮，生成二维码，扫码
  - faq（放到底部）：关于、教程、如何保证安全、联系方式等
- 第一版：消息直接生成二维码
  - 生成二维码 https://cdnjs.com/libraries/qrcode/1.5.1
  - 扫码、调用摄像头、点击拍照到 canvas 然后再解码
  - 解码库 https://www.jsdelivr.com/package/npm/jsqr
- 第二版
  - WebCrypto 加密、解密消息
  - 优化 https://github.com/soldair/node-qrcode fork 后做精简处理
  - 使用图片缩放，增加识别精准度 https://fengyuanchen.github.io/cropperjs/zh/
- 防截屏/缓存：建议二维码显示的页面增加 autocomplete="off"，并在生成二维码的 Canvas 容器上添加 CSS 保护（如 user-select: none）

---

## 📄 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

---

## 🙏 致谢

- [qrcode](https://github.com/soldair/node-qrcode/) - 生成二维码
- [jsQR](https://github.com/cozmo/jsQR/) - 二维码读取
- [Cloudflare Page](https://developers.cloudflare.com/pages/) - Cloudflare Pages 托管

---

**Made with ❤️ for Privacy**
