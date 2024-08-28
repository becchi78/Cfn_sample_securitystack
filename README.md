# Cfn_sample_securitystack

Cfn のネステッドスタックとクロススタック参照のサンプル（security）

## 準備

templates/にある security-group.yaml と iam-role.yaml、iam-policy はあらかじめ S3 の cfn-nested-sample に置いておく。

```bash
aws s3 cp ./templates/security-group.yaml s3://cfn-nested-sample/security/
aws s3 cp ./templates/iam-role.yaml s3://cfn-nested-sample/security/
aws s3 cp ./templates/iam-policy.yaml s3://cfn-nested-sample/security/
```

## デプロイ

```bash
aws cloudformation create-stack \
  --stack-name SecurityStack \
  --template-body file://root-template.yaml \
  --parameters file://param/parameters.json \
  --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND

```

## 削除

```bash
aws cloudformation delete-stack --stack-name SecurityStack
```

## 問題点

ingress や egress のルールを外部の json ファイルに記述して読み込ませることで、template の再利用性を高めようとしたがどうしても解決できず。
以下のようにしても`Transform AWS::Include failed with: The specified S3 object's content should be valid Yaml/JSON`というエラーが出てダメだった

```yaml
SecurityGroupIngress:
  Fn::Transform:
    Name: AWS::Include
    Parameters:
      Location: !Ref IngressFileURL
```
