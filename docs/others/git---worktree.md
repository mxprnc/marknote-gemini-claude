여러 인스턴스에서 같은 디렉터리 내에서 여러 작업을 수행할 경우 git 충돌이 날 가능성이 점점 높아지는데, 여기에 대한 해결책은 git worktree 다. git worktree 는 하나의 저장소에서 여러개의 작업 공간(디렉터리)을 만들어서 사용하는 개념이다.<br/>

git branch 와는 다른 개념인데, git branch 의 경우 checkout 을 해야 하지만, git worktree 는 별도의 디렉터리가 생기기에 checkout 없이 여러 브랜치를 동시에 열어놓고 작업할 수 있다. 핵심은 commit history 나 리모트 설정은 공유하면서 파일 시스템은 완전히 분리된다는 점이다. 수동으로 worktree 를 만들때의 명령어에 비해 claude 의 worktree 생성 명령어는 단순하다.<br/>
<br/>

e.g. 수동으로 worktree 를 만들때
```bash
$ git worktree add ../project-feature-auth

$ cd ../project-feature-auth
```
<br/>

e.g. claude 에서 worktree 생성
```bash
$ claude -w feature-auth
```
<br/>

e.g. gemini 에서 worktree 생성
```bash
$ gemini -w feature-auth
```
<br/>

- [Gemini CLI 공식 문서](https://geminicli.com/docs/cli/git-worktrees/#how-to-use-git-worktrees)에 따르면, `-w` 또는 `--worktree` 플래그를 통해 격리된 작업 세션을 위한 worktree를 생성할 수 있음이 명시되어 있다.
<br/>

