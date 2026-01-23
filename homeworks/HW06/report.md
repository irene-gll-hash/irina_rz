## 1. Dataset

- Какой датасет выбран: S06-hw-dataset-01.csv
- Размер: (12000, 26)
- Целевая переменная: target (класс 0: 65.4%, класс 1: 34.6%)
- Признаки: 24 числовых признака (num01-num24)

## 2. Protocol

- Разбиение: train/test (80/20, random_state=42, stratify=y)
- Подбор: CV на train (5 фолдов, оптимизация accuracy)
- Метрики: accuracy, F1, ROC-AUC

## 3. Models

- DummyClassifier: strategy='most_frequent'
- LogisticRegression: Pipeline(StandardScaler + LogisticRegression)
- DecisionTreeClassifier: max_depth [5, 10, None], min_samples_leaf [1, 5, 10], criterion [gini, entropy]
- RandomForestClassifier: n_estimators [100, 150], max_depth [10, None], min_samples_leaf [1, 3], max_features [sqrt, log2]
- GradientBoostingClassifier: n_estimators [100, 150], learning_rate [0.1, 0.2], max_depth [3, 5], min_samples_leaf [1, 3]
- StackingClassifier: базовые DecisionTree, RandomForest, GradientBoosting, мета-модель LogisticRegression, CV=5

## 4. Results

- DummyClassifier: Accuracy=0.6542, F1=0.0000, ROC-AUC=N/A
- LogisticRegression: Accuracy=0.9046, F1=0.8407, ROC-AUC=0.9598
- DecisionTreeClassifier: Accuracy=0.8675, F1=0.7857, ROC-AUC=0.8982
- RandomForestClassifier: Accuracy=0.9279, F1=0.8829, ROC-AUC=0.9670
- GradientBoostingClassifier: Accuracy=0.9350, F1=0.8964, ROC-AUC=0.9707
- StackingClassifier: Accuracy=0.9346, F1=0.8962, ROC-AUC=0.9704

Победитель: GradientBoostingClassifier (ROC-AUC=0.9707)

## 5. Analysis

- Устойчивость: При random_state [0, 10, 20, 30, 40] GradientBoosting ROC-AUC: 0.968-0.972

- Ошибки: Confusion matrix GradientBoosting:
  TN: 1542, FP: 85
  FN: 72, TP: 701

- Интерпретация: Permutation importance GradientBoosting (top-10):
  num18: 0.0729
  num19: 0.0649
  num07: 0.0349
  num04: 0.0177
  num24: 0.0145
  num20: 0.0124
  num01: 0.0100
  num14: 0.0090
  num22: 0.0076
  num16: 0.0064

## 6. Conclusion

1. GradientBoosting показал лучший ROC-AUC (0.9707)
2. RandomForest и Stacking близки к результатам GradientBoosting
3. DecisionTree хуже ансамблей
4. Permutation importance выявил num18, num19 как самые важные признаки
5. Протокол с train/test и CV обеспечил корректную оценку
6. ROC-AUC наиболее информативен для бинарной задачи с дисбалансом
