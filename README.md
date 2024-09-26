# Cfn_securitystack_sample

Cfn のネステッドスタックとクロススタック参照のサンプル（security）

CodePipeline による CI/CD を実施するためのファイルと GitHub Actions による Linting 用の workflow も含む。

- param/parameters-cli.json 手動デプロイ用の設定ファイル
- param/parameters-pipeline.json Pipeline 用の設定ファイル

## 手動準備

templates/にあるyamlはあらかじめ S3 の cfn-nested-sample/security に置いておく。

```bash
aws s3 cp ./templates/security-group.yaml s3://cfn-nested-sample/security/
aws s3 cp ./templates/iam-role.yaml s3://cfn-nested-sample/security/
aws s3 cp ./templates/iam-policy.yaml s3://cfn-nested-sample/security/
aws s3 cp ./templates/ec2keypair.yaml s3://cfn-nested-sample/security/
```

## 手動デプロイ

```bash
aws cloudformation create-stack \
  --stack-name SecurityStack \
  --template-body file://root-template.yaml \
  --parameters file://param/parameters-cli.json \
  --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND
```

## 削除

```bash
aws cloudformation delete-stack --stack-name SecurityStack
```

## Output

| キー                | 説明                          | エクスポート名                    |
| ------------------- | ----------------------------- | --------------------------------- |
| Ec2KeyPairName      | The Ec2 Key Pair Name         | SecurityStack-Ec2KeyPairName      |
| IAMPolicyArn        | The ARN of the managed policy | SecurityStack-IAMPolicyArn        |
| IAMRoleArn          | The ARN of the IAM role       | SecurityStack-IAMRoleArn          |
| IAMRoleName         | The Name of the IAM role      | SecurityStack-RoleName            |
| InstanceProfileName | The Name of the InstanceProfile name      | SecurityStack-InstanceProfileName |
| SecurityGroupId     | The ID of the security group  | SecurityStack-SecurityGroupId     |
