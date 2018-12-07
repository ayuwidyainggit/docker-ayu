# DEPLOYING YOUR FIRST DOCKER CONTAINER

## STEP 1 - Running A Container 
mengidentifikasi nama dari Docker Image yang mana dikonfigurasi untuk menjalankan Redis. Redis merupakan database. 
Dengan docker, semua kontainer akan dimulai berdasarkan Docker Image.
Gambar-gambar ini mengandung semua yang diperlukan untuk proses, host tidak memerlukan konfigurasi atau dependensi apa pun.

Jane dapat menemukan gambar yang ada dengan menggunakan command.
    docker search <name>
    docker search redis
    hasil :
![docker search redis](https://github.com/ayuwidyainggit/tct/blob/master/images/1.%20docker%20search%20redis.png)

Menggunakan perintah command, Redis Docker Image disebut redis dan ingin menjalankan rilis terbaru. 
Karena Redis adalah database, Jane ingin menjalankannya sebagai layanan background service  sementara dia terus bekerja.
Untuk menyelesaikan langkah ini, Docker CLI memiliki perintah yang disebut run yang akan memulai kontainer berdasarkan  Docker image. 
Strukturnya adalah <options> <nama-gambar>.
Secara default, Docker akan menjalankan perintah di background. Untuk berjalan di latar belakang, opsi -d perlu ditentukan.
     docker run -d redis
	 hasil 
![docker run -d redis](https://github.com/ayuwidyainggit/tct/blob/master/images/2.%20docker%20run%20-d%20redis.png)
Secara default, Docker akan menjalankan versi terbaru yang tersedia. Jika versi tertentu diperlukan, itu bisa ditetapkan sebagai tag.

## STEP 2 - Finding Running Containers
Container yang diluncurkan berjalan di latar belakang, perintah ps docker mencantumkan semua kontainer yang berjalan.
Gambar yang digunakan untuk memulai container dan uptime.

Perintah ini juga menampilkan nama dan ID yang ramah yang dapat digunakan untuk mencari informasi tentang masing-masing kontainer.
![docker ps](https://github.com/ayuwidyainggit/tct/blob/master/images/3.%20docker%20ps.png)

memberikan detail lebih lanjut tentang container yang berjalan, seperti alamat IP.

## STEP 3 - Accessing Redis

Jika suatu layanan harus dapat diakses oleh suatu proses yang tidak berjalan dalam suatu wadah, maka port tersebut harus diekspos melalui Host.
Setelah terbuka, dimungkinkan untuk mengakses proses seolah-olah berjalan pada OS host itu sendiri. Secara default, Redis berjalan di port 6379.

Port terikat ketika kontainer mulai menggunakan opsi -p <host-port>: <container-port> juga berguna untuk menentukan nama saat memulai penampung, 
ini berarti dia tidak harus menggunakan Bash piping atau terus mencari nama ketika mencoba mengakses log.

     docker run  -d --name redisHostPort -p 6379:6379 redis:latest
![4](https://github.com/ayuwidyainggit/tct/blob/master/images/4.png)

Secara default, port pada host dipetakan ke 0.0.0.0, yang berarti semua alamat IP. 
Anda dapat menentukan alamat IP tertentu ketika Anda menentukan pemetaan port, misalnya, -p 127.0.0.1:6379:6379

## STEP 4 - Accessing Redis
Hanya menggunakan opsi -p 6379 memungkinkannya untuk mengekspos Redis tetapi pada port yang tersedia secara acak.
docker run -d --name redisDynamic -p 6379 redis:latest
![5](https://github.com/ayuwidyainggit/tct/blob/master/images/5.png)

untuk mengetahui port mana yang ditugaskan maka menggunakan perintah :
     docker port redisDynamic 6379
![6](https://github.com/ayuwidyainggit/tct/blob/master/images/6.png)
 untuk menampilkan informasi pemetaan port :
 docker ps
![7](https://github.com/ayuwidyainggit/tct/blob/master/images/7.png)

## STEP 5 - Persisting Data
membuat data agar dapat dipertahankan dan digunakan kembali ketika ia membuat ulang sebuah wadah.
Dengan menggunakan dokumentasi Hub Docker untuk Redis, images Redis resmi menyimpan log dan data ke dalam direktori / data.
Data apa pun yang perlu disimpan di Host Docker, dan bukan di dalam kontainer, harus disimpan di / opt / docker / data / redis.
perintah :
     docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis
hasil :
![8](https://github.com/ayuwidyainggit/tct/blob/master/images/8.png)
Docker memungkinkan Anda menggunakan $ PWD sebagai placeholder untuk direktori saat ini.

## STEP 6 - Running A Container In The Foreground
Bagaimana kontainer bekerja dengan proses di latar depan, seperti ps atau bash?
Sebelumnya, Jane menggunakan -d untuk mengeksekusi kontainer dalam keadaan terpisah, latar belakang.
Tanpa menentukan ini, kontainer akan berjalan di foreground. Jika Jane ingin berinteraksi dengan kontainer (misalnya, untuk mengakses shell bash) 
dia bisa memasukkan opsi -it.

386/5000
Selain menentukan apakah wadah berjalan di background atau di foreground, images tertentu memungkinkan Anda mengganti perintah
yang digunakan untuk meluncurkan image. Sebagai contoh, images Ubuntu dapat menjalankan perintah OS atau menjalankan perintah bash interaktif 
menggunakan / bin / bash

Perintah docker menjalankan ubuntu ps meluncurkan sebuah kontainer Ubuntu dan mengeksekusi perintah ps untuk melihat semua proses yang berjalan 
dalam sebuah kontainer.
 perintah  :
 docker run ubuntu ps
 hasil :
 ![9](https://github.com/ayuwidyainggit/tct/blob/master/images/9.png)
 
 untuk mendapatkan akses ke shell bash di dalam wadah. jalankan :
 docker run -it ubuntu bash
 

# Docker - Deploy Static HTML Website as Container

## Step 1 - Create Dockerfile
Dalam contoh ini, gambar dasar kami adalah versi Alpine dari Nginx. Ini menyediakan server web yang dikonfigurasi pada distribusi Linux Alpine.
Buat Dockerfile Anda untuk membangun image Anda dengan perintah :
    FROM nginx:alpine
	//untuk mendefinisikan image dasar
    COPY . /usr/share/nginx/html
	// untuk menyalin isi direktori saat ini ke lokasi tertentu di dalam penampung.

## Step 2 - Build Docker Image
Dockerfile digunakan oleh perintah build Docker CLI. Perintah build menjalankan setiap instruksi dalam Dockerfile. 
Hasilnya adalah Docker Image yang dibangun yang dapat diluncurkan dan menjalankan aplikasi Anda yang telah dikonfigurasi.

Perintah build mengambil beberapa parameter yang berbeda. Formatnya adalah docker build -t <build-directory>. Parameter -t digunakan untuk menentukan nama 
yang ramah untuk gambar dan tag, yang biasa digunakan sebagai nomor versi. Ini memungkinkan Anda untuk melacak gambar yang dibangun dan yakin tentang versi 
yang sedang dimulai.

Buat image HTML statis  menggunakan perintah build di bawah ini:
     docker build -t webserver-image:v1 
	 ![10](https://github.com/ayuwidyainggit/tct/blob/master/images/10.png)

Untuk melihat daftar semua gambar pada host yang digunakan :
     ![11](https://github.com/ayuwidyainggit/tct/blob/master/images/11.png)
## Step 3 - Run
image yang dibangun dapat diluncurkan secara konsisten ke Gambar Docker lainnya. Ketika memulai sebuah kontainer, Anda perlu memberikan izin dan 
akses ke apa yang diperlukan.
Misalnya, untuk membuka dan mengikat ke port jaringan pada host, Anda perlu memberikan parameter -p <host-port>: <port-kontainer>.

Launch our newly built image providing the friendly name and tag. As it's a web server, bind port 80 to our host using the -p parameter.
Begitu mulai, Anda akan dapat mengakses hasil port 80 melalui
     ![12](https://github.com/ayuwidyainggit/tct/blob/master/images/12.png)
	 
	 
# Docker - Building Container Images

## Step 1 - Base Images
Base Images adalah images yang sama dari Registry Docker yang digunakan untuk memulai kontainer.
Base Images ini digunakan sebagai dasar untuk perubahan tambahan Anda untuk menjalankan aplikasi Anda. 
Misalnya, jika mengharuskan NGINX untuk dikonfigurasi dan berjalan pada sistem sebelum, maka dapat menerapkan file HTML statis kami. 
Karena itu  menggunakan NGINX sebagai images dasar.
Untuk menentukan base images, maka menggunakan instruksi FROM <nama-gambar>: <tag>

## Step 2 - Running Commands
perintah utama adalah COPY dan RUN.
RUN <command> = untuk menjalankan perintah apa pun seperti yang Anda lakukan pada prompt perintah, 
misalnya menginstal paket aplikasi yang berbeda atau menjalankan perintah build. Hasil RUN dipertahankan ke images sehingga penting untuk tidak 
meninggalkan file yang tidak perlu atau sementara pada disk karena ini akan dimasukkan dalam images.

COPY <src> <dest> = untuk menyalin file dari direktori yang berisi Dockerfile ke gambar kontainer. 
Ini sangat berguna untuk kode sumber dan aset yang ingin Anda terapkan di dalam kontainer .

## Step 4 - Default Commands
Setelah menentukan port mana yang ingin kita akses, kita sekarang perlu mendefinisikan perintah yang meluncurkan aplikasi.
 CMD line  dalam Dockerfile mendefinisikan perintah default untuk dijalankan ketika sebuah kontainer diluncurkan. 
 Jika perintah membutuhkan argumen maka disarankan untuk menggunakan array, misalnya 
 ["cmd", "-a", "nilai arga", "-b", "argb-value"], yang akan digabungkan bersama dan perintah cmd -a "nilai arga" -b argb-value akan dijalankan.
 
 Perintah untuk menjalankan NGINX  :
 nginx -g daemon off;
 
## Step 5 - Building Containers
 untuk melihat daftar images di mesin lokal :
  ![13](https://github.com/ayuwidyainggit/tct/blob/master/images/13.png)
 
## Step 6 - Launching New Image
