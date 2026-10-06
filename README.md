# bitcoin_autotradetest

맥미니에서 돌리는 **비트코인 자동매매 모의투자(페이퍼 트레이딩)** 테스트 저장소입니다. 오픈소스 트레이딩 봇 [Freqtrade](https://github.com/freqtrade/freqtrade)를 사용합니다.

**실제 돈을 쓰지 않습니다.** `dry_run: true`로 고정되어 있고, 거래소 API 키도 비워둔 상태로만 운영합니다 — 전부 가상 자본(1,000 USDT)으로 시뮬레이션합니다.

## 왜 이 저장소엔 freqtrade 소스코드가 없나요

이 저장소는 freqtrade를 포크한 게 아니라, **우리가 직접 작성한 설정·전략만** 추적합니다. freqtrade 본체는 공식 저장소에서 따로 설치합니다.

## 설치 방법

```bash
git clone --depth 1 --branch stable https://github.com/freqtrade/freqtrade.git freqtrade-src
cd freqtrade-src
./setup.sh -i

# 이 저장소의 설정을 가져와서 사용
cp ../bitcoin_autotradetest/user_data/config.json.example user_data/config.json
cp ../bitcoin_autotradetest/user_data/strategies/*.py user_data/strategies/
```

`user_data/config.json`을 열어서 `api_server.jwt_secret_key`, `username`, `password`를 본인만 아는 값으로 바꾸세요 (예: `openssl rand -hex 32`). **이 값들은 절대 커밋하지 마세요** — 그래서 `config.json`은 `.gitignore`로 제외되어 있고, `config.json.example`만 저장소에 있습니다.

## 실행

```bash
source .venv/bin/activate
freqtrade trade --config user_data/config.json --strategy SampleStrategy
```

웹 대시보드: `http://127.0.0.1:8080` (설정에서 바꾼 username/password로 로그인)

## 현재 설정

- 거래소: Binance (공개 시세만 사용, API 키 없음)
- 거래쌍: BTC/USDT, ETH/USDT
- 타임프레임: 5분봉
- 전략: `SampleStrategy` — freqtrade 기본 예제(RSI 기반), **교육용이라 실제로 수익이 나는 전략이 아닙니다.** 백테스팅으로 개선해나가는 중입니다.

## 주의사항

- 이 저장소는 모의투자(dry-run) 전용입니다. 실거래로 전환하려면 `dry_run: false`로 바꾸고 실제 거래소 API 키(거래 권한)를 넣어야 하는데, **그 순간부터 원금 손실 위험이 실제로 발생**합니다. 신중하게 결정하세요.
- `config.json`, `.secrets/`, 로그, DB 파일(`*.sqlite*`)은 전부 `.gitignore`로 제외되어 있습니다 — 민감한 정보가 실수로 커밋되지 않도록 되어 있습니다.
