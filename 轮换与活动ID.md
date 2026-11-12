# 每周轮换：新活动 id 怎么定

1. 客户端进游戏会拉 `assets/weekly/weeklyactivity`（zlib 压缩 XML），每条含
   `<id>` 周期号、`<actId>` **总线 id（发包 params）**、`<name>`、`<award>`、`<url>`。
   但它是**历届目录**（几十条），不是本周清单。
2. 拿目录里每个 actId 发 `open_ui` + `game_info`（只读、不扣次数），有回包的就是本周开着。
   实测一周 17 个，其中 10 个是历史活动复刻。
3. 结果写 `data/weekly_activities.json`；工具启动时会自动把未收录的按「只扫不打」挂上。

命令：

```bat
python rotation.py --diff              :: 列没收录的
python rotation.py --probe 账号.txt     :: 探本周真正开着哪些（需要登录一个号）
```

要**自动打满**新活动，还缺三样，只能抓包：`start_game` 参数、局内过程包、`end_game` 载荷。
