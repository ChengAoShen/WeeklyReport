# Weekly Report

## React Only Results

**Auto_MPG**

|               Methods               | FPR  | FNR  | Precision |  F1  | SHD  | NHD  |
| :---------------------------------: | :--: | :--: | :-------: | :--: | :--: | :--: |
|            Baseline(PC)             | 0.4  | 0.8  |   0.11    | 0.14 |  8   | 0.32 |
|        PC + Constrain agent         | 0.15 | 0.2  |   0.57    | 0.66 |  3   | 0.12 |
|     PC + Constrain Debate Agent     | 0.1  | 0.4  |    0.6    | 0.6  |  4   | 0.16 |
|     PC + React Constrain Agent      | 0.15 | 0.2  |   0.57    | 0.66 |  3   | 0.12 |
|          PC + React Agent           | 0.15 | 0.4  |    0.5    | 0.54 |  4   | 0.16 |
| **PC + Web agent+ Constrain agent** | 0.1  | 0.2  |   0.66    | 0.72 |  2   | 0.08 |
|         MAC(Tinghua paper)          |  /   |  /   |     /     |  /   |  4   | 0.16 |

**DWD_Climate**

|               Methods                | FPR  | FNR  | Precision |  F1  | SHD  |  NHD  |
| :----------------------------------: | :--: | :--: | :-------: | :--: | :--: | :---: |
|             Baseline(PC)             | 0.2  | 0.83 |   0.14    | 0.15 |  9   | 0.25  |
|         PC + Constrain agent         | 0.06 | 0.66 |    0.5    | 0.4  |  6   | 0.16  |
|     PC + Constrain Debate Agent      | 0.03 | 0.66 |   0.66    | 0.44 |  5   | 0.13  |
|      PC + React Constrain Agent      | 0.03 | 0.5  |   0.75    | 0.6  |  4   | 0.11  |
|              PC + React              | 0.06 | 0.66 |    0.6    | 0.4  |  6   | 0.16  |
| **PC + Web agent + Constrain agent** | 0.03 | 0.66 |   0.66    | 0.44 |  5   | 0.138 |
|          MAC(Tinghua paper)          |  /   |  /   |     /     |  /   |  5   | 0.138 |

**Sachs**

|               Methods                |  FPR   |  FNR   | Precision |   F1    | SHD  |  NHD   |
| :----------------------------------: | :----: | :----: | :-------: | :-----: | :--: | :----: |
|             Baseline(PC)             |  0.15  |  0.47  |   0.38    |  0.44   |  24  |  0.19  |
|         PC + Constrain agent         |  0.19  |  0.84  |   0.13    |  0.14   |  27  |  0.22  |
|     PC + Constrain Debate Agent      |  0.19  |  0.78  |   0.16    |  0.18   |  29  |  0.23  |
|      PC + React Constrain Agent      |  0.20  |  0.89  |   0.08    |  0.095  |  30  |  0.24  |
|             *PC + React*             | *0.20* | *0.94* |  *0.04*   | *0.048* | *29* | *0.23* |
| **PC + Web agent + Constrain agent** |  0.04  |  0.84  |   0.38    |  0.22   |  17  |  0.14  |
|          MAC(Tinghua paper)          |   /    |   /    |     /     |    /    |  18  |  0.15  |

+DYNOTEAR real Data

+GAIA 

We selected four widely used traditional causal discovery algorithms as baselines: PC (Spirtes, Glymour, and Scheines 2001), GES (Chickering 2002; Huang et al. 2018), GRaSP (Lam, Andrews, and Ramsey 2022), and ICALiNGAM (Shimizu et al. 2006).

## New Structure





