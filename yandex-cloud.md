Для того, чтобы авторизоваться через инструмент `yc` (также там есть информация о том, как получить OAuth-токен): https://yandex.cloud/en/docs/cli/operations/authentication/user

Ссылка, в которой содержатся роли для сервисных аккаунтов: https://yandex.cloud/en/docs/iam/roles-reference

```bash
yc iam key create -o key.json --cloud-id <cloud-id> --folder-id <folder-id> --service-account-id <service-account-id>
```
