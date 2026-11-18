# 公交绕行期的可靠性结算

城市公交两家承运方的合同与绕行日报；时间戳含本地偏移。
这里的记录全部为合成资料。日期按当地日历解释，未附时区的时间使用东八区。

## 交接资料

CSV 使用 UTF-8 和逗号分隔。值留空表示这条资料未给出该字段，不等于零。
`data/schema.json` 登记表头与跨表编号关系；`data/policy.json` 保存本期业务配置。

- `data/timetable.csv`：stops 为按服务顺序分号隔开的站点；origin_time 为首站计划时刻。 字段：`trip`, `day`, `carrier`, `origin_time`, `stops`。
- `data/detours.csv`：承运商及适用时段均满足才适用；只有指定站被替代。 字段：`order`, `approved`, `starts`, `ends`, `carrier`, `replaces`, `substitute`。
- `data/observations.csv`：piece 为一段设备记录；空 departure 表示缺测，不能视为未停。 字段：`obs`, `vehicle`, `piece`, `station`, `arrival`, `departure`, `captured`。
- `data/dispatch.csv`：verified 为签认；candidate 仅是待核线索，不排他的证据。 字段：`ticket`, `trip`, `from_piece`, `to_piece`, `accountable`, `at`, `received`, `state`。
- `data/device_status.csv`：missing 表示回传区间不完整；verified_empty 表示调度对未执行的确认。 字段：`source`, `starts`, `ends`, `state`。

## 资料校验

使用 Python 3.11.2 标准库，不需要安装扩展包。资料入口只核查交接格式，不计算业务结果。

```sh
python3 inspect_inputs.py
python3 -m unittest -v
```
