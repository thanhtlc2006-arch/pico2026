# North-South

```
load_module /usr/lib/nginx/modules/ngx_http_geoip2_module.so;

worker_processes 1;
events { worker_connections 1024; }

http {
    include       mime.types;
    default_type  application/octet-stream;

    geoip2 /etc/nginx/GeoLite2-Country.mmdb {
        auto_reload 5m;
        $geoip2_data_country_code default=ZZ country iso_code;
    }

    upstream north {
        server 127.0.0.1:8000;
    }

    upstream south {
        server 127.0.0.1:9000;
    }

    server {
        listen 80;

        location / {
            if ($geoip2_data_country_code = IS) {
                proxy_pass http://south;
            }

            proxy_pass http://north;
        }
    }
}

```

Trong bài này, server dùng IP để đoán vị trí người dùng bằng database như MaxMind GeoIP2. Dựa vào quốc gia , Nginx sẽ route request sang server khác nhau.

Mục tiêu là truy cập server chứa flag, nên cần làm sao để server “tưởng” mình đang ở đúng quốc gia yêu cầu.

dựa vào file đề cho, ta nhìn ra được **$geoip2\_data\_country\_code = IS**&#x20;

**⇒** quốc gia ta cần fake ip là **Iceland**

ở đây để free ta dùng UrbanVPN để chuyển

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

sau khi chuyển, truy cập vào web đc cấp trong đề ta sẽ nhận đc flag

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
