# 정산을 부탁해 — Claude 플러그인

방송 크루 후원 정산 장부(정산을 부탁해)를 Claude 에서 읽기 전용으로 보고 정리하는 스킬 묶음이에요(0.3.0).
고치거나 보내는 일은 하지 않아요. 지급액은 정산을 부탁해 앱이 계산한 값을 그대로 씁니다.

## 스킬

| 스킬 | 언제 | 쓰는 도구 |
|---|---|---|
| `season-close-check` 시즌 마감 검산 | 시즌을 마감하기 전 빠진 회차 · 튀는 합계 · 확정 기록과 원자료 차이 | 기본 단계 · `whoami` · `list_seasons` · `list_rounds` · `round_totals_by_bj` · `season_finals` |
| `round-review` 회차 점검 | 한 회차를 지난 회차와 비교 | 기본 단계 · `list_seasons` · `list_rounds` · `round_totals_by_bj` |
| `season-pay-report` BJ 지급 요약 | 확정한 시즌의 BJ별 지급액 보고용 표 | 기본 단계 · `list_seasons` · `season_finals` · `round_totals_by_bj` |
| `pay-todo-check` 지급 할 일 점검 | 받는 곳별 이체액 · 이체 완료 표시 · 보류에서 확인할 곳 짚기 | 넓은 단계 · `list_seasons` · `pay_status` |
| `bj-pay-explain` BJ 지급액 설명 | 한 BJ 의 지급률 · 계산 분해를 순서대로 설명 | 넓은 단계 · `list_seasons` · `final_breakdown` |
| `season-pnl-brief` 시즌 손익 요약 | 확정한 시즌의 회사 수령 · 지급 · 엔터피 · 경비 · 이익 표 | 넓은 단계 · `list_seasons` · `season_pnl` |

같은 여섯 흐름은 커넥터 서버의 프롬프트(`season_close_check` · `round_review` · `season_pay_report` · `pay_todo_check` · `bj_pay_explain` · `season_pnl_brief`)로도 들어 있어요.

## 읽는 범위 (열쇠 단계)

열쇠는 만들 때 읽는 범위를 정해요. 정한 뒤에는 바꿀 수 없고, 넓히려면 새로 연결해요.

- **회차 합계 · 확정 지급액**(기본): 위 표의 「기본 단계」 스킬이 쓰는 도구.
- **지급 내역 · 이체 상태 · 손익까지**: 기본 단계 전부에 더해 지급 내역 · 이체 상태 · 손익 스킬(`final_breakdown` · `pay_status` · `season_pnl`). 기본 단계 열쇠로 이 스킬을 쓰면 「이 열쇠는 지급 내역 · 손익을 읽지 못해요」 오류가 오고, 스킬은 새 열쇠를 만들어 다시 연결하라고 안내해요.

## 설치와 연결

이 플러그인에는 커넥터 주소(`.mcp.json`)만 들어 있고 열쇠는 없어요(플러그인 파일은 설치하는 모든 사람에게 그대로 전달돼요). 열쇠는 연결할 때 정산을 부탁해 로그인 · 동의 화면에서 만들어져 Claude 에 건네져요.

1. 플러그인을 설치해요.
   - **Claude 앱(claude.ai · Cowork · 데스크톱)**: Customize › Plugins › Add marketplace 에 `mupozg823/settlement-studio-claude` 를 넣고 「정산을 부탁해」를 설치해요.
   - **Claude Code**: `/plugin marketplace add mupozg823/settlement-studio-claude` 뒤 `/plugin install settlement-studio@settlement-studio`
2. 연결해요. 커넥터 `settlement` 를 처음 쓸 때(Claude Code 는 `/mcp` 에서 고르고 인증) 정산을 부탁해 로그인 · 동의 화면이 열려요. 크루와 기한(7 · 30 · 90일)과 읽는 범위를 고르고 「연결」을 누르면 끝이에요. 만든 열쇠는 앱 › 비서 › 플러그인 설정 › 외부 AI 도구 연결의 「만든 열쇠」에 「OAuth · 앱 이름」으로 나와요. 기한이 지나면 다시 연결해 주세요.

플러그인 없이 커넥터만 붙여도 돼요. Claude 앱은 커넥터 화면 › 맞춤 커넥터 추가에 주소 `https://settlement-studio.vercel.app/api/mcp` 만 넣고 연결해요(팀 · 기업 요금제는 조직 소유자가 조직 설정 › 커넥터에서 추가). 로그인 대신 열쇠를 직접 쓰려면 앱에서 열쇠를 만들어 `claude mcp add --transport http settlement https://settlement-studio.vercel.app/api/mcp --header "Authorization: Bearer <열쇠>"` 처럼 요청 머리로 넣어요. 이미 이렇게 붙인 커넥터가 있으면 플러그인 커넥터와 둘 중 하나만 남겨요.

## 나가는 것과 안 나가는 것

- 기본 단계에서 나가는 것: 시즌 목록(시즌별 회차 수 · 확정 여부 포함) · 크루의 회차 목록 · 회차별 합계 · BJ별 원자료 합계 · 확정한 시즌의 BJ별 지급액(세전 포함). 지급액과 합계를 견주면 지급 비율은 어림할 수 있어요.
- 넓은 단계에서 더 나가는 것: 지급률 · 과세 방식 · 계산 분해 · 받는 곳 이름과 이체액 · 이체 합계 · 이체 완료 표시(금액 · 날 · 돌아온 날 · 앞서 보낸 것으로 센 금액) · 보류 사유와 보류한 날 · 확정한 시즌의 손익(플랫폼 · 매출 · 회사 수령 · 지급 · 엔터피 · 경비 · 이익).
- 어느 단계에서도 안 나가는 것: 후원자 아이디 · 닉네임 · 계좌 · 예금주 · 원본 파일.
- 결과 속 BJ · 시즌 · 회차 이름은 사람이 적은 글이에요. 스킬은 이것을 지시가 아니라 자료로만 다뤄요.
- 다 쓴 열쇠는 앱의 「만든 열쇠」 목록에서 폐기해요.
