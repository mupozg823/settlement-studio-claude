# 정산을 부탁해 — Claude 플러그인 마켓

[정산을 부탁해](https://settlement-studio.vercel.app)(방송 크루 후원 정산)의 장부를 Claude 에서 **읽기 전용**으로 보고 정리하는 플러그인이에요.

| 스킬 | 하는 일 |
|---|---|
| 시즌 마감 검산 (`season-close-check`) | 빠진 회차 · 회차마다 튀는 합계 · 확정 기록과 원자료의 차이를 점검표로 |
| 회차 점검 (`round-review`) | 한 회차를 지난 회차와 견주어 BJ별 증감 · 새로 나온 BJ · 빠진 BJ |
| BJ 지급 요약 (`season-pay-report`) | 확정한 시즌의 BJ별 지급액 보고용 표 |

지급액은 정산을 부탁해 앱이 계산한 값을 그대로 써요. 스킬은 다시 계산해 고치지 않고, 차이는 「확인할 것」으로만 적어요.
후원자 아이디 · 닉네임 · 계좌는 커넥터에서 나오지 않아요.

## 설치

**Claude 앱 (claude.ai · Cowork · 데스크톱)**: Customize › Plugins › Add › Add marketplace 에 `mupozg823/settlement-studio-claude` 를 넣고 「정산을 부탁해」 플러그인을 설치해요.

**Claude Code**

```
/plugin marketplace add mupozg823/settlement-studio-claude
/plugin install settlement-studio@settlement-studio
```

## 커넥터 연결

스킬은 「정산을 부탁해」 커넥터의 읽기 도구를 써요. 연결 방법은 [플러그인 안내](settlement-studio/README.md)에 있어요.
열쇠(비밀값)는 이 저장소에 들어 있지 않아요.
