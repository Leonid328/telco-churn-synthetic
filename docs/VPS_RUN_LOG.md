# VPS run log - Leonid328 (2026-11-03)

**Host:** `root@168.231.116.71` (Hostinger KVM8, Ubuntu 24.04, 8 vCPU, 31 GiB)  
**Path:** `/opt/mlops-hw/telco-churn-synthetic`

First pass was on KVM2 `76.13.249.47`. Homework files were copied to KVM8. Live check 2026-11-03: Jupyter container Up, HTTP 302 on :8888.

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
# -> 10001 lines, ~1.5 MB

python src/generate_dataset_ext.py --samples 8000 --conv-samples 1200 --output-dir data
# -> telco_customers.csv, support_conversations.csv, knowledge_base.csv (+ json)
```

On KVM8 `ls data/` after migrate:

- telco_churn_demo.csv (~1.5 MB)
- telco_customers.csv (~7.2 MB)
- support_conversations.csv (~285 KB)
- knowledge_base.csv / knowledge_base.json

## Jupyter

```bash
docker compose up -d jupyter
docker compose ps
curl -s -o /dev/null -w '%{http_code}\n' http://127.0.0.1:8888/   # 302
```

Token is in `docker-compose.yml` (`secret123`). Notebook: `notebooks/explore_data.ipynb` (data under `/home/jovyan/work` or `/home/jovyan/data` depending on compose mount).

## Notes

- On 8 GB RAM do not pull Jupyter and ecommerce at the same time.
- Do not commit large `data/` artifacts; regenerate on VM per README.
