# Walrus

 Paketler

    sudo apt-get update
    apt install cargo
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh 
    
  Sui Clinet Yüklememiz gerekiyor
  
    cargo install --locked --git https://github.com/MystenLabs/sui.git --branch testnet sui
    rustup update stable
    sui client new-env --alias testnet --rpc https://fullnode.testnet.sui.io:443
  yazıyoruz. YES sonrasında bir isim girmemizi istiyor. Kelimleri kayıt ediniz.
   
    sui client switch --env testnet
 
 ![image](https://github.com/user-attachments/assets/a3257ef1-50a1-444c-a66b-c762ab5238d0)

    sui client envs

 ![image](https://github.com/user-attachments/assets/ca3196ec-2d07-483f-af3a-6d5647561488)

    sui client faucet
    sui client gas


Walrus Yükleyelim

    SYSTEM=ubuntu-x86_64
    curl https://storage.googleapis.com/mysten-walrus-binaries/walrus-testnet-latest-$SYSTEM -o walrus
    chmod +x walrus
    mv walrus /usr/local/bin/
    mkdir -p ~/.config/walrus
    curl -o ~/.config/walrus/client_config.yaml https://raw.githubusercontent.com/MystenLabs/walrus-docs/main/docs/client_config.yaml

    nano ~/.config/walrus/client_config.yaml
içerisine

    system_object: 0x50b84b68eb9da4c6d904a929f43638481c09c03be6274b8569778fe085c1590d
    staking_object: 0x37c0e4d7b36a2f64d51bba262a1791f844cfd88f31379f1b7c04244061d43914
    exchange_object: 0x0e60a946a527902c90bbc71240435728cd6dc26b9e8debc69f09b71671c3029b

faucet alalım
    
    walrus get-wal
![image](https://github.com/user-attachments/assets/0e75acf4-73bb-4bac-a125-5b54a7dbc027)


Bilgi 

     walrus info --dev
 
 
 
 Blob Saklama

     walrus store /root/DOSYAADI
     walrus blob-status --blob-id <BLOB_ID>

![image](https://github.com/user-attachments/assets/03a422e2-68dd-445e-a568-34f2aa6abb0b)


 Blob Okuma

    walrus read <BLOB_ID>
    
 Blob Silme İşlemi
 
    walrus delete --blob-id <BLOB_ID>

Json
 

    walrus json \
    '{
        "config": "/root/.config/walrus/client_config.yaml",
        "command": {
            "store": {
                "file": "/root/DOSYADI"
            }
        }
    }'


![image](https://github.com/user-attachments/assets/0968b535-3fd8-4bb1-8764-8471d7b442dc)



blob'un içeriğini okumak istiyorsanız, blob ID'sini kullanarak aşağıdaki komutu çalıştırabilirsiniz:


    walrus json \
    '{
        "config": "/root/.config/walrus/client_config.yaml",
        "command": {
            "read": {
                "blobId": "<BLOB_ID>"
            }
        }
    }'

![image](https://github.com/user-attachments/assets/15ca373a-5d0c-481f-9765-a95910f6dfad)

Aggregator Çalıştıralım

    screen -S aggregator
    walrus aggregator -b "127.0.0.1:31415"
    
![image](https://github.com/user-attachments/assets/5a8ec78b-16d8-45f9-b289-cd4701e3bb5c)

CTRL A+ D

    screen -S publisher
    walrus publisher -b "127.0.0.1:31416" 

CTRL A+ D

    AGGREGATOR=https://aggregator.walrus-testnet.walrus.space
    PUBLISHER=https://publisher.walrus-testnet.walrus.space

Bir string depolamak için:
    
    curl -X PUT "$PUBLISHER/v1/store" -d "Bu benim örnek string'im."
    
  ![image](https://github.com/user-attachments/assets/dd997e52-5b77-494c-b43d-a7e3cc942bfe)

  Blob'un içeriğini terminalde yazdırmak için:

    curl "$AGGREGATOR/v1/BLOBID"
    
 ![image](https://github.com/user-attachments/assets/ca5b4895-30df-4600-be18-a8f9691a0375)

Bir dosya depolamak için:

    curl -X PUT "$PUBLISHER/v1/store?epochs=5" --upload-file "/root/DOSYAADI"

Depoladığınız Dosyayı İndirmek için

    curl "$AGGREGATOR/v1/BLOBID" -o DOSYAADI



AYRICA BURADAN KELİMELERİNİZİ SUİ WALLETA İMPORT EDİP https://t.co/JGhWZk9hWq 1 ADET WAL STAKE EDİN

Storage-node Herkese açık değil kuralamaz
