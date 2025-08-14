# Composer-etf-strat-jfcustomized
# Live Composer strategy using momentum and mean reversion logic
# Overview of logic below


(defsymphony
 "JF Customized Momentum and Mean Reversion"
 {:asset-class "EQUITIES", :rebalance-frequency :daily}
 (weight-equal
  [(if
    (>
     (current-price "SPY")
     (moving-average-price "SPY" {:window 200}))
    [(weight-equal
      [(if
        (> (rsi "TQQQ" {:window 10}) 79)
        [(weight-equal
          [(asset "SHV" )])]
        [(weight-equal
          [(if
            (> (rsi "SPY" {:window 10}) 80)
            [(weight-equal
              [(asset "SHV" )])]
            [(weight-equal
              [(if
                (> (rsi "SPY" {:window 60}) 60)
                [(weight-equal
                  [(filter
                    (rsi {:window 10})
                    (select-top 1)
                    [(asset
                      "IEF")
                     (asset "GLD" )
                     (asset "BSV" )])])]
                     #can add more depending on regime
                [(asset "SSO" )])])])])])])]
    [(weight-equal
      [(if
        (< (rsi "TQQQ" {:window 10}) 31)
        [(asset "TECL")] #can change depending on regime
        [(weight-equal
          [(if
            (>
             (current-price "QQQ")
             (moving-average-price "QQQ" {:window 20}))
            [(weight-equal
              [(if
                (> (cumulative-return "QQQ" {:window 10}) 5.5)
                [(asset "PSQ")]
                [(asset "QQQ")])])]
            [(weight-equal
              [(filter
                (rsi {:window 10})
                (select-top 1)
                [(asset "IEF")
                 (asset "PSQ")
                 (asset"BSV")])])])])])])])])) #can add more depending on regime
