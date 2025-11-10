level = error and message : "504"

json_payload.request_id : "64b0a790-a927-4841-916f-f8abfa65e563"

Profile

User

Service account

Federated user

yc init

yc config list # see ccurrent project fodler-id and cloud-id

yc config set cloud-id 

yc config set folder-id 

yc dns zone list --folder-id <NEW_FOLDER_ID>

terraform import yandex_dns_zone.main dnsaha4s8g5e5p7q4c1a

terraform state list

terraform state rm yandex_dns_zone.main

yc iam create-token

root@development:/# yc init

Welcome! This command will take you through the configuration process.

Pick desired action:

 [1] Re-initialize this profile 'default' with new settings

 [2] Create a new profile

Please enter your numeric choice: 1

Please go to [https://oauth.yandex.ru/authorize?response_type=token&client_id=1a6990aa636648e9b2ef855fa7bec2fb](https://oauth.yandex.ru/authorize?response_type=token&client_id=1a6990aa636648e9b2ef855fa7bec2fb) in order to obtain OAuth token.

Please enter OAuth token: <YOUR_OAUTH_TOKEN>

You have one cloud available: 'cloud-48759' (id = <CLOUD_ID>). It is going to be used by default.

Please choose folder to use:

 [1] base-catalog (id = <FOLDER_ID>)

 [2] Create a new folder

Please enter your numeric choice: 1

Your current folder has been set to 'base-catalog' (id = <FOLDER_ID>).

Do you want to configure a default Compute zone? [Y/n] n

token: <IAM_TOKEN>

cloud-id: <CLOUD_ID>

folder-id: <FOLDER_ID> 

Create token

[https://cloud.yandex.com/en/docs/iam/operations/iam-token/create-for-sa#via-cli](https://cloud.yandex.com/en/docs/iam/operations/iam-token/create-for-sa#via-cli)



Static access key

<STATIC_ACCESS_KEY_ID>
<STATIC_SECRET_ACCESS_KEY>



API Key

<API_KEY>



Public Key


- [Authorized keys](https://cloud.yandex.com/en/docs/iam/concepts/authorization/key) — keys used to [get an IAM token](https://cloud.yandex.com/en/docs/iam/operations/iam-token/create-for-sa).
- [API keys](https://cloud.yandex.com/en/docs/iam/concepts/authorization/api-key) — keys used in some services for simplified authentication instead of IAM tokens.
- [Static access keys](https://cloud.yandex.com/en/docs/iam/concepts/authorization/access-key) — keys used in services with AWS-compatible APIs.