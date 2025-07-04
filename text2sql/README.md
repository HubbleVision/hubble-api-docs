# Text to SQL API (V1) 文档

Text to SQL API 用于将用户输入的文本转换为 SQL 查询语句，并返回查询结果。

**V1版本不太稳定，会于近期停止更新，建议使用V2版本**

使用时，需要先请求一次获取到唯一的`runId`，然后使用`runId`发起请求。

## 请求1，获取runId

```bash
curl -X 'POST' \
  'https://api.hubble-rpc.xyz/agent/api/workflows/hubbleWorkflow/create-run' \
  -H 'HUBBLE-API-Key: XXXXXXXXXXXX' \
  -H 'Content-Type: application/json'
```

会返回一个`runId`

```json
{
    "runId": "624c05a9-bd2e-4cf2-9561-3f3837f87d4b"
}
```

## 请求2，获取数据

根据上一步的`runId`，发起请求，携带具体的自然语言查询

```bash
curl -X POST "https://api.hubble-rpc.xyz/agent/api/workflows/hubbleWorkflow/start-async" \
  -H "Content-Type: application/json" \
  -H 'HUBBLE-API-Key: XXXXXXXXXXXX' \
  -d '{
    "runId": "624c05a9-bd2e-4cf2-9561-3f3837f87d4b",
    "inputData": {
      "message": "latest 2 txns"
    }
  }'
```

返回的结果格式如下（其中，`result.db_results`是最终返回的查询结果表格数据。）
```json
{
  "status": "success",
  "steps": {
    "input": {
      "message": "latest 2 txns"
    },
    "generateSql": {
      "payload": {
        "message": "latest 2 txns"
      },
      "startedAt": 1751442146615,
      "status": "success",
      "output": {
        "sql_query": "SELECT * FROM hubble.distributed_dex_token_trade_transaction ORDER BY trade_timestamp DESC LIMIT 2",
        "threadId": "thread_1751442146615",
        "resourceId": "user_1751442146615",
        "ragMetrics": {
          "totalDuration": 10899,
          "vectorQueryDuration": 6825,
          "modelInferenceDuration": 0,
          "ragDocuments": 0
        }
      },
      "endedAt": 1751442157515
    },
    "executeSql": {
      "payload": {
        "sql_query": "SELECT * FROM hubble.distributed_dex_token_trade_transaction ORDER BY trade_timestamp DESC LIMIT 2",
        "threadId": "thread_1751442146615",
        "resourceId": "user_1751442146615",
        "ragMetrics": {
          "totalDuration": 10899,
          "vectorQueryDuration": 6825,
          "modelInferenceDuration": 0,
          "ragDocuments": 0
        }
      },
      "startedAt": 1751442157522,
      "status": "success",
      "output": {
        "sql_query": "SELECT * FROM hubble.distributed_dex_token_trade_transaction ORDER BY trade_timestamp DESC LIMIT 2",
        "db_results": [
          {
            "type": "TOKEN_SELL",
            "trade_timestamp": "1751411365",
            "transaction_slot": "350507348",
            "trader_wallet_address": "kwWBe1AmtWqFTLkyc9KTRk1NvHnxjBVdjzw4EnXqYq3",
            "token_mint_address": "6mxz2TtsykCGFZ1XitjCuMNK1JARyzcRqzMR7cBVpump",
            "sol_price": 146.7,
            "signers": [
              "kwWBe1AmtWqFTLkyc9KTRk1NvHnxjBVdjzw4EnXqYq3"
            ],
            "signer_balance_changes": "{\"kwWBe1AmtWqFTLkyc9KTRk1NvHnxjBVdjzw4EnXqYq3\":-0.000243782}",
            "buy_direction": 0,
            "buy_currency": 0,
            "buy_amount": 0,
            "buy_from_account_address": "",
            "buy_to_account_address": "",
            "buy_price_sol": 0,
            "buy_sol_amount": 0,
            "sell_direction": 1,
            "sell_currency": 0,
            "sell_amount": 200000,
            "sell_from_account_address": "9Yj1nfCinybr8d9na3ckiWqGPqehJJjkqzvgRqVY2GbW",
            "sell_to_account_address": "6EoXyRtfbANfR7vWGwxLHyeVomJHdBmQ3e52rGFfvqJz",
            "sell_price_sol": 0.000001374883045,
            "sell_sol_amount": 0.274976609,
            "arbitrage_profit_loss": null,
            "arbitrage_price_change": null,
            "arbitrage_volume": null,
            "pool_state_real_token_reserves_pre_reserves": null,
            "pool_state_real_token_reserves_post_reserves": null,
            "pool_state_real_token_reserves_change": null,
            "pool_state_completion_rate": null,
            "net_sol_balance_change": -0.000243782,
            "transaction_fee": 243782,
            "transaction_signature": "4HBCuQwHgXKZWpnJ9HrDapL4PydM3cuWAgmdYdmLbCA9NwPXsFM6V2FADWB3vLq5n4FYmnLXwaVVV152WfePvFn",
            "reason": "",
            "source_name": "Pump Fun AMM",
            "source_program": "pAMMBay6oceH9fJKBRHGP5D4bD4sWpmSwMn52FMfXEA,pAMMBay6oceH9fJKBRHGP5D4bD4sWpmSwMn52FMfXEA",
            "version": "0"
          },
          {
            "type": "TOKEN_BUY",
            "trade_timestamp": "1751411365",
            "transaction_slot": "350507349",
            "trader_wallet_address": "fLipggLqMEofWHqiZTBR579Bbu2tT7emw4EWS7Md9QG",
            "token_mint_address": "EKpQGSJtjMFqKZ9KQanSqYXRcF8fBopzLHYxdM65zcjm",
            "sol_price": 146.7,
            "signers": [
              "fLipggLqMEofWHqiZTBR579Bbu2tT7emw4EWS7Md9QG"
            ],
            "signer_balance_changes": "{\"fLipggLqMEofWHqiZTBR579Bbu2tT7emw4EWS7Md9QG\":-0.000008804}",
            "buy_direction": 0,
            "buy_currency": 1,
            "buy_amount": 0.000006,
            "buy_from_account_address": "3KdVj9AyxQ8c52LQevDyTsqM8FDvnhn2ts2dQiSLHTgE",
            "buy_to_account_address": "HJG5t97TobX5GjH6HUsgCYjx7iS7Nb8zYoZ9UhMccEXg",
            "buy_price_sol": 0.005666666666666667,
            "buy_sol_amount": 3.4E-8,
            "sell_direction": 1,
            "sell_currency": 0,
            "sell_amount": 0,
            "sell_from_account_address": "",
            "sell_to_account_address": "",
            "sell_price_sol": 0,
            "sell_sol_amount": 0,
            "arbitrage_profit_loss": null,
            "arbitrage_price_change": null,
            "arbitrage_volume": null,
            "pool_state_real_token_reserves_pre_reserves": null,
            "pool_state_real_token_reserves_post_reserves": null,
            "pool_state_real_token_reserves_change": null,
            "pool_state_completion_rate": null,
            "net_sol_balance_change": -0.000008804,
            "transaction_fee": 8770,
            "transaction_signature": "ym9AspULPtfLLpd5upkXFDqZAWX1yLPiij6Si5mjUWBNPapAGbNWcqyirFEvuMxi9TdejBNgXkfv2vdsX5DHRwo",
            "reason": "",
            "source_name": "Raydium Liquidity Pool V4",
            "source_program": "675kPX9MHTjS2zt1qfr1NYHuzeLXfQM9H24wFSUt1Mp8",
            "version": "0"
          }
        ],
        "threadId": "thread_1751442146615",
        "resourceId": "user_1751442146615",
        "performance": {
          "sqlGenerationDuration": 10899,
          "dbQueryDuration": 1241,
          "totalDuration": 12140,
          "resultCount": 2
        }
      },
      "endedAt": 1751442158764
    }
  },
  "result": {
    "sql_query": "SELECT * FROM hubble.distributed_dex_token_trade_transaction ORDER BY trade_timestamp DESC LIMIT 2",
    "db_results": [
      {
        "type": "TOKEN_SELL",
        "trade_timestamp": "1751411365",
        "transaction_slot": "350507348",
        "trader_wallet_address": "kwWBe1AmtWqFTLkyc9KTRk1NvHnxjBVdjzw4EnXqYq3",
        "token_mint_address": "6mxz2TtsykCGFZ1XitjCuMNK1JARyzcRqzMR7cBVpump",
        "sol_price": 146.7,
        "signers": [
          "kwWBe1AmtWqFTLkyc9KTRk1NvHnxjBVdjzw4EnXqYq3"
        ],
        "signer_balance_changes": "{\"kwWBe1AmtWqFTLkyc9KTRk1NvHnxjBVdjzw4EnXqYq3\":-0.000243782}",
        "buy_direction": 0,
        "buy_currency": 0,
        "buy_amount": 0,
        "buy_from_account_address": "",
        "buy_to_account_address": "",
        "buy_price_sol": 0,
        "buy_sol_amount": 0,
        "sell_direction": 1,
        "sell_currency": 0,
        "sell_amount": 200000,
        "sell_from_account_address": "9Yj1nfCinybr8d9na3ckiWqGPqehJJjkqzvgRqVY2GbW",
        "sell_to_account_address": "6EoXyRtfbANfR7vWGwxLHyeVomJHdBmQ3e52rGFfvqJz",
        "sell_price_sol": 0.000001374883045,
        "sell_sol_amount": 0.274976609,
        "arbitrage_profit_loss": null,
        "arbitrage_price_change": null,
        "arbitrage_volume": null,
        "pool_state_real_token_reserves_pre_reserves": null,
        "pool_state_real_token_reserves_post_reserves": null,
        "pool_state_real_token_reserves_change": null,
        "pool_state_completion_rate": null,
        "net_sol_balance_change": -0.000243782,
        "transaction_fee": 243782,
        "transaction_signature": "4HBCuQwHgXKZWpnJ9HrDapL4PydM3cuWAgmdYdmLbCA9NwPXsFM6V2FADWB3vLq5n4FYmnLXwaVVV152WfePvFn",
        "reason": "",
        "source_name": "Pump Fun AMM",
        "source_program": "pAMMBay6oceH9fJKBRHGP5D4bD4sWpmSwMn52FMfXEA,pAMMBay6oceH9fJKBRHGP5D4bD4sWpmSwMn52FMfXEA",
        "version": "0"
      },
      {
        "type": "TOKEN_BUY",
        "trade_timestamp": "1751411365",
        "transaction_slot": "350507349",
        "trader_wallet_address": "fLipggLqMEofWHqiZTBR579Bbu2tT7emw4EWS7Md9QG",
        "token_mint_address": "EKpQGSJtjMFqKZ9KQanSqYXRcF8fBopzLHYxdM65zcjm",
        "sol_price": 146.7,
        "signers": [
          "fLipggLqMEofWHqiZTBR579Bbu2tT7emw4EWS7Md9QG"
        ],
        "signer_balance_changes": "{\"fLipggLqMEofWHqiZTBR579Bbu2tT7emw4EWS7Md9QG\":-0.000008804}",
        "buy_direction": 0,
        "buy_currency": 1,
        "buy_amount": 0.000006,
        "buy_from_account_address": "3KdVj9AyxQ8c52LQevDyTsqM8FDvnhn2ts2dQiSLHTgE",
        "buy_to_account_address": "HJG5t97TobX5GjH6HUsgCYjx7iS7Nb8zYoZ9UhMccEXg",
        "buy_price_sol": 0.005666666666666667,
        "buy_sol_amount": 3.4E-8,
        "sell_direction": 1,
        "sell_currency": 0,
        "sell_amount": 0,
        "sell_from_account_address": "",
        "sell_to_account_address": "",
        "sell_price_sol": 0,
        "sell_sol_amount": 0,
        "arbitrage_profit_loss": null,
        "arbitrage_price_change": null,
        "arbitrage_volume": null,
        "pool_state_real_token_reserves_pre_reserves": null,
        "pool_state_real_token_reserves_post_reserves": null,
        "pool_state_real_token_reserves_change": null,
        "pool_state_completion_rate": null,
        "net_sol_balance_change": -0.000008804,
        "transaction_fee": 8770,
        "transaction_signature": "ym9AspULPtfLLpd5upkXFDqZAWX1yLPiij6Si5mjUWBNPapAGbNWcqyirFEvuMxi9TdejBNgXkfv2vdsX5DHRwo",
        "reason": "",
        "source_name": "Raydium Liquidity Pool V4",
        "source_program": "675kPX9MHTjS2zt1qfr1NYHuzeLXfQM9H24wFSUt1Mp8",
        "version": "0"
      }
    ],
    "threadId": "thread_1751442146615",
    "resourceId": "user_1751442146615",
    "performance": {
      "sqlGenerationDuration": 10899,
      "dbQueryDuration": 1241,
      "totalDuration": 12140,
      "resultCount": 2
    }
  }
}
```