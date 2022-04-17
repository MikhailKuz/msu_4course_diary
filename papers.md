<!--- 
activate p2
python -m readme2tex --nocdn --output papers_v2.md --readme papers.md
-->
# Конспекты статей

1. [T Nguyen, R Novak, L Xiao, J Lee (2021) Dataset Distillation with Infinitely Wide Convolutional Networks](#1)
2. [G Katz, ECR Shin, D Song (2016) Explorekit: Automatic feature generation and selection](#2)
3. [Kaul A, Maheshwary S, Pudi V (2017) Autolearn — Automated feature generation and selection](#3)
4. [Luo Y, Wang M (2019) Autocross: Automatic feature crossing for tabular data in real-world applications](#4)
5. [Liu Q (2018) DNN2LR: Automatic Feature Crossing for Credit Scoring](#5)
6. [Xie Y, Wang Z, Li Y (2021) Fives: Feature interaction via edge search for large-scale tabular data](#6)

## <a name="1"/> T Nguyen, R Novak, L Xiao, J Lee (2021) [Dataset Distillation with Infinitely Wide Convolutional Networks](https://arxiv.org/abs/2107.13034)
### KIP method  
<img src="./images/2107_13034_17.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_16.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_18.png" alt="drawing" width="850"/>

### Paper highlights  
<img src="./images/2107_13034_1.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_2.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_3.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_4.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_5.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_6.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_7.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_8.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_9.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_10.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_11.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_12.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_13.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_14.png" alt="drawing" width="850"/>
<img src="./images/2107_13034_15.png" alt="drawing" width="850"/>  

## <a name="2"/> G Katz, ECR Shin, D Song (2016) [Explorekit: Automatic feature generation and selection](http://people.eecs.berkeley.edu/~dawnsong/papers/icdm-2016.pdf)
<img src="./images/ExploreKit_1.png" alt="drawing" width="650"/>
<img src="./images/ExploreKit_2.png" alt="drawing" width="950"/>
<img src="./images/ExploreKit_3.png" alt="drawing" width="950"/>  

### Feature generation operators  
Unary operators:
* Discretizers: used to convert continuous and datetime features into discrete (i.e. categorical) ones
* Normalizers: used to ﬁt the scale of continuous (i.e. numeric) features to specific distributions

Binary operators:
* +, −, ×, ÷

Higher-order operators:
* GroupByThenMax, GroupByThenMin
* GroupByThenAvg, GroupByThenStdev
* GroupByThenCount

Generating the Candidate Features Set:
1. Apply the unary operators on on all non-discrete
2. Apply binary and higher-order operators
3. For every non-discrete feature again apply all the unary operators
4. Union all generated features  

Meta-Features consist of:  
* **Dataset-based meta-features**:  
  - *General information*: number of instances and classes, statistics on the dataset size and other statistics  
  - *Initial evaluation*: statistics on the current performance of the classifier when applied on current set of features  
  - *Entropy-based measures*: partition current set of features in type-based groups and calculate Information Gain  
  - *Feature diversity*: partition current set of features in type-based groups and use the chi-squared and paired-t test to calculate the similarity of each pair in a group  
* **Candidate feature-based meta-features for feature F**  
  - calculate statistics on correlation of F to each of the type-based groups
  - statistics on parent features
  - statistics on the inter-correlation of the parent features of F, as well as their correlation with the remaining features
    
### Training the ranking classifier
* Set threshold in percentages of improvement of AUC-ROC to make feature good one to include
* Clf trained with loo strategy

### Comments
* In the heart of selection algorithm authors use score function. We need to make sure that model is calibrated.
<img src="./images/ExploreKit_4.png" alt="drawing" width="450"/>  


## <a name="3"/> Kaul A, Maheshwary S, Pudi V (2017) [Autolearn — Automated feature generation and selection](https://github.com/saket-maheshwary/AutoLearn)
<img align="right" width="300" src="./images/Autolearn_1.png">  

**IG** - mutual information between feature and target  
**dcor** - distance correlation:  
 - can capture all types of non-linear dependency  
 - it is applicable to random vectors of arbitrary and not necessarily equal dimension  
 - vary in [-1, 1]  

**Contruct features** - fit reg on i-feature and predict j-feature, add predicted feature and j-feature - pred_f  
**Stability selection** - fit RandomizedLasso and get features based on thresholding scores  

### Results
- Have 10x smaller set of generated features and +5% accuracy compared with TFC, FCTree, ExploreKit  

<br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  

## <a name="4"/> Luo Y, Wang M (2019) [Autocross: Automatic feature crossing for tabular data in real-world applications](https://arxiv.org/abs/1904.12857) 
<img src="./images/AutoCross_1.png" alt="drawing" width="500"/>  
<img align="right" width="500" src="./images/AutoCross_3.png">  
<img align="left" width="450" src="./images/AutoCross_2.png">  

<br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  <br />  
  
**Feature Set Generation**:  
 - Discretize numeric features by multiple bins size. Select half of them based on logreg's quality. *(Multi-granularity Discretization)*
 - Make all pairs on current feature set F_cur and select one feature F_sel after Feature Set Evaluation. Add F_sel to F_cur. Let's denote a set of new cross-features as F_new. *(Beam Search)*

**Feature Set Evaluation**:  
 - Fit logreg on F_cur, refit with warm start logreg on F_new while fixing F_cur coeffs (*Field-wise LR*)

**Feature Selection** *(Successive Mini-batch GD)*:

<img src="./images/AutoCross_4.png" alt="drawing" width="500"/>

### Results
 - Online feature construction + [feature hashing](https://arxiv.org/pdf/0902.2206.pdf)
 - Most of new features are high-ordered
 - New features are useful for nn too  

<img src="./images/AutoCross_5.png" alt="drawing" width="500"/>


## <a name="5"/> Liu Q. (2018) [DNN2LR: Automatic Feature Crossing for Credit Scoring](https://arxiv.org/abs/2102.12036) 
<img src="./images/DNN2LR_1.png" alt="drawing" width="850"/>

**Global and Local Interpretation**:  
*Local Interpretation of Feature* - градиенты по входному представлению признака @ (его значение).T (зависит от объекта)  
*Global Interpretation of Feature* - усредненные градиенты по входному представлению признака и объектам @ (его значение).T (не зависит от объекта)

**Interpretation Inconsistency** - L2-norm(Local Interpretation - Global Interpretation) (чем больше, тем больше признак находится в нелинейном отношении с другими признаками)  
**Filtering** - бинаризация по квантилю (>= 95% -> =1)  
**Generation** - выбор 3 * |F_orig| новых признаков на основе их встречаемости (новый признак - кортеж)  
**Searching**  

<img src="./images/DNN2LR_2.png" alt="drawing" width="550"/>  

### Results
 - На порядок быстрее AutoCross 
 - Можно находить нелинейные взаимосвязи в текстах тоже

<img src="./images/DNN2LR_3.png" alt="drawing" width="800"/>  


## <a name="6"/> Xie Y, Wang Z, Li Y (2021) [Fives: Feature interaction via edge search for large-scale tabular data](https://arxiv.org/abs/2007.14573) 
<img src="./images/Fives_1.png" alt="drawing" width="650"/>
<img src="./images/Fives_2.png" alt="drawing" width="550"/>
<img src="./images/Fives_3.png" alt="drawing" width="550"/>
<img src="./images/Fives_4.png" alt="drawing" width="550"/>
<img src="./images/Fives_5.png" alt="drawing" width="550"/>

**Main idea** - Search for cross features through graph adjacency matrix learning  
**Preprocessing** - all numeric features are discretized by 10, 100, 1000 and OHE  
**Learning** - concat all node representations from last time + MLP + sigmoid + BCE  

### Results
- Need to specify threshhold for binarization (phi func)
- Temperature scaling deals with problem of ~0.5 prob before applying phi func
- 1e-4 wd + 0.3 dropout for preventing overfitting
- SOTA results + 1 week AB testing

<img src="./images/Fives_6.png" alt="drawing" width="800"/>  

### Comments
- only LR were tested on generated features 
