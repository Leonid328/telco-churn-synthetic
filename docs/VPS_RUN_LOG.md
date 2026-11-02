# VPS run log — Leonid328 (2026-11-02)

**Host:** `root@76.13.249.47`  
**Path:** `/opt/mlops-hw/telco-churn-synthetic`

## Setup

```bash
git clone https://github.com/Leonid328/telco-churn-synthetic.git /opt/mlops-hw/telco-churn-synthetic
cd /opt/mlops-hw/telco-churn-synthetic
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt
mkdir -p data
```

## Dataset generation

```bash
python src/generate_dataset.py --samples 10000 --output data/telco_churn_demo.csv
# → 10001 lines, ~1.5 MB

python src/generate_dataset_ext.py --samples 8000 --conv-samples 1200 --output-dir data
# → telco_customers.csv, support_conversations.csv, knowledge_base.csv (+ json)
```

## Jupyter

```bash
docker compose up -d jupyter
docker compose ps
curl -s -o /dev/null -w '%{http_code}\n' http://127.0.0.1:8888/   # 302
# Token: secret123 (see docker-compose.yml)
# URL: http://76.13.249.47:8888/?token=secret123
```

Notebook: `notebooks/explore_data.ipynb` (data mounted at `/home/jovyan/data`).

## Notes

- After concurrent Docker pulls/builds, SSH may timeout until memory frees — run Jupyter and ecommerce builds sequentially.
- Do not commit large `data/` artifacts; regenerate on VM per README.
