# Brain-to-Text Decoding
 
Decode intended speech directly from high-resolution intracortical neural recordings (256 electrodes in speech motor cortex) into readable text — a critical step toward assistive communication for individuals with severe speech impairments (ALS, locked-in syndrome).

**Metrics**: *Phoneme Error Rate (PER)* & *Word Error Rate (WER)* via edit distance.

## Dataset
- Input: Neural firing rates (T × 512), T ≈ 500–1200 timesteps  
- Targets: 41 classes (40 phonemes + CTC blank)  
- Train: 8,072 trials (45 sessions)  
- Validation: 1,426 trials (41 sessions)
- Format: HDF5 (lazy-loaded for efficiency)

## Repository Structure
```
Brain-to-text/
├── full_transformer/
|     ├── script.py
|     ├── train/
├── subset_experiments/
|     ├── per-convbigru.ipynb
|     ├── residual-gru.ipynb
│     └── transformer.ipynb
└── README.md

```


## Models & Results

| Model                  | Data used (train/val)                  | Val PER    | Val WER    | Notes                                   |
|-----------------------|----------------------------|------------|------------|-----------------------------------------|
| Conv-BiGRU            | 2,000 / 300 trials         | 96.0%      | –          | failed to generalize             |
| **Transformer Encoder** (final) | **Full 8,072 / 1,426** | **23.90%** | **37.66%** | **Best model** – 4 layers, 8 heads, d=256 |

**Training details (best model)**  
- Batch size: 4 (train) / 2 (val)  
- Epochs: 30  
- Loss: CTC  
- Decoding: Greedy CTC → small dictionary (~120 words) → text

## Sample Predictions (Validation)
- True: `you can see the code at this point as well`  
  Pred: `you can see the code at this point at will`
- True: `how does it keep the cost down`  
  Pred: `how the it keep the what an`

## Key Takeaways
- Transformers vastly outperform RNNs on neural time-series speech decoding  
- CTC loss enables end-to-end training without alignment  
- WER limited by tiny dictionary and absence of language model / beam search



**Authors:** Anjelica, Ishita Rathore, Mishika Sood, Tanu Adhikari