# 基于多 Agent 协同的工业设备故障预测与自主维修调度系统

这是一个可本地运行的智能工厂自主运维原型系统，覆盖从 IoT 异常识别、RAG 故障诊断、维修方案生成、备件库存联动、MES 派工到修复后闭环验证的完整流程。

## 功能

- 感知 Agent：持续接入模拟 PLC/传感器数据，计算异常评分与告警等级。
- 诊断 Agent：使用轻量 RAG 检索设备手册和历史案例，生成可解释的根因推理链。
- 方案策划 Agent：输出备件清单、维修 SOP、安全注意事项和预计工时。
- 调度 Agent：根据技能、排班、负载和库存生成最优工单派发结果。
- 闭环验证：工单完成后进入验证窗口，对修复后的 telemetry 进行恢复性检测。
- Web 控制台：展示 OEE、MTTR、告警、诊断链、库存、技师和工单。

## 启动

本项目只依赖 Python 标准库，建议使用 Python 3.10+。

```powershell
python app/server.py
```

然后访问：

```text
http://localhost:8000
```

如果 8000 端口被占用：

```powershell
python app/server.py --port 8010
```

## API

- `GET /api/dashboard`：系统总览、资产、告警、工单、库存、技师。
- `POST /api/demo/anomaly`：注入一次高风险示例异常并自动触发 Agent 流程。
- `POST /api/agents/run`：对指定资产运行完整 Agent 协作链。
- `POST /api/workorders/{id}/complete`：完成工单并生成闭环验证记录。
- `GET /api/telemetry?asset_id=CNC-01`：查看最近传感器数据。

## 项目结构

```text
app/
  agents/              多 Agent 实现
  static/              Web 控制台
  data/                运行时 SQLite 数据库
  server.py            HTTP API 和静态资源服务
  datastore.py         SQLite 持久化与示例数据
  rag.py               轻量检索增强模块
  simulator.py         IoT 数据模拟器
```

