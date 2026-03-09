# 새 서버 적용 가이드

## 변경 파일

### 1. `.env.prod`

```
GRAFANA_ADMIN_PASSWORD=새비밀번호
ORACLE_MAIN_DSN=oracle://유저:비밀번호@호스트:1522/서비스명?wallet=/wallet
```

### 2. `prometheus/prometheus.yml`

```yaml
static_configs:
  - targets: ["<IP>:<포트>"]    # 모니터링할 서버 IP
    labels:
      env: "main"
      vm: "main-vm"
```

### 3. 대시보드 JSON (job_name 변경 시에만)

`grafana/dashboards/springboot-application-dashboard.json` 에서 전체 치환:

```
job="one-question-api"  -->  job="새이름"
```

### 4. Oracle Wallet

OCI 콘솔에서 Wallet zip 다운로드 후:

```bash
unzip Wallet_xxx.zip -d oracle/wallet/main/
```

Wallet은 git에 포함되지 않으므로 VM에 직접 전송:

```bash
scp -i <SSH_KEY> -r oracle/wallet/main/* user@VM_IP:~/observability/oracle/wallet/main/
```

## 실행

```bash
# Oracle 모니터링 없이
docker compose --env-file .env.prod up -d

# Oracle 모니터링 포함
docker compose --env-file .env.prod --profile oracle up -d
```

## 확인

1. `http://<VM-IP>:9090/targets` — 타겟 UP 여부
2. `http://<VM-IP>:3000` — 대시보드 데이터 표시 여부
3. Oracle exporter — `http://<VM-IP>:9161/metrics` 응답 여부
