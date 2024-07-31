# gh

Zalig, met simpel commando kan ik alle private repo's uit organization verwijderen 😄


```bash
gh repo list Cowadar --visibility=private | awk '{print $1}' | xargs -n 1 bash -c 'gh repo delete $0 --confirm'
```