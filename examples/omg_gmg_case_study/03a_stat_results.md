# 统计结果报告 — Step 3a

## 模型性能对比

| 模型 | AUC | 95% CI | 排名 |
|------|-----|--------|------|
| **随机森林** | **0.796** | 0.718-0.866 | 🥇 |
| Logistic回归 | 0.777 | 0.697-0.853 | 🥈 |
| XGBoost | 0.708 | 0.601-0.796 | 🥉 |

## 模型1: Logistic回归（逐步回归）

### 入选变量

| 变量 | OR | 95% CI | p值 |
|------|-----|--------|------|
| **RNS（阳性）** | **4.63** | 2.63-8.16 | **<0.001** |
| **Thymic radiology** | **3.75** | 1.29-10.88 | **0.015** |
| Onset age（每岁） | 1.02 | 1.01-1.04 | 0.005 |
| Follow up time（每月） | 0.97 | 0.95-0.98 | <0.001 |

**逐步回归过程：** 从8个变量开始，逐步剔除了 Gender、Onset age group、onset symptoms、neostigmin（p>0.1），最终保留4个独立预测因子。

### 排除的变量
- Gender（p=0.134）
- Onset age group（p=0.626，被Onset age连续变量替代）
- onset symptoms（p=0.408）
- neostigmin（p=0.136）

## 模型2: 随机森林

**最佳参数：** n_estimators=300, max_depth=7, min_samples_split=10, class_weight='balanced'

**特征重要性排序：**
1. Follow up time（30.5%）
2. Onset age（28.0%）
3. RNS（19.6%）
4. Onset age group（7.1%）
5. Thymic radiology（5.7%）

## 模型3: XGBoost

**参数：** n_estimators=400, max_depth=5, learning_rate=0.05, subsample=0.8

**特征重要性排序：**
1. RNS（22.2%）
2. Thymic radiology（18.3%）
3. Follow up time（12.4%）
4. neostigmin（11.2%）
5. Onset age（10.8%）

## 基线特征（Table 1）

| 变量 | 未转化(n=319) | 转化(n=135) | p值 |
|------|--------------|------------|------|
| **Onset age** | 39.6±22.6 | 51.0±16.8 | **<0.001** |
| **Follow up time** | 30.0±22.1 | 15.7±14.4 | **<0.001** |
| Gender（男） | 164（51.4%） | 67（49.6%） | 0.807 |
| Onset age group 3 | 123（38.6%） | 78（57.8%） | **<0.001** |
| RNS（阳性） | 97（36.6%）* | 75（73.5%）* | **<0.001** |
| Thymic radiology 2 | 13（4.3%） | 23（17.3%） | **<0.001** |
| neostigmin（阳性） | 253（87.8%） | 118（95.2%） | **0.036** |
| onset symptoms（2型） | 105（32.9%） | 57（42.2%） | 0.074 |

> *RNS有缺失值，百分比按有效样本计算

## 关键发现总结

1. **随机森林表现最佳（AUC 0.796）**，其次是Logistic回归（0.777），XGBoost（0.708）
2. **RNS是最稳定、最强的预测因子** — 三个模型中均排前3，Logistic中OR=4.63
3. **胸腺影像学异常**的预测权重很高（Logistic OR=3.75）
4. **发病年龄**是持续性的风险因素（每增加1岁，转化风险增加2%）
5. **随访时间短**者转化率高（可能是早期转化的患者随访时间短即被观察到转化）
6. 性别和首发症状类型在三种方法中均未显示出显著预测价值

## 输出文件

- **图表：** /mnt/d/general/figures/
  - figure1_roc.png — ROC曲线（三模型对比，带Bootstrap CI）
  - figure2_calibration.png — 校准曲线
  - figure3_dca.png — 决策曲线分析
  - figure4_importance.png — 特征重要性排序
- **表格：** /mnt/d/general/tables/
  - table1_baseline.csv — 基线特征
  - table2_logistic_or.csv — Logistic OR表
  - table3_comparison.csv — 模型性能对比
