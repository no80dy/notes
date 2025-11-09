Для того, чтобы авторизоваться через инструмент `yc` (также там есть информация о том, как получить OAuth-токен): https://yandex.cloud/en/docs/cli/operations/authentication/user

```bash
yc iam key create -o key.json --cloud-id <cloud-id> --folder-id <folder-id> --service-account-id <service-account-id>
```
