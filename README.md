# 정산을 부탁해 — Claude 플러그인 마켓

[정산을 부탁해](https://settlement-studio.vercel.app)(방송 크루 후원 정산)의 장부를 Claude 에서 **읽기 전용**으로 보고 정리하는 플러그인이에요.

| 스킬 | 하는 일 |
|---|---|
| 시즌 마감 검산 (`season-close-check`) | 빠진 회차 · 회차마다 튀는 합계 · 확정 기록과 원자료의 차이를 점검표로 |
| 회차 점검 (`round-review`) | 한 회차를 지난 회차와 견주어 BJ별 증감 · 새로 나온 BJ · 빠진 BJ |
| BJ 지급 요약 (`season-pay-report`) | 확정한 시즌의 BJ별 지급액 보고용 표 |
| 지급 할 일 점검 (`pay-todo-check`) ※ | 받는 곳별 이체액 · 이체 완료 표시 · 보류를 표로, 확인할 곳을 따로 |
| BJ 지급액 설명 (`bj-pay-explain`) ※ | 한 BJ 의 지급률 · 계산 분해를 순서대로 |
| 시즌 손익 요약 (`season-pnl-brief`) ※ | 확정한 시즌별 회사 수령 · 지급 · 엔터피 · 경비 · 이익 |

※ 표시는 연결할 때 읽는 범위를 「지급 내역 · 이체 상태 · 손익까지」로 고른 열쇠가 필요해요. 기본 범위(회차 합계 · 확정 지급액)로 연결했다면 앱에서 그 범위의 열쇠로 다시 연결해요.

지급액은 정산을 부탁해 앱이 계산한 값을 그대로 써요. 스킬은 다시 계산해 고치지 않고, 차이는 「확인할 것」으로만 적어요.
후원자 아이디 · 닉네임 · 계좌는 커넥터에서 나오지 않아요.

## 설치

**Claude 앱 (claude.ai · Cowork · 데스크톱)**: Customize › Plugins › Add › Add marketplace 에 `mupozg823/settlement-studio-claude` 를 넣고 「정산을 부탁해」 플러그인을 설치해요.

**Claude Code**

```
/plugin marketplace add mupozg823/settlement-studio-claude
/plugin install settlement-studio@settlement-studio
```

## 연결

플러그인을 설치하면 「정산을 부탁해」 커넥터(`.mcp.json`, 주소만)가 같이 들어와요. 처음 쓸 때(Claude Code 는 `/mcp` 에서 인증) 정산을 부탁해 로그인 · 동의 화면에서 크루 · 기한 · 읽는 범위를 고르고 「연결」을 누르면 돼요. 자세한 내용은 [플러그인 안내](settlement-studio/README.md)에 있어요.
열쇠(비밀값)는 이 저장소에 들어 있지 않아요.
