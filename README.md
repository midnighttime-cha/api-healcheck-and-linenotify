# วิธีเช็ค Service API ว่ากำลังทำงานอยู่ปกติไหม+ส่ง Line notify บน Unbuntu server 20.04 LTS

## ติดตั้ง Package ต่อไปนี้
```bash
sudo apt update
sudo apt install -y nano
sudo apt install -y curl
```

## ทำการรันคำสั่งต่อไปนี้เพื่อสร้างไฟล์
```bash
nano health_check.sh
```

## ใส่คำสั่งต่อไปนี้
```bash
# File: health_check.sh

response=$(curl --write-out '%{http_code}' --silent --output /dev/null http://localhost:9999/v1/testCheck)

if [[ "$response" -ne 200 ]] ; then
  
  # เมื่อ API ไม่ทำการ response ค่ากลับตาม http_code=200 จะทำการส่งแจ้งเตือนไปทาง Line notify
  curl https://notify-api.line.me/api/notify \
  -H 'Authorization: Bearer [--LINE DEVELOPER BEARER TOKEN--]' \
  -F "message=1 Site status changed to $response"
  
  # ตรงนี้สามารถทำคำสั่ง restart service ได้
else
  exit 0
fi
```


