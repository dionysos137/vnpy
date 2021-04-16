# Some notes on how vnpy works
## How vnpy strategy execute when each tick is fed in
1. When each tick is sent in, the function `on_tick()`, which was defined in your strategy file, is called.
2.  Now, whatever your strategy is, you have to call bg.update_tick(tick). In other words your `on_tick` function should always like this:
```
def on_tick(self, tick: TickData):
    bg.update_tick(tick)
```
