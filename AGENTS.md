# AGENTS.md —— LenoTrae 的默认写法

> 给 AI / 新加入的人。**新代码照这 5 条写**；旧写法见文末"遗留（不要扩散）"。
> 完整论证见 `Leno/docs/idioms.md`。

| 场景 | 用这个 | 不要 |
|---|---|---|
| 数据 | `struct` + `Array[struct]` | 字符串记录 + `field(rec, idx)`（`trae_history.leno` 的历史写法）|
| 状态 | `enum { todo = 0 ... }` | 魔法整数 / `_phase == "..."` 链（`trae_auto.leno` 已改 `switch`）|
| 分支 | ≥3 个同值比较用 `switch` | 长 `if - else if` 链 |
| 集合变换 | `.map` / `.filter` | `Array[x] out = []` + `for` + `add` |
| 错误 | `Result` struct（`ok` + `error`）| 手拼 JSON 字符串 |

已经存在、不要手写的（标准库）：

```leno
jsons.encode(obj)                    // ★ 生成 JSON 用它，**不要** "{\"a\":\"" + v + "\""
                                     //   —— 手拼时值里有 " 或换行会产生**非法 JSON** 且不报错
strings.pad_start(_str(v), 2, "0")   // 补零（本项目的 pad2 就是它）
strings.split(s, ",") / strings.lines(s)
strings.fmt_size(1536)               // "1 KB"
arr.sort(func(int a, int b) { return b - a })   // 自定义比较器（稳定）
```

## 遗留（不要扩散）

- `trae_history.leno` 的 `"brand|user|field"` 复合键 + `field(rec, idx)` —— 新代码用 `struct`
- 手写 `argv` 循环（`trae_sign.leno`）—— 但**不要**为了好看就换 `cli`，
  那里对"解释器吃掉脚本旗标"有历史兼容注释，改动前先读那段
- 路径手拼 `"\\history.json"` —— 跨平台会用 `dirs.join`

## 自检

```powershell
..\Leno\build\leno.exe --check trae_xxx.leno          # 单文件
..\Leno\build\leno.exe trae_sign.leno status          # 真实 CLI（需要本机 Trae 登录态）
Get-ChildItem test -Filter *.leno | % { ..\Leno\build\leno.exe $_.FullName }
```
