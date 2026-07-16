# NaviTour — Smart Transport Platform & AI Road Intelligence

**Graduation Project — Faculty of Artificial Intelligence, Kafrelsheikh University**
Academic Year 2025/2026

NaviTour is an integrated smart-mobility platform for Egyptian cities (primary deployment in **Cairo**, secondary deployment in **Kafr El-Sheikh**). It unifies six architecturally integrated subsystems into a single decision-support system for urban mobility: multi-modal transit routing, an Arabic conversational assistant, a road-issue reporting/classification service, a hybrid place & tourism recommender, a route-safety scoring module, and a GNN-based travel-time estimator.

---

## 🎥 Demo & Walkthrough Videos

| Video | Link |
|---|---|
| 🚀 Project Demo | [Watch on YouTube](https://youtu.be/n-Ral8fcr9s?si=qZd03gKK8qz2xhVa) |
| 🇪🇬 Project Walkthrough (Arabic) | [Watch on YouTube](https://youtu.be/jm6ERK093Ao?si=n12rSkmAdbBUUugY) |
| 🇬🇧 Project Walkthrough (English) | [Watch on YouTube](https://youtu.be/XVNyL1QriRQ?si=_al6s0K2rhB-SPZj) |

---

## ✨ Key Features

- **🚌 Multi-modal Transit Routing (RAPTOR)** — A multi-criteria Round-Based Public Transit Routing (RAPTOR) engine computes Pareto-optimal journeys (arrival time vs. number of transfers) over a GTFS network of **2,997 bus stops, 1,011 routes, and 44,743 stop-time records**, plus Cairo Metro data. A dedicated `KafrAdvancedRouter` (BFS-based) serves the Kafr El-Sheikh network.
- **🗣️ Arabic Conversational Assistant** — A BERT-based (AraBERT) intent classifier (navigation / general / greeting / out-of-scope) with a confidence-thresholded decision rule, paired with a fine-tuned **Nile-Chat-4B** (QLoRA) entity extractor that turns free-form Egyptian Arabic text into structured navigation JSON. An optional Whisper-based ASR front end adds voice input.
- **🛣️ Road Intelligence & Prioritization (Cairo Roads)** — Citizen road complaints in colloquial Egyptian Arabic are normalized and vectorized with TF-IDF, then classified by a **Logistic Regression** category model (5 classes, **88% accuracy**) and scored by a **Ridge Regression** severity model (**MAE 1.47** on a 1–10 scale), with emergency-keyword boosting mapped to actionable priority levels.
- **📍 Hybrid Place & Tourism Recommender** — Combines proximity decay, SVD-based collaborative filtering, popularity weighting, and category-preference (content-based / cosine-similarity) signals for both Cairo and Kafr El-Sheikh datasets — designed to avoid the cold-start problem of pure collaborative filtering.
- **⚠️ Route Safety Scoring** — Integrates active, citizen-reported road hazards into a per-route safety score (0–100), classified into **Safe / Moderate / Risky / Dangerous** tiers.
- **⏱️ TransitSAGE — GNN Travel-Time Prediction** — A GraphSAGE-based model trained on a transit graph of **3,105 nodes and 5,068 edges** predicts real-world travel speeds for surface-transport segments, replacing static schedule durations with data-driven estimates that feed back into the RAPTOR router.

---

## 🏗️ System Architecture

The platform follows a **service-oriented architecture**. User input (text or voice) enters through a FastAPI gateway, is interpreted by the NLP service, and is routed to the transit-routing service — which in turn consults the travel-time service for edge durations and the road-intelligence service for hazard avoidance, alongside the recommendation service for place suggestions. All services persist to their respective databases and return results back through the gateway to the web interface.

| Component | Technology |
|---|---|
| API gateway / backend | Python, **FastAPI** (async), Pydantic validation |
| Road-intelligence service | **Flask** REST API, Flask-CORS |
| Frontend | HTML, CSS, JavaScript, **Leaflet** interactive maps |
| Data processing | Pandas, NumPy |
| Machine learning | PyTorch, PyTorch Geometric, scikit-learn, Hugging Face Transformers |
| Geospatial database | **PostgreSQL** with **PostGIS** |
| Complaint database | **Microsoft SQL Server** (`SmartCityDB`) via `pyodbc` |
| Security | `bcrypt` (password hashing), `PyJWT` (tokens) |
| Routing engines | RAPTOR (Cairo network), `KafrAdvancedRouter` (Kafr El-Sheikh) |
| Transit data format | General Transit Feed Specification (**GTFS**) |
| Maps / visualization | Leaflet, Folium |

---

## 📊 Data Sources

| Dataset | Description | Size |
|---|---|---|
| GTFS (Cairo + Kafr El-Sheikh) | Stops, routes, trips, stop_times, shapes, frequencies | Two networks |
| Places (Cairo) | Tourist places with binary interest features | Catalogue |
| Places (Kafr El-Sheikh) | Local POIs with rating and review attributes | Catalogue |
| Road complaints | Arabic complaints, 5 categories, severity 1–10 | 240 records |
| Egyptian-ASR-MGB-3 | Egyptian Arabic transcriptions (intent source) | 1,159 transcriptions |
| Intent dataset (final) | Navigation, General, Greeting, Out-of-Scope | 915 samples |
| Transit graph (`network.pkl`) | Nodes, edges, spatial/temporal/vehicle features, speeds | Derived artifact |

---

## 🎯 Project Objectives

- Extract navigation entities (origin/destination) from free-form Egyptian Arabic via a fine-tuned, low-resource LLM, producing production-ready structured JSON.
- Classify user intent robustly, filtering out irrelevant or ambiguous utterances.
- Deliver personalized, rating-free tourism/place recommendations that solve the cold-start problem.
- Generate fast, Pareto-optimal, congestion-aware transit routes with fuzzy Arabic stop-name matching.
- Automate triage of citizen road complaints (category + severity + priority) and expose the results via a documented, geospatially-aware REST API for downstream mobility consumers.
- Score route safety using live citizen-reported hazards.

---

## 🧪 Results Summary

| Module | Metric | Result |
|---|---|---|
| Road complaint category classifier | Accuracy (5 classes) | **88%** |
| Road complaint severity regressor | Mean Absolute Error | **1.47** (1–10 scale) |
| TransitSAGE travel-time model | Graph size | 3,105 nodes / 5,068 edges |
| RAPTOR routing network (Cairo) | Coverage | 2,997 stops / 1,011 routes / 44,743 stop-times |

---

## 👥 Authors

Samia Soliman Soliman El-Saedy · Hanan Al-Saeed Ibrahem Al-Hosary · Youssef Mohamed Samir Zhiry · Ibrahem Mahmoud Ahmed El-Naggar · Khaled Fares Hassan Ebeed · Mohamed Mahmoud Mohamed Mowafy · Omar Adel Abdel Salam Al-Sheikh · Mohamed El-Sayed Abdel Latif El-Ashry

**Supervised by:** Asst. Prof. Dr. Mohamed Abdo Kassem, Vice Dean for Learning and Student Affairs

Faculty of Artificial Intelligence, Kafrelsheikh University

---

## 📄 License

This project has no open-source license. It was developed and submitted as a graduation project, and is registered with the Faculty of Artificial Intelligence, Kafrelsheikh University.

## 📚 Citation

If you use this work, please cite the accompanying graduation project report:

> *NaviTour: Smart Transport Platform and AI Road Intelligence*, Faculty of Artificial Intelligence, Kafrelsheikh University, 2025/2026.
