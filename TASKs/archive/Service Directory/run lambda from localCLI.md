#### run lambda from localCLI
```
aws lambda invoke --function-name testServiceDirectory --cli-binary-format raw-in-base64-out --payload '{}' output.json
```

```
aws lambda update-function-configuration \
  --function-name testServiceDirectory \
  --environment 'Variables={GCE_URL="https://www.google.com",OTHER_VAR="some_value"}'
```


10.164.0.2/34.13.159.164

```
aws lambda update-function-configuration \
  --function-name testServiceDirectory \
  --environment 'Variables={GCE_URL="https://34.13.159.164",OTHER_VAR="some_value"}'
```