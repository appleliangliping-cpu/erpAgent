# ERP Agent - 基于本体的时序知识图谱

🚀 基于 FastAPI + Vue 3 + Neo4j 的 ERP 知识图谱可视化与分析平台

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.9+-green.svg)](https://www.python.org/)
[![Vue](https://img.shields.io/badge/vue-3.x-brightgreen.svg)](https://vuejs.org/)
[![Neo4j](https://img.shields.io/badge/neo4j-5.x-orange.svg)](https://neo4j.com/)

---

## 📖 项目简介

ERP Agent 是一个将传统关系型 ERP 数据转换为图数据库模型的知识图谱系统。通过 Neo4j 图数据库，实现对企业资源计划数据的深度关联分析和可视化展示。

### 核心功能

- 🔗 **知识图谱构建** - 将 PostgreSQL 中的 ERP 数据映射到 Neo4j 图数据库
- 📊 **可视化分析** - 基于 Vue 3 的关系图谱可视化，支持交互式探索
- 🔍 **智能查询** - 支持 Cypher 查询和自然语言查询
- 📈 **业务链路追踪** - 完整实现 P2P（采购到付款）、O2C（订单到收款）、R2R（记录到报告）链路

---

## 🏗️ 技术架构

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Frontend      │────▶│    Backend      │────▶│   Neo4j         │
│   Vue 3 + Vite  │     │   FastAPI       │     │   Graph DB      │
│   GraphCanvas   │     │   REST API      │     │   erpgraph      │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                               │
                               ▼
                        ┌─────────────────┐
                        │   PostgreSQL    │
                        │   ERP Database  │
                        └─────────────────┘
```

### 技术栈

| 层级 | 技术 | 说明 |
|------|------|------|
| **前端** | Vue 3 + Vite | 响应式前端框架 |
| **可视化** | Cytoscape.js / D3.js | 图谱可视化库 |
| **后端** | FastAPI | 高性能 Python Web 框架 |
| **图数据库** | Neo4j | 企业级图数据库 |
| **关系数据库** | PostgreSQL | ERP 源数据存储 |
| **数据同步** | Python Scripts | ETL 数据迁移脚本 |

---

## 📦 项目结构

```
erpAgent/
├── backend/              # FastAPI 后端服务
│   ├── api/             # API 路由
│   ├── models/          # 数据模型
│   ├── services/        # 业务逻辑
│   ├── utils/           # 工具函数
│   └── main.py          # 应用入口
├── frontend/            # Vue 3 前端应用
│   ├── src/
│   │   ├── components/  # Vue 组件
│   │   ├── views/       # 页面视图
│   │   ├── stores/      # Pinia 状态管理
│   │   ├── utils/       # 工具函数
│   │   └── App.vue      # 根组件
│   └── package.json
├── scripts/             # 数据迁移脚本
│   ├── import_*.py      # 数据导入脚本
│   ├── verify_*.py      # 数据验证脚本
│   └── *.cypher         # Cypher 查询脚本
└── docs/                # 项目文档
    ├── 映射设计.md       # 关系型到图模型映射
    └── 验证报告.md       # 数据导入验证报告
```

---

## 🚀 快速开始

### 环境要求

- Python 3.9+
- Node.js 18+
- Neo4j 5.x
- PostgreSQL 14+

### 1. 克隆项目

```bash
git clone https://github.com/appleliangliping-cpu/erpAgent.git
cd erpAgent
```

### 2. 后端配置

```bash
cd backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt

# 配置环境变量
cp .env.example .env
# 编辑 .env 文件，配置数据库连接
```

### 3. 前端配置

```bash
cd frontend
npm install
cp .env.example .env
```

### 4. 启动服务

```bash
# 后端服务 (端口 8000)
cd backend
uvicorn main:app --reload --host 0.0.0.0 --port 8000

# 前端服务 (端口 5173)
cd frontend
npm run dev
```

访问 http://localhost:5173 查看应用

---

## 📊 数据模型

### 核心业务模块

| 模块 | 节点数 | 关系数 | 完成度 |
|------|--------|--------|--------|
| 供应商管理 | 473 | 3,750 | ✅ 100% |
| 采购管理 | 1,217 | 2,134 | ✅ 100% |
| 应付管理 | 45,778 | 31,012 | ✅ 100% |
| 应收管理 | 570 | - | ✅ 100% |
| XLA 会计引擎 | 4,000 | 1,000 | ✅ 100% |
| 总账管理 | 172 | 1,365 | ✅ 100% |
| **总计** | **85,225** | **56,205** | **98%** |

### 业务链路

```
采购到付款 (P2P):
Supplier → PurchaseOrder → Invoice → Payment → GLAccount

订单到收款 (O2C):
Customer → ARTransaction → Payment → GLAccount

记录到报告 (R2R):
GLJournal → GLAccount → GLBalance → FinancialReport
```

---

## 🔧 数据迁移

### 从 PostgreSQL 导入到 Neo4j

```bash
cd scripts

# 1. 导入基础数据
python import_suppliers.py
python import_purchase_orders.py

# 2. 导入财务数据
python import_invoices.py
python import_payments.py

# 3. 导入会计数据
python import_xla.py
python import_gl.py

# 4. 验证导入结果
python verify_import.py
```

### 导入脚本列表

| 脚本 | 功能 | 数据量 |
|------|------|--------|
| `import_payment_data_v2.py` | 导入付款数据 | 48,028 条 |
| `import_missing_nodes.py` | 补充缺失节点 | 30,245 条 |
| `fix_missing_data.py` | 修复金额字段 | - |
| `verify_payment_import.py` | 数据验证 | - |

---

## 📝 API 文档

启动后端服务后，访问以下地址查看 API 文档：

- **Swagger UI**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

### 主要 API 端点

| 端点 | 方法 | 说明 |
|------|------|------|
| `/api/graph/nodes` | GET | 获取节点列表 |
| `/api/graph/relationships` | GET | 获取关系列表 |
| `/api/graph/query` | POST | 执行 Cypher 查询 |
| `/api/graph/trace` | POST | 业务链路追踪 |
| `/api/suppliers` | GET | 供应商查询 |
| `/api/invoices` | GET | 发票查询 |
| `/api/payments` | GET | 付款查询 |

---

## 📖 文档

详细文档请参阅 [docs](docs/) 目录：

- [ERP 关系型模型到图模型的映射设计](docs/ERP 关系型模型到图模型的映射设计.md) - 完整的映射规则和设计
- [数据导入验证报告](docs/数据导入验证报告_20260323.md) - 数据质量和完整性验证

---
这个是EBS表清单的线上版，所有模块的都在  https://etrm.live/etrm-12.2.2/etrm.oracle.com/pls/trm1222/etrm_search.html 
其中主要模块的地址是PO：https://etrm.live/etrm-12.2.2/etrm.oracle.com/pls/trm1222/PO_Tables120b.html?n_file_id=1157&c_mode=INLINE
AP：https://etrm.live/etrm-12.2.2/etrm.oracle.com/pls/trm1222/SQLAP_Tables918c.html?n_file_id=1154&c_mode=INLINE
AR：https://etrm.live/etrm-12.2.2/etrm.oracle.com/pls/trm1222/AR_Tables9ecf.html?n_file_id=1181&c_mode=INLINE
XLA：https://etrm.live/etrm-12.2.2/etrm.oracle.com/pls/trm1222/XLA_Tables4d89.html?n_file_id=1499&c_mode=INLINE
GL：https://etrm.live/etrm-12.2.2/etrm.oracle.com/pls/trm1222/SQLGL_Tables0494.html?n_file_id=1112&c_mode=INLINE
## 🧪 测试

```bash
# 后端测试
cd backend
pytest

# 前端测试
cd frontend
npm run test
```

---

## 📄 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件

---

## 👥 作者

- **liangliping** - [appleliangliping-cpu](https://github.com/appleliangliping-cpu)

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

1. Fork 本项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

---

## 📅 更新日志

### v1.1.0 (2026-03-23)
- ✅ 补充 InvoiceDistribution、SupplierSite 等节点
- ✅ 修复金额字段缺失问题
- ✅ 完善付款链路关系
- ✅ 新增数据验证脚本

### v1.0.0 (2026-03-22)
- 🎉 初始版本发布
- ✅ 基础图谱构建
- ✅ 核心业务模块映射

---

## 🙏 致谢

- [Neo4j](https://neo4j.com/) - 图数据库
- [FastAPI](https://fastapi.tiangolo.com/) - Web 框架
- [Vue.js](https://vuejs.org/) - 前端框架
- [Cytoscape.js](https://js.cytoscape.org/) - 图谱可视化

---

<div align="center">

**⭐ 如果这个项目对你有帮助，请给一个 Star！⭐**

Made with ❤️ by liangliping

</div>
