### กำหนด
`192.0.0.1` เป็นเครื่องเก่า `192.0.0.2` เป็นเครื่องใหม่ ทั้งสองเครื่องเป็น Ubuntu server ใช้ database เป็น MySQL และ web app เป็น Laravel ที่รันอยู่บน vesta 

#### STEP 1
Export database จากเป็นเครื่องเก่า
```sh
$ mysqldump admin_database > admin_database.sql
```

#### STEP 2
บีบอัน storge ในกรณีนี้หลาย directory ก็ระบุ path ไป
```sh
$ tar -czvf storge.tar.gz storage/app storage/excel/exports public/uploads
```

#### STEP 3
ส่ง file ไปยังเครื่องใหม่ ผ่าน ftp protocol โดยใช้ curl วิธีนี้จะทำให้เห็น progress ว่าส่ง file ไปถึงไหนแล้ว
```sh
$ curl -T admin_database.sql ftp://192.0.0.2 --user <username>:<password>

$ curl -T storge.tar.gz ftp://192.0.0.2 --user <username>:<password>
```
วิธีนี้ต้องลง ftp และเปิด port ftp ที่เครื่องใหม่ก่อน อ่านวิธีนั้นที่นี่ [install-ftp-server-on-ubuntu](https://phoenixnap.com/kb/install-ftp-server-on-ubuntu-vsftpd)

#### STEP 4
Import Database ไปยังเครื่องใหม่
```sh
$ mysql admin_database < admin_database.sql
```

#### STEP 5
ย้าย file `storge.tar.gz` ไปยัง web path และ untar file 
```sh
$ mv storge.tar.gz /home/admin/web/domain_name/public_html
$ cd /home/admin/web/domain_name/public_html
$ tar -xvf storge.tar.gz
```

### Reference
- [How to tar a file in Linux using command line](https://www.cyberciti.biz/faq/how-to-tar-a-file-in-linux-using-command-line/)
- [How To Add a User and Grant Root Privileges on Ubuntu 18.04](https://www.liquidweb.com/kb/add-user-grant-root-privileges-ubuntu-18-04/)
