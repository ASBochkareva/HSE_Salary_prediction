\documentclass[a4paper]{article}

\usepackage[T2A]{fontenc}
\usepackage[utf8]{inputenc}
\usepackage[english,russian]{babel}

\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{booktabs}
\usepackage{longtable}
\usepackage{array}
\usepackage{float}
\usepackage{hyperref}
\usepackage{graphicx}
\usepackage{enumitem}

\hypersetup{
    colorlinks=true,
    linkcolor=blue,
    urlcolor=blue
}

\begin{document}

\section*{ПРОГНОЗИРОВАНИЕ ЗАРАБОТНОЙ ПЛАТЫ С ПРИМЕНЕНИЕМ АЛГОРИТМОВ МАШИННОГО ОБУЧЕНИЯ И МЕТОДОВ ОБРАБОТКИ ЕСТЕСТВЕННОГО ЯЗЫКА НА ОСНОВЕ ДАННЫХ О ВАКАНСИЯХ}

% ------------------------------------------------
\subsection*{Описание проекта}

Проект посвящен прогнозированию заработной платы на российском рынке труда на основе данных онлайн-вакансий. 
В исследовании используются структурные характеристики вакансий, геопространственные признаки, региональные макроэкономические показатели и текстовые описания вакансий. 

В рамках проекта реализован и сравнен ряд моделей машинного обучения и NLP:
\begin{itemize}
    \item линейные модели (OLS, Ridge);
    \item ансамблевые методы (CatBoost);
    \item трансформерные модели (ruBERT);
    \item мультимодальная нейросетевая архитектура.
\end{itemize}

% ------------------------------------------------
\subsection*{Цель проекта}

Разработка и сравнительный анализ моделей прогнозирования заработной платы с применением алгоритмов машинного обучения и методов обработки естественного языка.

% ------------------------------------------------
\subsection*{Основные результаты}

\begin{itemize}
    \item Добавление геопространственных и макроэкономических признаков улучшает качество табличных моделей.
    \item Наиболее высокое качество показала текстовая модель на основе ruBERT.
    \item Текст вакансии содержит наиболее информативные признаки уровня заработной платы.
    \item Мультимодальный подход показал потенциал, однако потребовал большего объема данных и более устойчивых механизмов fusion.
\end{itemize}

% ------------------------------------------------
\subsection*{Pipeline исследования}

\begin{table}[H]
\centering
\small
\begin{tabular}{p{1.2cm} p{2.8cm} p{3.2cm} p{6.5cm}}
\toprule
\textbf{Этап} & \textbf{Модель} & \textbf{Данные} & \textbf{Цель этапа} \\
\midrule

M1 & OLS / Ridge &
structure &
Построение интерпретируемого baseline-уровня без использования геопространственных и текстовых признаков \\

M2.1 & CatBoost &
structure + geo &
Выявление пространственных зависимостей и влияния географических факторов на заработную плату \\

M2.2 & CatBoost &
structure + macro &
Оценка вклада региональных макроэкономических показателей \\

M2.3 & CatBoost &
structure + geo + macro &
Совместный анализ геопространственных и макроэкономических факторов \\

M3 & ruBERT &
text &
Извлечение семантической информации из текстовых описаний вакансий \\

M4 & Multimodal NN &
structure + geo + macro + text &
Интеграция всех типов признаков в единую мультимодальную архитектуру \\

\bottomrule
\end{tabular}
\end{table}

% ------------------------------------------------
\subsection*{Метрики моделей}
Оценка качества: \textbf{GroupKFold} (5 фолдов) по \texttt{region\_name}.
Метрики усреднены по фолдам; таргет \texttt{salary\_from\_log}.
\begin{table}[H]
\centering
\caption{Сравнение моделей (CV, среднее по 5 фолдам)}
\begin{tabular}{lccc}
\toprule
\textbf{Модель} & \textbf{RMSE} & \textbf{MAE} & \textbf{R$^2$} \\
\midrule
OLS                    & 0.367 $\pm$ 0.027 & 0.292 $\pm$ 0.023 & 0.348 $\pm$ 0.037 \\
Ridge                  & 0.367 $\pm$ 0.027 & 0.292 $\pm$ 0.023 & 0.348 $\pm$ 0.037 \\
CatBoost (geo)         & 0.326 $\pm$ 0.025 & 0.255 $\pm$ 0.020 & 0.443 $\pm$ 0.043 \\
CatBoost (macro)       & 0.338 $\pm$ 0.010 & 0.264 $\pm$ 0.008 & 0.440 $\pm$ 0.020 \\
CatBoost (geo+macro)   & 0.315 $\pm$ 0.022 & 0.245 $\pm$ 0.018 & 0.481 $\pm$ 0.031 \\
ruBERT                 & 0.244 $\pm$ 0.013 & 0.178 $\pm$ 0.013 & 0.692 $\pm$ 0.010 \\
Multimodal NN          & 0.422 $\pm$ 0.112 & 0.340 $\pm$ 0.109 & 0.055 $\pm$ 0.415 \\
\bottomrule
\end{tabular}
\end{table}

% ------------------------------------------------
\subsection*{Структура репозитория}

\begin{table}[H]
\centering
\begin{tabular}{p{5cm} p{9cm}}
\toprule
\textbf{Ноутбук} & \textbf{Описание} \\
\midrule

00\_data\_preperation.ipynb &
EDA и обогащение структурных + гео + макро данных, подготовка витрин для M1, M2.1, M2.2, M2.3 \\

00\_text\_preperation.ipynb &
EDA и предобработка текстовых данных, подготовка витрины для M3 \\

00\_full\_text\_preperation.ipynb &
EDA  предобработка text данных для M3 \\

01\_model\_M1\_linear.ipynb &
M1, Модели OLS и Ridge регрессия, structure признаки\\

02\_1\_M2\_geo\_catboost.ipynb &
M2.1, Модели CatBoost, structure + geo признаки \\

02\_2\_M2\_macro\_catboost.ipynb &
M2.2, Модели CatBoost, structure + macro признаки \\

02\_3\_M2\_geo\_macro\_catboost.ipynb &
M2.3, Модели CatBoost, structure + geo + macro признаки \\

03\_M3\_text\_ruBERT.ipynb &
M3, Модель ruBERT, text признаки \\

04\_M4\_full.ipynb &
Мультимодальная нейросетевая архитектура, structure + geo + macro + text признаки \\

10\_metrics.ipynb &
Сравнение метрик моделей\\

\bottomrule
\end{tabular}
\end{table}

% ------------------------------------------------
\subsection*{Используемые данные}

\begin{itemize}
    \item данные онлайн-вакансий;
    \item структурированные характеристики вакансий;
    \item текстовые описания вакансий;
    \item геопространственные признаки;
    \item региональные макроэкономические показатели.
\end{itemize}

% ------------------------------------------------
\subsection*{Используемые технологии}

\begin{itemize}
    \item Python
    \item PyTorch
    \item Transformers (HuggingFace)
    \item CatBoost
    \item Scikit-learn
    \item SHAP
    \item Pandas / NumPy
\end{itemize}

% ------------------------------------------------
\subsection*{Ограничения проекта}

\begin{itemize}
    \item Межрегиональная неоднородность рынка труда.
    \item Ограничение вычислительных ресурсов при обучении мультимодальных моделей.
    \item Высокая чувствительность геопространственных embeddings к распределительному сдвигу между регионами.
    \item Разнородность и шум текстовых описаний вакансий.
\end{itemize}

% ------------------------------------------------
\subsection*{Дальнейшие направления исследования}

\begin{itemize}
    \item Развитие более устойчивых мультимодальных архитектур.
    \item Использование cross-attention и end-to-end fusion механизмов.
    \item Расширение объема обучающих данных.
    \item Исследование transfer learning между регионами.
\end{itemize}

% ------------------------------------------------
\subsection*{Ссылка на репозиторий}

\url{https://github.com/ASBochkareva/HSE_Salary_prediction}

% ------------------------------------------------
\begin{thebibliography}{9}

\bibitem{}
Список литературы будет добавлен позже.

\end{thebibliography}

\end{document}
