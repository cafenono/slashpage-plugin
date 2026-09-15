# Slashpage MCP Plugin

[English](README.md) | **한국어**

Claude Code와 Codex에서 [Slashpage](https://slashpage.com)의 원격 MCP 서버를 연결하는 플러그인입니다.

- MCP 서버: `https://mcp.slashpage.com/`
- 인증: Slashpage 계정으로 OAuth 로그인
- 마켓플레이스 / 플러그인 이름: `slashpage` / `slashpage`
- 버전: `0.1.2`

플러그인은 연결 설정과 클라이언트가 제공하는 도구를 찾아 호출하는 `slashpage-tools` 스킬을 설치합니다. 사용자는 자신의 계정으로 로그인하고 연결할 사이트를 승인해야 합니다. API 키나 토큰을 파일에 입력할 필요가 없습니다.

## Claude Code 설치

Claude Code 대화창에서 실행합니다.

```text
/plugin marketplace add cafenono/slashpage-plugin
/plugin install slashpage@slashpage
```

Claude Code를 다시 시작한 뒤 `/mcp`에서 Slashpage 서버를 선택하고 로그인합니다. 플러그인 서버는 `plugin:slashpage:slashpage`로 표시될 수 있습니다. 브라우저에서 연결할 사이트를 선택하고 권한을 승인합니다.

## Codex 설치

터미널에서 실행합니다.

```bash
codex plugin marketplace add cafenono/slashpage-plugin
codex plugin add slashpage@slashpage
```

설치 과정에서 인증이 안내되면 로그인하고, 새 Codex 세션의 `/mcp`에서 연결 상태를 확인합니다. 인증이 남아 있으면 플러그인의 연결/로그인 화면에서 완료합니다.

설치 명령은 Codex CLI `0.137.0`의 문법을 기준으로 작성했습니다. `plugin` 하위 명령이 없다면 해당 기능을 지원하는 버전으로 업데이트하세요. `codex plugin add`에는 `slashpage@slashpage`처럼 마켓플레이스 이름도 지정합니다.

`codex mcp login slashpage`는 CLI가 `slashpage`라는 MCP 설정을 찾을 수 있을 때 사용할 수 있습니다. 먼저 `codex mcp list`에서 확인하세요. 플러그인 서버가 목록에 없다면 플러그인의 인증 화면을 이용합니다. 기존에 수동 등록한 `slashpage`의 인증 상태와 플러그인의 인증 상태를 혼동하지 마세요.

## 플러그인 없이 연결

직접 등록을 선호한다면 아래 방식을 사용합니다. 동일한 서버를 플러그인과 수동 설정으로 중복 등록하지 않는 것을 권장합니다.

### Claude Code

터미널에서 등록한 뒤 Claude Code의 `/mcp`에서 로그인합니다.

```bash
claude mcp add --transport http --scope user slashpage https://mcp.slashpage.com/
```

### Codex

```bash
codex mcp add slashpage --url https://mcp.slashpage.com/
codex mcp login slashpage
```

## 연결 확인과 문제 해결

- 로그인 후 연결한 사이트의 정보를 조회하도록 요청해 확인합니다. 플러그인 설치 성공만으로 인증과 도구 호출이 완료된 것은 아닙니다.
- 연결할 사이트가 보이지 않거나 권한 오류가 나면 로그인 계정과 해당 사이트의 관리 권한을 확인하세요.
- 인증을 취소했거나 토큰이 만료되었다면 클라이언트의 연결 화면에서 다시 로그인하세요.
- 서버 URL은 `https://mcp.slashpage.com/`입니다. 뒤에 `/mcp`를 추가하지 않습니다.
- 문제를 신고할 때 클라이언트 버전과 오류 메시지를 남기되, 액세스 토큰이나 개인 사이트 콘텐츠는 포함하지 마세요.

### Unknown tool 오류

`Unknown tool: slashpage.get-domain-info`가 나오면 에이전트에게 포함된 `slashpage-tools` 스킬을 사용해 도구 목록을 다시 조회하고, 클라이언트가 제공한 정확한 이름으로 호출하도록 요청하세요. 화면에 표시되는 이름과 실제 호출 이름은 다를 수 있습니다. 이 오류만으로 로그인 만료라고 판단하지 않습니다.

`0.1.2`는 도구 탐색과 오류 복구 지침을 추가한 버전이며, 클라이언트 내부의 도구 중계 코드를 수정하지는 않습니다. 목록의 정확한 이름으로도 실패하면 클라이언트의 도구 세션을 새로 열고, 클라이언트 버전과 오류를 함께 알려 주세요.

기존 Codex 설치는 다음 명령으로 업데이트합니다.

```bash
codex plugin marketplace upgrade slashpage
codex plugin add slashpage@slashpage
```

업데이트한 스킬을 불러오도록 새 세션을 시작하세요.

## 검증 범위

JSON, 플러그인 매니페스트, 스킬 메타데이터 검증을 통과했습니다. 2026-09-15 기존 인증 계정으로 Codex의 실제 도구 목록에 있는 이름을 사용해 사이트 정보와 채널 목록 조회에 성공했습니다. 점으로 구분한 도구 이름에서는 보고된 오류가 재현됐습니다. 새 사용자 설치와 OAuth 전체 흐름, Claude Code의 실제 도구 호출, 스킬 자동 선택은 전체 검증하지 않았습니다.

2026-09-15 서버 점검에서 OAuth 메타데이터는 조회됐지만, 비인증 응답의 인증 헤더 이름 변경과 메타데이터 URL의 중복 `/`를 관찰했습니다. 클라이언트별 인증 영향은 추가 확인이 필요합니다.

## 저장소 구조

```text
.agents/plugins/marketplace.json         # Codex 마켓플레이스
.claude-plugin/marketplace.json          # Claude Code 마켓플레이스
plugins/slashpage/
  .codex-plugin/plugin.json              # Codex 플러그인
  .claude-plugin/plugin.json             # Claude Code 플러그인
  .mcp.json                             # 공통 MCP 연결 설정
  skills/slashpage-tools/SKILL.md        # 도구 탐색과 오류 복구
```

두 마켓플레이스는 같은 플러그인 폴더를 가리킵니다. `.agents/plugins/marketplace.json`의 `source.path`는 저장소 루트 기준입니다. 이 패키지는 `.codex-plugin`과 `.claude-plugin` 호환 형식을 사용합니다.

### 유지보수

Claude Code가 설치된 환경에서 다음을 실행합니다.

```bash
claude plugin validate --strict .
claude plugin validate --strict ./plugins/slashpage
```

변경 시 두 플러그인 매니페스트의 이름과 버전을 함께 관리하고, MCP 설정 파일이 패키지에 포함되는지 확인합니다. 릴리스 검증은 기존 MCP 설정과 토큰이 없는 별도 테스트 사용자 환경에서 양쪽 클라이언트로 수행합니다.

## 참고

- [Claude Code 마켓플레이스](https://code.claude.com/docs/en/plugin-marketplaces)
- [Claude Code MCP](https://code.claude.com/docs/en/mcp)
- [OpenAI 플러그인 패키징](https://developers.openai.com/plugins/build/plugins)
- [Codex MCP](https://developers.openai.com/codex/mcp)
