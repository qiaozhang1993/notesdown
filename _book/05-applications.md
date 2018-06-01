
# 案例分析 {#applications}

## 喀麦隆及周边地区眼线虫病的空间分布

Loa loa (eyeworm) 是一种可致盲的热带疾病，APOC (African Programme for Onchocerciasis Control) 搜集了168个村庄的21938个样本，另外在研究区域1公里的范围内添加了样本周围的环境变量[@Thomson2004Mapping]，从美国地质调查局获得海拔信息(<https://www.usgs.gov/>)，以及来自卫星数据的植被绿色度(<http://free.vgt.vito.be>)。如表\@ref(tab:loaloa-data)所示


\begin{longtable}[t]{rrrrr}
\caption{(\#tab:loaloa-data)Loa loa 数据集（部分）}\\
\toprule
LONGITUDE & LATITUDE & NO\_EXAM & NO\_INF & ELEVATION\\
\midrule
8.04 & 5.74 & 162 & 0 & 108\\
8.00 & 5.68 & 167 & 1 & 99\\
8.91 & 5.35 & 88 & 5 & 783\\
8.10 & 5.92 & 62 & 5 & 104\\
8.18 & 5.11 & 167 & 3 & 109\\
8.93 & 5.36 & 66 & 3 & 909\\
\bottomrule
\end{longtable}

搜集的Loa loa 数据的空间分布，如图\@ref(fig:loaloa-ratio)，其中圆圈的大小分六个等级： 0.5, 1.0, 1.5, 2.0, 2.5, 3.0 分别对应 Loa loa 流行度的六个区间: [0,0.05), [0.05,0.15), [0.15,0.25), [0.25,0.35), [0.35,0.45),
[0.45,0.55)，这里超过 0.2，就列为高感染，需要对该地区采取措施，如派遣医疗队和药品等。




\begin{figure}

{\centering \includegraphics[width=0.6\linewidth]{figures/loaloa-map} 

}

\caption{Loa loa 流行度观测结果，黑点是采样点}(\#fig:loaloa-ratio)
\end{figure}

建立模型 \@ref(eq:SGLMM) $\log\{p_{ij}/(1-p_{ij})\} = \alpha + \beta'z_{ij} + U_{i} + S(x_{i}),$ 基于限制极大似然估计，计算得到固定效应参数如下表

| 参数        | 估计       | 条件标准差 | t 统计量 |
| :---------- | :--------- | :-------- | :------ |
| (Intercept) | -1.009e+01 | 2.9790516 | -3.3874 |
| elev1       | -2.825e-05 | 0.0006196 | -0.0456 |
| elev2       | 8.087e-04  | 0.0014786 | 0.5469  |
| elev3       | -1.138e-02 | 0.0025495 | -4.4629 |
| elev4       | 1.067e-02  | 0.0031547 | 3.3814  |
| maxNDVI1    | 1.072e+01  | 2.7334338 | 3.9221  |
| seNDVI      | -2.906e+00 | 4.4210991 | -0.6574 |

随机效应的参数$\nu = 0.24326,\phi = 0.01345,\sigma^2 = 6.236$，相应的空间预测结果如图 \@ref(fig:spamm-loaloa)所示

\begin{figure}

{\centering \includegraphics[width=0.7\linewidth]{figures/spaMM-loaloa} 

}

\caption{Loa loa 数据集上的预测结果}(\#fig:spamm-loaloa)
\end{figure}

## 冈比亚儿童疟疾的空间分布

2002年，Peter Diggle等人分析过冈比亚儿童疟疾数据，该数据采集自冈比亚的5个地域，65个村庄，2035个5岁以下儿童的血液样本，如图\@ref(fig:map-gambia)。

\begin{figure}

{\centering \includegraphics[width=0.7\linewidth]{figures/gambia-map} 

}

\caption{采样的村庄}(\#fig:map-gambia)
\end{figure}

并记录了他们的年龄、村庄的位置（GPS坐标）、血液中是否含有疟疾寄生虫、蚊帐是否使用、蚊帐是否杀虫、村庄周围绿色植物的覆盖度（RS测量）、村庄是否有医疗中心[@Diggle2002]。调查所得的数据如表\@ref(tab:gambia-malaria) 所示（篇幅所限展示部分）。数据各指标说明如下：

| 变量 | 含义 |
| :-- | :-- |
| $(x,y)$ | 村庄的坐标 |
| pos | 血样中是否出现寄生虫 （1表示是，0表示否） |
| age | 儿童的年龄（按天计算） |
| netuse | 儿童是否睡在蚊帐中 （1表示是，0表示否） |
| treated | 蚊帐是否杀虫 （1表示是，0表示否） |
| green | 村庄附近的绿色植物的覆盖度 |
| phi | 村庄里是否有医疗中心（1表示有，0表示没有） |

\newpage

\begin{table}

\caption{(\#tab:gambia-malaria)冈比亚儿童疟疾数据（部分）}
\centering
\begin{tabular}[t]{rrrrrrrr}
\toprule
x & y & pos & age & netuse & treated & green & phc\\
\midrule
349631 & 1458055 & 1 & 1783 & 0 & 0 & 40.9 & 1\\
349631 & 1458055 & 0 & 404 & 1 & 0 & 40.9 & 1\\
349631 & 1458055 & 0 & 452 & 1 & 0 & 40.9 & 1\\
349631 & 1458055 & 1 & 566 & 1 & 0 & 40.9 & 1\\
349631 & 1458055 & 0 & 598 & 1 & 0 & 40.9 & 1\\
349631 & 1458055 & 1 & 590 & 1 & 0 & 40.9 & 1\\
\bottomrule
\end{tabular}
\end{table}

在建模之前，这些搜集的数据对疟疾产生的影响可以通过探索性分析获得直观的认识，植被覆盖度、蚊帐以及杀虫对疟疾流行度的影响，分别如图\@ref(fig:gambia-prevalence)和图\@ref(fig:bed-net)所示。总体上，植被越茂盛，疟疾流行度越大，医疗中心对疟疾的控制作用比较有限，主要原因是冈比亚医疗卫生条件太差。蚊帐对疟疾的预防效果很好，没有使用蚊帐的人群中感染比例接近50\%，而使用了蚊帐的人群中比例降至30.5\%，此外，对蚊帐杀虫也有很好的保护效果。

\begin{figure}

{\centering \includegraphics[width=0.6\linewidth]{figures/gambia-prevalence} 

}

\caption{疟疾流行度与村庄周围的植被覆盖度：绿色表示有医疗中心，橘黄色表示没有}(\#fig:gambia-prevalence)
\end{figure}
\begin{figure}

{\centering \includegraphics[width=0.6\linewidth]{figures/gambia-bed-net} 

}

\caption{蚊帐与杀虫对疟疾流行度的影响（是否有蚊帐，蚊帐是否杀虫，RDT 表示快速诊断结果：0 表示没有感染疟疾，1 表示感染疟疾）}(\#fig:bed-net)
\end{figure}

\newpage

为进一步作出定量分析，建立模型如下：\begin{equation}
\log\{p_{ij}/(1-p_{ij})\} = \alpha + \beta'z_{ij} + U_{i} + S(x_{i})
\end{equation}

其中，$z_{ij}$ 表示对第$i$个村庄的第$j$个儿童的观测值，如前所述的年龄、蚊帐使用情况等固定效应，相应地，$p_{ij}$ 表示感染疟疾的概率。$S(x_{i})$ 表示空间随机效应，$U_{i}$ 表示除空间效应以外的村庄水平上的变化，也是随机效应。固定效应$\beta$和截距项$\alpha$结果如下

| 参数        | 估计      | 条件标准差 | t 统计量 |
| :---------- | :-------- | :------- | :------ |
| (Intercept) | -1.182665 | 2.491648 | -0.4747 |
| avg_age     | 0.002613  | 0.001544 | 1.6930  |
| netuse      | -0.023868 | 0.009401 | -2.5390 |
| treated     | -0.002662 | 0.009053 | -0.2940 |
| green       | -0.026757 | 0.028247 | -0.9473 |
| phc         | -0.376871 | 0.247394 | -1.5234 |

此外，$\nu = 0.16465,\phi = 0.000367,\sigma^2 = 2.705$，相应的空间预测如图 \@ref(fig:spamm-gambia) 所示

\newpage

\begin{figure}

{\centering \includegraphics[width=0.7\linewidth]{figures/spaMM-gambia} 

}

\caption{冈比亚儿童疟疾空间分布预测}(\#fig:spamm-gambia)
\end{figure}