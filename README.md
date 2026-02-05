# security-workflows

# 🔒 Security Workflows

GitHub Actions 재사용 가능한 보안 스캔 워크플로우 모음

## 📋 포함된 워크플로우

### 1. Trivy Security Scan
컨테이너 이미지 및 파일시스템 취약점 스캔

**활성화**: `auto-trivy` 토픽 추가

```yaml
jobs:
  security:
    uses: spectrakr/security-workflows/.github/workflows/trivy-scan.yml@main
```

### 2. ZAP Baseline Scan (수동 스캔)
웹 애플리케이션 기본 보안 점검 - **실제 공격 없음**, 안전

**활성화**: `auto-zap-baseline` 토픽 추가

```yaml
jobs:
  zap-baseline:
    uses: spectrakr/security-workflows/.github/workflows/zap-baseline-scan.yml@main
    with:
      target_url: 'http://localhost:3000'
      app_start_command: 'npm install && npm start'
      app_port: '3000'
```

### 3. ZAP API Scan
REST/GraphQL API 전용 스캔 (OpenAPI/Swagger 지원)

**활성화**: `auto-zap-api` 토픽 추가

```yaml
jobs:
  zap-api:
    uses: spectrakr/security-workflows/.github/workflows/zap-api-scan.yml@main
    with:
      target_url: 'http://localhost:8080/api'
      app_start_command: 'npm install && npm start'
      app_port: '8080'
      api_spec_url: 'http://localhost:8080/api-docs/swagger.json'
```

### 4. ZAP Full Scan (능동 스캔)
전체 웹 보안 스캔 - **⚠️ 실제 공격 수행, 테스트 환경에서만 사용**

**활성화**: `auto-zap-full` 토픽 추가

```yaml
jobs:
  zap-full:
    if: github.event_name == 'workflow_dispatch'  # 수동 실행 권장
    uses: spectrakr/security-workflows/.github/workflows/zap-full-scan.yml@main
    with:
      target_url: 'http://localhost:3000'
      app_start_command: 'npm install && npm start'
      app_port: '3000'
      scan_timeout: '45'
```

## 🚀 빠른 시작

### 1단계: 토픽 추가
```bash
# Trivy 스캔 활성화
gh repo edit YOUR-ORG/YOUR-REPO --add-topic auto-trivy

# ZAP Baseline 스캔 활성화
gh repo edit YOUR-ORG/YOUR-REPO --add-topic auto-zap-baseline
```

### 2단계: 워크플로우 파일 생성
`.github/workflows/security.yml` 생성:

```yaml
name: Security Scans

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  trivy:
    uses: spectrakr/security-workflows/.github/workflows/trivy-scan.yml@main
  
  zap-baseline:
    uses: spectrakr/security-workflows/.github/workflows/zap-baseline-scan.yml@main
    with:
      target_url: 'http://localhost:3000'
      app_start_command: 'npm install && npm start'
      app_port: '3000'
```

## 📊 스캔 결과 확인

스캔 리포트는 Actions 탭의 Artifacts에서 다운로드 가능:
- HTML 리포트: 브라우저에서 확인
- Markdown 리포트: 텍스트 에디터에서 확인
- JSON 리포트: 자동화 처리용

## 🔧 권장 사항

- **Trivy**: 모든 프로젝트에 기본 적용
- **ZAP Baseline**: 웹 애플리케이션에 적용
- **ZAP API**: REST/GraphQL API에 적용
- **ZAP Full**: 주 1회 또는 월 1회, 수동 실행 권장

## 📝 라이선스

MIT License
