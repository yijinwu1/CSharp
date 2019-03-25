<h1><B>Session、Cookie</B></h1>

在 HTTP 通訊協定中，並沒有設計保存連線狀態的能力(稱為Stateless Connection)。</br>
對Server而言，每次送出網頁都是重新建立連線。</br>
每個 HTTP 連線之間都是獨立互不相干，資訊無法給下網頁使用。</br>
Cookie與Session為解決此一問題而來。</br>

<h2><B>Cookie運作原理</B></h2>

![1](../master/images/sk1.JPG)

Cookie儲存Clinet端。
第一回連線時，Cookie未產生。</br>
Browser支援Cookie，且Clinet端User未關閉此功能，Browser會依程式要求，存在Clinet端電腦上。</br>
User不將之刪除，資料即可保留一段時間，不因關閉瀏覽器而消失，同一網站的各網頁也可共享資料，有安全性問題。</br>
Browser限制Cookie最大不超過4096Bytes，適合存放少量資料。</br>
建立Cookie後，只有同一Browser、同一網站、同一目錄與子目錄下的網頁，才可以存取該Cookie。</br>

<h2><B>Session運作原理</B></h2>

![1](../master/images/sk2.JPG)

Session儲存Server端。</br>
User開啟Browser連線網站，直到關閉Browser為止的工作期間。</br>
Session ID會存放在沒有到期日的Cookie內，其他資料存在Server上。</br>
同一個網站的相同與不同網頁之間可藉由Cookie掌握同一Session ID，存取同一份資料。</br>
Session是隨著瀏覽器的開啟與關閉而生滅的機制，將資料記錄在Server。</br>

<h2><B>Session與Cookie</B></h2>

![1](../master/images/sk3.JPG)


![1](../master/images/sk4.JPG)
