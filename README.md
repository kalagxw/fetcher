# JAVClub fetcher
🔞 Data fetcher for JAVClub core

❗ | **因架构调整，本项目已不再维护并将存档。新项目将支持泛媒体文件管理，相关开发工作将迁移至 [@UsagiHouse](https://github.com/UsagiHouse) 进行，请知悉**
:---: | :---

## 简介

这是一个涩情(划掉) Repo，用来配合涩情(划掉)核心工作

重写后已重新支持 OneJAV, 具体可参考配置文件

## 使用

完整支持 Docker, 仅需几部即可完成部署

### Docker 安装

请参考 [JAVClub/docker](https://github.com/JAVClub/docker/tree/master/fetcher) 项目

### 非 Docker

#### 环境

- 任意 Linux 发行版
- Node.js 10+
- qBittorrent 4.2.1
- rclone
- ffmpeg
- Your brain

**只能使用 qBittorrent 4.2.1**, 原因是随着版本变动 API endpoint 也会不断变动, 所以只能选择一个版本, 而作者正好使用这个版本, 所以就确定是 4.2.1 了
该版本对应的 Docker image 是 `linuxserver/qbittorrent:14.2.0.99201912180418-6819-118af03ubuntu18.04.1-ls62`, 已写入 docker-compose.yaml

#### 安装

直接 clone 加 npm i 一梭子

```bash
git clone https://github.com/JAVClub/fetcher.git
cd fetcher
npm i
```

#### 配置

```json
{
    "system": {
        "logLevel": "info"
    },
    "remote": [
        {
            "driver": "RSS",
            "type": "MT",
            "url": "https://pt.m-team.cc/torrentrss.php?https=1&rows=50&cat410=1&isize=1&search=-&search_mode=1&linktype=dl&passkey=yourkeyhere",
            "interval": 300
        },
        {
            "driver": "OneJAV",
            "url": "https://onejav.com/popular/",
            "interval": 300
        }
    ],
    "qbittorrent": {
        "baseURL": "http://127.0.0.1:8080",
        "username": "admin",
        "password": "adminadmin",
        "savePath": ""
    }
}
```

在 `dev.json` 文件中填写你的服务器信息, 对一些细节做解释

- **remote[]**
  - **driver:** 目前支持 RSS / OneJAV 订阅
  - **type:** RSS 的解析方法 (仅在驱动为 RSS 时生效)
    - **MT:** MT 代表 PT 站 M-Team 的 RSS 格式; 具体: 以车牌号开头(XXX-0NN), 车牌号后紧跟空格, 以大小结尾([xx.xxG]), 可在 Nexus RSS 订阅中勾选包括大小得到
  - **url:** 订阅地址(OneJAV 直接填写列表页地址即可)
- **qbittorrent**
  - **savePath:** 下载文件保存路径, 非 Docker 使用留空即可, 若使用 [JAVClub/docker](https://github.com/JAVClub/docker) 部署则填写 `/usr/app/tmp/downloads/JAVClub/` 即可

#### 运行

重构后不再需要从视频中截取分镜, 而是直接从 JAVbus 读取 DMM 的数据, 所以性能消耗有所下降

目前将监测模式更改为了 qBittorrent 自带的指定条件自动暂停, 在使用前请在 qBittorrent 的选项 -> BitTorrent 中按照你的需求勾选做种限制中的任意一项(或两项一起)

运行只需执行 `NODE_ENV=dev node src/app.js` 即可, 可以使用 systemd/pm2 等持久化工具

当种子处理完毕后, 脚本会自动将处理好的文件移至 `./tmp/sync` 目录, 只需要使用 `rclone move` 监听此目录即可

## 捐赠

这个项目虽然不算麻烦但还是挺繁琐的, 所以如果想请我喝一杯咖啡也是可以的

用[爱发电](https://afdian.net/@isXiaoLin) (大雾

## 免责声明

本程序仅供学习了解, 请于下载后 24 小时内删除, 不得用作任何商业用途, 文字、数据及图片均有所属版权, 如转载须注明来源

使用本程序必循遵守部署服务器所在地、所在国家和用户所在国家的法律法规, 程序作者不对使用者任何不当行为负责
