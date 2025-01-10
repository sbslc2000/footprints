Sentry가 Logs Writer (roles/loggin.logWriter) 권한을 요구하는 것으로 보입니다.

## Solution

GCP → IAM → 위에서 언급한 서비스 계정 → GRANT ACCESS → Logs Writer 할당


[Serverless http google cloud functions not working as documented · Issue #3096 · getsentry/sentry-javascript](https://github.com/getsentry/sentry-javascript/issues/3096)

[node.js - @sentry/serverless module doesn't report Firebase functions unhandled errors - Stack Overflow](https://stackoverflow.com/questions/75235115/sentry-serverless-module-doesnt-report-firebase-functions-unhandled-errors)


![]()
