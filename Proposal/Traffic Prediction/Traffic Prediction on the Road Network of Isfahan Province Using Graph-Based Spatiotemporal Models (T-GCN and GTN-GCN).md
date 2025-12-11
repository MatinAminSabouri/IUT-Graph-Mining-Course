# **Traffic Prediction on the Road Network of Isfahan Province Using Graph-Based Spatiotemporal Models (T-GCN and GTN-GCN)**

**Authors:** Amirhossein Lavafi Barzaki, Matin Aminsabouri

## **1\. Problem Statement**

 Most graph-based traffic prediction methods—particularly those that leverage Graph Neural Networks (GNNs)—are developed and evaluated on high-frequency datasets. For example, in the recent paper *GTN-GCN: Real-Time Traffic Forecasting Using Graph Neural Networks* (Applied Computational Intelligence and Soft Computing, 2025), the T-GCN model is reported to achieve MAE \= 2.68 and RMSE \= 5.33 on the benchmark METR-LA dataset (a collection of 5-minute interval traffic measurements from 207 sensors in Los Angeles).

However, publicly available Iranian traffic datasets such as those published by the national data portal [data.gov.ir](http://data.gov.ir) are recorded at daily resolution. This coarse temporal format presents a unique challenge for spatiotemporal models. While the spatial connections between road segments can be naturally represented as a graph, the lack of fine-grained temporal dynamics raises questions about the applicability and performance of established models like T-GCN.

This research project seeks to answer the following:

**Can graph-based spatiotemporal models such as T-GCN and the more recent GTN-GCN be effectively applied to low-resolution daily traffic data from Isfahan Province, and how do their performances compare with established results on high-frequency benchmarks like METR-LA?**

Addressing this will not only deepen understanding of model transferability across resolutions and regions, but also provide practical insights into modeling Iranian road traffic.

## **2\. Proposed Methodology**

### **Base Models:** 

### **T-GCN**

T-GCN integrates Graph Convolutional Networks (GCN) to capture spatial dependencies in road networks and GRU units to model temporal dynamics.  
 Code repositories:

* [https://github.com/zouchangjie/T-GCN](https://github.com/zouchangjie/T-GCN)  
* [https://github.com/lehaifeng/T-GCN](https://github.com/lehaifeng/T-GCN)

### 

### **GTN-GCN (Graph Temporal Network \+ Graph Convolutional Network)**

 designed to address several limitations of earlier graph-based traffic forecasting models. Unlike T-GCN, which relies on fixed temporal gating through GRUs, GTN-GCN introduces learnable temporal kernels that adaptively capture time-dependent patterns while simultaneously modeling spatial dependencies through GCN layers.

This hybrid design enables the model to better represent long-range temporal correlations and dynamic spatial interactions, leading to significant performance improvements on standard benchmarks. For example, on the METR-LA dataset, GTN-GCN reports lower error metrics than T-GCN, with T-GCN achieving MAE \= 2.68 and RMSE \= 5.33, as documented in the study.

The full article is available at:  
  [https://onlinelibrary.wiley.com/doi/full/10.1155/acis/5572638](https://onlinelibrary.wiley.com/doi/full/10.1155/acis/5572638)

In this project, T-GCN serves as the classical baseline model, while GTN-GCN is used as a more recent comparator that reflects the current state of the art in graph-based traffic forecasting.

### 

### **Dataset: Isfahan Traffic Counters (2018)**

We use daily-count traffic data from the Isfahan Province traffic control stations, available from the national open data portal:

* [https://data.gov.ir/dataset/?tags=%D8%A7%D8%B5%D9%81%D9%87%D8%A7%D9%86](https://data.gov.ir/dataset/?tags=%D8%A7%D8%B5%D9%81%D9%87%D8%A7%D9%86)

**Nodes:** Road segments such as code 214953 (Kashan–Ardestan)  
 **Temporal resolution:** Daily (00:00–00:00), 1440-minute window  
 **Features:**

* Total traffic volume  
* Average speed  
* Five vehicle-class counts  
* Speeding / distance / overtaking violations  
   Period: Early August 2018 (Mordad 1397; expandable using monthly datasets)

### **Extended Feature Description**

The dataset provides a rich set of daily aggregated traffic attributes for each monitored road segment. Each record contains the following columns:

* **Segment Code:** Unique numeric identifier for each road station.  
* **Segment Name :** Descriptive name of the road or highway.  
* **Start / End Time:** Start and end timestamps of the 24-hour measurement window.  
* **Duration:** Always 1440 minutes, indicating a full-day recording.  
* **Total Vehicle Count:** Total number of vehicles passing during the day.  
* **Vehicle Class Counts (Class 1–5):** Number of vehicles in five distinct categories (motorcycles, light vehicles, heavy trucks, buses, etc.).  
* **Average Speed:** Mean measured vehicle speed over the 24-hour period.  
* **Speed Violations:** Count of vehicles exceeding the speed limit.  
* **Unsafe Distance Violations:** Number of detected cases of insufficient following distance.  
* **Illegal Overtaking:** Count of overtaking violations.  
* **Estimated Count:** Adjusted traffic volume after station calibration.

This multi-feature structure enables modeling of not only overall traffic volume but also composition, behavior, and violation patterns, offering significantly richer information than standard datasets such as METR-LA.

 Graph construction:

* Spatial k-nearest neighbors  
* OpenStreetMap topology ([OSM Isfahan network](https://download.geofabrik.de/asia/iran.html))

This dataset forms the basis of the graph, where stations are nodes and edges are determined by geographical adjacency or road network topology.

**Sample (ِDates recorded in the Persian calendar)**:

| code      | date           | volume | speed  
|------------|------—-------|------------|--------|  
| 214953 | 1397/05/01 | 4090     | 569  |  
| 214953 | 1397/05/02 | 3914     | 580  |

## **Processing Pipeline**

1. **Preprocessing:**   
   Construct feature tensor  
    XNT   
    where N is the number of road segments and T the number of days.  
    Each Xn,t​ contains the volume or multi-feature vector of segment *n* on day *t*.  
2. **Graph Construction:**  
    A  NN from spatial coordinates  
3. **Training:**   
   Optimize T-GCN using loss:  
   	L \= 1Tt=1T||X t-Xt||22    
4. **Evaluation (vs. METR-LA) :**  
* MAE (Mean Absolute Error)  
* RMSE (Root Mean Squared Error)  
* MAPE (Mean Absolute Percentage Error)


## **3\. Expected Contributions**

## **Comparative Analysis**

| Metric | [METR-LA](https://www.kaggle.com/datasets/annnnguyen/metr-la-dataset) | Esfahan Roads (Predicted) |
| :---: | :---: | :---: |
| Nodes | 207 sensors [kaggle](https://www.kaggle.com/datasets/annnnguyen/metr-la-dataset)​ | 10-50 segments [data](https://data.gov.ir/dataset/?q=&_groups_limit=0&tags=%D9%81%D8%B1%D9%88%D8%B1%D8%AF%DB%8C%D9%86+97&tags=%D8%A7%D8%B3%D8%AA%D8%A7%D9%86+%D8%A7%D8%B5%D9%81%D9%87%D8%A7%D9%86&organization=11c99d566b1018225779a80acdcceac67a1881d1&groups=transportation)​ |
| Time | 5-min [arxiv](http://www.arxiv.org/abs/1811.05320)​ | Daily [data](https://data.gov.ir/dataset/?_groups_limit=0&groups=transportation&_organization_limit=0&tags=%D8%A7%D8%B5%D9%81%D9%87%D8%A7%D9%86&organization=11c99d566b1018225779a80acdcceac67a1881d1)​ |
| MAE | **2.68 mph** [wiley](https://onlinelibrary.wiley.com/doi/full/10.1155/acis/5572638) | To be evaluated |
| RMSE | **5.33 mph** [wiley](https://onlinelibrary.wiley.com/doi/full/10.1155/acis/5572638) | To be evaluated |

*Due to the coarse temporal resolution of Iranian traffic datasets (daily aggregates), the performance on the Esfahan network is expected to be lower than on METR-LA. However, the exact margin of degradation will be quantified experimentally.*

### 

### **Novelty**

* First systematic application of T-GCN to Iranian provincial road data  
* Identifies performance gap caused by coarse temporal sampling  
* Investigates local traffic behavior (e.g., Thursday/Friday peaks)  
* Opens the door for GNN-based transport forecasting in Iran

### **Risks / Challenges**

* Graph sparsification due to limited sensors  
* Risk of overfitting → addressed using cross-validation across multiple months  
* Variability in station reliability  
* Temporal smoothness reducing GRU efficiency

## **4\. References**

### **Foundational Papers & Code**

* Zhao et al., *T-GCN: A Temporal Graph Convolutional Network*  
   [https://arxiv.org/abs/1811.05320](https://arxiv.org/abs/1811.05320)   
* https://onlinelibrary.wiley.com/doi/full/10.1155/acis/5572638

* GitHub Implementations

  * [https://github.com/zouchangjie/T-GCN](https://github.com/zouchangjie/T-GCN)  
  * [https://github.com/lehaifeng/T-GCN](https://github.com/lehaifeng/T-GCN)

* Scribd summary  
   [https://www.scribd.com/document/790604638/T-GCN-A-Temporal-Graph-Convolutional-Network-for-Traffic-Prediction](https://www.scribd.com/document/790604638/T-GCN-A-Temporal-Graph-Convolutional-Network-for-Traffic-Prediction)

  ### **Traffic Prediction & GNN Research**

* 2024 GNN advances in traffic prediction  
   [https://www.scitepress.org/Papers/2024/129022/129022.pdf](https://www.scitepress.org/Papers/2024/129022/129022.pdf)

* Alternative spatiotemporal models  
   [https://papers.ssrn.com/sol3/papers.cfm?abstract\_id=5220881](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5220881)

* IET ITS article  
   [https://ietresearch.onlinelibrary.wiley.com/doi/10.1049/itr2.12224](https://ietresearch.onlinelibrary.wiley.com/doi/10.1049/itr2.12224)

* ScienceDirect traffic studies  
   [https://www.sciencedirect.com/science/article/abs/pii/S2352485525000970](https://www.sciencedirect.com/science/article/abs/pii/S2352485525000970)

* IEEE ITS applications  
   [https://ieeexplore.ieee.org/iel8/6287639/6514899/11097318.pdf](https://ieeexplore.ieee.org/iel8/6287639/6514899/11097318.pdf)

* ACM ST-GNN survey  
   [https://dl.acm.org/doi/10.1145/3716554.3716619](https://dl.acm.org/doi/10.1145/3716554.3716619)

  ### **Datasets**

* METR-LA  
   [https://www.kaggle.com/datasets/annnnguyen/metr-la-dataset](https://www.kaggle.com/datasets/annnnguyen/metr-la-dataset)

* Isfahan datasets  
   [https://data.gov.ir/dataset/?tags=%D8%A7%D8%B5%D9%81%D9%87%D8%A7%D9%86](https://data.gov.ir/dataset/?tags=%D8%A7%D8%B5%D9%81%D9%87%D8%A7%D9%86)  
   [https://data.gov.ir/dataset/?q=&\_groups\_limit=0\&tags=%D9%81%D8%B1%D9%88%D8%B1%D8%AF%DB%8C%D9%86+97\&tags=%D8%A7%D8%B3%D8%AA%D8%A7%D9%86+%D8%A7%D8%B5%D9%81%D9%87%D8%A7%D9%86\&organization=11c99d566b1018225779a80acdcceac67a1881d1\&groups=transportation](https://data.gov.ir/dataset/?q=&_groups_limit=0&tags=%D9%81%D8%B1%D9%88%D8%B1%D8%AF%DB%8C%D9%86+97&tags=%D8%A7%D8%B3%D8%AA%D8%A7%D9%86+%D8%A7%D8%B5%D9%81%D9%87%D8%A7%D9%86&organization=11c99d566b1018225779a80acdcceac67a1881d1&groups=transportation)

  ### **Regional Traffic Studies**

* Journal of Road Systems (Iran)  
   [https://jrsgr.sru.ac.ir/article\_2122.html](https://jrsgr.sru.ac.ir/article_2122.html)

