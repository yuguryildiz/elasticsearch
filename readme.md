
# Elasticsearch Kurulum

Referans: https://www.elastic.co/guide/en/elasticsearch/reference/current/deb.html

`wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg`

`sudo apt-get install apt-transport-https`

`echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list`

Aşağıdaki komut sonrasında kurulum tamamlanacak ve elastic kullanıcısı için default bir şifre verecek. Bunu kaydetmekte fayda var. Kaydedilmezse tekrar sıfırlanabilir.

`sudo apt-get update && sudo apt-get install elasticsearch`

`sudo systemctl daemon-reload`
`sudo systemctl enable elasticsearch`
`sudo systemctl start elasticsearch`

 Aşağıdaki komut ile elasticsearch restart edilebilir.

`sudo systemctl restart elasticsearch`

Start sonrasında hata verirse aşağıdaki komut ile durum kontrol edilebilir.

`sudo systemctl status elasticsearch`

Kurulum tamamlandıktan sonra aşağıdaki komut ile config dosyası açılıp path.data bölümü aşağıdaki şekilde set edilmelidir.

`chmod -R 777 /data`
`path.data: /data/elasticsearch`

Clustured kurulumu için master node (Örnek: node-1) üzerinde aşağıdaki komut çalıştırılarak token çıktısı alınır.

`/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s node`

Yeni eklenecek sunucu (Örnek: node-2) üzerinde aşağıdaki komut çalıştırılır.

`/usr/share/elasticsearch/bin/elasticsearch-reconfigure-node --enrollment-token <enrollment-token>`

Node-2 üzerinde aşağıdaki config düzenlenir.

`discovery.seed_hosts: ["<node-1-ip>:9300"]`

Tüm sunucularda aşağıdaki comment'li alanlar aktif edilir.

`transport.host: 0.0.0.0`

Daha sonrasında tüm node'lar restart edilir.

`sudo systemctl restart elasticsearch`
