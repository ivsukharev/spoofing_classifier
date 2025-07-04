# Обнаружение атак Spoofing в IoT-трафике

Этот Jupyter-ноутбук демонстрирует построение и интерпретацию классификатора для обнаружения атак типа **Spoofing** (в частности, **DNS Spoofing** и **MITM-ArpSpoofing**) в сетевом трафике IoT-устройств. Особое внимание уделяется **объяснению предсказаний** модели с помощью SHAP и **анализу устойчивости** к адаптивным атакам.

**Датасет:**  
[IoT Intrusion Detection Dataset на Kaggle](https://www.kaggle.com/datasets/subhajournal/iotintrusion/data)  
**Файл:** `IoT_Intrusion.csv`

---

## Структура проекта

### 1. Загрузка и предобработка данных
- Чтение исходного CSV-файла и анализ признаков.
- Отбор только нужных классов: `DNS_Spoofing`, `MITM-ArpSpoofing`, `Others`.
- Очистка и масштабирование числовых признаков (`StandardScaler`).
- Кодирование классов с помощью `LabelEncoder`.

### 2. Построение классификатора
- Разделение на обучающую и тестовую выборки.
- Обучение модели `RandomForestClassifier`.
- Оценка по метрикам:
  - accuracy  
  - precision  
  - recall  
  - f1-score
- Визуализация **матрицы ошибок**.

### 3. Интерпретация модели с SHAP
- Вычисление SHAP-значений с помощью `TreeExplainer`.
- Глобальный анализ:
  - `bar_plot` по средним значениям SHAP.
- Локальный анализ:
  - `waterfall_plot` для отдельных примеров.
  - Описание вкладов признаков в предсказание.
- Аналитические таблицы:
  - Ранги признаков по важности.
  - Средние значения SHAP для классов.

### 4. Адаптивная атака (Adversarial perturbation)
- Создание копий подмножества атакующих примеров.
- Малые модификации ключевых признаков:
  - `IAT`, `Number`, `Weight`, `rst_count`, `urg_count`, `flow_duration` и др.
- Замена оригиналов на модифицированные данные.
- Повторный прогон предсказания:
  - Сравнение метрик ДО и ПОСЛЕ атаки.
  - Значительное снижение recall/F1 для spoofing-классов.

---

## Часть полученного результата

```text
=== BEFORE attack ===
DNS_Spoofing recall: 0.64
MITM-ArpSpoofing recall: 0.80

=== AFTER attack ===
DNS_Spoofing recall: 0.10
MITM-ArpSpoofing recall: 0.31
```

---

## Быстрый старт

1. **Клонировать репозиторий** или скачать ноутбук и датасет в одну папку.
2. **Создать виртуальное окружение** и активировать его:
   ```bash
   python3 -m venv venv
   source venv/bin/activate      # для macOS/Linux
   ```
3. **Убедиться, что ipywidgets работают в JupyterLab**:
   ```bash
   pip install ipywidgets
   ```
4. **Запустить Jupyter Notebook**:
   ```bash
   jupyter notebook
   ```
5. Открыть `Spoofing classifier.ipynb` и выполнить ячейки сверху вниз.

---
