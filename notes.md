# Some notes on how vnpy works
## How vnpy strategy executes when each tick is fed in

A barGenerator object is initiated with 4 argument, `on_bar: Callable`, `window: Int`, `on_window_bar: Callable`, `interval: Interval`.
That is, we need two functions, one integer, and one Interval object. 

`on_bar()` is where the 1-min bar will be sent to after its production via repeated call of the `update_tick` method. Therefore, in `on_bar()` you should define what the strategy should do for each newly generated 1-min bar.

By default, the last three arguments are turned off. However, when your strategy is based on longer period like 5min bar or 2h bar, you need to provide the logic in `on_window_bar()`. And you need specify the `window` and `interval` so that the generator is able to synthesize the correct bar and send it to `on_window_bar()`.  

In summary, for a strategy based on x-min bar, you should have the following functions:
```
    def __int__(self.cta_engine, strategy_name, vt_symbols, setting):
        super().__init(self.cta_engine, strategy_name, vt_symbols, setting):
        self.bg = barGenerator(self.on_bar, x, on_window_bar, Interval.minute)
        # ... other things you need
     
    # ... other functions
    
    # you always need this since it is the tick info that is fed in
    def on_tick(self, tick: TickData):
         bg.update_tick(tick)
    
    # you always need this to tell what the strategy needs to do with 1-min bar
    def on_bar(self, bar: BarData):
         bg.update_bar(bar)
    
    def on_window_bar(self, bar: BarData):
         # your strategy here
         
    # ...other functions
    
```

Notice that if the `on_bar()` function is removed and `update_bar()` is called automatically in `update_tick()`, then the user won't need to define it by himself. This way no matter what is the window value, be it 1 or not, the code structures on the user side are the same. This separated dealing with 1-min bar and x-min bar is confusing and unneccessary.




