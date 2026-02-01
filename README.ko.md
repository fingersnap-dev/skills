# Agent Skills

[English](README.md) | 한국어

Agent Skills는 AI 에이전트가 특정 작업을 수행하기 위해 발견하고 사용할 수 있는 지침, 스크립트, 리소스 폴더입니다. 한 번 작성하면 어디서든 사용할 수 있습니다.

이 레포지토리는 내부적으로 사용하던 프롬프트 템플릿을 스킬로 만들어 오픈소스로 공개한 것입니다.

## Agent Skills 소개

스킬은 AI 에이전트가 특정 작업을 반복 가능한 방식으로 완료하는 방법을 알려줍니다. 각 스킬은 에이전트가 사용하는 지침과 메타데이터가 포함된 `SKILL.md` 파일과 함께 자체 폴더에 독립적으로 존재합니다.

Agent Skills 표준에 대해 더 알아보기: [agentskills.io](https://agentskills.io)

## 사용 가능한 스킬

| 스킬 | 설명 |
|------|------|
| [git-commit-message](skills/git-commit-message) | 커밋되지 않은 변경 사항을 분석하고 Conventional Commits 형식의 영어 커밋 메시지를 제안합니다 |

## 설치 방법

### Codex

GitHub URL을 사용하여 `$skill-installer`로 스킬을 설치할 수 있습니다:

```
$skill-installer install https://github.com/fingersnap-dev/skills/tree/main/skills/git-commit-message
```

> 스킬 설치 후 Codex를 재시작하여 새 스킬을 적용하세요.

### Claude Code

마켓플레이스를 추가하고 플러그인을 설치하세요:

```bash
/plugin marketplace add fingersnap-dev/skills
/plugin install git-commit-message@fingersnap-skills
```

설치 후 프롬프트에서 스킬을 언급하여 사용할 수 있습니다:

> "git-commit-message 스킬을 사용해서 현재 변경 사항에 대한 커밋 메시지를 추천해줘"

## 라이선스

각 스킬의 라이선스는 해당 디렉토리 내 `LICENSE.txt` 파일에서 확인할 수 있습니다. 이 레포지토리의 대부분의 스킬은 MIT 라이선스로 배포됩니다.

## 기여하기

기여를 환영합니다! 새로운 스킬이나 기존 스킬 개선에 대한 Pull Request를 자유롭게 제출해 주세요.
