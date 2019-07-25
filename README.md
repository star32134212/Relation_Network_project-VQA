Relation Network project for VQA
===
# VQA
# Data
# Currency
# Last Term

## 1221

使用**虛擬貨幣_data_return_diff**data
vqa_pair的部分寫入迴圈，可以rolling兩年資料而不會有記憶體不夠的問題(採用分段讀取)  
`1221_相關設定：`  
```
epochs = 400, batch_size = 4096
time length = 36 (3 hours)
thain_length = 3
test_length = 1
(three months for train and one month for test)
currency = [ BTC, DASH, ETH, LTC, XMR] 
question = [ trend, volatility]
question_type = [bigger, same, smaller]
pair by Combination  
```
  
## 1221 ver2
改版重點：  
(1)解決rolling第二圈index對錯的bug。  
(2)印出每次rolling最後一個epoch的acc走勢圖。  
(3)將網路參數量調得與baseline差不多。  

## 1221 ver2_vol
同時考慮每種貨幣的五分鐘交易量，直接concatenate在V後面一起跑。  
```
Train_data=np.zeros(((n-l+1,len(currency)*2,l)))
for p in range(n-l+1):
    Train_data[p,0,:]=btc5months[p:p+l]
    Train_data[p,1,:]=dash5months[p:p+l]
    Train_data[p,2,:]=eth5months[p:p+l]
    Train_data[p,3,:]=ltc5months[p:p+l]
    Train_data[p,4,:]=xmr5months[p:p+l]
    Train_data[p,5,:]=btcmonthsvol[p:p+l]
    Train_data[p,6,:]=DASHmonthsvol[p:p+l]
    Train_data[p,7,:]=ETHmonthsvol[p:p+l]
    Train_data[p,8,:]=LTCmonthsvol[p:p+l]
    Train_data[p,9,:]=XMRmonthsvol[p:p+l] 
```
## 1226vol
(1)把問題減少成1種 (只問larger)  
(2)時間長度改成72 (6 hours)  
`1226_相關設定：`  
```
epochs = 400, batch_size = 4096
time length = 72 (6 hours) 
thain_length = 3
test_length = 1
(three months for train and one month for test)
currency = [ BTC, DASH, ETH, LTC, XMR] 
question = [ trend, volatility]
question_type = [bigger]
pair by Combination  
```

## 1226
與1226vol的實驗設定相同，但不加交易量。  

## 1229oneD
改版重點：  
(1)從C改成P  
(2)問題問[‘bigger’,’smaller’]，big判斷為大於等於，因此答案理論上還是互補  
`1226_相關設定：`  
```
epochs = 400, batch_size = 4096
time length = 72 (6 hours) 
thain_length = 3
test_length = 1
(three months for train and one month for test)
currency = [ BTC, DASH, ETH, LTC, XMR] 
question = [ trend, volatility]
question_type = [bigger, smaller]
pair by Permutation
```

## 1229twoD
1229oneD的**2DCNN**版本  

## 0118twoD
改版重點：  
(1)為了加快實驗進度，只rolling 2017年一整年的資料。  
(2)maxpooling改用(1,5)來加速。  
(3)test用predict做，並且將vol和trend分開，發現vol的acc高，trend的acc低，每次rolling的train和test算出的acc都有記錄下來。  
(4)檢查互補問題是否合理--->目前一切正常。  
`0118_相關設定：`  
```
epochs = 100, batch_size = 4096
time length = 60 (5 hours) 
thain_length = 3
test_length = 1
(three months for train and one month for test)
currency = [ BTC, DASH, ETH, LTC, XMR] 
question = [ trend, volatility]
question_type = [bigger, smaller]
pair by Permutation
```

## 0118oneD
0118twoD的**1DCNN**版本  

2019 Next Term
====

## 0312_1D_RN_MIX6
使用新資料前處理 虛擬真實貨幣各三
currency = ["BTC","DASH","ETH","JPY","EUR","AUD"]

## MIX_72_2D_vol
虛實貨幣 時間長度72 2D加交易量
使用ver2價資料和ver1量資料

## 0331_RN1D_final
時間拉長144
使用七種虛擬貨幣["SEK","CHF","CAD","GBP","JPY","EUR","AUD"](SEK:瑞典克朗  CHF:瑞士法郎 CAD:加拿大幣 GBP:英鎊 )

新增Benchmark 前漲猜後漲 前跌猜後跌
記憶體優化

## 0331_RN2D_final
同上 2D版本

## 0331_ALL_VIR_1D
全部都是虛擬貨幣(五種)
1D版本
其餘同上

## 0331_ALL_VIR_2D
同上 2D版

## 0403_10Real_1D
10種虛擬貨幣 1D
新增SGD(新加坡幣) HKD(港幣) NZD(紐西蘭元)

## 0403_10Real_2D
同上 2D版

## 0407_1DRN_BS1024.     (7/25晚上跑)
再優化
BS改成1024 跑很久
epoch 20 再往後acc會下降
改用P(排列)
