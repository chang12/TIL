```python
aws_access_key_id = "<AWS_ACCESS_KEY_ID>"  # From AWS IAM
aws_secret_access_key = "<AWS_SECRET_ACCESS_KEY>"  # From AWS IAM

s3 = boto3.resource("s3", aws_access_key_id=aws_access_key_id, aws_secret_access_key=aws_secret_access_key)
bucket = s3.Bucket("my-bucket-name")
data = open("test.txt", "rb")
bucket.put_object(Key="test.txt", Body=data)
```

`aws_access_key_id`, `aws_secret_access_key` 는 **AWS IAM** 에서 **security credential** 을 만들어 획득한다. 이때 `Policy` 를 생성해서 `User` 에게 부여하게 되는데 S3 Bucket 의 `ARN` 을 입력할때 bucket 이름뿐 아니라 하위 경로들도 `/*` 로 포함시켜줘야 함에 유의하자.