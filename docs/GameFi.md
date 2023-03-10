# 目錄：

* [GameFi 缺點](#1)
* [Web3.0](#2)
* [代幣編碼](#3)
* [智能合約](#4)
* [穩定幣 (Stablecoin)](#5)
* [Oasys Blockchain for Game](#6)
* [文章收集](#7)

<span id="1"></span>

## :sunny:GameFi 缺點

* 黏著度低，無法長久營運

  GameFi 結合了 Gaming, DeFi 與 NFT的三大議題，現今已發展出眾多項目，但到目前為主，多數遊戲的玩家黏著度仍不高，是因為其打著P2E的特性吸引了較多投機者期望藉由遊戲賺錢而進場，並非真的因為遊戲本身的趣味性而成為用戶，這樣也相對容易在遊戲的經濟體動搖或者是使用者並未賺到其預想的利益時流失用戶。若項目持續流失用戶或無法維持一定數量的新用戶的話，可能使得遊戲的經濟體崩塌。
* 熱錢集中於前幾項目，新興項目無法獲得支持

  而且由於GameFi是新的發展領域，用戶與投資者通常都只會選擇具有完整經濟體系和較成熟的項目，而不願將錢拿去投資小的項目，導致新興項目無法吸引目光，這樣子的斷層就形成GameFi發展的一大問題。因此廠商可以利用更有創意的營銷方式來向用戶與投資者證明他們項目的價值，藉以突破發展的瓶頸。
* 玩家入門門檻高

  現在大多數項目在進入遊戲之前需要先學習如何購買加密貨幣以及使用錢包，有些也需要先行購買NFT才能進入遊戲開始遊玩，相對於一般傳統遊戲只要註冊帳號就可以開始遊戲來說，GameFi的入門門檻較高，也變成無法廣泛發展的其中一個問題。

<span id="2"></span>

## :sunny:Web3.0

* 去中心化可以解決這樣的問題，也就是不用再以信任任何第三方的方式，任何人也都不需要授權就可以參與，並最有趣的是用戶將可以擁有自己數據的“隱私“、”控制“及”所有“。
* 然而，Web3.0 並不是要來取代 Web2.0，而是成為他的擴展。Google Cloud 成立了Web3部門，並非要直接涉入加密貨幣發行，而是提供運行平台讓企業用戶可以開發及執行 Web3.0 應用及NFT；Facebook 和 Instagram 都可以連結自己的錢包，並發 NFT 貼文分享給大家。

<span id="3"></span>

## :sunny:代幣編碼

### ERC-20

一個代幣如果是基於 ERC-20 標準，那它最重要的特性就是代幣間的「同質化」，就好比說你手上的10塊錢硬幣，和我錢包內的10塊錢硬幣一樣，兩者價值完全相同，沒有哪一枚硬幣比較特殊的問題。

ERC-20 同質化代幣具有兩種特性，分別是可替代性以及可分割性：

* 可替代性：
  表示每顆加密貨幣就有同等價值、功能相同，不同用戶手中的以太幣並沒有區別，彼此之間可以任意交換使用，可以使用公定的價格進行交易。
* 可分割性：
  與我們經常使用的法幣不同，法幣購買時的最小單位是1元，加密貨幣雖然使用顆或者枚作為單位，但卻不用以整數進行交易，例如使用以太幣購買NFT時，就能以0.08顆以太幣進行交易，這就是它的可分割性。

### ERC-721

ERC-721旨在創造具有不可替代性以及不可分割性的代幣，也就是大家所熟悉的非同質化代幣–NFT (Non-Fungible Token)。

* 不可替代性：
  每個NFT都具有它的獨特性，獨一無二且無法取代，同樣被儲存在鏈上後，也無法隨便刪除。
* 不可分割性：
  除非智能合約允許，否則NFT是沒辦法像加密貨幣一樣，被拆成更小的份數進行交易。

### ERC-1155

ERC-1155 的用途為再製、包裝或組合一個至多個 Token Type 或 NFT Collection，讓 Token Type 或 NFT Collection 有繼承、多型、封裝等功能。

舉例來說，小明想購買某遊戲的帽子+衣服+褲子+鞋子，以往遊戲只能讓小明一個一個買，一筆交易一筆交易打，有了 ERC-1155 後，遊戲項目方開始可以賣小明一個全身套裝了，四種 NFT 也可以一次性打給小明。

這個協議主要功能是因為區塊鏈容量並非無上限，因此要阻止開發者們無限量開發智能合約，藉由傳統物件導向概念降低智能合約發布數量、提升開發效率。附加效果則是增加使用者體驗，讓使用者擁有套裝、多型態代幣等豐富選擇。

<span id="4"></span>

## :sunny:智能合約

智能合約是一種將雙方的協議條款，並用代碼形式在區塊鏈上運行，儲存在一個公共資料庫中，不能被更改。

1. 智能合約是一種儲存在區塊鏈中的計算機代碼(節點)，它最大的好處是不需要第三方機構的介入，可確保合約的公開透明，交易的記錄運行在區塊鏈上，達到指定條件就會自動執行，無法更改。
2. 智能合約可以用來串聯Dapp (去中心化應用程式)與區塊鏈的橋樑，Dapp和我們常使用的應用程式APP類似，但APP為中心化、Dapp為去中心化程式。
3. 智能合約的需求規格不夠嚴謹時，會造成開發人員誤解需求，而導致程式的執行結果與用戶的預期不符。另外，也有許多法律方面的議題與挑戰要考慮。
4. 智能合約在區塊鏈平台做程式化的資產移轉，而這些資產都是加密貨幣(數位資產)，會必須承擔交易加密貨幣的風險。

![智能合約](/images/%E6%99%BA%E8%83%BD%E5%90%88%E7%B4%84.jpg) 

<span id="5"></span>

## :sunny:穩定幣 (Stablecoin)

* Wiki
	由於依靠算法或權益證明等產生的虛擬貨幣容易受到波動，同時缺乏價值儲存的功能使之無法替代中心化貨幣，因此加密貨幣只被視為投機資產。
	
	穩定幣的核心想法是要打造一種底層分散式帳本，但維持貨幣穩定價值的機制。穩定幣被有些人認為是中心化資產抵押發行的代幣，穩定幣價值是與被抵押資產價值直接連動。
	
* 穩定幣根據其與美元維持穩定的方式主要可分為三類: 
由實體資產作為抵押的法幣抵押資產穩定幣
由加密貨幣資產抵押的加密貨幣抵押穩定幣
利用智能合約與美元價格做錨定的算法穩定幣
	
* 經理人：穩定幣是什麼？價格波動真的較小？從最早發行的穩定幣 USDT 說起
避免價格大幅波動的最佳方法，就是依靠儲備：即為創造出來的每一單位加密貨幣保留一單位的法定貨幣（例如一美元），並保證前者隨時可以兌換成後者。
	
* USDT
泰達幣的功能很簡單，就像它的白皮書所詳述的：客戶向泰達有限公司（Tether Limited，或其姊妹公司泰達國際有限公司）支付一定數額的法定貨幣，以換取等量的代幣（即泰達幣）。換句話說，如果你向泰達幣支付十萬美元，你就會拿到十萬枚泰達幣。 這些代幣可以交換其他的加密貨幣或移轉給其他用戶，也可以換回當初的法定貨幣現金，但需扣除手續費（泰達幣主要的收入來源）。 

2020 年 7 月，紐約上訴法院批准了一項調查泰達公司的穩定幣是否真的和實體美元掛鉤的計畫。自 2019 年底泰達公司調整其服務條款以來，這個問題變得越來越耐人尋味，也就是說，其儲備不只限於貨幣，還可包括「現金等價物」以及其他資產。到 2020 年 9 月，有超過 140 億美元等值的泰達幣在流通；後來該公司承認只有 74% 的代幣是有完全擔保的，但完整的審計查核仍未完成。 

比特幣被定義為開源的、透明的，而泰達幣卻從未經過適當的審計，並且一直未正面回答有關其儲備的問題。比特幣被譽為一種略過作為中間人的金融機構的工具，而泰達公司即是一個中間人，所以在監管機構的要求下，它可以將特定用戶列入黑名單。

<span id="6"></span>

## Oasys Blockchain for Game
官網URL https://www.oasys.games/


<span id="7"></span>

## :sunny:文章收集

* FTX 風投負責人談 Axie、Stepn 與 Gamefi：「我認為目前沒有有趣的區塊鏈遊戲」(2022-05-26)
  https://www.blocktempo.com/interview-with-ftx-ventures-head-amy-about-stepn-axie-and-gamefi/
* Luna幣崩盤沒在怕？日本率先合法「穩定幣」為哪樁？(2022-06-09)  
  https://www.gvm.com.tw/article/90720
* 穩定幣是什麼？價格波動真的較小？從最早發行的穩定幣 USDT 說起  
  https://www.managertoday.com.tw/articles/view/65404?
* 穩定幣是什麼？算法穩定幣是什麼？穩定幣分哪些種類  
  https://bshare.io/stablecoin/stablecoin_1/
* Crypto Tokens vs Coins — What’s the Difference?  
  https://crypto.com/university/crypto-tokens-vs-coins-difference
* 智能合約是什麼意思？有什麼優點與缺點要注意？  
  https://rich01.com/what-is-smart-contract/
* 什麼是 ERC-20、ERC-721、ERC-1155？差別為何？NFT 是哪一種？  
  https://bshare.io/nft/erc20_721_1155/

---

<br>

[返回目錄](/README.md)
