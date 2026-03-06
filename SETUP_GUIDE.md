# 새 서버 적용 가이드

## 변경 파일

### 1. `.env.prod`

```
GRAFANA_ADMIN_PASSWORD=새비밀번호
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

## 실행

```bash
docker compose --env-file .env.prod up -d
```

## 확인

1. `http://<VM-IP>:9090/targets` — 타겟 UP 여부
2. `http://<VM-IP>:3000` — 대시보드 데이터 표시 여부
