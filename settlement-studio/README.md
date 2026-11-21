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

## 먼저: 커넥터 연결

이 플러그인에는 열쇠가 들어 있지 않아요(플러그인 파일은 설치하는 모든 사람에게 그대로 전달돼요). 커넥터는 따로 붙여요.

1. 정산을 부탁해 앱 › 비서 › 플러그인 설정 › 외부 AI 도구 연결에서 열쇠를 만들어요. 열쇠는 크루 하나에 묶이고 7 · 30 · 90일 뒤 만료돼요.
2. 연결해요.
   - **Claude 앱(claude.ai · Cowork · 데스크톱)**: 커넥터 화면에서 추가 › 맞춤 커넥터 추가 → 이름 「정산을 부탁해」, 주소 `https://settlement-studio.vercel.app/api/mcp` → 인증 「로그인 없음」 → 요청 머리 `authorization` = `Bearer <열쇠>`. 팀 · 기업 요금제는 조직 소유자가 조직 설정 › 커넥터에서 추가해요. 요청 머리 칸은 Claude 쪽 베타라 안 보일 수 있어요.
   - **Claude Code**: `claude mcp add --transport http settlement https://settlement-studio.vercel.app/api/mcp --header "Authorization: Bearer <열쇠>"`
3. 플러그인을 설치해요. 이 폴더가 든 저장소를 마켓으로 추가(Customize › Plugins › Add › Add marketplace)하거나, Claude Code 에서 `claude --plugin-dir ./settlement-studio` 로 시험해요.

## 나가는 것과 안 나가는 것

- 나가는 것: 크루의 회차 목록 · 회차별 합계 · BJ별 원자료 합계 · 확정한 시즌의 BJ별 지급액.
- 안 나가는 것: 후원자 아이디 · 닉네임 · 계좌 · 예금주.
- 결과 속 BJ · 시즌 · 회차 이름은 사람이 적은 글이에요. 스킬은 이것을 지시가 아니라 자료로만 다뤄요.
- 다 쓴 열쇠는 앱의 「만든 열쇠」 목록에서 폐기해요.
