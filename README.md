# Network Intrusion Detection System with Explainable AI

A network intrusion detection system (NIDS) that does not just say "attack". For every detected attack it shows the **attack type, confidence, time of detection, a plain-English explanation (from SHAP), and a recommended action**, on a web dashboard.

Final-year major project, Department of Computer Science and Engineering (Data Science), Lords Institute of Engineering and Technology, Hyderabad.

## Why this project
Small businesses run their own websites and servers but have no full-time security analyst. A model that only prints "attack" gives them nothing to act on. This system explains each alert in simple words and tells the owner what to do next.

## What it detects
| Class | Traffic source in CICIDS2017 |
|---|---|
| Normal | Benign user profiles (HTTP, HTTPS, FTP, SSH, e-mail) |
| PortScan 
| BruteForce | FTP-Patator, SSH-Patator |
| DoS | Hulk, GoldenEye, Slowloris, Slowhttptest |
| DDoS 

## How it works
```
Traffic record -> Classifier (XGBoost) -> attack type + confidence
              -> Detection time recorded
              -> TreeSHAP explanation (top 3 reasons, plain English)
              -> Recommended action (rule table)
              -> Alert card on dashboard
```
- Alert rule: reported as an attack only if the class is not Normal and confidence is at least 60%.
- Confidence is the raw model probability, not a calibrated one.
- Attacks from the same source IP and of the same type within 10 seconds are merged into one alert card with a flow count.

## Dataset and models
- Dataset: [CICIDS2017](https://www.unb.ca/cic/datasets/) (eight daily CSV files, from the Canadian Institute for Cybersecurity).
- Preprocessing: labels mapped to 5 classes, ID columns (flow ID, IPs, timestamps) dropped, infinite/missing values set to 0, stratified sample of at most 500,000 flows, 80% train / 20% test.
- Models compared: Random Forest and XGBoost. The one with the higher macro F1-score is kept.
- Result: **XGBoost, macro F1 = 0.9998** on the held-out test set.

> Note: a score this close to 1 on a random split of one benchmark should be read with care. Flows from the same attack run can fall in both train and test data.

## Modes
- **Replay Mode:** held-out test records are passed through the pipeline one by one.
- **Live Mode:** two laptops on the same network. The attacker laptop sends records of a chosen attack type (labels removed) over TCP. The detector laptop classifies them, explains them, and shows alert cards in real time. The source IP in each alert comes from the network connection.

Live Mode sends dataset records, not real attack packets.

## Installation
```bash
git clone <your-repo-url>
cd <your-repo-folder>
pip install -r requirements.txt
```
Main libraries: scikit-learn, xgboost, shap, streamlit, pandas, numpy.

## Usage
```bash
# 1. Put the CICIDS2017 CSV files in <data-folder>/
# 2. Prepare data and train both models
python <your_training_script>.py

# 3. Start the dashboard (detector laptop)
streamlit run <your_app_file>.py

# 4. Live Mode: on the attacker laptop
python <your_sender_script>.py --host <detector-laptop-ip> --attack <portscan|bruteforce|dos|ddos>
```

## Limitations
- Trained and tested on one dataset from 2017 (laboratory testbed).
- Live Mode replays dataset records; real packet capture and flow extraction is future work.
- SHAP explains the model's behaviour, which is not always the true cause of an attack.
- Slow or low-volume attacks may look like normal traffic.
- Alert correlation and MITRE ATT&CK mapping are not part of this version.

## Future work
Real-time packet capture and flow extraction, tests on CSE-CIC-IDS2018, time-based data split, calibrated confidence, automatic response actions.

## Team
- Mohammed Mujtaba Quadri
- Kotagiri Charan Kumar
- Mohammed Abdul Kareem

Guide: Ms. Zoya Tabassum

## References
1. Sharafaldin et al., "Toward generating a new intrusion detection dataset and intrusion traffic characterization," ICISSP 2018.
2. Lundberg and Lee, "A unified approach to interpreting model predictions," NIPS 2017.
3. Lundberg et al., "From local explanations to global understanding with explainable AI for trees," Nature Machine Intelligence, 2020.
4. Mane and Rao, "Explaining network intrusion detection system using explainable AI framework," arXiv:2103.07110, 2021.
5. Houssel et al., "eX-NIDS," arXiv:2507.16241, 2025.
