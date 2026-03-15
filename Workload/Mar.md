### part 1  evaluation
- testing in asvspoof 2021
- radnom choose data in 200d data in LA and DF , each 100 T/F
- run in raspberry pi 5 , for testing performance , eer and auc

### part 2 Android 
- Research android , list out how i have to do . 

there
### 

## 1/3/2026
- (done) download asvspoof 2021 dataset 
## 15/3/2026
(venv2_asvspoof) carmen@raspberrypi:~/asvspoof/results $ ls
    eval_platform_results.csv             platform_eval_results_LA_200_N1.csv    platform_eval_summary_LA_200_N1.json
    eval_platform_summary.json            platform_eval_results_LA_500_N1.csv    platform_eval_summary_LA_500_N1.json
    platform_eval_results_df_1000_N1.csv  platform_eval_summary_df_1000_N1.json
    platform_eval_results_LA_1000_N1.csv  platform_eval_summary_LA_1000_N1.json

- ASVspoof 2021 LA – Detector Performance and Timing

| Setting / Dataset size | #Real | #Fake | Real score min | Real score max | Real score avg | Fake score min | Fake score max | Fake score avg | Accuracy (thr=0.5) | AUC    | EER (%) | EER threshold | Separation ratio | Total time (s) | Avg time / file (s) | Analyze avg (s) |
| ---------------------- | ----- | ----- | -------------- | -------------- | -------------- | -------------- | -------------- | -------------- | ------------------ | ------ | ------- | ------------- | ---------------- | -------------- | ------------------- | --------------- |
| LA-200 (N=138)         | 68    | 70    | 3.30e-04       | 1.00           | 0.8839         | 1.54e-06       | 0.4883         | 0.0247         | 0.8824             | 0.9834 | 1.3061  | 3.25e-04      | 35.77            | 406.72         | 2.95                | 2.94            |
| LA-500 (N=340)         | 171   | 169   | 6.85e-05       | 1.00           | 0.8760         | 1.54e-06       | 0.9427         | 0.0318         | 0.8610             | 0.9815 | 0.5020  | 6.83e-05      | 27.56            | 1190.85        | 3.50                | 3.49            |
| LA-1000 (N=684)        | 337   | 347   | 3.49e-05       | 1.00           | 0.8524         | 4.49e-07       | 0.9823         | 0.0425         | 0.8353             | 0.9756 | 0.1000  | 3.44e-05      | 20.06            | 2550.23        | 3.73                | 3.71            |


- Deepfake (DF) Dataset – Timing and Scores

| Dataset | #Real | #Fake | Fake score min | Fake score max | Fake score avg | Total runtime (s) | Program total (s) | Min time (s) | Max time (s) | Avg time / file (s) | p95 time (s) | Upload avg (s) | Analyze avg (s) | Analyze min (s) | Analyze max (s) |
| ------- | ----- | ----- | -------------- | -------------- | -------------- | ----------------- | ----------------- | ------------ | ------------ | ------------------- | ------------ | -------------- | --------------- | --------------- | --------------- |
| DF-100  | 0     | 64    | 5.56e-07       | 1.00           | 0.0257         | 150.69            | 171.98            | 1.21         | 4.34         | 2.35                | 3.66         | 0.0076         | 2.35            | 1.21            | 4.33            |

- ASVspoof 2019 LA - Detailed Timing Tables (All Sample Sizes)

| Dataset  | #Real | #Fake | Fake score min | Fake score max | Fake score avg | Total runtime (s) | Program total (s) | Min time (s) | Max time (s) | Avg time/file (s) | p95 time (s) | Upload avg (s) | Analyze avg (s) | Analyze min (s) | Analyze max (s) |
| -------- | ----- | ----- | -------------- | -------------- | -------------- | ----------------- | ----------------- | ------------ | ------------ | ----------------- | ------------ | -------------- | --------------- | --------------- | --------------- |
| LA19-200 | 100   | 100   | 7.69e-07       | 0.8765         | 0.0394         | 563.19            | –                 | –            | –            | 2.8159            | 4.4855       | 0.0082         | 2.8077          | –               | –               |
| LA19-100 | 50    | 50    | 7.69e-07       | 0.8765         | 0.0266         | 259.30            | –                 | –            | –            | 2.5930            | 4.3195       | 0.0079         | 2.5852          | –               | –               |
| LA19-50  | 25    | 25    | 3.50e-06       | 0.8765         | 0.0510         | 131.10            | –                 | –            | –            | 2.6219            | 4.5173       | 0.0075         | 2.6144          | –               | –               |
| LA19-20  | 10    | 10    | 3.50e-06       | 0.8765         | 0.0903         | 49.43             | –                 | –            | –            | 2.4714            | 3.2913       | 0.0074         | 2.4640          | –               | –               |

- Complete Deepfake Audio Detection Performance

| Dataset   | Total | Real | Fake | Real Avg | Fake Avg | Fake Min/Max | AUC    | EER (%) | Accuracy@0.5 | Avg Time/File (s) | Real-time?  |
| --------- | ----- | ---- | ---- | -------- | -------- | ------------ | ------ | ------- | ------------ | ----------------- | ----------- |
| LA19-200  | 200   | 100  | 100  | 0.9919   | 0.0394   | 7.7e-07/0.88 | 1.0    | 0.0     | 100%         | 2.82              | ✅ Excellent |
| LA21-200  | 138   | 68   | 70   | 0.8839   | 0.0247   | 1.5e-06/0.49 | 0.9834 | 1.31    | 88.24%       | 2.95              | ✅ Excellent |
| LA21-500  | 340   | 171  | 169  | 0.8760   | 0.0318   | 1.5e-06/0.94 | 0.9815 | 0.50    | 86.10%       | 3.50              | ✅ Good      |
| LA21-1000 | 684   | 337  | 347  | 0.8524   | 0.0425   | 4.5e-07/0.98 | 0.976  | 0.10    | 83.53%       | 3.73              | ✅ Good      |
| DF-100    | 64    | 0    | 64   | –        | 0.0257   | 5.6e-07/1.00 | –      | –       | 98.44%       | 2.35              | ✅ Excellent |

