# 使用示例：Get top 10 tokens by volume

第一步，发送请求，注意一定要携带HUBBLE-API-Key
```bash
curl -X POST "https://api.hubble-rpc.xyz/agent/api/v1/text2sql" \
  -H "Content-Type: application/json" \
  -H 'HUBBLE-API-Key: XXXXXXXXXXXX' \
  -d '{
    "query": "Show me the top 10 token trades by volume today"
  }'
```

请求发出后，得到流式返回，详见[streams.txt](streams.txt)。

查看文件内容，可以看到返回的是Server-Sent Events (SSE)格式的数据流，每个事件以"data:"开头。

根据文件内容，可以看到以下事件类型和数据结构：

首先是`workflow_start`事件：
```json
{
  "type": "workflow_start",
  "message": "Starting Text2SQL agent",
  "query": "Show me the top 10 token trades by volume today",
  "timestamp": "2025-07-04T04:56:04.937Z"
}
```
然后是`llm_start`事件，表示LLM生成开始：
```json
{
  "type": "llm_start",
  "node": "step-0",
  "step": 0,
  "message": "LLM generation started",
  "runId": "0127c99b-dee0-4e79-9c9d-5bc0c691b692",
  "timestamp": "2025-07-04T04:56:04.946Z"
}
```
接下来是多个`token`事件，这些是LLM生成的实时令牌。如果我们将所有token事件中的token字段拼接起来，可以看到LLM正在生成一个JSON对象，内容大致为：
```json
{
  "tasks": [
    "[Query] Get the top 10 token trades by volume today from the DEX table, ordered by volume in descending order and limited to 10 results."
  ],
  "parallel": [[0]]
}
```

然后是SQL生成阶段，系统生成了如下SQL查询：
```sql
SELECT token_mint_address, SUM(COALESCE(buy_sol_amount, 0) + COALESCE(sell_sol_amount, 0)) AS total_volume FROM hubble.distributed_dex_token_trade_transaction WHERE toDate(toDateTime(trade_timestamp)) = today() GROUP BY token_mint_address ORDER BY total_volume DESC LIMIT 10
```

最后返回执行结果：
```json
{
  "type": "result",
  "data": {
    "isComplete": true,
    "finalResponse": "✅ Query Executed Successfully\n\n**Query:** Show me the top 10 token trades by volume today\n**Executed At:** 7/4/2025, 5:18:35 AM\n**Rows Returned:** 10\n\n**Generated SQL:**\n```sql\nSELECT token_mint_address, SUM(COALESCE(buy_sol_amount, 0) + COALESCE(sell_sol_amount, 0)) AS total_volume FROM hubble.distributed_dex_token_trade_transaction WHERE toDate(toDateTime(trade_timestamp)) = today() GROUP BY token_mint_address ORDER BY total_volume DESC LIMIT 10\n```\n\n**Query Results:**\n| token_mint_address | total_volume |\n| --- | --- |\n| 3L96hBnqQq8gdYdkRevHYNxtmeZmgcBRSQp2g5EdnCF7 | 240832.56350999078 |\n| EDMuFEdHYihS6gn8BtLEDZSs4v2xukeQQ8YXYQHUPump | 213778.08038039348 |\n| BW2F8mKmLRuGRZ4Hp5DVsyVL7yujvS5iGoB8Szi7Pump | 201941.751430432 |\n| 4rkCTQbLn26bYYDxHnjJ4LzVSxn5gyvDa4sqzg5Vpump | 177171.26933865214 |\n| 578sbQ3gkrRSxCc9FHVa6myyYYztK8Djp7JxyWoHeGoW | 164138.71412292877 |\n| EEMjxJekErkwGMbQRmUttVrzsR6HaGBn9CTaQk1TPUMP | 160700.45606376187 |\n| 9cxcHc7ad8BDxc9Pn5iNMbdkfHkh8psmpNw2YfGHp4E9 | 157419.52292925052 |\n| D1chmum7xJ4CpoFDdvsmzndG2Pq2JDzaBMWASfxVpump | 153014.6836715561 |\n| DNxC6WpRAeKhSUtvDzx772gPAwb9uv2cEg46ETT7PUMP | 146993.84736804076 |\n| 52cJZgS1xyjXVGmA6iBpJQdqwgvyFziDKqFWf7ducTDL | 144776.71564573617 |\n\n\n✨ This query was completed directly as a simple data retrieval task."
  },
  "totalSteps": 4,
  "timestamp": "2025-07-04T05:18:35.486Z"
}
```