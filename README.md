# Judith Barrios — ITAI-4370 Course Portfolio
### AI/5G Communications and ORAN Networks
Instructor: Tawanda Chiyangwa | Houston Community College

This portfolio documents my learning journey through ITAI-4370, tracing my growth from foundational telecommunications concepts to applied AI techniques in network simulation, agent-based modeling, and predictive analytics.

---

## 1. Selected Works

| Module | Artifact | What It Covers | Folder |
|---|---|---|---|
| 1 | Assignment 01 | Telecommunications fundamentals: transmitter-channel-receiver model, analog vs. digital signals, network topology comparison (star, mesh, ring, bus) | `/module-01/` |
| 1 | Laboratory 01 | Communication system block diagram and BPSK signal flow simulation in Python; simulated bit error rate matched the theoretical curve across 0-14 dB SNR | `/module-01/` |
| 2 | Laboratory 02 | Wireshark capture of live Wi-Fi traffic (1,287 packets) plus Free-Space Path Loss modeling for a 2.4 GHz signal from 0.1-20 km | `/module-02/` |
| 2 | Assignment 02 | 5G Core Network architecture: EPC to 5GC transition, network slicing, Multi-access Edge Computing, Network Repository Function | `/module-02/` |
| 3 | Assignment 03 | AI/ML applications in RAN, Self-Organizing Networks, and the Open RAN Intelligent Controller (RIC) | `/module-03/` |
| 3 | Laboratory 03 | Random Forest network traffic prediction using lag and rolling-average features (test R² = 0.893) | `/module-03/` |
| 6 | Laboratory 04 | ARIMA vs. Linear Regression time-series forecasting of a full year of hourly traffic data | `/module-06/` |
| 7 | Laboratory 05 | Edge computing model compression: pruning, INT8 quantization, and knowledge distillation on an IoT sensor classifier | `/module-07/` |
| 13 | Assignment 04 | Ethical AI (30-question written assignment): algorithmic bias, EU AI Act risk tiers, self-driving car ethics | `/module-13/` |
| 10 | Mid Term Project — Network Simulation and Predictive Routing | Three-part coding project: SimPy packet simulation, Mesa agent-based congestion model, scikit-learn predictive routing | `/projects/network-simulation-predictive-routing/` |
| 14 | Capstone Project — Final Exam Portfolio | Cumulative write-up covering Modules 1, 2, 3, 6, 7, and 13 | `/final-portfolio/` |



---

## 2. Reflective Essays

### Essay 1: How My Understanding of Telecommunications Evolved

When I started ITAI-4370, my picture of telecommunications was mostly mechanical: networks moved data from one point to another, and performance meant speed and coverage. Assignment 01 gave me the real vocabulary for that picture, the transmitter-channel-receiver model, and the topology comparison was what actually stuck. Star, mesh, ring, and bus all trade cost for reliability in different ways. A mesh network fixes the single-point-of-failure problem a star network has, but only by adding more cables and cost, and that same tradeoff came back later when the course got to Open RAN design.

Laboratory 01 made that abstraction feel real. I simulated a BPSK communication link in Python, generating bits, adding Gaussian noise, decoding with a threshold rule, and comparing my simulated bit error rate against the known theoretical curve across a 0-14 dB signal-to-noise range. My results landed right on top of the theoretical curve, which was a good check that I actually understood how noise degrades a digital signal. Laboratory 02 extended that into the physical and practical layers: capturing live Wi-Fi traffic in Wireshark and separately modeling Free-Space Path Loss, where even a 2.4 GHz signal loses about 80 dB at just 0.1 km. Assignment 02 then reframed the network architecture itself, showing me that the move from the 4G core (EPC) to the 5G core (5GC) is really about an API-based design that makes network slicing possible, letting one physical network act like several tuned networks at once.

The turning point was Module 7, IoT and Edge Computing with AI. I had assumed centralizing processing in the cloud was always more efficient, but Laboratory 05 made the tradeoffs concrete by compressing a neural network three different ways: pruning, INT8 quantization, and knowledge distillation. Quantization cut inference time from about 71 ms to under 1 ms with barely any accuracy loss, while distillation shrank the model to under 5 KB at the cost of a few accuracy points, and each fit a different tier of hardware, from microcontrollers to edge gateways to full edge servers. That is what made Open RAN click for me: disaggregating hardware and software in the radio access network is not just a cost-saving move, it is what makes intelligent, distributed AI-driven network management possible in the first place.

### Essay 2: Applying AI to Telecommunications Problems, Skills Gained, and Areas for Growth

Assignment 03 was where AI and telecommunications fully connected for me. I wrote about how a network can adjust spectrum and power automatically based on traffic, how Self-Organizing Networks catch problems before they become outages, and how Open RAN's RAN Intelligent Controller splits into a slower policy-and-training half and a fast, sub-second radio-decision half. One question asked me to connect a normalization technique from an earlier lab to real Open RAN scheduling, and writing that out showed me normalization isn't just a data-cleaning step, it's what lets a scheduler treat a video stream and a small sensor reading fairly based on need instead of raw numbers.

The Network Simulation and Predictive Routing project let me apply that across three different AI techniques. In the SimPy simulation, packets moved through a fixed chain of three routers with no intelligence involved, and the mean latency of 6.5 seconds came entirely from queuing delay at each hop. The Mesa agent-based model added adaptive intelligence: when a router's queue got too long, users rerouted to whichever router had the shortest queue, and during a simulated traffic burst, about 17% of all packets ended up rerouted, with the network fully recovering within about ten steps after the burst ended. The scikit-learn model took that a step further into prediction: a Random Forest model, trained on lag and time-of-day features, predicted traffic with an R² of about 0.79 and used those predictions to flag routers for rerouting before they actually became congested, catching 80% of real overload events (recall) while being right 91% of the time it chose to act (precision). Seeing all three side by side made the difference between reactive, adaptive, and predictive network management concrete in a way that no single technique could have shown on its own.

Laboratory 03, Laboratory 04, and Assignment 04 rounded that out. Comparing a Random Forest traffic predictor against ARIMA and Linear Regression models taught me that engineered features (lag values, rolling averages, cyclical time encodings) mattered more to accuracy than which algorithm I picked. Assignment 04, on Ethical AI, added a dimension I hadn't weighed as carefully: I used Amazon's biased hiring tool as an example of how bias usually comes from outdated training data reflecting old patterns rather than deliberate discrimination, and worked through how the EU AI Act tiers regulation by risk level, banning some uses outright while requiring heavy oversight for high-risk ones like hiring or policing.

Across the course, the skill I improved the most was translating a real network problem into a working model and reading the results critically instead of accepting them at face value. Looking ahead, I want to get more comfortable scaling these models beyond classroom scope, since my SimPy, Mesa, and scikit-learn projects worked well at a small scale, but real ORAN deployments involve far more nodes and messier data than synthetic traffic. I'd also like to go deeper into reinforcement learning approaches to dynamic spectrum and resource allocation, and to keep practicing the habit from Assignment 04 of checking for bias throughout a project rather than only before launch.

---

## 3. Project Documentation

### Mid Term Project: Network Simulation and Predictive Routing (SimPy, Mesa, scikit-learn)

**Question 1 — Network Traffic Simulation using SimPy**

*Problem statement:* Simulate a network of three routers connected in a chain, where each router processes packets and forwards them to the next, to measure total network latency under queuing delay.

*Method:* Built a `Router` class wrapping a SimPy `Resource` with capacity 1, so each router can only process one packet at a time. Fifty packets were generated at random intervals (mean interarrival 2.0s) and passed through Router-A, Router-B, and Router-C in sequence, with a random exponential processing delay (mean 1.0s) at each hop.

*Results:* All 50 packets were delivered. Mean latency was 6.502s, ranging from 1.442s (fastest) to 16.787s (slowest). Throughput was 0.444 packets/sec.

*Interpretation:* The slowest packets were the ones stuck waiting behind others, since each router handles only one packet at a time — queuing delay compounds across hops even when each individual router is fast.

**Question 2 — Agent-Based Network Modeling using Mesa**

*Problem statement:* Design an agent-based model with multiple routers and users, where routers adaptively reroute packets when congestion occurs.

*Method:* Built `RouterAgent` (tracks a packet queue, flags itself congested above a threshold of 4) and `UserAgent` (sends packets to a home router, or to the least-busy alternate router if the home router is congested) classes in Mesa. Four routers and 10 users ran for 50 steps, with an artificial traffic burst injected between steps 10-21.

*Results:* Users sent 306 packets total; 52 (17.0%) were rerouted. Per-router: Router-0 processed 90 packets (10 congested steps), Router-1 processed 92 (11 congested steps), Router-2 processed 63 (9 congested steps, 16 rerouted in), Router-3 processed 61 (10 congested steps, 19 rerouted in).

*Interpretation:* Total queue length and cumulative reroutes both spiked once the burst began around step 10, and queue length dropped back to near zero by roughly step 30 — the network recovered on its own. Routers 2 and 3 absorbed most of the rerouted traffic, showing that simple local rules (each router reacting only to its own queue) can redistribute load without any central controller.

**Question 3 — Predictive Optimization using Machine Learning**

*Problem statement:* Use AI to predict future network traffic and use those predictions to trigger routing decisions before congestion happens, rather than reacting after the fact.

*Method:* Generated two weeks of synthetic hourly traffic for 3 routers with a daily sine-wave pattern, a weekend effect, and noise. Added 1-hour-lag and 24-hour-lag features, then trained and compared Linear Regression and Random Forest (scikit-learn), keeping the train/test split in time order. The better model's predictions were compared against a congestion capacity threshold to flag "REROUTE" vs. "NORMAL" hours.

*Results:* Random Forest outperformed Linear Regression on every metric (MAE 5.291 vs. 5.414, RMSE 6.700 vs. 6.883, R² 0.7926 vs. 0.7811). Using Random Forest's predictions for congestion flagging achieved precision of 0.910 and recall of 0.803.

*Interpretation:* The model explained about 79% of traffic variance and correctly caught 80% of real overload events while keeping unnecessary reroutes low (91% precision). Unlike Question 2's reactive rerouting, this approach lets the network act ahead of the problem instead of after it.

Full write-up: `Barrios_Judith_Network_Simulation_Assignment.docx`. Notebooks: `Question_1_SimPy_Network_Simulation.ipynb`, `Question_2_Mesa_Agent_Based_Model.ipynb`, `Question_3_ML_Predictive_Routing.ipynb`.

### Capstone Project: Final Exam Portfolio (submitted July 9, 2026)

**Problem Statement**
Consolidate and reflect on all lab and assignment work across Modules 1, 2, 3, 6, 7, and 13, showing how each result was produced and what it demonstrated about telecommunications and AI.

**Methods and Tools Used**
Python (NumPy, pandas, scikit-learn, statsmodels, TensorFlow/Keras), Wireshark for packet capture, and written analysis for each module's key takeaway.

**Key Results Documented**
- **Module 1 (Lab 01):** BPSK link simulation — simulated bit error rate matched the theoretical curve across 0-14 dB SNR
- **Module 2 (Lab 02):** Wireshark capture of 1,287 Wi-Fi packets; Free-Space Path Loss modeling showing ~80 dB loss at 0.1 km, rising to ~125 dB at 18 km
- **Module 3 (Lab 03):** Random Forest traffic prediction — test R² of 0.893, with the 1-hour lag feature carrying 81.6% of feature importance
- **Module 6 (Lab 04):** ARIMA(2,1,2) vs. engineered Linear Regression on a full year of hourly traffic (8,760 rows); ADF test confirmed stationarity (p < 0.001)
- **Module 7 (Lab 05):** Edge model compression — quantization cut inference time from ~71 ms to under 1 ms with minimal accuracy loss; distillation shrank the model to under 5 KB at a small accuracy cost
- **Module 13 (Assignment 04):** Ethical AI analysis using Amazon's biased hiring tool and the EU AI Act's risk-tiered regulation as worked examples

**Diagrams, Results, and Interpretation**
Full figures, tables, and per-module "what I learned" reflections are documented in the source PDF.

Full write-up: `FE_Judith_Barrios_ITAI4370.pdf`.

---

## 4. Assessment Artifacts

| Assessment | Module | Topic |
|---|---|---|
| Assignment 01 | 1 | Telecommunications fundamentals |
| Laboratory 01 | 1 | BPSK signal simulation |
| Laboratory 02 | 2 | Wireshark capture + RF propagation modeling |
| Assignment 02 | 2 | 5G Core Network & architecture |
| Assignment 03 | 3 | AI/ML in RAN, SON & Open RAN |
| Laboratory 03 | 3 | Network traffic prediction (Random Forest) |
| Laboratory 04 | 6 | Time-series forecasting (ARIMA & Linear Regression) |
| Laboratory 05 | 7 | Edge computing model compression |
| Assignment 04 | 13 | Ethical AI (30 questions) |
| Mid Term Project — Network Simulation & Predictive Routing | 10 | SimPy, Mesa, scikit-learn coding project |
| Quiz | 11 | Overview of everything |
| Capstone Project — Final Exam Portfolio | 14 | Cumulative write-up covering Modules 1, 2, 3, 6, 7, and 13 |


---

## 5. Growth Evidence: Basic to Advanced

| Stage | Module(s) | Evidence |
|---|---|---|
| Basic | 1 | Transmitter-channel-receiver model, network topologies, BPSK signal simulation |
| Basic-Intermediate | 2 | Live packet capture (Wireshark), RF propagation modeling, 5G Core / network slicing |
| Intermediate | 3 | AI/ML applied to RAN operations, Random Forest traffic prediction (R² = 0.893) |
| Intermediate | 6 | ARIMA vs. Linear Regression forecasting on a full year of traffic data |
| Intermediate-Advanced | 10 | Mid Term Project — Network Simulation & Predictive Routing: reactive (SimPy) → adaptive (Mesa) → predictive (scikit-learn) network management |
| Advanced | 7 | Edge model compression (pruning, INT8 quantization, distillation) for resource-constrained AI deployment |
| Advanced | 13 | Ethical AI: algorithmic bias, EU AI Act risk tiers, personal framework for responsible AI practice |
| Cumulative | 14 | Capstone Project — Final Exam Portfolio |

The clearest thread across the course is a recurring question: how do you make the most of a limited resource (bandwidth, memory, processing time) when demand keeps shifting? That question shows up from the BPSK noise lab all the way through the predictive routing model, just in different forms.

---

