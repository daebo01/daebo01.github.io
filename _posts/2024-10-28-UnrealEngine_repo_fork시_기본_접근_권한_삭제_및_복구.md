---
title: UnrealEngine repo fork시 기본 접근 권한 삭제 및 복구
date: 2024-10-28 12:49:00 +0900
categories: []
tags: []
---

![img1](/assets/attachments/2024-10-28-UnrealEngine_repo_fork시_기본_접근_권한_삭제_및_복구/img1.png)

UnrealEngine repo를 개인 계정으로 fork시 EpicGames/developers 팀이 read 권한이 기본으로 부여되어 있습니다  
(org로 fork시에는 org 소속 계정들만 접근 가능)  

private지만 developers 팀에 속해 있는 계정은 접근이 가능합니다  
삭제를 하셔도 되지만 만약 복구하고 싶다면 github api를 호출해야 합니다  

zaproxy로 한다면
```text
PUT https://api.github.com/orgs/EpicGames/teams/Developers/repos/{본인의 계정}/UnrealEngine HTTP/1.1
host: api.github.com
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36
pragma: no-cache
cache-control: no-cache
Accept: application/vnd.github+json
Authorization: Bearer {본인의 github token}
X-GitHub-Api-Version: 2022-11-28
content-length: 21

{"permission":"push"}
```

curl로 한다면
```shell
curl -o - -XPUT https://api.github.com/orgs/EpicGames/teams/Developers/repos/{본인의 계정}/UnrealEngine \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  -H "Authorization: Bearer {본인의 github token}" \
  -d '{"permission":"push"}'
```

미세팁  
github token은 classic token으로 만들고 repo, admin:org 권한을 주시면 됩니다.  
(개인 계정의 fine-grained token은 org쪽 rw가 안됨 org 설정 때문인듯)

api response로 server error가 반환 될 수 도 있습니다  
EpicGames/developers 팀에 계정이 많아서 그런거니 무시하셔도 됩니다
