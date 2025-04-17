# 前端风控系统开发与数据爬虫实战

## 项目背景
本课题要求候选人综合运用前端开发、接口安全、自动化测试等技术，完成一套具备风控能力的系统开发，并结合数据爬虫完成特定场景的数据采集。

## 一、任务要求
### 1. 前端风控系统开发（重点）
**技术栈**：Vue3 + TypeScript + Web Crypto API + WebSocket

#### 核心功能模块
1. ​**接口安全加固**
   - 实现AES-256-GCM动态加密传输
   - 添加Referer/Origin防盗链校验
   - 生成并验证请求签名（时间戳+nonce+secretKey）
   - 使用Proxy代理隐藏真实接口地址

2. ​**分页验证码机制**
   - 第3次分页请求时触发图形验证码
   - 验证通过后携带token加载数据
   - 5分钟内重复请求需重新验证
   - 错误3次后锁定10分钟

3. ​**行为安全监控**
   - 监测页面焦点变化/键盘鼠标事件
   - 15分钟无操作自动登出
   - 连续5次错误操作触发验证码
   - 敏感操作前进行生物识别二次验证

#### 技术要求
- 使用Vue3 Composition API开发
- 集成Web Crypto API进行加解密
- 结合WebSocket实现实时风控策略更新
- 完善单元测试覆盖率≥80%

### 2. 数据爬虫开发（Automa实践）
**目标**：自动化采集USITC官网Excel数据并结构化存储

#### 任务分解
1. 页面元素定位
   - 找到`publisher=Office of Tariff Affairs and Trade Agreements`筛选条件
   - 定位`res_format=EXCEL`的复选框
   - 获取搜索结果中的Excel下载链接

2. 自动化流程（Automa脚本示例）
   ```python
   # Automa伪代码示例
   open("https://catalog.data.gov/dataset")
   select_publisher("Office of Tariff Affairs and Trade Agreements")
   check_excel_format()
   submit_search()
   wait_for_download_link()
   download_file("usitc_tariff_data.xlsx")

3. 数据处理
    解析harmonized-system.csv 中的HS Code数据
    提取商品描述、父级分类、层级关系
    ```json
     # 存储为JSON格式 示例：
    json
    {
    "hscode": "0101",
    "description": "Animals; live",
    "parent": "TOTAL",
    "level": 2
    } 



## 交付要求

### 代码仓库结构
```text
├── frontend-guardian/
│   ├── src/
│   └── package.json
└── usitc-data-scraper/
    └── automa_script.au
```

### 功能验收标准

#### 1.1 接口安全验证
- 通过Postman验证接口加密有效性（需提供请求签名算法说明）
- 提交接口攻击模拟报告（包含至少以下攻击向量测试）：
  ```text
  ▫️ SQL注入测试
  ▫️ XSS跨站脚本攻击测试
  ▫️ 请求头伪造测试
  ```

#### 1.2 风控机制验证
- 录制验证码触发/解除全程录屏（MP4格式）
- 提供行为监控日志截图（需包含时间戳、操作类型、风险等级字段）

#### 1.3 数据爬虫验证
- 展示爬取数据的样本截图（≥10条有效记录）
- 提交数据清洗前后对比表（需包含字段级变化说明）

### 2 文档要求

#### 2.1 README.md 必须包含
| 文档内容                | 格式要求                  |
|-------------------------|--------------------------|
| 技术架构图              | PlantUML或Mermaid格式     |
| 风控策略时序图          | PlantUML序列图            |
| Automa脚本执行步骤注释  | 逐行关键操作说明          |

#### 2.2 附加文档
1. **数据字段映射关系说明表**
   ```csv
   原始字段,清洗后字段,转换规则
   HS Code,hscode,去除特殊字符
   Commodity Description,description,简体中文转码
   ```

2. **异常处理流程文档**
   ```mermaid
   graph TD
   A[检测到异常] --> B{异常类型}
   B -->|网络超时| C[重试3次]
   B -->|验证码出现| D[触发人工介入]
   B -->|元素丢失| E[终止任务并报警]
   ```

---

## 评分标准

| 考核维度   | 占比  | 评分细则                                                                 |
|------------|-------|--------------------------------------------------------------------------|
| 代码质量   | 30%   | 1. 规范性（10%）<br>2. 健壮性（10%）<br>3. 可维护性（10%）               |
| 功能完整性 | 40%   | 1. 核心功能实现（25%）<br>2. 异常处理（10%）<br>3. 扩展性（5%）           |
| 创新性     | 20%   | 1. 风控绕过方案（10%）<br>2. 数据清洗优化（5%）<br>3. 性能提升（5%）       |
| 文档规范性 | 10%   | 1. 文档完整性（6%）<br>2. 可读性（4%）                                   |
