# Heart Failure Prediction Dataset
  
In this repository I learnt kaggle CVD dataset (https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction).  
EDA was made and ML-model was trained.  

# Data
  
1. `Age`: age of the patient, years;  
2. `Sex`: sex of the patient:  
        M - Male,  
        F - Female;  
3. `ChestPainType`: chest pain type:  
        TA - Typical Angina,  
        ATA - Atypical Angina,  
        NAP - Non-Anginal Pain,  
        ASY - Asymptomatic;  
4. `RestingBP`: resting blood pressure, mm Hg;  
5. `Cholesterol`: serum cholesterol, mm/dl;  
6. `FastingBS`: fasting blood sugar:  
        1 - if FastingBS > 120 mg/dl,  
        0 - otherwise;  
7. `RestingECG`: resting electrocardiogram results:  
        Normal - Normal,  
        ST - having ST-T wave abnormality (T wave inversions and/or ST elevation or depression of > 0.05 mV),  
        LVH - showing probable or definite left ventricular hypertrophy by Estes' criteria;  
8. `MaxHR`: maximum heart rate achieved, bpm;  
9. `ExerciseAngina`: exercise-induced angina:  
        Y - Yes,  
        N - No;  
10. `Oldpeak`: oldpeak = ST, Numeric value measured in depression;  
11. `ST_Slope`: the slope of the peak exercise ST segment:  
        Up: upsloping,  
        Flat: flat,  
        Down: downsloping;  
12. **`HeartDisease`**: output class:  
        1 - heart disease,  
        0 - Normal.  

# EDA   
  
Рассмотрим количественные признаки и их отношения с целевым признаком:  
- Средний возраст здорового человека ниже среднего возраста больного;  
- Больные ССЗ в среднем имеют более выское кровяное давление в спокойном состоянии, чем здоровые;  
- Уровень холестерина в крови у больных и здоровых пациентов в среднем не отличается;  
- У людей с сердечно-сосудистыми заболеваниями в среднем более низкий пульс;  
- Сильные различия между группами пациентов наблюдаются в показаниях депрессии сегмента ST рассмотренного совместно с T-волной на ЭКГ.  
  
<img src="Num Cols Distrib.png">  
  
Перечисленные выше выводы являются статистически значимыми на при принятом уровне значимости $\alpha = 0.05$. Кроме вывода о равенстве уровня холестерина - это утверждение будет статистически значимым при $\alpha = 0.001$.
  
Составим усредненный портрет больного ССЗ:  
1. Мужчина;  
2. Жалуется на бессимптомную боль в груди;  
3. Наличие аномалии зубца ST-T на ЭКГ;  
4. Наблюдается стенокардия, вызванная физической нагрузкой;  
5. Плоский пиковый ST-сегмент;  
6. Уровень сахара в крови натощак больше 120 мг/дл.  
   
<img src="Cat Cols Distrib.png">
  
# Models
  
## Decision Tree
  
**Описание узла дерева:**
- Первоя строка - *условие* разделения, налево - *True*, направо - *False*;  
- Вторая строка `samples` - *количество* пациентов в этом узле/листе;  
- Третья строка `values` - разделение пациентов [**ЕСТЬ ССЗ**, **НЕТ ССЗ**];  
- Четвертая строка  - *метка* класса.  
  
Если безприкословно слушаться этой схемы, то можно определять наличие или отсутствие ССЗ в точностью 81,35%, чего делать ***не рекомендуется***.  
Данная схема играет роль *вспомогательного* инструмента врача при диагностике заболеваний.  
  
<img src="Decision Tree.png">
  
## Final metrics

Способ объединения моделей:  
`final preds` = $\dfrac{\sum_{i=1}^{n}y_ic_i}{\sum_{i=1}^{n}c_i}$, где  
  
- $y_i$ - перекодированное предсказание i-ой модели, $y_i$ ~ $[-1; 1]$  
- $c_i$ - среднее метрик качества i-ой модели.
  
<img src="Result Metrics.png">
  
# ML Results & Conclusions
  
Таким образом, была построена модель, конечным предсказанием которой является *взвешенное по основным метрикам предсказание других моделей машинного обучения*:
1) Метод Опроных Векторов;  
2) Случайный Лес;  
3) Логистическая Регрессия;  
4) Градиентный бустинг.  
  
Такое объединение не дало увеличения метрик.  
Наулучшей одиночной моделью является случайный лес.  
