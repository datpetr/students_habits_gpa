\documentclass[12pt,a4paper]{article}

% Required packages
\usepackage{float}
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage{amsmath,amsfonts,amssymb}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage{geometry}
\usepackage{fancyhdr}
\usepackage{titlesec}
\usepackage{booktabs}
\usepackage{array}
\usepackage{xcolor}
\usepackage{enumitem}
\usepackage{caption}
\usepackage{subcaption}
\usepackage{tcolorbox}
\usepackage{pgfplots}
\usepackage{tikz}
\usepackage{multirow}
\pgfplotsset{compat=1.18}

% Page setup
\geometry{margin=1in}
\pagestyle{fancy}
\fancyhf{}
\fancyhead[C]{STAT250 Final Project}
\fancyfoot[C]{\thepage}

% Custom colors
\definecolor{titleblue}{RGB}{0,102,204}
\definecolor{sectiongray}{RGB}{240,240,240}
% Define custom colors for the bars
\definecolor{lowstress}{RGB}{102, 194, 165}     % Teal green
\definecolor{modstress}{RGB}{252, 141, 98}      % Orange
\definecolor{highstress}{RGB}{231, 138, 195}    % Pink

% Define colors for box plot (matching your original chart)
\definecolor{lowstressbox}{RGB}{134, 175, 156}
\definecolor{modstressbox}{RGB}{204, 142, 103}
\definecolor{highstressbox}{RGB}{132, 147, 168}

% Additional colors for regression analysis
\definecolor{darkblue}{RGB}{0,51,102}
\definecolor{lightblue}{RGB}{173,216,230}
\definecolor{darkgreen}{RGB}{0,100,0}

% Title formatting
\titleformat{\section}{\large\bfseries\color{titleblue}}{\thesection}{1em}{}
\titleformat{\subsection}{\normalsize\bfseries}{\thesubsection}{1em}{}

% Custom boxes
\newtcolorbox{importantbox}{
    colback=yellow!10,
    colframe=orange!75!black,
    boxrule=0.5pt,
    rounded corners,
    left=5pt,
    right=5pt,
    top=5pt,
    bottom=5pt
}

\newtcolorbox{methodbox}{
    colback=blue!5,
    colframe=blue!50!black,
    boxrule=0.5pt,
    rounded corners,
    left=5pt,
    right=5pt,
    top=5pt,
    bottom=5pt
}

\newtcolorbox{conclusionbox}{
    colback=darkgreen!10,
    colframe=darkgreen,
    boxrule=0.5pt,
    rounded corners,
    left=5pt,
    right=5pt,
    top=5pt,
    bottom=5pt
}

% Document settings
\setlength{\parskip}{6pt}
\setlength{\parindent}{0pt}

\begin{document}

% Title Page
\begin{titlepage}
    \centering
    \vspace*{2cm}
    
    {\Huge\bfseries\color{titleblue} STAT250 FINAL PROJECT}
    
    \vspace{2cm}
    
    {\LARGE\bfseries "Impact of Lifestyle Factors and Time Allocation on Students' GPA and Stress Levels"\\}

      
    \vspace{10cm}
    
    {\Large 
    \textbf{Made by:}\\[0.5cm]
    Elizaveta Polyakova\\
    Petr Datsenko\\
    Samuil Datsenko\\
    }

    
    
\end{titlepage}

% Table of Contents
\tableofcontents
\newpage

% Abstract Section - Insert this before the EDA section
\section{Abstract}

This project examines how university students' lifestyle habits, including study time, sleep duration, and participation in extracurricular activities, influence their stress levels and academic performance (GPA). Using R and Python, we analyzed survey data from 2,000 students, beginning with an exploration of distributions through visualizations and descriptive statistics. 

We then conducted hypothesis tests to assess whether students meet recommended sleep thresholds (z-test), whether GPA varies significantly by stress level (ANOVA), regression analysis to quantify the relationship between study hours and GPA, and a comparison of social interaction time between high and low study groups (Welch's t-test). The findings offer important insights into the relationship between student well-being and academic outcomes, supported by clear R and Python generated graphs and statistical evidence.

% Introduction Section - Insert this after the Abstract section
\section{Introduction}

University students often face challenges in balancing academic responsibilities, personal health, and social engagement, with stress playing a critical role in both their well-being and academic performance. This project investigates how daily habits such as study time, sleep duration, and involvement in extracurricular activities relate to students' stress levels and GPA. 

By analyzing survey data from 2,000 students, we aim to better understand these relationships and offer insights that may help students structure their routines to support both academic success and overall well-being.

\vspace{0.5cm}

% Main Content
\section{Exploratory Data Analysis (EDA)}

\begin{importantbox}
\textbf{Dataset Overview:} Our dataset contains results of a 2024 Google Form survey of 2000 university students, primarily from India. The survey's aim was to collect information about the students' lifestyle, academic performance and stress levels. Our goal is to arrive to conclusions about effects of different factors in students' lives on their grades and well being.
\end{importantbox}

\subsection{Variable Description}

We have eight variables in our dataset:

\begin{itemize}[leftmargin=*]
    \item \textbf{Student ID:} Discrete (nominal) variable
    \item \textbf{Stress Level:} Ordinal variable  
    \item \textbf{Six continuous (interval) variables:}
    \begin{itemize}
        \item Study hours per day
        \item Extracurricular activity hours per day
        \item Sleep hours per day
        \item Social interaction hours per day
        \item Physical activity hours per day
        \item GPA
    \end{itemize}
\end{itemize}

Below we will analyze the discrete and continuous variables separately for better understanding.

\subsection{Discrete Variables}

First we will look at the \textbf{"Student ID"} and \textbf{"Stress Level"} variables which are discrete.

Student ID is a simple sequence of numbers from 1 to 2000 to show that each observation is unique and belongs to a certain student. Since it's a discrete (nominal) variable there's no point in creating a graph for it or finding any correlation with the ID and other factors which is why we will not be using it in our research.

Stress level is an ordinal variable with 3 possible levels of stress that students could identify with: \textbf{Low}, \textbf{Moderate} and \textbf{High}. As we can deduce from the data analysis, \underline {more than half of students claim to have "High" stress levels}.

\begin{figure}[H] 
\centering
\begin{tikzpicture}
\begin{axis}[
    ybar,
    width=12cm,
    height=8cm,
    title={\textbf{Percentage of Students by Stress Level}},
    title style={yshift=0.3cm, font=\normalsize\bfseries},
    xlabel=\textbf{Stress Level},
    ylabel=\textbf{Percentage of Students (\%)},
    xmin=-0.5,
    xmax=2.5,
    ymin=0,
    ymax=60,
    xtick={-0.65 ,1,2.65},  % Positions of the bars
    xticklabels={Low, Moderate, High},  % Labels centered on bars
    xticklabel style={font=\small, yshift=-2pt},
    ytick={0,10,20,30,40,50},
    yticklabel style={font=\small},
    grid=major,
    grid style={color=gray!20, line width=0.2pt},
    axis background/.style={fill=white},
    axis line style={line width=0.5pt, color=black},
    tick style={line width=0.5pt, color=black},
    bar width=0.6,
    enlarge x limits=0.25,
    nodes near coords,
    nodes near coords style={
        color=black,
        font=\small,
        anchor=south,
        yshift=2pt
    },
    nodes near coords={\pgfmathprintnumber{\pgfplotspointmeta}\%},
]

% Colorful bars with proper positioning
\addplot[
    fill=green!60!black,  % Dark green for low stress
    draw=black,
    line width=0.3pt
] coordinates {(0, 14.8)};

\addplot[
    fill=yellow!70!orange,  % Gold/yellow for moderate stress
    draw=black,
    line width=0.3pt
] coordinates {(1, 33.7)};

\addplot[
    fill=red!80!black,  % Dark red for high stress
    draw=black,
    line width=0.3pt
] coordinates {(2, 51.4)};

\end{axis}
\end{tikzpicture}
\caption{Distribution of stress levels among university students (N=2000). The majority of students (51.4\%) report high stress levels, while only 14.8\% report low stress levels.}
\label{fig:stress_distribution}
\end{figure}

\subsection{Continuous Variables}

Secondly, we will be analyzing the continuous variables with descriptive statistics.

\begin{table}[h!]
\centering
\caption{Descriptive Statistics for Continuous Variables}
\begin{tabular}{|l|c|c|c|c|c|}
\hline
\textbf{Variable} & \textbf{Median} & \textbf{Mean} & \textbf{Standard Dev.} & \textbf{Minimum} & \textbf{Maximum} \\
\hline
Study Duration & 7.40 & 7.48 & 1.4 & 5.0 & 10.0 \\
\hline
Physical Activity & 2.00 & 4.33 & 2.5 & 0.0 & 13.0 \\
\hline
Sleep Duration & 7.50 & 7.50 & 1.5 & 5.0 & 10.0 \\
\hline
Social Interaction & 2.60 & 2.70 & 1.7 & 0.0 & 6.0 \\
\hline
Extracurricular Time & 4.33 & 1.99 & 1.2 & 0.0 & 4.0 \\
\hline
GPA & 3.00 & 3.12 & 0.3 & 2.2 & 4.0 \\
\hline
\end{tabular}
\label{tab:descriptive_stats}
\end{table}

\subsubsection{Distribution Analysis}

\begin{importantbox}
The table below presents the explanatory variables for our interval factors. The \textbf{mean ($\bar{x}$)} provides the average value for each factor, while the \textbf{standard deviation ($s$)} indicates the amount of dispersion within the data. Additionally, the maximum and minimum values reveal the highest and lowest observations for each variable.
\end{importantbox}

\begin{figure}[h]
\centering
\includegraphics[width=0.9\textwidth]{images/histogram_distributions.png}
\caption{Distributions of Student Habits (in Hours Per Day) and GPA}
\label{fig:histograms}
\end{figure}

\textbf{Key Observations from Distribution Analysis:}

\begin{itemize}[leftmargin=*]
    \item The \textbf{"GPA"} is somewhat \textcolor{blue}{\textbf{normally distributed}}
    \item The \textbf{"Physical Activity"} hours per day is \textcolor{red}{\textbf{right-skewed}}
    \item \textbf{"Social"} hours per day is only very slightly \textcolor{red}{\textbf{right-skewed}}
    \item Other variables don't seem to have a particular lean and are somewhat random, having sudden peaks and lows
    \item \textbf{Important:} Neither of the factors has any extreme outliers
\end{itemize}

\section{Statistical Methods and Inference}

\subsection{Research Question 1: Sleep Duration Analysis}

\textbf{Question:} Do students on average get at least 7 hours of sleep, which is the recommended minimum amount for adults?

\textbf{Approach:} We have a null hypothesis of the population mean being equal to 7 hours or more which will be tested using the z-test.

\subsubsection{Assumptions for Z-Test}

Since we will be using z-test we have certain assumptions and conditions which need to be met:

\begin{enumerate}[leftmargin=*]
    \item \textbf{Large Sample Size (Central Limit Theorem):} Since we have a large sample of 2000 observations we can argue that the sample mean will be approximately normally distributed even though the population is not. Thus, we apply the CLT.
    
    \item \textbf{Unknown Population Variance:} Since we don't know the population variance we will be using the $s$ instead of $\sigma$. Due to the large sample size, the sample variance is a reliable estimator of the population variance.
    
    \item \textbf{Independence of Observations:} We assume each data point to be independent. We can't check this, but since we got the dataset from a reliable website we need to trust that the sampling technique used was not biased.
    
    \item \textbf{Extreme Outliers:} As we have seen during the data analyzation process, our data does not have any extreme outliers and thus we don't need to worry about it sabotaging our measures.
\end{enumerate}

\subsubsection{Test Statistics and Calculations}

\textbf{Basic Statistics and What We're Testing}

\textbf{What we know:}
\begin{itemize}[leftmargin=*]
    \item \textbf{Sample Size:} 2,000 students
    \item \textbf{Sample Mean:} $\bar{x} = 7.5$ hours of sleep (average from our sample) 
    \item \textbf{Standard Deviation:} $s = 1.5$ hours (how much individual sleep times vary)
    \item \textbf{Standard Error:} $SE = \frac{s}{\sqrt{n}} = \frac{1.5}{\sqrt{2000}} = 0.0335$ (precision of our estimate)
    \item \textbf{Significance Level:} $\alpha = 0.05$ (5\% chance we're wrong when rejecting null hypothesis)
\end{itemize}

\textbf{Hypotheses:}
\begin{itemize}[leftmargin=*]
    \item \textbf{Null Hypothesis ($H_0$):} Students sleep 7 hours or less per day ($\mu \leq 7$)
    \item \textbf{Alternative Hypothesis ($H_1$):} Students sleep more than 7 hours per day ($\mu > 7$)
\end{itemize}

{Calculating the Test Statistic and p-value}

\begin{methodbox}
\textbf{Z-statistic calculation:}

The Z-statistic measures how far away the sample mean is from the hypothesized population mean:

$$Z = \frac{\bar{x} - \mu_0}{SE} = \frac{7.5 - 7}{0.0335} = \frac{0.5}{0.0335} = 14.93$$

\textbf{p-value calculation:}

The p-value gives us the probability of getting an observation as or more extreme than the one obtained:

$$\text{p-value} = P(Z > 14.93) = 1.95 \times 10^{-53}$$

\textbf{What this means:} Since p-value is extremely small (almost zero), we can be almost certain that the population has mean of more than 7 and we reject the null hypothesis.\\
\\
\textbf{99\% confidence interval calculation:}

The CI formula: $\bar{x} \pm z_{\alpha/2} \times SE$ \\

\textbf{Steps to calculate CI:}
\begin{enumerate}[leftmargin=*]
    \item The $z_{\alpha/2}$ for 99\% confidence is: $z_{0.005} = 2.576$
    \item The margin of error: $2.576 \times 0.0335 = 0.0863$
    \item The interval: $7.5 \pm 0.0863$
\end{enumerate}

\textcolor{blue}{\textbf{99\% Confidence Interval: [7.41, 7.59]}}

\textbf{What this means:} We're 99\% confident that the true average sleep time for all students (our population) is between 7.41 and 7.59 hours.
\end{methodbox}

\textbf{Conclusion for Sleep Analysis}

According to the z-test, p-value and the CI, we can confidently claim that the population average is higher than 7. Thus, we believe that the students, on average, get around 7 hours and 30 minutes of sleep which is not too high but at least it is more than bare healthy minimum for adults.

\subsection{Research Question 2: GPA and Stress Level Analysis}

\textbf{Question:} Is there a significant difference in GPA among students with different stress levels?

\textbf{Approach:} We will use One-Way ANOVA (Analysis of Variance) to test if there are significant differences in mean GPA across the three stress level groups (Low, Moderate, High).

\subsubsection{Assumptions for One-Way ANOVA}

Before conducting ANOVA, we must verify that our data satisfies the following assumptions:

\begin{enumerate}[leftmargin=*]
    \item \textbf{Independence of Observations:} Each student's GPA should be independent of others. We assume this is satisfied based on the survey design where individual responses were collected independently.
    
    \item \textbf{Normality:} The GPA values within each stress level group should be approximately normally distributed. We will test this using the Shapiro-Wilk test for each group.
    
    \item \textbf{Homogeneity of Variances (Homoscedasticity):} The variance of GPA should be approximately equal across all three stress level groups. We will test this using Levene's test.
\end{enumerate}

\subsubsection{Descriptive Statistics by Stress Level}

Before conducting the formal test, let's examine the descriptive statistics for GPA across stress levels:

\begin{table}[H]
\centering
\caption{Descriptive Statistics for GPA by Stress Level}
\begin{tabular}{|l|c|c|c|c|c|}
\hline
\textbf{Stress Level} & \textbf{N} & \textbf{Mean} & \textbf{Std. Dev.} & \textbf{Min} & \textbf{Max} \\
\hline
Low & 296 & 2.78 & 0.28 & 2.25 & 3.60 \\
\hline
Moderate & 674 & 2.99 & 0.27 & 2.45 & 3.75 \\
\hline
High & 1028 & 3.23 & 0.29 & 2.30 & 4.00 \\
\hline
\textbf{Overall} & \textbf{1998} & \textbf{3.12} & \textbf{0.30} & \textbf{2.25} & \textbf{4.00} \\
\hline
\end{tabular}
\label{tab:gpa_descriptive}
\end{table}

\subsubsection{Visualization of GPA Distribution by Stress Level}

\begin{figure}[H]
\centering
\begin{tikzpicture}
\begin{axis}[
    width=12cm,
    height=8cm,
    xlabel={Stress Level},
    ylabel={GPA},
    xmin=0.5, xmax=3.5,
    ymin=2.2, ymax=4.1,
    xtick={1,2,3},
    xticklabels={\textbf{Low}, \textbf{Moderate}, \textbf{High}},
    ytick={2.25, 2.50, 2.75, 3.00, 3.25, 3.50, 3.75, 4.00},
    grid=major,
    grid style={line width=.1pt, draw=gray!30},
    axis background/.style={fill=white},
    tick label style={font=\small},
    label style={font=\large},
    title={GPA Distribution by Stress Level},
    title style={font=\Large\bfseries, yshift=0.3cm}
]

% Low Stress Box Plot (x=1)
\draw[lowstressbox, very thick] (axis cs:0.85,2.63) rectangle (axis cs:1.15,2.92);
\draw[lowstressbox, very thick] (axis cs:0.85,2.78) -- (axis cs:1.15,2.78);
\draw[lowstressbox, thick] (axis cs:1.00,2.63) -- (axis cs:1.00,2.25);
\draw[lowstressbox, thick] (axis cs:1.00,2.92) -- (axis cs:1.00,3.35);
\draw[lowstressbox, thick] (axis cs:0.95,2.25) -- (axis cs:1.05,2.25);
\draw[lowstressbox, thick] (axis cs:0.95,3.35) -- (axis cs:1.05,3.35);

% Moderate Stress Box Plot (x=2)
\draw[modstressbox, very thick] (axis cs:1.85,2.82) rectangle (axis cs:2.15,3.16);
\draw[modstressbox, very thick] (axis cs:1.85,2.99) -- (axis cs:2.15,2.99);
\draw[modstressbox, thick] (axis cs:2.00,2.82) -- (axis cs:2.00,2.45);
\draw[modstressbox, thick] (axis cs:2.00,3.16) -- (axis cs:2.00,3.55);
\draw[modstressbox, thick] (axis cs:1.95,2.45) -- (axis cs:2.05,2.45);
\draw[modstressbox, thick] (axis cs:1.95,3.55) -- (axis cs:2.05,3.55);

% High Stress Box Plot (x=3)
\draw[highstressbox, very thick] (axis cs:2.85,3.05) rectangle (axis cs:3.15,3.41);
\draw[highstressbox, very thick] (axis cs:2.85,3.23) -- (axis cs:3.15,3.23);
\draw[highstressbox, thick] (axis cs:3.00,3.05) -- (axis cs:3.00,2.55);
\draw[highstressbox, thick] (axis cs:3.00,3.41) -- (axis cs:3.00,4.00);
\draw[highstressbox, thick] (axis cs:2.95,2.55) -- (axis cs:3.05,2.55);
\draw[highstressbox, thick] (axis cs:2.95,4.00) -- (axis cs:3.05,4.00);

\end{axis}
\end{tikzpicture}
\caption{Box plot showing GPA distribution across stress levels. There's a clear upward trend in median GPA as stress level increases.}
\label{fig:gpa_boxplot_improved}
\end{figure}

\subsubsection{Testing ANOVA Assumptions}

\textbf{1. Normality Test (Shapiro-Wilk Test)}

\begin{table}[H]
\centering
\caption{Normality Tests by Stress Level}
\begin{tabular}{|l|c|c|c|}
\hline
\textbf{Stress Level} & \textbf{W-statistic} & \textbf{p-value} & \textbf{Normal?} \\
\hline
Low & 0.987 & 0.089 & Yes (p > 0.05) \\
\hline
Moderate & 0.991 & 0.156 & Yes (p > 0.05) \\
\hline
High & 0.985 & 0.0004 & No (p < 0.05) \\
\hline
\end{tabular}
\label{tab:normality_test}
\end{table}

\textbf{2. Homogeneity of Variances (Levene's Test)}

\begin{methodbox}
\textbf{Levene's Test Results:}
\begin{itemize}[leftmargin=*]
    \item \textbf{Test Statistic:} F = 4.82
    \item \textbf{p-value:} p = 0.008
    \item \textbf{Conclusion:} Variances are \textbf{not equal} across groups (p < 0.05)
\end{itemize}
\end{methodbox}

\subsubsection{ANOVA Assumption Violations}

Since our data violates both the normality assumption (for the High stress group) and the homogeneity of variances assumption, we have two options:

\textbf{Dealing with Assumption Violations:}
\begin{enumerate}[leftmargin=*]
    \item \textbf{Welch's ANOVA:} Robust to unequal variances
    \item \textbf{Kruskal-Wallis Test:} Non-parametric alternative when normality is violated
\end{enumerate}

We will proceed with \textbf{Welch's ANOVA} as it handles unequal variances and is reasonably robust to moderate violations of normality, especially with large sample sizes.

\subsubsection{Hypothesis Testing}

\begin{methodbox}
\textbf{Hypotheses:}
\begin{itemize}[leftmargin=*]
    \item \textbf{Null Hypothesis ($H_0$):} $\mu_{\text{Low}} = \mu_{\text{Moderate}} = \mu_{\text{High}}$
    
    All stress level groups have equal mean GPA
    
    \item \textbf{Alternative Hypothesis ($H_1$):} At least one group mean is different
    
    \item \textbf{Significance Level:} $\alpha = 0.05$
\end{itemize}
\end{methodbox}

\subsubsection{Welch's ANOVA Results}

\begin{table}[h]
\centering
\caption{Welch's One-Way ANOVA Results: GPA by Stress Level}
\begin{tabular}{|l|c|c|c|c|c|}
\hline
\textbf{Source} & \textbf{Sum of Squares} & \textbf{df} & \textbf{Mean Square} & \textbf{F-statistic} & \textbf{p-value} \\
\hline
Between Groups & 84.23 & 2 & 42.115 & 467.65 & < 0.001 \\
\hline
Within Groups (adjusted) & 77.74 & 863.39 & 0.090 & - & - \\
\hline
\end{tabular}
\label{tab:welch_anova}
\end{table}

\begin{methodbox}
\textbf{Welch's ANOVA Test Results:}

\begin{itemize}[leftmargin=*]
    \item \textbf{F-statistic:} F(2, 863.39) = 467.65
    \item \textbf{p-value:} p < 0.001 (highly significant)
    \item \textbf{Degrees of Freedom:} Numerator df = 2, Denominator df = 863.39
    \item \textbf{Decision:} \textcolor{red}{\textbf{Reject H$_0$}} at $\alpha = 0.05$
    \item \textbf{Conclusion:} There are statistically significant differences in mean GPA across stress levels
\end{itemize}
\end{methodbox}

\subsubsection{Post-Hoc Analysis: Games-Howell Test}

Since we used Welch's ANOVA due to unequal variances, we use the Games-Howell post-hoc test (which doesn't assume equal variances) to determine which specific groups differ:

\begin{table}[H]
\caption{Post-Hoc Comparisons (Games-Howell Test)}
\begin{tabular}{|l|c|c|c|c|}
\hline
\textbf{Comparison} & \textbf{Mean Difference} & \textbf{95\% CI} & \textbf{p-value} & \textbf{Significant?} \\
\hline
Moderate vs Low & 0.208 & [0.161, 0.255] & < 0.001 & Yes \\
\hline
High vs Low & 0.445 & [0.403, 0.487] & < 0.001 & Yes \\
\hline
High vs Moderate & 0.237 & [0.205, 0.269] & < 0.001 & Yes \\
\hline
\end{tabular}
\label{tab:posthoc_improved}
\end{table}

\subsubsection{Interpretation of Results}

\textbf{Key Findings:}

\begin{enumerate}[leftmargin=*]
    \item \textbf{Statistically Significant Relationship:} There is strong evidence (p < 0.001) that GPA differs significantly across stress levels.
    
    \item \textbf{Direction of Relationship:} \textcolor{blue}{\textbf{Positive correlation}} - Higher stress is associated with higher GPA:
    \begin{itemize}
        \item Low Stress: Mean GPA = 2.78
        \item Moderate Stress: Mean GPA = 2.99
        \item High Stress: Mean GPA = 3.23
    \end{itemize}
    
    \item \textbf{All Pairwise Differences Significant:} Every comparison shows significant differences, indicating a clear stepwise pattern.
\end{enumerate}

\subsection{Research Question 3: Linear Regression Analysis - Study Hours vs GPA}

\textbf{Question:} What is the relationship between daily study hours and student GPA? Can we quantify how much GPA increases for each additional hour of study?

\subsubsection{Assumptions for Linear Regression}

Before conducting the regression analysis, we must verify that our data satisfies the following assumptions:

\begin{enumerate}[leftmargin=*]
    \item \textbf{Linearity:} The relationship between study hours and GPA should be linear
    \item \textbf{Independence:} Each student's data should be independent of others
    \item \textbf{Homoscedasticity:} The variance of residuals should be constant across all levels of the predictor
    \item \textbf{Normality of residuals:} The residuals should be approximately normally distributed
\end{enumerate}

\subsubsection{Initial Correlation Analysis}

Before conducting the formal regression analysis, we examine the correlation between study hours and GPA:

\textbf{Correlation Coefficient:} $r = 0.7344$

This indicates a \textbf{strong positive correlation} between study hours and GPA, suggesting that students who study more hours per day tend to achieve higher GPAs.

\subsubsection{Model Specification}

The linear regression model is specified as:
\begin{equation}
\text{GPA}_i = \beta_0 + \beta_1 \times \text{Study Hours}_i + \epsilon_i
\end{equation}

where:
\begin{itemize}[leftmargin=*]
    \item $\text{GPA}_i$ is the \textbf{response variable} (Grade Point Average for student $i$)
    \item $\text{Study Hours}_i$ is the \textbf{explanatory variable} (daily study hours for student $i$)
    \item $\beta_0$ is the intercept parameter
    \item $\beta_1$ is the slope parameter (effect of study hours on GPA)
    \item $\epsilon_i$ is the error term for student $i$
\end{itemize}

\subsubsection{Fitted Regression Model}

\begin{methodbox}
\textbf{Fitted Regression Equation:}
\begin{equation}
\widehat{\text{GPA}} = 1.9642 + 0.1541 \times \text{Study Hours}
\end{equation}

\textbf{Interpretation:}
\begin{itemize}[leftmargin=*]
    \item \textbf{Intercept ($\beta_0 = 1.9642$):} The predicted GPA when study hours = 0 is approximately 1.96
    \item \textbf{Slope ($\beta_1 = 0.1541$):} For each additional hour of daily study, GPA increases by approximately 0.154 points
    \item \textbf{R-squared = 0.539:} The model explains 53.9\% of the variance in GPA
\end{itemize}
\end{methodbox}

\subsubsection{Assumption Testing}

\textbf{Homoscedasticity Assessment:} The Breusch-Pagan test was conducted to check for constant variance of residuals. With a p-value of 0.9180, we reject the null hypothesis of homoscedasticity, indicating that heteroscedasticity is present in our model. This violation affects the reliability of standard errors.

\textbf{Normality of Residuals:} The Shapiro-Wilk test for normality of residuals yielded a p-value of 0.4635. Since this is greater than 0.05, we fail to reject the null hypothesis, indicating that the residuals appear to be normally distributed.

\subsubsection{Bootstrap Analysis for Robust Inference}

Due to the violation of the heteroskedasticity assumption, we employ bootstrap methods to obtain robust confidence intervals.

\begin{importantbox}
\textbf{Bootstrap Procedure:}
\begin{itemize}[leftmargin=*]
    \item \textbf{Number of Bootstrap Samples:} 5000
    \item \textbf{Sampling Method:} Random sampling with replacement
    \item \textbf{Parameters Estimated:} Intercept and slope coefficients
\end{itemize}
\end{importantbox}

\begin{table}[H]
\centering
\caption{Bootstrap Confidence Intervals (95\%)}
\begin{tabular}{|l|c|c|c|c|}
\hline
\textbf{Parameter} & \textbf{OLS Estimate} & \textbf{Bootstrap SE} & \textbf{95\% CI Lower} & \textbf{95\% CI Upper} \\
\hline
Intercept & 1.9642 & 0.02456 & 1.9164 & 2.0126 \\
\hline
Slope & 0.1541 & 0.00322 & 0.1477 & 0.1603 \\
\hline
\end{tabular}
\label{tab:bootstrap_results}
\end{table}

\begin{conclusionbox}
\textbf{Bootstrap Significance Testing:}
\begin{itemize}[leftmargin=*]
    \item \textbf{Intercept Significance:} True (95\% CI does not contain 0)
    \item \textbf{Slope Significance:} True (95\% CI does not contain 0)
\end{itemize}

\textbf{Robust Conclusion:} We are 95\% confident that the true population slope lies between [0.1477, 0.1603], confirming the significant positive relationship between study hours and GPA.
\end{conclusionbox}

\subsection{Research Question 4: Study Time vs Social Interaction Analysis}

\textbf{Question:} Is the average daily social interaction time significantly different between students who study more than the median number of hours per day (7.4) and those who study less or equal?

\textbf{Approach:} We aim to compare the average daily social interaction time between two groups of students — those who study more than the median and those who study less — using appropriate statistical tests based on assumptions.

\subsubsection{Assumptions for Two-Sample Hypothesis Testing}

Before deciding which statistical test to use, we check the following assumptions:

\begin{enumerate}[leftmargin=*]
    \item \textbf{Independence:} Observations in each group are assumed to be independent.
    \item \textbf{Scale of Measurement:} The dependent variable, social interaction time, is measured on a continuous scale.
    \item \textbf{Normality:} Each group's social interaction times should be approximately normally distributed.
    \item \textbf{Homogeneity of Variances:} The variances between the two groups should be equal.
\end{enumerate}

\subsubsection{Assumption Testing}

\textbf{Normality Assessment:} The Shapiro-Wilk test was applied to both groups. The resulting p-values were less than $2.2 \times 10^{-16}$ for both groups, indicating strong evidence against normality.

\textbf{Homogeneity of Variances:} Levene's Test was used to evaluate variance equality. The variance for the High Study group was $s_1^2 = 2.694$ and for the Low Study group it was $s_2^2 = 2.908$. The computed F-ratio was $F = \frac{2.694}{2.908} \approx 0.926$, and the p-value was 0.02189, indicating a statistically significant difference in variances.

\begin{importantbox}
\textbf{Conclusion on Assumptions:} Since the normality assumption is violated but the sample sizes are large ($n > 30$), we rely on the Central Limit Theorem. Given that the variances are unequal, we proceed with Welch's t-test.
\end{importantbox}

\subsubsection{Welch's t-Test Analysis}

\textbf{Formula for Test Statistic:}
\begin{equation} \label{eq:welch}
t = \frac{\bar{X}_1 - \bar{X}_2}{\sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}}
\end{equation}

where:
\begin{itemize}[leftmargin=*]
  \item $\bar{X}_1 = 2.48$, $s_1^2 = 2.694$, $n_1 = 983$ (High Study Group's Social Interaction Time Info)
  \item $\bar{X}_2 = 2.92$, $s_2^2 = 2.908$, $n_2 = 1017$ (Low Study Group's Social Interaction Time Info)
\end{itemize}

\begin{methodbox}
\textbf{Calculations:}

\textbf{Standard Error:}
\[
SE = \sqrt{\frac{2.694}{983} + \frac{2.908}{1017}} \approx 0.0740
\]

\textbf{Test Statistic:}
\[
t = \frac{2.48 - 2.92}{0.0740} = -5.9365
\]

\textbf{Degrees of Freedom:}
\[
df = \frac{\left(\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}\right)^2}{\frac{\left(\frac{s_1^2}{n_1}\right)^2}{n_1 - 1} + \frac{\left(\frac{s_2^2}{n_2}\right)^2}{n_2 - 1}} \approx 1998
\]
\end{methodbox}

\subsubsection{Test Summary and Results}

\begin{table}[H]
\centering
\caption{Welch's t-Test Results Summary}
\begin{tabular}{|l|c|}
\hline
\textbf{Statistic} & \textbf{Value} \\
\hline
$t$-statistic & -5.9365 \\
\hline
Degrees of Freedom & $\approx 1998$ \\
\hline
$p$-value & $1.712 \times 10^{-9}$ \\
\hline
95\% CI & $(-0.5822, -0.2778)$ \\
\hline
High Study Group Average Social Time & 2.48 hours (2h 29min) \\
\hline
Low Study Group Average Social Time & 2.92 hours (2h 55min) \\
\hline
\end{tabular}
\label{tab:welch_ttest}
\end{table}

\begin{conclusionbox}
\textbf{Statistical Interpretation:} The extremely small p-value ($1.712 \times 10^{-9}$) provides strong evidence to reject the null hypothesis at $\alpha = 0.05$. 

\textbf{Practical Conclusion:} Students who study more than the median number of hours tend to spend significantly less time socializing (2 hours 29 minutes vs 2 hours 55 minutes) compared to their peers who study less. The difference of approximately 26 minutes is both statistically significant and practically meaningful.
\end{conclusionbox}

\section{Results and Conclusions}

\subsection{Summary of Key Findings}

Our comprehensive statistical analysis of 2,000 university students has revealed several important insights about the relationship between lifestyle factors and academic performance:


\textbf{Major Findings:}

\begin{enumerate}[leftmargin=*]
    \item \textbf{Sleep Duration Analysis:} Students on average get 7.5 hours of sleep per day, which exceeds the recommended minimum of 7 hours (99\% CI: [7.41, 7.59])
    
    \item \textbf{Stress and GPA Relationship:} There is a significant positive relationship between stress levels and GPA:
    \begin{itemize}
        \item Low Stress: Mean GPA = 2.78
        \item Moderate Stress: Mean GPA = 2.99  
        \item High Stress: Mean GPA = 3.23
    \end{itemize}
    
    \item \textbf{Study Hours Impact:} Each additional hour of daily study increases GPA by approximately 0.154 points, with the model explaining 53.9\% of GPA variance
    
    \item \textbf{Study-Social Trade-off:} Students who study more than the median hours socialize significantly less (2h 29min vs 2h 55min daily), suggesting a time allocation trade-off
    
    \item \textbf{Student Stress Distribution:} 51.4\% of students report high stress levels, indicating significant mental health concerns in the student population
\end{enumerate}

\subsection{Implications for Students and Educators}

\textbf{For Students:}
\begin{itemize}[leftmargin=*]
    \item Increasing study time has a measurable positive impact on academic performance
    \item Higher stress levels, while concerning for well-being, are associated with better academic outcomes in this sample
    \item Most students achieve adequate sleep duration, supporting healthy learning
    \item There appears to be a trade-off between study time and social interaction time
\end{itemize}

\textbf{For Educators and Administrators:}
\begin{itemize}[leftmargin=*]
    \item The high prevalence of student stress (51.4\% reporting high stress) warrants attention to mental health support services
    \item Study time recommendations can be quantified: approximately 2 additional hours per day may increase GPA by ~0.3 points
    \item The complex relationship between stress and performance suggests need for balanced approaches to academic rigor
    \item The study-social time trade-off highlights the importance of helping students balance academic and social needs
\end{itemize}

\subsection{Final Conclusions}

This study provides valuable quantitative insights into factors affecting student academic performance and lifestyle choices. The strong positive relationship between study hours and GPA offers practical guidance for students, while the unexpected positive correlation between stress and academic performance raises important questions about student well-being and academic pressure.

The findings reveal a complex ecosystem of time allocation where increased study time correlates with higher academic performance but also with reduced social interaction time. This suggests that while academic intensity may drive higher performance, educational institutions should remain vigilant about student mental health and social well-being, striving to support academic excellence without compromising overall student development.

\section{References}

\begin{itemize}[leftmargin=*]
    \item Steve R. (2024). Student lifestyle and academic performance dataset. Kaggle.,\\\href{https://www.kaggle.com/datasets/steve1215rogg/student-lifestyle-dataset/data}{https://www.kaggle.com/datasets/steve1215rogg/student-lifestyle-dataset/data}
    \item Statistical Methods: R Statistical Software, Python Statistical Software
    
\end{itemize}

\end{document}
