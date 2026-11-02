# Network Intrusion Detection System with Explainable AI

> 🚧 **Project status: under construction.** This is our final-year major project and it is still being built. The code, setup steps and results will be added to this repository as the work progresses.

Department of Computer Science and Engineering (Data Science)
Lords Institute of Engineering and Technology, Hyderabad, India

## Abstract
Network intrusion detection systems (NIDS) based on machine learning reach high detection rates, but most of them report an alert without explaining it. This limits their use in small organisations that have no full-time security analyst. This project presents a Network Intrusion Detection System with Explainable AI that protects a web server or a small-business network against four attack types: port scan, brute force, denial of service (DoS) and distributed denial of service (DDoS). The system is trained on the CICIDS2017 dataset with Random Forest and XGBoost, and the model with the higher macro F1-score is kept; XGBoost reached a macro F1-score of 0.9998. For every detected attack, the system reports the attack type, a confidence value, the time of detection, a plain-English explanation built from SHAP values, and a recommended action. A Live Mode connects an attacker laptop and a detector laptop over a network, so that attacks sent from one computer are detected and explained on the other in real time on a web dashboard.

**Keywords:** network intrusion detection, explainable AI, SHAP, XGBoost, Random Forest, CICIDS2017, cybersecurity


## What the system does
For every detected attack, the dashboard shows an alert card with:
- Attack type (PortScan, BruteForce, DoS or DDoS)
- Confidence (model probability)
- Time of detection
- Why it was flagged: the top 3 reasons from SHAP, in plain English
- Recommended action
- Flow count (repeated alerts from the same source are merged within 10 seconds)

## How it works
```
Traffic record -> Classifier -> attack type + confidence
              -> Detection time recorded
              -> TreeSHAP explanation (top 3 reasons)
              -> Recommended action (rule table)
              -> Alert card on dashboard
```
An alert is raised only if the predicted class is not Normal and the confidence is at least 60%.

## Dataset
[CICIDS2017](https://www.unb.ca/cic/datasets/ids-2017.html) from the Canadian Institute for Cybersecurity. Five classes are used:

| Class | Tools used to generate the traffic |
|---|---|
| Normal | Normal user profiles (HTTP, HTTPS, FTP, SSH, e-mail) |
| PortScan | 
| BruteForce | FTP-Patator, SSH-Patator |
| DoS | 
| DDoS | LOIC |

## Tech stack
Python, scikit-learn, XGBoost, SHAP (TreeSHAP), Streamlit, TCP sockets

## Project status

| Part | Status |
|---|---|
| Data preparation | ✅ Done |
| Model training and comparison (Random Forest vs XGBoost) | ✅ Done |
| SHAP explanations and recommended actions | ✅ Done |
| Dashboard with alert cards | 🚧 In progress |
| Live Mode (two laptops over TCP) | 🚧 In progress |
| Real packet capture and flow extraction | 📝 Planned |
| Tests on other datasets (e.g. CSE-CIC-IDS2018) | 📝 Planned |

## Installation
🚧 Under construction. Setup steps will be added soon.

## Usage
🚧 Under construction. Usage instructions will be added soon.

## Limitations
- Trained and tested on one dataset from 2017 (laboratory testbed).
- A score close to 1 on a random split of one benchmark should be read with care.
- Live Mode currently sends dataset records, not real attack packets.
- SHAP explains the model's behaviour, which is not always the true cause of an attack.

## Team
| Name | Roll No. |
|---|---|
| Mohammed Mujtaba Quadri | 160923750304 |
| Mohammed Abdul Kareem | 160923750303 |
| Kotagiri Charan Kumar | 160923750301 |

**Guide:** Ms. Zoya Tabassum

## References
1. I. Sharafaldin, A. H. Lashkari, and A. A. Ghorbani, "Toward generating a new intrusion detection dataset and intrusion traffic characterization," ICISSP, 2018.
2. S. M. Lundberg and S.-I. Lee, "A unified approach to interpreting model predictions," NIPS, 2017.
3. S. M. Lundberg et al., "From local explanations to global understanding with explainable AI for trees," Nature Machine Intelligence, 2020.
4. S. Mane and D. Rao, "Explaining network intrusion detection system using explainable AI framework," arXiv:2103.07110, 2021.
5. P. R. B. Houssel et al., "eX-NIDS: A framework for explainable network intrusion detection leveraging large language models," arXiv:2507.16241, 2025.
