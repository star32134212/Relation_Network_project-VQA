# Relation Network project for VQA

2018
====

1221
使用虛擬貨幣_data_return_diff資料
將建vqa_pair的部分一起納入迴圈 可以rolling兩年資料而不會有記憶體不夠的問題
重新對過index 設定epochs=400 batch_size=4096 以3個月train和1個月test

1221 ver2
解決rolling第二圈index對錯的bug
印出每次rolling最後一個epoch的acc走勢圖
將網路參數調得與baseline差不多

1221 ver2_vol
1221ver2加進vol一起跑


1226vol
有加量
把問題減少成1種(只問big)
時間長度改成72
train_length代表拿多少個月train 
test_length代表拿多少個月test

1226
同1226vol 但不加量

1229oneD
1.從C改成P
2.網路新架構
3.問題問[‘big’,’small’] big判斷為大於等於 因此答案理論上還是互補

1229twoD
2dCNN版本

0118twoD
1.區間用60 epochs=100 batch_size=4096 用P取 rolling 2017年一整年
2.maxpooling改用(1,5)來加速
3.test用predict做 並且vol trend分開 (vol高 trend低) train和test的acc都有記錄下來
4.檢查互補問題是否合理--->目前一切正常

0118oneD
同0118twoD  oneD version







2019
====

0312_1D_RN_MIX6
使用新資料前處理 虛擬真實貨幣各三
currency = ["BTC","DASH","ETH","JPY","EUR","AUD"]

MIX_72_2D_vol
虛實貨幣 時間長度72 2D加交易量
使用ver2價資料和ver1量資料

0331_RN1D_final
時間拉長144
使用七種虛擬貨幣["SEK","CHF","CAD","GBP","JPY","EUR","AUD"](SEK:瑞典克朗  CHF:瑞士法郎 CAD:加拿大幣 GBP:英鎊 )

新增Benchmark 前漲猜後漲 前跌猜後跌
記憶體優化

0331_RN2D_final
同上 2D版本

0331_ALL_VIR_1D
全部都是虛擬貨幣(五種)
1D版本
其餘同上

0331_ALL_VIR_2D
同上 2D版

0403_10Real_1D
10種虛擬貨幣 1D
新增SGD(新加坡幣) HKD(港幣) NZD(紐西蘭元)

0403_10Real_2D
同上 2D版

0407_1DRN_BS1024.     (7/25晚上跑)
再優化
BS改成1024 跑很久
epoch 20 再往後acc會下降
改用P(排列)
