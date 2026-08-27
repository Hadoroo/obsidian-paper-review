# Informasi
- Tahun: 2025
- Penulis: Lee et al.
- DOI: -

# Masalah yang diselesaikan
- Ingin mengevaluasi kontribusi dari 3 modalitas (sinyal psikologi, audio, dan video)
- mencoba menggabungkan modalitas (multimodal) dan juga single modal

# Metode
- menggunakan model ML (Random Forest, SVM, MLP, KNN, dan Deep Belief Network (DBN)) 
- tuning parameter untuk masing2 modalitas
- modalitas:
	- sinyal psikologi
		- electrocardiogram (ECG)
		- electrodermal activity (EDA)
		- respiration (numbers of breaths taken per minute)
	- audio
		- audio
	- video
		- facial expression
- preprocessing:
	- sinyal psikologi
		- butterworth filter untuk mengurangi noise di frekuensi tinggi dan mengurangi baseline drift
	- audio
		- downsample ke 16kHz
		- apply voice activity detection (VAD) dan menghilangkan non-speech segment
	- video
		- openface dihitung menggunakan mean dan standar deviasi untuk menemukan titik tengah dari seluruh data
- fitur:
	- sinyal psikologi
		- 35 fitur dari ECG
		- 23 fitur dari EDA
		- 40 fitur dari respirasi
		- fitur2 tersebut berisi statistical dan physiological measures seperti heart rate variability (HRV), skin conductance level (SCL), skin conductance response (SCR), dan respiratory rate variability (RRV)
	- audio
		- audio
			- 114 dimensi fitur
			- Mel-frequency cepstral coefficients (MFCCs) dan turunannya
			- spectral centroid dan fitur spectral lainnya
		- text (digunakan untuk model klasifikasi linear)
			- 513 dimensi per utterance
			- speech embedding dari wav2vec 2.0 setiap 20ms
	- video
		- 84 dimensi fitur
		- action units (AUs)
		- eye gaze trajectories
- penggabungan modalitas:
	- feature level fusion (intermediate fusion -> high dimension feature)
	- decision level fusion (ensemble model (train model for each feature))
- imbalance handling:
	- SMOTE untuk biner
	- 3 kelas tetap dilakukan SMOTE tapi tidak maksimal karena kelas relaxed sangat sedikit, jadi kurang efektif
# Dataset
- StressID (audio, video, sinyal psikologi)
	- 65 participant
	- 39 jam audio recording, 711 physiological signal, dan 587 video segment
	- participant isi survey untuk report precieved stress, relaxation, valence and arousal

# Hasil
- single modal terbaik untuk klasifikasi biner adalah sinyal psikologi (f1-socre = 0.751)
- audio sebagai single modal untuk klasifikasi 3 kelas terbaik (f1-score = 0.625)
- feature level fusion lebih stabil pada binary classification
- decision level fusion lebih baik performanya untuk klasifikasi 3 kelas


| Modalitas             | Model/Metode         | Binary   | <        | 3 Class  | <        |
| --------------------- | -------------------- | -------- | -------- | -------- | -------- |
|                       |                      | F1-Score | Accuracy | F1-Score | Accuracy |
| Video                 | RF                   | 0.702    | 0.703    | 0.557    | 0.555    |
| ^                     | SVM                  | 0.701    | 0.701    | 0.565    | 0.559    |
| ^                     | KNN                  | 0.706    | 0.706    | 0.563    | 0.558    |
| ^                     | MLP                  | 0.708    | 0.708    | 0.564    | 0.557    |
| Audio                 | RF                   | 0.689    | 0.629    | 0.515    | 0.478    |
| ^                     | SVM                  | 0.713    | 0.664    | 0.577    | 0.535    |
| ^                     | KNN                  | 0.576    | 0.627    | 0.526    | 0.491    |
| ^                     | MLP                  | 0.718    | 0.671    | 0.558    | 0.519    |
| ^                     | w2v classifier       | 0.725    | 0.667    | 0.625    | 0.564    |
| Physio                | RF                   | 0.751    | 0.744    | 0.569    | 0.565    |
| ^                     | SVM                  | 0.733    | 0.725    | 0.576    | 0.574    |
| ^                     | KNN                  | 0.696    | 0.689    | 0.561    | 0.552    |
| ^                     | MLP                  | 0.712    | 0.709    | 0.537    | 0.53     |
| Feature-level Fusion  | MLP                  | 0.72     | 0.66     | 0.62     | 0.61     |
| ^                     | KNN                  | 0.61     | 0.63     | 0.53     | 0.56     |
| ^                     | RF                   | 0.67     | 0.57     | 0.54     | 0.49     |
| ^                     | DBN                  | 0.63     | 0.57     | 0.34     | 0.35     |
| ^                     | SVM                  | 0.72     | 0.66     | 0.57     | 0.51     |
| Decision-level Fusion | Sum level fusion     | 0.72     | 0.65     | 0.65     | 0.6      |
| ^                     | Product level fusion | 0.72     | 0.64     | 0.64     | 0.6      |
| ^                     | average level fusion | 0.72     | 0.65     | 0.65     | 0.6      |
| ^                     | maximum level fusion | 0.72     | 0.63     | 0.63     | 0.59     |

# Kelebihan
- 

# Kekurangan
- belum mencoba temporal modeling dan handle data imbalance
- 

# Ide yang bisa digunakan
- 

# Paper terkait (Link Paper)
https://aclanthology.org/2025.rocling-main.4.pdf 
file:///E:/Sekolah/KULIAH/S2/Thesis/Referensi/Lee%20et%20al.%20(2025).pdf