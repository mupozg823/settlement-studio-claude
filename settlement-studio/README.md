# 정산을 부탁해 — Claude 플러그인

방송 크루 후원 정산 장부(정산을 부탁해)를 Claude 에서 읽기 전용으로 보고 정리하는 스킬 묶음이에요.
고치거나 보내는 일은 하지 않아요. 지급액은 정산을 부탁해 앱이 계산한 값을 그대로 씁니다.

## 스킬

| 스킬 | 언제 | 쓰는 도구 |
|---|---|---|
| `season-close-check` 시즌 마감 검산 | 시즌을 마감하기 전 빠진 회차 · 튀는 합계 · 확정 기록과 원자료 차이 | `whoami` · `list_rounds` · `round_totals_by_bj` · `season_finals` |
| `round-review` 회차 점검 | 한 회차를 지난 회차와 비교 | `list_rounds` · `round_totals_by_bj` |
| `season-pay-report` BJ 지급 요약 | 확정한 시즌의 BJ별 지급액 보고용 표 | `season_finals` · `round_totals_by_bj` |

같은 세 흐름은 커넥터 서버의 프롬프트(`season_close_check` · `round_review` · `season_pay_report`)로도 들어 있어요.

## 설치와 연결

이 플러그인에는 커넥터 주소(`.mcp.json`)만 들어 있고 열쇠는 없어요(플러그인 파일은 설치하는 모든 사람에게 그대로 전달돼요). 열쇠는 연결할 때 정산을 부탁해 로그인 · 동의 화면에서 만들어져 Claude 에 건네져요.

1. 플러그인을 설치해요.
   - **Claude 앱(claude.ai · Cowork · 데스크톱)**: Customize › Plugins › Add marketplace 에 `mupozg823/settlement-studio-claude` 를 넣고 「정산을 부탁해」를 설치해요.
   - **Claude Code**: `/plugin marketplace add mupozg823/settlement-studio-claude` 뒤 `/plugin install settlement-studio@settlement-studio`
2. 연결해요. 커넥터 `settlement` 를 처음 쓸 때(Claude Code 는 `/mcp` 에서 고르고 인증) 정산을 부탁해 로그인 · 동의 화면이 열려요. 크루와 기한(7 · 30 · 90일)을 고르고 「연결」을 누르면 끝이에요. 만든 열쇠는 앱 › 비서 › 플러그인 설정 › 외부 AI 도구 연결의 「만든 열쇠」에 「OAuth · 앱 이름」으로 나와요. 기한이 지나면 다시 연결해 주세요.

플러그인 없이 커넥터만 붙여도 돼요. Claude 앱은 커넥터 화면 › 맞춤 커넥터 추가에 주소 `https://settlement-studio.vercel.app/api/mcp` 만 넣고 연결해요(팀 · 기업 요금제는 조직 소유자가 조직 설정 › 커넥터에서 추가). 로그인 대신 열쇠를 직접 쓰려면 앱에서 열쇠를 만들어 `claude mcp add --transport http settlement https://settlement-studio.vercel.app/api/mcp --header "Authorization: Bearer <열쇠>"` 처럼 요청 머리로 넣어요. 이미 이렇게 붙인 커넥터가 있으면 플러그인 커넥터와 둘 중 하나만 남겨요.

## 나가는 것과 안 나가는 것

- 나가는 것: 크루의 회차 목록 · 회차별 합계 · BJ별 원자료 합계 · 확정한 시즌의 BJ별 지급액.
- 안 나가는 것: 후원자 아이디 · 닉네임 · 계좌 · 예금주.
- 결과 속 BJ · 시즌 · 회차 이름은 사람이 적은 글이에요. 스킬은 이것을 지시가 아니라 자료로만 다뤄요.
- 다 쓴 열쇠는 앱의 「만든 열쇠」 목록에서 폐기해요.
