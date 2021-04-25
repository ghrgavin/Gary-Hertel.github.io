---
title: "一个示例策略"
date: 2021-04-26T03:49:20+08:00
draft: false
---

这是一个简单的示例策略：

+ main.py

```python
from quant.entrance import *         # 导入模块


class Strategy:

    def __init__(self):
        """ 类初始化
        """
        platform = const.BINANCESWAP        # 交易平台
        symbol = "BTC-USDT-SWAP"            # 交易币对

        self.rest_api = RestApi(platform=platform, symbol=symbol)

        Event(
            platform=platform,
            channels=["orderbook"],
            symbols=[symbol],
            orderbook_update_callback=self.on_event_orderbook_update_callback,   # 订单簿数据更新回调函数
            kline_update_callback=self.on_event_kline_update_callback,           # k线数据更新回调函数
            trade_update_callback=self.on_event_trade_update_callback,           # 逐笔成交数据更新回调函数
            order_update_callback=self.on_event_order_update_callback,           # 订单数据更新回调函数
            position_update_callback=self.on_event_position_update_callback,     # 持仓数据更新回调函数
            asset_update_callback=self.on_event_asset_update_callback,           # 资产数据更新回调函数
        )

        task_id = Quant.create_loop_task(3, self.get_open_orders)
        Quant.call_later(10, Quant.cancel_loop_task, task_id)

    def get_open_orders(self):
        """查询当前未成交委托"""
        success, error = self.rest_api.get_open_orders()
        logger.info("success:", success, caller=self)
        logger.info("error:", error, caller=self)

    @method_locker("on_event_orderbook_update_callback.lock", wait=False)
    def on_event_orderbook_update_callback(self, orderbook: Orderbook):
        """订单簿更新"""
        logger.debug("orderbook:", orderbook, caller=self)

    @method_locker("on_event_kline_update_callback", wait=False)
    def on_event_kline_update_callback(self, kline: Kline):
        """k线更新"""
        logger.debug("kline:", kline, caller=self)

    @method_locker("on_event_trade_update_callback", wait=False)
    def on_event_trade_update_callback(self, trade: Trade):
        """成交数据更新"""
        logger.debug("trade:", trade, caller=self)

    @method_locker("on_event_order_update_callback.lock", wait=True)
    def on_event_order_update_callback(self, order: Order):
        """订单更新"""
        logger.debug("order:", order, caller=self)

    @method_locker("on_event_position_update_callback", wait=True)
    def on_event_position_update_callback(self, position: Position):
        """持仓更新"""
        logger.debug("position:", position, caller=self)

    @method_locker("on_event_asset_update_callback", wait=True)
    def on_event_asset_update_callback(self, asset: Asset):
        """资产更新"""
        logger.debug("asset:", asset, caller=self)


if __name__ == '__main__':

    Quant.start("config.json", Strategy)
```



+ config.json

```json
{
    "LOG": {
        "level": "debug",
        "console": true,
        "backup_count": 10,
        "clear": true
    },
    "RABBITMQ": {
        "host": "101.36.117.251",
        "user": "admin",
        "password": "123456"
    },
    "PLATFORMS": {
        "binance": {
            "access_key": "rlEDsLykoyrEoADBqgKNfSOATulhZrVCwobsNNsUEBn6LrU8gYPfZk",
            "secret_key": "lfZC929N4CkkyrMwusldVqEoNixREu0X4C9xcgCutpkOVnox9Lh"
        },
        "huobi": {
            "access_key": "6011689b-mk0e-e093da77-3db5e",
            "secret_key": "f5dd82f1-9-9d1758f5-7d023"
        },
        "okex": {
            "access_key": "1fcd5fc121-4795-b52731ffb9d2a8",
            "secret_key": "C40BC4AFEBABAA1F9070E00E089AAC",
            "passphrase": "123456"
        }
    },
    "PROXY": "127.0.0.1:9999",
    "DINGTALK": "https://oapi.dingtalk.com/robot/send?access_token=d3a368f823cccca302e51bd79d74b453"
}
```



+ requirements.txt

```tex
requests==2.25.1
websocket-client==0.57.0
pika==1.2.0
pymongo==3.11.3
```

