# 研究概览 — Step 0

## 基本信息
- **课题：** 眼肌型重症肌无力（OMG）向全身型转化风险预测模型
- **研究类型：** 回顾性预测模型研究（RETROSPECTIVE PREDICTION MODEL）
- **工作目录：** D:\general\
- **数据文件：** data.csv（454例，11变量）
- **目标期刊：** European Journal of Neurology
- **缺失值处理：** 多重插补（Multiple Imputation）
- **第三种方法：** 由统计专家（Agent2/3a）在SAP阶段确定

## PICOS
- **P（人群）：** 眼肌型重症肌无力患者（OMG）
- **I（预测因素）：** 性别、自身免疫共病、发病年龄、发病症状、新斯的明试验、RNS、随访时间、胸腺影像学
- **C（对照）：** 无需对照（预测模型研究）
- **O（结局）：** 全身型转化（Generalization: 0=未转化，1=转化），转化率29.7%
- **S（设计）：** 回顾性队列，预测模型构建与验证

## 数据概览

| 变量 | 类型 | 取值 | 说明 |
|------|------|------|------|
| Gender | 二分类 | 1=231, 2=223 | 性别 |
| Autoimmune.comorbidities | 二分类 | 0=444, 1=10 | 自身免疫合并症，严重不平衡 |
| Onset age group | 三分类 | 1=75, 2=178, 3=201 | 发病年龄组 |
| Onset age | 连续 | 1-87, 均值42.3 | 发病年龄 |
| onset symptoms | 二分类 | 1=292, 2=162 | 首发症状类型 |
| neostigmin | 三分类 | 0=41, 1=371, NA=42 | 新斯的明试验 |
| RNS | 三分类 | 0=195, 1=172, NA=87 | RNS检测结果 |
| Follow up time | 连续 | 3-60月, 均值29.7 | 随访时间 |
| Generalization | 二分类 | 0=319, 1=135 | **结局变量** |
| Thymic radiology | 三分类 | 1=399, 2=36, NA=19 | 胸腺影像学 |
| ~~Thymussrug~~ | 常数 | 全部=2 | **排除** |

## 统计方法设计（初步）

**预测模型——3种方法：**
1. **Logistic回归** — 基础模型，变量筛选+多因素
2. **随机森林（Random Forest）** — 机器学习方法
3. **待定第3种方法** — 哲哥说"3种方法"，第三种选什么？XGBoost/LASSO/SVM？

**模型评价：**
- AUC（ROC曲线）
- 校准曲线（Calibration）
- 决策曲线分析（DCA）
- 内部验证：Bootstrap或交叉验证

## 数据注意事项
- `Thymussrug` 全部为常数2 → 排除，不能作为预测变量
- `Autoimmune.comorbidities` 严重不平衡（0:444, 1:10），需特殊处理
- 多个变量含NA值（neostigmin 42例, RNS 87例, Thymic radiology 19例）→ 需要讨论缺失值处理策略
