## 🔧 Architecture & Performance Enhancements

| Area | Current | Improved |
|------|---------|----------|
| **IoT Simulation** | Python + MQTT | Add `fault_injection.py`, timestamp drift, and batch mode |
| **Ingestion** | AWS IoT Core → Lambda | Add IoT Rules → Kinesis for scalable routing |
| **Processing** | Lambda + Glue | Use Glue streaming or MSK for real-time pipelines |
| **ML/AI** | SageMaker + SHAP | Add ONNX export, quantization, and model registry |
| **Storage** | S3 + DynamoDB | Partitioned S3 (Parquet), TTL on DynamoDB |
| **Monitoring** | CloudWatch + SNS | Add X-Ray, SLO dashboards, and anomaly alerts |
| **Security** | IAM, KMS, CloudTrail | Add Config Rules, Secrets Manager, and tfsec/Checkov gates |

---

## 💰 1. Resource Pricing Optimization

- **Replace SageMaker with lightweight alternatives**:
  - Use **Amazon Bedrock** (if available) for inference-only workloads.
  - Or run **scikit-learn/XGBoost in Lambda** for small models.
- **Athena query cost control**:
  - Partition S3 data by timestamp or device ID.
  - Compress data (Parquet/ORC) to reduce scan size.
- **CI/CD cost savings**:
  - Use **GitHub Actions** for build/test, trigger AWS CodePipeline only on merge to `main`.
- **Dashboard alternatives**:
  - Use **Streamlit or Dash** hosted on EC2 Free Tier or ECS Fargate instead of QuickSight.

---

## 🚀 2. Resource Performance Enhancements

- **IoT throughput**:
  - Batch MQTT messages or use **IoT Rules Engine** to route directly to Kinesis.
- **Lambda cold start mitigation**:
  - Use **provisioned concurrency** for critical ingestion functions.
- **ML model latency**:
  - Quantize models with **ONNX or TensorRT** for faster inference.
- **Data pipeline tuning**:
  - Use **Glue streaming jobs** for near-real-time ETL if batch latency is too high.

---

## 🔧 3. Improvements to Architecture and Workflow

- **Fault injection module**:
  - Add `fault_injection.py` to simulate sensor dropouts, spikes, or timestamp drift.
- **Governance annotations**:
  - Include `audit_trail_notes.md` per module with IAM rationale and encryption scope.
- **Multi-tier onboarding**:
  - Add `NOTES.md` with CLI, Terraform, and CI/CD onboarding paths for interns, engineers, and auditors.

---

## 🗂️ 4. Project Structure Enhancements
```
📦 virtual-smart-factory-capstone/
├── README.md
├── architecture/
│   └── diagram.png
├── terraform/
│   └── modules/
├── iot_simulation/
│   ├── mqtt_simulator.py
│   ├── fault_injection.py
├── ml_model/
│   ├── train_model.ipynb
│   ├── explainability_shap.ipynb
│   └── model.onnx
├── ci_cd/
│   ├── pipeline.yml
│   └── security_scan.yml
├── governance/
│   ├── audit_rationale.md
│   ├── iam_scope_matrix.csv
│   ├── encryption_zones.md
├── dashboards/
│   ├── streamlit_app.py
│   └── sample_data.csv
├── sre/
│   ├── runbooks/
│   ├── slo_definitions.md
│   └── alert_matrix.md
└── onboarding/
|   ├── NOTES.md
|   ├── setup_checklist.md
│   └── screenshots/
├── test/
│   ├── unit/
│   ├── integration/
│   └── fault_injection/
```
---

## 🔁 5. CI/CD Flow Enhancements

- **Multi-stage pipeline**:
  - `build → test → security scan → deploy → notify`
- **Security gates**:
  - Integrate **Checkov or tfsec** for Terraform scanning.
  - Use **Bandit** for Python static analysis.
- **Artifact tagging**:
  - Tag Lambda and ML artifacts with Git SHA and timestamp for traceability.
- **Pipeline YAML**:
  - Include `pipeline.yml` with comments explaining each stage for onboarding clarity.


## 🔁 CI/CD Flow (Improved)

```yaml
# pipeline.yml
stages:
  - build
  - test
  - security_scan
  - deploy
  - notify

security_scan:
  tools: [tfsec, Bandit]
  fail_on: [critical, high]
```

- Artifact tagging with Git SHA + timestamp
- Slack/email notifications via SNS
- Terraform plan → apply gated by approval
                            
---

## 🔐 6. DevSecOps Enhancements

- **Secrets management**:
  - Use **AWS Secrets Manager** or **SSM Parameter Store** for MQTT credentials, ML model metadata
- **IAM boundary enforcement**:
  - Add `iam_scope_matrix.csv` mapping roles to services and actions.
- **Audit hooks**:
  - Enable **Config Rules** for drift detection, , enforce encryption
  - Use **CloudTrail Insights** for anomaly detection in API usage.
- **Security test suite**:
  - Add `test/security/` with IAM policy validation and encryption checks.
- **Audit Trail**: CloudTrail Insights + governance notes per module

---

## 🌐 7. Web Application Idea (Frontend + API)

**Title**: “Smart Factory Dashboard”

- **Frontend**: React or Streamlit
- **Backend**: Flask or FastAPI
- **Features**:
  - Real-time sensor feed (via WebSocket or polling)
  - Anomaly alerts with SHAP explanations
  - IAM event logs and audit trail viewer
  - Cost dashboard (S3 usage, Lambda invocations)
  - Alert viewer (SNS + CloudWatch)
- **Deployment**: ECS Fargate or EC2 Free Tier

---

## 📈 8. Monitoring & SRE Practices

- **SLOs and SLIs**:
  - Define SLOs for ingestion latency, ML inference time, and alert delivery.
- **Observability stack**:
  - Use **CloudWatch Logs + Metrics + Alarms**
  - Add **X-Ray tracing** for Lambda and API flows
- **Synthetic monitoring**:
  - Simulate sensor traffic and validate end-to-end delivery
- **Runbooks**:
  - Add `sre/runbooks/` for fault recovery, alert triage, and escalation paths


 
Next up, I’ll scaffold the enhanced folders and CI/CD pipeline YAML, then generate your Streamlit dashboard and SRE runbooks. Let’s lock in the next deliverables:

---

## 🗂️ Folder Scaffolding: Governance, SRE, CI/CD

### 📁 `governance/`
- `audit_rationale.md`: Justifies IAM roles, encryption, and service boundaries
- `iam_scope_matrix.csv`: Maps IAM roles to actions and services
- `encryption_zones.md`: Lists KMS usage across S3, DynamoDB, SageMaker

### 📁 `sre/`
- `slo_definitions.md`: Defines SLIs/SLOs for ingestion, inference, alerting
- `alert_matrix.md`: Maps CloudWatch alerts to severity and response
- `runbooks/`: Fault recovery, alert triage, escalation paths

### 📁 `ci_cd/`
- `pipeline.yml`: Multi-stage pipeline with security gates and artifact tagging
- `security_scan.yml`: tfsec + Bandit integration with fail-on-high policy

---

## 🧪 CI/CD Pipeline Snippet

```yaml
# pipeline.yml
name: SmartFactory CI/CD

on:
  push:
    branches: [main]

jobs:
  build-test-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Terraform Init & Plan
        run: |
          terraform init
          terraform plan -out=tfplan

      - name: Security Scan
        run: |
          tfsec .
          bandit -r iot_simulation/

      - name: Deploy
        run: terraform apply tfplan

      - name: Notify
        run: aws sns publish --topic-arn $SNS_TOPIC --message "Deployment complete"
```

## 🧭 Streamlit Dashboard UI — Clean Version

### 🔹 Dashboard Panels

| Panel | Description |
|-------|-------------|
| **📈 Sensor Trends** | Line charts for temperature, humidity, vibration over time |
| **🚨 Anomaly Alerts** | Table with timestamp, severity, SHAP explanation |
| **🔐 IAM Audit Viewer** | List of IAM changes from CloudTrail logs |
| **📦 Cost Dashboard** | Lambda invocations, S3 usage, DynamoDB writes |
| **📊 Alert Viewer** | SNS alerts with timestamps and source service |

---

### 🧩 Streamlit Layout (Python Snippet)

```python
import streamlit as st
import pandas as pd
import plotly.express as px

st.title("📊 Virtual Smart Factory Dashboard")

# Load sample data
data = pd.read_csv("sample_data.csv")

# Sensor Trends
st.subheader("📈 Sensor Trends")
fig = px.line(data, x="timestamp", y=["temperature", "humidity", "vibration"])
st.plotly_chart(fig)

# Anomaly Alerts
st.subheader("🚨 Anomaly Alerts")
anomalies = data[data["anomaly"] == True]
st.dataframe(anomalies[["timestamp", "severity", "shap_reason"]])

# IAM Audit Viewer
st.subheader("🔐 IAM Audit Events")
audit_logs = pd.read_csv("iam_events.csv")
st.dataframe(audit_logs)

# Cost Dashboard
st.subheader("📦 Resource Usage")
st.metric("Lambda Invocations", "12,340")
st.metric("S3 Storage (GB)", "1.2")
st.metric("DynamoDB Writes", "8,900")

# Alert Viewer
st.subheader("📊 SNS Alerts")
alerts = pd.read_csv("sns_alerts.csv")
st.dataframe(alerts)
```

---

Would you like me to generate the sample CSVs (`sample_data.csv`, `iam_events.csv`, `sns_alerts.csv`) next, or bundle this into your `dashboards/` folder with README annotations for onboarding and executive clarity? We’re nearly at full polish.


Absolutely, Shilpa. Here's a complete set of modular, annotated code snippets for your virtual smart factory capstone—covering simulation, ingestion, ML, explainability, CI/CD, and dashboard UI. Each snippet is designed for auditability, onboarding, and executive clarity.

---

## 🧪 1. Synthetic IoT Data Generator (`mqtt_simulator.py`)

```python
import json, time, random
import paho.mqtt.client as mqtt
import ssl

ENDPOINT = "your-iot-endpoint.amazonaws.com"
CLIENT_ID = "virtual_sensor_001"
TOPIC = "iot/sensor/data"

client = mqtt.Client(client_id=CLIENT_ID)
client.tls_set(ca_certs="AmazonRootCA1.pem",
               certfile="device-certificate.pem.crt",
               keyfile="private.pem.key",
               tls_version=ssl.PROTOCOL_TLSv1_2)
client.connect(ENDPOINT, 8883)

while True:
    payload = {
        "temperature": round(random.uniform(20, 80), 2),
        "humidity": round(random.uniform(30, 90), 2),
        "vibration": round(random.uniform(0.0, 1.0), 2),
        "timestamp": time.strftime("%Y-%m-%d %H:%M:%S")
    }
    client.publish(TOPIC, json.dumps(payload))
    print("Published:", payload)
    time.sleep(5)
```

---

## ⚠️ 2. Fault Injection Module (`fault_injection.py`)

```python
def inject_fault(payload):
    if random.random() < 0.1:
        payload["temperature"] = None  # simulate dropout
    if random.random() < 0.05:
        payload["vibration"] *= 10  # simulate spike
    return payload
```

---

## 📊 3. Terraform IoT Core Setup (`main.tf`)

```hcl
resource "aws_iot_thing" "virtual_sensor" {
  name = "virtual_sensor_001"
}

resource "aws_iot_policy" "iot_policy" {
  name = "virtual_sensor_policy"
  policy = jsonencode({
    "Version": "2012-10-17",
    "Statement": [{
      "Effect": "Allow",
      "Action": ["iot:Connect", "iot:Publish", "iot:Receive", "iot:Subscribe"],
      "Resource": "*"
    }]
  })
}

resource "aws_iot_certificate" "cert" {
  active = true
}
```

---

## 📓 4. ML Model Training + SHAP Explainability (`train_model.ipynb`)

```python
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
import shap

df = pd.read_csv("sample_data.csv")
X = df[["temperature", "humidity", "vibration"]]
y = df["anomaly"]

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
model = RandomForestClassifier().fit(X_train, y_train)

explainer = shap.TreeExplainer(model)
shap_values = explainer.shap_values(X_test)
shap.summary_plot(shap_values[1], X_test)
```

---

## 📦 5. ONNX Export

```python
import skl2onnx
from skl2onnx import convert_sklearn
from skl2onnx.common.data_types import FloatTensorType

initial_type = [('float_input', FloatTensorType([None, 3]))]
onnx_model = convert_sklearn(model, initial_types=initial_type)
with open("model.onnx", "wb") as f:
    f.write(onnx_model.SerializeToString())
```

---

## 🔁 6. CI/CD Pipeline YAML (`pipeline.yml`)

```yaml
name: SmartFactory CI/CD

on:
  push:
    branches: [main]

jobs:
  build-test-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Terraform Init & Plan
        run: terraform init && terraform plan -out=tfplan

      - name: Security Scan
        run: tfsec . && bandit -r iot_simulation/

      - name: Deploy
        run: terraform apply tfplan

      - name: Notify
        run: aws sns publish --topic-arn $SNS_TOPIC --message "Deployment complete"
```

---

## 🌐 7. Streamlit Dashboard UI (`streamlit_app.py`)

```python
import streamlit as st
import pandas as pd
import plotly.express as px

st.title("📊 Virtual Smart Factory Dashboard")

data = pd.read_csv("sample_data.csv")

st.subheader("📈 Sensor Trends")
fig = px.line(data, x="timestamp", y=["temperature", "humidity", "vibration"])
st.plotly_chart(fig)

st.subheader("🚨 Anomaly Alerts")
anomalies = data[data["anomaly"] == True]
st.dataframe(anomalies[["timestamp", "severity", "shap_reason"]])

st.subheader("🔐 IAM Audit Events")
audit_logs = pd.read_csv("iam_events.csv")
st.dataframe(audit_logs)

st.subheader("📦 Resource Usage")
st.metric("Lambda Invocations", "12,340")
st.metric("S3 Storage (GB)", "1.2")
st.metric("DynamoDB Writes", "8,900")

st.subheader("📊 SNS Alerts")
alerts = pd.read_csv("sns_alerts.csv")
st.dataframe(alerts)
```

---

Would you like me to bundle these into a GitHub folder structure next, or generate sample CSVs and screenshots for dashboard simulation? We’re at full portfolio polish.


