# **Deep Learning for Non-Intrusive Load Monitoring (NILM): A Sequence-to-Point Approach**

## **Abstract**

Non-Intrusive Load Monitoring (NILM) is the process of disaggregating a total household energy signal into individual appliance consumption patterns. This project implements a **Sequence-to-Point (Seq2Point)** deep learning framework to estimate the power draw of specific household appliances—specifically **Kettles, Microwaves, Fridges, and Televisions**—from aggregate mains data. By leveraging high-resolution datasets such as **UKDALE** and **REFIT**, this research explores the efficacy of Convolutional Neural Networks (CNNs) in learning temporal "fingerprints" for accurate energy auditing and demand-side management.

---

## **1. Methodology**

### **1.1 Datasets**

The project utilizes two primary benchmarks in NILM research:

* **UKDALE:** Data collected from UK households, focusing on specific channels (e.g., Kettle on channel 10/8, Microwave on 13/15).
* **REFIT:** A large-scale longitudinal dataset from 20 UK homes, providing a diverse set of appliance signatures for cross-house validation.

### **1.2 Data Preprocessing Pipeline**

The raw data undergoes a rigorous transformation process to ensure model convergence:

1. **Alignment & Resampling:** Aggregate mains and individual appliance data are joined and resampled to a consistent 8-second frequency.
2. **Normalization:** Both aggregate signals and target appliance readings are standardized using the formula:



For example, a mean () of 522W and a standard deviation () of 814W are typically applied to the aggregate signal.
3. **Sliding Window Transformation:** An input window length of **599 samples** is utilized to provide the network with sufficient temporal context around the target midpoint.

### **1.3 Neural Network Architecture**

The architecture follows the **Sequence-to-Point (Seq2Point)** paradigm, where the network maps a sequence of aggregate power readings to a single point estimate of the appliance's power at the center of that sequence.

* **Input:** 
* **Backbone:** 1D Convolutional Neural Network (CNN).
* **Output:** Predicted scalar power value for the target appliance.

---

## **2. Experimental Setup**

### **2.1 Appliance Parameters**

Training is tailored to the unique power characteristics of each appliance:

| Appliance | Window Length | ON Threshold | Max ON Power |
| --- | --- | --- | --- |
| **Kettle** | 599 | 2000W | 3998W |
| **Microwave** | 599 | 200W | 3969W |
| **Fridge** | 599 | 50W | 3323W |
| **Television** | 599 | 30W | 400W |

**

### **2.2 Training Protocols**

* **Optimization:** Stochastic Gradient Descent variants (Adam/RMSProp).
* **Loss Function:** Mean Squared Error (MSE) / Mean Absolute Error (MAE).
* **Transfer Learning:** Pre-trained weights from one dataset (e.g., UKDALE) can be fine-tuned on another (e.g., REFIT) to improve generalization across different home environments.

---

## **3. Results and Visualization**

Performance is assessed through visual comparison of ground truth power traces versus model disaggregation. The `Display.ipynb` notebook provides a suite for:

* Visualizing raw appliance power traces across multiple houses.
* Comparing "Aggregate" vs "Individual" consumption to identify overlapping load signatures.

---

## **4. Repository Structure**

* `UKDALE seq2point.ipynb`: End-to-end training and testing on the UKDALE dataset.
* `Refit seq2point .ipynb`: Scripts for dataset creation and model application on the REFIT dataset, including transfer learning implementations.
* `Display.ipynb`: Exploratory data analysis and prediction visualization tools.

---

## **5. Dependencies**

* Python 3.x
* TensorFlow / Keras
* Pandas (for time-series manipulation)
* Matplotlib & Seaborn (for signal visualization)
* NumPy

---

**Author Note:** This research contributes to the development of "Smart Home" systems capable of providing itemized electricity bills without the need for expensive per-plug sub-meters.
