[English](#openclaw-installation--configuration-linuxubuntu) | [日本語](#openclaw-インストール--設定-linuxubuntu) | [中文](#openclaw-安装配置-linuxubuntu)

---

# OpenClaw Installation & Configuration Linux(Ubuntu)

This guide is primarily for setting up the basic functionality of OpenClaw in a virtualized Linux environment.

It is suitable as a foundational preparation for high-customization use cases (especially multi-Agent setups), not a minimal installation, and not suitable for containers.

Subsequent configuration and Skills installation should be completed by instructing the Agent.

## 1. Set up Cloud Server or Local Virtualization Environment
**GCP CE (AWS EC2):** 2 vCPUs, 4GB memory, 20GB disk (Recommended memory >= 8GB)
Open ports for SSH, HTTP, HTTPS access.

**Virtualization Tools (VMware Workstation / VirtualBox):** 2 CPU cores, 4GB memory, 40GB disk (Recommended memory >= 8GB)
Set network to NAT mode to share local IP, or Bridged mode to connect to the physical network (Recommended).

If you need SSH access at any time, a static IP or domain name is recommended (using tools like Termius allows SSH access from mobile devices).

**Cloud Platforms:** Static IP
**Home Network Local Deployment:** IPv6 intranet penetration + Domain name

**Non-static IP:** You can use the Web console for SSH access (The downside is unstable mobile access; use this if you don't need constant SSH access).

## 2. Configure SSH Connection (**Strongly Recommended: Key Authentication**)
Generate SSH key pair locally (Replace `key_name` and `user_name`):
```powershell
ssh-keygen -t rsa -f key_name -C user_name
```

Upload the **public key (XXX.pub)** to the virtual machine console.
[Console] - [VM Instance Name (Details Page)] - [Edit]
Add item to SSH Keys.
Paste the public key and save.
Wait for the VM authentication info to update or restart the VM (Recommended).

## 3. SSH Login
Open any PowerShell terminal or SSH tool (Tabby/TeraTerm) locally and **login to the VM using the private key**.
```powershell
ssh -i .\your_key_path\your_key_name user_name@your_server_ip
```

If connection fails, try clearing old key records:
```powershell
ssh-keygen -R your_server_ip
```

## 4. Configure Environment
Update system:
```bash
sudo apt update && sudo apt upgrade -y
```

Install nvm:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

Set nvm environment variables:
```bash
source ~/.bashrc
```

Install Node.js:
```bash
nvm install node
```

Install dependencies:
```bash
sudo apt install libatomic1 -y
```

Verify Node and npm installation:
```bash
node -v
npm -v
```

## 5. Install OpenClaw
Install OpenClaw toolkit (Main program):
```bash
npm install -g openclaw@latest
```

Check OpenClaw version:
```bash
openclaw --version
```

Install PM2 (To keep OpenClaw running in background / Manual multi-Agent operation):
```bash
npm install -g pm2
```

Check PM2 version:
```bash
pm2 --version
```

## 6. Other Preparations (Skip as needed)
Prepare the following API keys:

**Gemini Api Key:** (Recommended)
<https://aistudio.google.com/app/api-keys>

**Google Workspace Api Key:**
<https://console.cloud.google.com/>

**Brave Search Api Key:**
<https://brave.com/search/api/>

**Telegram Bot Api Key:** (Recommended)
<https://t.me/BotFather>

**Discord Bot Api Key:**
<https://discord.com/developers/applications>

## 7. Configure OpenClaw
```bash
openclaw onboard --install-daemon
```

![pic1](./pic/图片1.png "pic1")

**Risk Warning:** It is recommended not to configure OpenClaw on your main PC; give it an isolated environment.

![pic2](./pic/图片2.png "pic2")

QuickStart is enough. You can let the Agent configure other functions later.

![pic3](./pic/图片3.png "pic3")

Recommended Main Agent: Gemini/Anthropic (Claude)

![pic4](./pic/图片4.png "pic4")

Enter API Key.

![pic5](./pic/图片5.png "pic5")

Select default model. Make sure the API key you entered supports the selected model.
For a completely free option, choose Gemini 2.0 Flash (Current as of 2026/2/9).

![pic6](./pic/图片6.png "pic6")

Connect session method. Recommended: Telegram, Discord. Not recommended: WhatsApp.
You can add other session methods later; multiple channels can coexist.

![pic7](./pic/图片7.png "pic7")

Add necessary Skills.

![pic8](./pic/图片8.png "pic8")
![pic9](./pic/图片9.png "pic9")

Recommended to choose `npm` for installation. For multi-Agent deployment, you can choose `pnpm`.

![pic10](./pic/图片10.png "pic10")

Use Up/Down arrow keys to move cursor, Space to select, Enter to confirm installation.
Initial necessary skills: github, summarize.

![pic11](./pic/图片11.png "pic11")

Yes!

![pic12](./pic/图片12.png "pic12")

If you have prepared Telegram (see previous section), you can now send a greeting to the Agent to confirm the pairing code.

![pic13](./pic/图片13.png "pic13")

Enter the above command:
```bash
openclaw pairing approve telegram XXX123XXX
```

Common Gateway commands:
```bash
openclaw gateway                    # Start gateway
openclaw gateway stop               # Stop gateway
openclaw gateway status             # Check gateway status
openclaw gateway uninstall          # Uninstall gateway
openclaw gateway install --force    # Reinstall gateway
openclaw gateway restart            # Restart gateway
openclaw logs --follow              # View logs
```
TUI interface: CTRL+C to exit.

## 8. Start OpenClaw with PM2
```bash
pm2 start "openclaw gateway" --name openclaw00
```

You can also use systemd+cron for management. PM2 is easier for visual control of independent multi-Agents.

## 9. Epilogue
OpenClaw updates frequently (daily). Do not rely completely on AI tools for operations; combine with official documentation.

Main Agent is not recommended to directly access Moltbook/MoltX (Do not access before establishing an AI instruction security system and API Key privacy protection system).

It is suggested to let the Agent build its own monitoring system, health check system, and memory backup system.

**Skill Library ClawHub:**
<https://clawhub.ai/>

**OpenClaw Official Documentation (English):**
<https://docs.openclaw.ai/>

---

# OpenClaw インストール & 設定 Linux(Ubuntu)

本文は主に、仮想Linux環境でOpenClawの基本機能を構築するためのガイドです。

高カスタマイズ用途（特にマルチエージェント）の基礎準備に適しており、最小限のインストールではありません。また、コンテナには適していません。

その後の設定やスキルのインストールは、ユーザーがエージェントに指示して完了させる必要があります。

## 1. クラウドサーバー環境またはローカルホスト仮想化環境の構築
**GCP CE (AWS EC2):** 2 vCPU, 4GBメモリ, 20GBディスク (推奨メモリ >= 8GB)
SSH, HTTP, HTTPSアクセスを開放してください。

**仮想化ツール (VMware Workstation / VirtualBox):** 2 CPUコア, 4GBメモリ, 40GBディスク (推奨メモリ >= 8GB)
NATモードでローカルIPを共有するか、ブリッジモードで物理ネットワークに接続することを設定してください（推奨）。

いつでもSSHアクセスが必要な場合は、固定IPまたはドメイン名の使用を推奨します（Termiusなどのツールを使用すると、モバイル端末からSSHアクセスが可能です）。

**クラウドプラットフォーム:** 固定IP
**ホームネットワークローカルデプロイ:** IPv6イントラネット透過 + ドメイン名

**非固定IP:** Webコンソールを使用してSSHアクセスが可能です（欠点はモバイルアクセスの不安定さです。常時SSHアクセスの必要がない場合に使用してください）。

## 2. SSH接続の設定 (**鍵認証の使用を強く推奨**)
ローカルでSSHキーペアを生成します（`key_name` と `user_name` を置き換えてください）:
```powershell
ssh-keygen -t rsa -f key_name -C user_name
```

**公開鍵 (XXX.pub)** を仮想マシンコンソールにアップロードします。
[コンソール] - [VMインスタンス名(詳細ページ)] - [編集]
SSHキーの項目を追加
公開鍵を入力して保存
仮想マシンの認証情報が更新されるのを待つか、再起動してください（推奨）。

## 3. SSHログイン
ローカルで任意のPowerShellターミナルまたはSSHツール (Tabby/TeraTerm) を開き、**秘密鍵を使用して仮想マシンにログイン**します。
```powershell
ssh -i .\your_key_path\your_key_name user_name@your_server_ip
```

接続に失敗する場合は、古いキー記録を削除してみてください:
```powershell
ssh-keygen -R your_server_ip
```

## 4. 環境設定
更新:
```bash
sudo apt update && sudo apt upgrade -y
```

nvmのインストール:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

nvm環境変数の設定:
```bash
source ~/.bashrc
```

Node.jsのインストール:
```bash
nvm install node
```

関連ライブラリのインストール:
```bash
sudo apt install libatomic1 -y
```

Nodeとnpmのインストール確認:
```bash
node -v
npm -v
```

## 5. OpenClawのインストール
OpenClawツールキット（メインプログラム）のインストール:
```bash
npm install -g openclaw@latest
```

OpenClawバージョンの確認:
```bash
openclaw --version
```

PM2のインストール（OpenClawのバックグラウンド実行/手動マルチエージェント実行を維持するため）:
```bash
npm install -g pm2
```

PM2バージョンの確認:
```bash
pm2 --version
```

## 6. その他の準備作業（状況に応じてスキップ可）
以下のAPIキーを準備してください。

**Gemini Api Key:** (推奨)
<https://aistudio.google.com/app/api-keys>

**Google Workspace Api Key:**
<https://console.cloud.google.com/>

**Brave Search Api Key:**
<https://brave.com/search/api/>

**Telegram Bot Api Key:** (推奨)
<https://t.me/BotFather>

**Discord Bot Api Key:**
<https://discord.com/developers/applications>

## 7. OpenClawの設定
```bash
openclaw onboard --install-daemon
```

![pic1](./pic/图片1.png "pic1")

**リスク警告:** メインPC上でOpenClawを設定しないことをお勧めします。独立した環境を与えてください。

![pic2](./pic/图片2.png "pic2")

QuickStartで十分です。その他の機能は後でエージェントに設定させることができます。

![pic3](./pic/图片3.png "pic3")

推奨メインエージェント: Gemini/Anthropic (Claude)

![pic4](./pic/图片4.png "pic4")

APIキーを入力します。

![pic5](./pic/图片5.png "pic5")

デフォルトモデルを選択します。入力したAPIキーが選択したモデルをサポートしているか確認してください。
完全無料なら Gemini 2.0 Flash を選択（2026/2/9現在）。

![pic6](./pic/图片6.png "pic6")

セッション接続方法。推奨: Telegram, Discord。非推奨: WhatsApp。
後で他のセッション方法を追加でき、マルチチャンネル共存が可能です。

![pic7](./pic/图片7.png "pic7")

必要なスキルを追加します。

![pic8](./pic/图片8.png "pic8")
![pic9](./pic/图片9.png "pic9")

インストールのために `npm` を選択することを推奨します。マルチエージェント展開の場合は `pnpm` を選択できます。

![pic10](./pic/图片10.png "pic10")

上下矢印キーでカーソル移動、スペースキーで選択、エンターキーでインストール確認。
初期に必要なスキル: github, summarize。

![pic11](./pic/图片11.png "pic11")

Yes!

![pic12](./pic/图片12.png "pic12")

Telegramの準備ができている場合（前の章を参照）、エージェントに挨拶を送ってペアリングコードを確認できます。

![pic13](./pic/图片13.png "pic13")

上記のコマンドを入力します:
```bash
openclaw pairing approve telegram XXX123XXX
```

一般的なGatewayコマンド:
```bash
openclaw gateway                    # Gateway起動
openclaw gateway stop               # Gateway停止
openclaw gateway status             # Gatewayステータス確認
openclaw gateway uninstall          # Gatewayアンインストール
openclaw gateway install --force    # Gateway再インストール
openclaw gateway restart            # Gateway再起動
openclaw logs --follow              # ログ表示
```
TUIインターフェース: CTRL+Cで終了。

## 8. PM2を使用したOpenClawの起動
```bash
pm2 start "openclaw gateway" --name openclaw00
```

systemd+cronを使用して管理することもできますが、PM2の方が独立したマルチエージェントを視覚的に制御しやすいです。

## 9. 結び
OpenClawは更新が頻繁（毎日更新）ですので、AIツールの操作に完全に依存せず、公式ドキュメントと組み合わせて操作してください。

メインエージェントがMoltbook/MoltXに直接アクセスすることは推奨しません（AI指示セキュリティシステム、APIキープライバシー保護システムを構築するまではアクセスしないでください）。

エージェント自身に監視システム、ヘルスチェックシステム、メモリバックアップシステムを構築させることをお勧めします。

**スキルライブラリ ClawHub:**
<https://clawhub.ai/>

**OpenClaw 公式ドキュメント (英語):**
<https://docs.openclaw.ai/>

---

# OpenClaw 安装&配置 Linux(Ubuntu)

本文主要用于在虚拟Linux环境搭建openclaw 配置基础功能

适用于为高定制化用途(特别是多Agent)做基础准备 并非最低限安装 不适用于容器

后续配置以及Skills安装需要用户指挥Agent完成

## 1. 建立云端服务器环境or本地主机虚拟化环境
GCP CE(AWS EC2): 2 vCPU 4GB memory  20GB disk  (推荐memory >= 8GB)
开放SSH,HTTP,HTTPS访问

虚拟化工具(VMware-Workstation/VirtualBox): 2CPU核心 4GB memory 40GB disk (推荐memory >= 8GB)
设置NAT模式共享本地IP 或 设置桥接模式连接本地物理网络(推荐)

有随时ssh访问需求的话推荐使用固定IP或域名 (使用Termius等工具可以手机端进行ssh访问) 

云平台：固定IP 
家庭网络本地部署：IPv6内网穿透+域名

非固定IP可以使用Web控制台进行ssh访问 (缺点是手机访问不稳定 没有随时ssh访问需求的话可以用用)

## 2. 配置SSH连接(**强烈建议使用密钥认证**)
本地生成ssh密钥对(替换 key_name 和 user_name)
```powershell
ssh-keygen -t rsa -f key_name -C user_name
```

将**公钥(XXX.pub)**上传到虚拟机控制台
[控制台]-[虚拟机实例名(详情页)]-[修改(编辑)]
ssh密钥 添加项
将公钥输入 保存
等待虚拟机认证信息更新 或 重启虚拟机(推荐)

## 3. SSH登录
在本地打开任意一个powershell终端或ssh连接工具(Tabby/TeraTerm) **使用私钥登录虚拟机**
```powershell
ssh -i .\your_key_path\your_key_name user_name@your_server_ip
```

连接失败可以尝试清除旧密钥记录
```powershell
ssh-keygen -R your_server_ip
```

## 4. 配置环境
更新
```bash
sudo apt update && sudo apt upgrade -y
```

安装nvm
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

设置nvm环境变量
```bash
source ~/.bashrc
```

安装node
```bash
nvm install node
```

安装相关库
```bash
sudo apt install libatomic1 -y
```

确认安装node npm
```bash
node -v
npm -v
```

## 5. 安装openclaw
安装openclaw的工具包(主程序)
```bash
npm install -g openclaw@latest
```

查看openclaw版本
```bash
openclaw --version
```

安装PM2(为了保持openclaw后台运行/手动多Agent运行)
```bash
npm install -g pm2
```

查看PM2版本
```bash
pm2 --version
```

## 6. 其他准备工作(可择情跳过)
准备下列Api key

Gemini Api Key:(推荐)
<https://aistudio.google.com/app/api-keys>

Google wprkplace Api Key:
<https://console.cloud.google.com/>

Brave Search Api Key:
<https://brave.com/search/api/>

Telegram Bot Api Key:(推荐)
<https://t.me/BotFather>

Discord Bot Api Key:
<https://discord.com/developers/applications>

## 7. 配置openclaw
```bash
openclaw onboard --install-daemon
```

![pic1](./pic/图片1.png "pic1")

风险警告 建议不要在主力PC上配置openclaw 给它一个独立的环境

![pic2](./pic/图片2.png "pic2")

QuickStart足够了 后续可以让Agent自己配置其他功能

![pic3](./pic/图片3.png "pic3")

主Agent推荐：Gemini/Anthropic(Claude)

![pic4](./pic/图片4.png "pic4")

输入API-key

![pic5](./pic/图片5.png "pic5")

选择默认模型 注意确认你输入的API-key是否支持所选模型
完全免费选择Gemini 2.5 flash (当前2026/2/9)

![pic6](./pic/图片6.png "pic6")

接入会话方式 推荐Telegram Discord 不推荐WhatApp
后续可以再添加其他会话方式 多频道共存

![pic7](./pic/图片7.png "pic7")

添加必要的Skills

![pic8](./pic/图片8.png "pic8")
![pic9](./pic/图片9.png "pic9")

推荐选择npm方式安装 多Agent部署可以选择pnpm方式

![pic10](./pic/图片10.png "pic10")

按上下方向键移动光标 按空格键选择 按回车键确认安装
初期必要skills：github summarize

![pic11](./pic/图片11.png "pic11")

Yes！

![pic12](./pic/图片12.png "pic12")

如果已经准备好Telegram(见上一章节) 此时可以向Agent发送问候 进行配对码确认

![pic13](./pic/图片13.png "pic13")

输入上述指令
```bash
openclaw pairing approve telegram XXX123XXX
```

常用gateway命令
```bash
openclaw gateway                    # 开启gateway
openclaw gateway stop               # 关闭gateway
openclaw gateway status             # 查看gateway状态
openclaw gateway uninstall          # 卸载gateway
openclaw gateway install --force    # 重新安装gateway
openclaw gateway restart            # 重启gateway
openclaw logs --follow              # 查看日志
```
TUI界面 CTRL+C退出

## 8. 使用PM2启动openclaw
```bash
pm2 start "openclaw gateway" --name openclaw00
```

也可以用systemd+cron进行管理 PM2更容易可视化控制独立的多Agent

## 9. 尾声
OpenClaw更新较为频繁(日更),相关操作不要完全参照AI工具，结合官方文档操作即可。

主力Agent不建议直接访问Moltbook/MoltX。(在未建立AI指令安全系统、API Key隐私保护系统前不要访问)

建议让Agent建立自身监视系统、健康检查系统、记忆备份系统。

Skill库ClawHub：
<https://clawhub.ai/>

OpenClaw官方文档 中文版：
<https://docs.openclaw.ai/zh-CN>
