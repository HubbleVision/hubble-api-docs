# Hubble API 文档

欢迎查看 Hubble API 文档。

## API_KEY

请求<https://api.hubble-rpc.xyz>时，需要在请求头中添加`HUBBLE-API-Key`字段，值为您的`API_KEY`。

示例
```bash
curl -X 'POST' \
  'https://api.hubble-rpc.xyz/agent/api/workflows/hubbleWorkflow/create-run' \
  -H 'accept: */*' \
  -H 'HUBBLE-API-Key: XXXXXXXXXXXXXX' \
  -H 'Content-Type: application/json'
```

## 具体业务API接口详情

- [Text to SQL API 说明](text2sql/README.md)

