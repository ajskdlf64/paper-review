# Zero-Resource Cross-Domain Named Entity Recognition
![스크린샷 2022-06-09 오후 1 00 32](https://user-images.githubusercontent.com/41967014/172761460-f680e003-a6b0-4c2a-91c7-ca19af96c618.png)

### Abtract
Existing models for cross-domain named entity recognition (NER) rely on numerous unlabeled corpus or labeled NER training data in target domains. 
However, collecting data for low-resource target domains is not only expensive but also time-consuming. 
Hence, we propose a cross-domain NER model that does not use any external resources. 
We first introduce a Multi-Task Learning (MTL) by adding a new objective function to detect whether tokens are named entities or not. 
We then introduce a framework called Mixture of Entity Experts (MoEE) to improve the robustness for zero-resource domain adaptation. 
Finally, ex- perimental results show that our model outperforms strong unsupervised cross-domain sequence labeling models, 
and the performance of our model is close to that of the state-of-the-art model which leverages extensive resources.
