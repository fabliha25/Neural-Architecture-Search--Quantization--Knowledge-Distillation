Wi-Fi Gesture Recognition with Quantization and Knowledge Distillation


Overview
This repository implements a lightweight transformer model for Wi-Fi-based gesture recognition using the Widar 3.0 dataset.
The work explores model compression through Neural Architecture Search (NAS), Post-Training Quantization (PTQ), Quantization-Aware Training (QAT), and Knowledge Distillation (KD)to enable deployment on resource-constrained devices.

Key Result: The distilled quantized transformer achieves 93.77% accuracy in cross-domain settings, closely matching the complex teacher model (95.59%)


Input Source
- Dataset: Widar 3.0 
- Input type used: Pre-computed Body-coordinate Velocity Profile (BVP) samples  
- Task: Activity/Gesture Recognition only, (Push&Pull, Sweep, Clap, Slide, Draw-O(Horizontal), Draw-Zigzag(Horizontal))
- User identity, orientation, and location are not considered rather Cross Environment gesture recognition


Session & Room Selection
Only specific session folders were selected from the Widar 3.0 BVP archive based on room identity:
Session Folder: `20181130` → Room: Room 1 → Training Set             
Session Folder: `20181204`, `20181209` → Room: Room 2 → Test Set (Room 2 only)
Session Folder: `20181211` → Room: Room 3 → Test Set (Room 3 only)
Session Folder: `20181130`+ `20181204`, `20181209` + `20181211`→ Test Set (All samples from Room 1, 2, 3)


Methodology
1. Neural Architecture Search (NAS)
NAS was used offline to determine the optimal architectures for both models before training. The resulting architectures are already implemented in the provided scripts.

2. Models
Two models were developed:
Complex Model → Convolutional-Recurrent Network → Teacher 
Transformer   → Lightweight Transformer         → Student 

3. Quantization
Both models were evaluated under two quantization strategies:
PTQ: Post-Training Quantization — applied after training 
QAT: Quantization-Aware Training — quantization during training 

Each model was tested in 3 versions: Full Precision, PTQ, and QAT.

4. Knowledge Distillation (KD)
Nine teacher-student pairs were evaluated (3 teacher versions × 3 student versions):

| Teacher (Complex Model) | Student (Transformer) |
|-------------------------|-----------------------|
| Full Precision          | Full Precision        |
| Full Precision          | PTQ                   |
| Full Precision          | QAT                   |
| PTQ                     | Full Precision        |
| **PTQ**                 | **QAT** (Best Pair)*  |
| PTQ                     | Full Precision        |
| QAT                     | Full Precision        |
| QAT                     | PTQ                   |
| QAT                     | QAT                   |

The best performing pair was PTQ Complex Model (Teacher) + QAT Transformer (Student)






