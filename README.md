# A Lightweight Contrastive System for Misinformation Detection in Social Media Tweets

### Author: Priyam Saha<br>
### Kaggle Public Notebook: [Link](https://www.kaggle.com/code/priyamsaha17/promid-track-3-v2/notebook?scriptVersionId=280927719)

### Abstract: 
A compact classification system was developed and submitted to Prompt RecOvery For MisInformation Detection (PROMID) Subtask 3 for the detection of misinformation in tweets about the Russia–Ukraine conflict as provided by the workshop organisers. The proposed solution combines a frozen RoBERTa encoder, a small projection head trained with a supervised contrastive objective, and a lightweight classifier trained jointly with binary cross-entropy. Design choices were driven by compute and memory constraints; several practical implementation details and evaluation outcomes are reported to support reproducibility. On the official test set the system produced a weighted F1 score of 0.82 (misinformation precision 0.87, recall 0.80). The approach, training recipe and error analysis are documented in order to assist future participants and applied researchers working under limited resource conditions.

### Pipeline: 
The pipeline was intentionally simple and reproducible. The core modules are:
1. Encoder. A pretrained roberta-base model was used to extract contextual token embeddings. The encoder parameters were frozen during head-only training to reduce memory consumption and runtime.
2. Pooling. Mean pooling across token embeddings (masked by the attention mask) was used to obtain a single vector representation per tweet.
3. Projection head. A small feedforward projection head (Dense → Dropout → Dense → Layer-Norm) was trained with stochastic dropout active during training so that calling the projection head twice produced two stochastic views of the same example.
4. Classifier head. A compact classifier (Dense(256, gelu) → Dropout → Dense(1, sigmoid)) was trained jointly to produce final binary predictions. The training objective combined a supervised contrastive loss and a binary cross-entropy loss so that the representation space was encouraged to bring same-label examples closer while the classifier learned decision boundaries on the pooled vectors.

#### Flowchart illustrating the complete pipeline of the proposed lightweight contrastive system:
![Pipeline Diagram](PROMID_Pipeline_Flowchart.png)

### Final evaluation:
Final predictions were produced on the provided test CSV and scored by the organizers’ program. The submission was made by the name of priyam_saha17 (ID 431064) on the PROMID leaderboard for the hackathon hosted on CodaBench. The official classification report returned the following per-class and aggregated metrics:
- nonmisinfo: precision = 0.96, recall = 0.80, F1 = 0.87, support = 2002.
- misinfo: precision = 0.4567, recall = 0.8285, F1 = 0.59, support = 414.
- weighted average F1: 0.8213.

The PROMID Leaderboard can be accessed here: [Leaderboard Link](https://www.codabench.org/competitions/10869/#/pages-tab)
