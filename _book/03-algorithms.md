
# 算法综述 {#algorithms}

## 贝叶斯

1998年 Diggle 等人最早提出基于模型的地质统计学框架，将高斯空间随机过程和（广义）线性混合模型结合应用到空间流行病数据分析中，通过贝叶斯推断方法进行参数估计和预测[@Diggle1998]。2002年 Diggle 等人使用空间广义线性混合模型分析冈比亚儿童疟疾的数据，在贝叶斯框架下，通过 Metropolis-Hastings 采样算法实现 MCMC 方法进行参数估计和模型预测[@Diggle2002]。


## 最大似然

由于贝叶斯方法构造马尔科夫链，需要很多次反复迭代，收敛速度慢，求解模型\@ref(eq:SGLMM)需要花费很多时间，将最大似然和重要性采样相结合的方法出现了，称之为蒙特卡罗最大似然法，简称 MCML。1994年 Charles J. Geyer 首先从理论证明 MCML 方法的收敛性及相关似然估计的渐进正态性，其中包括 profile 似然、近似似然和精确似然等，为后续算法的开发、改进以及应用提供了理论支持[@Geyer1994On]。2002年张在做模型的估计和预测的时候提出蒙特卡罗--期望极大梯度 (Monte Carlo EM Gradient) 算法，简称 MCEMG [@Zhang2002On]。2016 年 Hosseini 在 MCEMG 的基础上提出近似蒙特卡罗--期望极大梯度 (Approximate Monte Carlo EM Gradient) 算法，简称 AMCEMG [@Hosseini2016]。2004 年 Ole F Christensen 将 MCML 方法用于地质统计模型 [@Christensen2004]，2016 年 Peter J. Diggle 和 Emanuele Giorgi  将 MCML 方法应用于分析西非流行病调查数据。

为描述方便起见，令 $\bm{\theta}^{\top} = (\sigma^2,\phi,\tau^2)$， 这里和之前先验分布一节的$\bm{\theta}$含义一样， 是模型参数构成的一个集合， 只是没有将 $\beta$ 纳入进来， 因为在贝叶斯框架下， 要求对所有未知参数给定先验分布， 包括模型系数。$D$ 表示 $n\times p$ 的协变量矩阵,  $y^{\top} = (y_1,y_2,\cdots,y_n)$，$T$ 的边际分布是 $N(D\beta,\Sigma(\theta))$。 给定 $T^{\top}=t^{\top}=(t_1,t_2,\cdots,t_n)$ 下， $Y^{\top}=(Y_1,\cdots,Y_n)$ 的条件分布是独立二项概率分布函数的乘积$f(y|t)=\prod_{i=1}^{n}f(y_{i}|t_{i})$，则$\beta$ 和 $\bm{\theta}$ 的似然函数可以写成：\begin{equation}
\begin{aligned}
L(\beta,\theta)
& = f(y;\beta,\bm{\theta}) \\
& = \int_{\mathbb{R}^{n}} N(t;D\beta,\Sigma(\theta))f(y|t)dt\\
& = \int_{\mathbb{R}^{n}} \frac{N(t;D\beta,\Sigma(\theta))f(y|t)}{N(t;D\beta_{0},\Sigma(\theta_{0}))f(y|t)}f(y,t)dt\\
& \varpropto \int_{\mathbb{R}^{n}} \frac{N(t;D\beta,\Sigma(\theta))}{N(t;D\beta_{0},\Sigma(\theta_{0}))}f(t|y)dt \\
&= E_{T|y}\left[\frac{N(t;D\beta,\Sigma(\theta))}{N(t;D\beta_{0},\Sigma(\theta_{0}))}\right] (\#eq:likelihood2)
\end{aligned}
\end{equation}

其中，$\beta_{0},\theta_{0}$给定，$Y$ 和 $T$ 的联合分布是 $f(y,t)=N(t;D\beta_{0},\Sigma(\theta_{0}))f(y|t)$，再使用 MCMC 算法从条件分布$f(T|Y=y;\beta_0,\theta_0)$抽取 $m$ 个样本 $t_{(i)}$，那么，可以用如下方程近似 \@ref(eq:likelihood2) \begin{equation}
L_{m}(\beta,\theta)=\frac{1}{m}\sum_{i=1}^{n}\frac{N(t_{i};D\beta,\Sigma(\theta))}{N(t_{i};D\beta_{0},\Sigma(\theta_{0}))} (\#eq:likelihood-approx)
\end{equation}

给定合适的初始值 $\beta_{0}$ 和 $\theta_{0}$，用 $\hat{\beta}_{m}$ 和 $\hat{\theta}_{m}$ 表示最大化 $L_{m}(\beta,\theta)$ 获得的 MCML 估计，重复迭代$\beta_{0}=\hat{\beta}_{m}$ 和 $\theta_{0}=\hat{\theta}_{m}$ 直到收敛。最大化 $L_{m}(\beta,\theta)$的过程中，我们可以选择 BFGS 算法。

## 低秩近似

模型\@ref(eq:SGLMM)中 $S(x_i)$ 来自高斯过程 $\mathcal{S} = S(x),x\in \mathbb{R}^2$，可以被表示成高斯噪声的卷积形式\begin{equation}
S(x) = \int_{\mathbb{R}^2} K(\|x-t\|; \phi, \kappa) \: d B(t) (\#eq:convolution)
\end{equation}

其中，$B$表示布朗运动，$\|\cdot\|$ 表示欧氏距离，$K(\cdot)$ 表示梅隆核，形如\begin{equation}
K(u; \phi, \kappa) = \frac{\Gamma(\kappa + 1)^{1/2}\kappa^{(\kappa+1)/4}u^{(\kappa-1)/2}}{\pi^{1/2}\Gamma((\kappa+1)/2)\Gamma(\kappa)^{1/2}(2\kappa^{1/2}\phi)^{(\kappa+1)/2}}\mathcal{K}_{\kappa}(u/\phi), u > 0. (\#eq:matern-kernel)
\end{equation}

通过离散方程 \@ref(eq:convolution)，并且让 $r$ 充分大，可以获得低秩近似\begin{equation}
S(x) \approx \sum_{i = 1}^r K(\|x-\tilde{x}_{i}\|; \phi, \kappa) Z_{i}, (\#eq:lr-approx)
\end{equation}

其中，$(\tilde{x}_{1},\ldots,\tilde{x}_{r})$ 表示空间网格的格点，$Z_{i}$ 是独立同分布的高斯变量，均值为0，方差$\sigma^2$，特别地，当尺度参数$\phi$比较大的时候，这种近似变得很有效，如图\@ref(fig:matern-2d)所示，$\phi$越大，空间曲面越平缓，即使格点数目$r$比较小也能得到很好的效果。此外，空间格点数目$r$与样本量$n$是独立的，因此这种方法在大样本的时候，很有计算上的吸引力。对于具有复杂空间结构的模型\@ref(eq:SGLMM)，保持高计算效率是一个非常有意义的方面。

## 相关软件

[Stan](http://mc-stan.org/) 是一种概率编程语言[@Stan2017JSS]，可以替代 BUGS ( **B**ayesian inference **U**sing **G**ibbs **S**ampling ) [@BUGS] 作为 MCMC 的高效实现，可用于贝叶斯框架下，标准地质统计模型的参数估计，Stan 提供多种语言的接口实现，方便起见，本文采用它提供的 R 语言接口 -- rstan 包 [@RStan]。基于 GPU 加速是一个不错的选择， Stan 开发者也把 GPU 加速列入开发日程。SCIKIT-CUDA [@scikitcuda2015] 和 ArrayFire [@ArrayFire2015] 等基于 CUDA 开发的通用加速框架获得越来越多的关注。

R语言作为自由的统计计算和绘图环境，因其免费，更新快，社区庞大，扩展包更是多达12500个，提供了大量的前沿统计技术的代码实现。如用于一元和多元时空模型选择和预测的 spBayes 包 [@spBayes2015]；可以对 MCMC 的输出进行诊断和分析的 coda 包 [@coda2006]；MCMCvis 包提取模型参数，MCMC 算法输出的结果并可视化，产生出版级的图形，支持转化 JAGS、Stan 和 BUGS 软件输出结果[@R-MCMCvis]；基于贝叶斯方法的空间线性混合效应模型选择和预测的 geoR 包 [@geoR2001]，geoRglm 包将其扩展到空间广义线性混合效应模型 [@geoRglm2002]；glmmBUGS 包提供WinBUGS、 OpenBUGS 和 JAGS 软件的统一接口，使求解 BUGS 模型的过程放在 R 环境中[@R-glmmBUGS;@glmmBUGS2010MCMC]；gstat 包是迁移自S语言的地质统计扩展包，提供了各种各样的克里金插值方法[@gstat2004;@gstat2016]；brms 包基于Stan框架 拟合贝叶斯广义线性和非线性混合效应模型[@brms2017JSS]。