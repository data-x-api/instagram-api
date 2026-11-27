# Instagram 数据接口（Instagram API）：账号、帖子和 Reels、评论、话题

**Instagram API · Instagram profile · Instagram posts and reels · Instagram comments**

这个仓库是 [DataXAPI](https://dataxapi.com/zh/?ref=gh-instagram-api) 的Instagram接口说明和能直接跑的脚本：9 个接口，一把 Key，按次付费，$0.002 起，**出错不收费**。不用登录平台账号，不用 Cookie，不开浏览器。

[免费拿 Key：试用最多 333 次，不用绑卡](https://dataxapi.com/zh/signin/?button=gh-instagram-api) · [全部接口文档](https://dataxapi.com/zh/apis/?ref=gh-instagram-api) · [接进 AI（MCP）](#接进-aimcp) · [English](#english)

最低充 $10，余额不过期；支持支付宝、微信支付和银行卡。

## 全部 9 个接口

### 评论

| 接口 | 拿到什么 | 价格 | 编号 |
|---|---|---|---|
| [Instagram 评论回复](https://dataxapi.com/zh/apis/instagram/comments/replies/list/?ref=gh-instagram-api) | 按评论编号和它所在的帖子短码取一页回复：正文、回复者、发布时间。 | $0.002 一次 | `instagram.comments.replies.list` |

### 话题标签

| 接口 | 拿到什么 | 价格 | 编号 |
|---|---|---|---|
| [Instagram 话题帖子](https://dataxapi.com/zh/apis/instagram/hashtags/posts/list/?ref=gh-instagram-api) | 按话题名取一页带这个话题的最新 Instagram 帖子，每页约 33 条：文案、作者编号、点赞评论数、发布时间、图片。 | $0.002 一次 | `instagram.hashtags.posts.list` |

### 内容

| 接口 | 拿到什么 | 价格 | 编号 |
|---|---|---|---|
| [Instagram 帖子评论](https://dataxapi.com/zh/apis/instagram/posts/comments/list/?ref=gh-instagram-api) | 按帖子短码或链接取一页 Instagram 评论，每页约 15 条，最新在前：正文、评论者、点赞数、发布时间。 | $0.002 一次 | `instagram.posts.comments.list` |
| [Instagram 帖子详情](https://dataxapi.com/zh/apis/instagram/posts/get/?ref=gh-instagram-api) | 按帖子短码或链接取一条 Instagram 帖子或 Reel：文案、作者、点赞评论播放数、图片和视频地址，联名帖的其他作者。 | $0.002 一次 | `instagram.posts.get` |
| [Instagram 视频字幕](https://dataxapi.com/zh/apis/instagram/posts/transcript/get/?ref=gh-instagram-api) | 按帖子短码或链接取 Instagram Reel 或视频帖里说的话，全文；只支持两分钟以内的视频。 | $0.002 一次 | `instagram.posts.transcript.get` |
| [Instagram 视频和图片下载地址](https://dataxapi.com/zh/apis/instagram/posts/media/get/?ref=gh-instagram-api) | 给一条 Instagram 帖子或 Reel 的短码或链接，返回它的全部文件地址：视频和它的封面图，多图帖里的每张图片和每段视频（按顺序编号），带宽高和过期时间；过期了再调一次即可。 | $0.002 一次 | `instagram.posts.media.get` |

### 账号

| 接口 | 拿到什么 | 价格 | 编号 |
|---|---|---|---|
| [Instagram 用户资料](https://dataxapi.com/zh/apis/instagram/users/get/?ref=gh-instagram-api) | 按账号名取 Instagram 用户资料：显示名、简介、粉丝数、关注数、是否认证、是否私密，主页上的网站和链接。 | $0.002 一次 | `instagram.users.get` |
| [Instagram 用户帖子](https://dataxapi.com/zh/apis/instagram/users/posts/list/?ref=gh-instagram-api) | 按账号名取一页 Instagram 用户发布的帖子，每页 12 条，可只要 Reels：文案、点赞评论播放数、发布时间、图片和视频地址，联名帖的其他作者。 | $0.002 一次 | `instagram.users.posts.list` |
| [Instagram 用户搜索](https://dataxapi.com/zh/apis/instagram/users/search/?ref=gh-instagram-api) | 按关键词搜索 Instagram 账号，一次约 50 个，不翻页：账号名、显示名、头像、是否认证、是否私密。 | $0.004 一次 | `instagram.users.search` |

## 真实返回长什么样

**Instagram 用户帖子**（`instagram.users.posts.list`，参数 `{"username":"nasa"}`），录下的真实返回，个人信息换成了 …：

```json
{
  "data": {
    "items": [
      {
        "id": "…",
        "url": "https://example.com/…",
        "type": "video",
        "text": "…",
        "published_at": "2026-10-24T04:02:49.000Z",
        "author": {
          "id": "…",
          "username": "…",
          "name": "…",
          "avatar_url": "https://example.com/…",
          "verified": true
        },
        "metrics": {
          "likes": 2630,
          "comments": 65,
          "views": 70166
        },
        "images": [
          "https://example.com/…"
        ]
      }
    ]
  },
  "page": {
    "next_cursor": "example-cursor-1",
    "cursor_expires_at": "2026-11-01T09:42:13.000Z",
    "has_more": true,
    "returned": 12
  }
}
```

每个接口都是同一种结构：列表在 `data.items`，单个在 `data.item`，下一页用 `page.next_cursor`，这次扣了多少在 `meta.billing.amount`，平台不给的字段为 `null` 并在 `meta.unavailable` 里写明。

## 快速开始

1. [拿一把 Key](https://dataxapi.com/zh/signin/?button=gh-instagram-api)（`dx_` 开头），放进环境变量：`export DATAXAPI_API_KEY=dx_...`
2. 调一次接口：

```bash
curl -G "https://api.dataxapi.com/v1/instagram/users/posts/list" \
  -H "Authorization: Bearer $DATAXAPI_API_KEY" \
  --data-urlencode 'username=nasa'
```

3. 或者跑 `examples/` 里的脚本。只用 Python 标准库（3.9 以上）；不加 `--yes` 只报价、不调用，加了才真跑，跑完告诉你实际花了多少：

| 脚本 | 做什么 | 先看要花多少 |
|---|---|---|
| [quickstart.py](examples/quickstart.py) | 取几页结果，存成 CSV | `cd examples && python3 quickstart.py nasa` |

## 接进 AI（MCP）

Claude Code：

```bash
claude mcp add --transport http --scope user dataxapi https://api.dataxapi.com/mcp --header "Authorization: Bearer $DATAXAPI_API_KEY"
```

Cursor、Cherry Studio、WorkBuddy 等支持远程 MCP 的客户端：

```json
{
  "mcpServers": {
    "dataxapi": {
      "url": "https://api.dataxapi.com/mcp",
      "headers": {
        "Authorization": "Bearer dx_..."
      }
    }
  }
}
```

接上以后直接用中文问，比如“帮我看看这个关键词最近一个月在抖音上的热度”。AI 花钱前可以先免费查接口和价格。也可以装成技能：`npx skills add data-x-api/skills`。

## 常见问题

**要不要平台账号、Cookie 或扫码？**

不要。只要一把 DataXAPI Key，不碰你的账号，也就不存在账号被风控的问题。

**调用出错扣钱吗？**

不扣。每次返回里的 `meta.billing.amount` 写明这次扣了多少（美元）。

**数据有多新？**

默认在你调用时从平台实时读取，返回里的 `meta.fetched_at` 写明读取时间。

**怎么翻页？**

列表接口一次返回一页，把返回里的 `page.next_cursor` 作为 `cursor` 传回去取下一页，每一页单独计费。

## English

Instagram API: profiles, posts and reels, comments and hashtags, through one key, paid per call.

No platform account, cookie or QR login. Paid per call from $0.002; errors are not charged. Free trial: up to 333 calls, no card. [Get a key](https://dataxapi.com/signin/?button=gh-instagram-api) · [All endpoints](https://dataxapi.com/apis/?ref=gh-instagram-api) · MCP: `https://api.dataxapi.com/mcp`.

## License

MIT

<sub>接口和价格取自 DataXAPI 的目录 0264e2d6dd4ff743、价格版本 23；以 [dataxapi.com/zh/apis](https://dataxapi.com/zh/apis/?ref=gh-instagram-api) 为准。</sub>
