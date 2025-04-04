# Weekly Report

## Dataset Info

Product Review

* 20210517
  * Node number: 16
  * Series length: 1663
  * Root Cause: catalogue

* 20210524
  * Node number: 14
  * Series length: 1766
  * Root Cause: catalogue
* 20211203
  * Node number: 20
  * Series length: 745
  * Root Cause: mongodb-v1
* 20220606
  * Node number: 20
  * Series length: 1077
  * Root Cause: istio-ingressgateway

Cloud Computing

* 20231207
  * Node number: 19
  * Series length: 792
  * Root Cause: 
* 20240124
  * Node number: 16
  * Series length: 1722
  * Root Cause: 

## iProduct Review

Ranking of truth root cause

|          |  PC  | PC + Constrain Agent | PC + Constrain Agent + Information Provider |
| :------: | :--: | :------------------: | :-----------------------------------------: |
| 20210517 |  1   |          /           |                      /                      |
| 20210524 |  1   |          /           |                      /                      |
| 20211203 |  7   |          6           |                                             |
| 20220606 |  2   |          3           |                                             |

## Problems

1. Some Log file have different Structure

   <img src="https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/image-20241121102825377.png" alt="image-20241121102825377" style="zoom:50%;" />

2. Neet use undirected graph when doing Randomwalk

   ![QQ_1732156153487](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/QQ_1732156153487.png)

3. Some Grount truth is ranking at 1 with only use PC algorithm.
