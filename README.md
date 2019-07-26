Relation Network project for VQA
===
# VQA
圖像問答(Visual Question Answering，VQA)是一種包含電腦視覺和自然語言處理的技術。其初期相關技術簡介出現於Mateusz Malinowski及Mario Fritz 於2014年所提出的論文中。其定義如下：將一張圖片和一個關於這張圖片的問題作為輸入，目標為輸出一句符合自然語言且合理的答案。簡單來說，VQA便是給定一張圖片，與電腦進行問答。  
隨著深度學習在電腦視覺和自然語言處理領域中的廣泛應用，如運用卷積神經網絡(CNN)及循環神經網路(RNN)，其強大的特徵學習能力推動了多項研究，尤其在
語音辨識、機器翻譯與文本生成等方面，皆有傑出的研究成果。由於圖像問答技術涵蓋上述兩種領域，常見圖像問答議題的解決方案便是結合CNN和RNN模型來建構
組合式模型。如Agrawal等人提出以CNN 抽取圖像資訊、以LSTM抽取問題所包含的語意資訊，使模型學習到問題的含意和圖片的特徵，最後產生答案。近期許多
關於VQA之研究除了持續嘗試改善圖像與問題的模型，更致力於關注問題本身，如應專注於圖像何處以及找到最適當的問法。  
# Data
**差分**:對兩個中間為連續的0的數值，等分數值差，平均分配差至數值為0的個數。  
**後減前**:第一筆資料為原始資料中(第二筆減第一筆)的資料。  
**return取log**:整份資料的數值先取Log再做相減。  
# Currency
## 真實貨幣
SEK(瑞典克朗), CHF(瑞士法郎), CAD(加拿大元), GBP(英鎊), JPY(日圓), EUR(歐元), AUD(澳洲), SGD(新加坡幣), HKD(港幣), NZD(紐西蘭元), PLN(波蘭茲羅提), TRY(土耳其里拉), NOK(挪威克朗)
## 虛擬貨幣
BTC(比特幣), DASH(達氏幣), ETH(乙太幣), LTC(萊特幣), XMR(門羅幣)  

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


---

2019 Next Term
====

## 0311_1dCNN
(1)資料前處理方式改成單純的**候減前**  
(2)虛實整合，選虛擬真實貨幣各三種  

`0331_相關設定：`  
```
epochs = 20, batch_size = 4096
time length = 60 (5 hours) 
thain_length = 3
test_length = 1
(three months for train and one month for test)
currency = [ BTC, DASH, ETH, JPY, EUR, AUD] 
question = [ trend, volatility]
question_type = [bigger, smaller]
pair by Combination  
```
## MIX_72_2D_vol
改版重點：  
(1)虛實貨幣  
(2)time length = 72  
(3)2dCNN加入交易量  
(4)使用ver2價資料和ver1量資料  

## 0331_RN1D_final
改版重點：  
(1)time length = 144  
(2)使用七種真實貨幣[ SEK, CHF, CAD, GBP, JPY, EUR, AUD]  
  (SEK:瑞典克朗  CHF:瑞士法郎 CAD:加拿大幣 GBP:英鎊 )  
(3)新增Benchmark 前漲猜後漲 前跌猜後跌  
(4)加入記憶體優化  

## 0331_RN2D_final
0331_RN2D_final的**2DCNN**版本  

## 0331_ALL_VIR_1D
0331實驗設定的五種虛擬幣1dCNN版本  

## 0331_ALL_VIR_2D
0331實驗設定的五種虛擬幣2dCNN版本  

## 0403_10Real_1D
10種真實貨幣的1dCNN版本  
新增SGD(新加坡幣) HKD(港幣) NZD(紐西蘭元)  

## 0403_10Real_2D
0403_10Real_1D的2dCNN版本  

## 0407_1DRN_BS1024.   
(1)BS改成1024，跑很久，建議隔夜跑。  
(2)epoch 20 再往後acc會下降  
(3)改用Permutation  
