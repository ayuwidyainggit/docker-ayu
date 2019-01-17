# skenario : menggunakan redis sebagai container docker. 

## Pengertian Docker
Docker adalah sebuah aplikasi yang bersifat open source yang berfungsi sebagai wadah/container untuk 
mengepak/memasukkan sebuah software secara lengkap beserta semua hal lainnya yang dibutuhkan oleh software tersebut dapat berfungsi.

1. DEPLOYING FIRST DOCKER
Pertama adalah mendefinisikan nama image docker yang dikonfigurasikan untuk menjalankan redis. Semua kontainer dimulai berdasarkan image docker.
Untuk menemukan image adalah dengan menggunakan perintah docker search <name> atau di registry.hub.docker.com/ 
Perintah untuk menemukan image untuk redis
     docker search redis
     ![1](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/1/1.PNG)
Secara default, docker akan menjalankan perintah di foreground. Namun jika akan di run di latar belakang, maka harus menggunakan perintah -d.
Dalam kasus ini, setelah Jane menemukan imagenya, dia mengidentifikasi bahwa Redis docker image yang disebut redis merupakan database. Oleh karena itu 
dia ingin menjalankannya di latar belakang sementara itu dia tetap bisa melanjutkan pekerjaannya.
 Perintah untuk menjalankan redis di latar belakang :
 ![2](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/1/2.PNG)
 
 2. MENEMUKAN CONTAINER YANG SEDANG DIJALANKAN
 Perintah docker ps untuk melihat semua kontainer yang berjalan, gambar yang digunakan untuk memulai kontainer dan waktu aktif.
 ![3](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/1/3.PNG)
 Hasilnya menampilkan nama dan ID ramah yang dapat digunakan untuk mencari tahu informasi tentang masing-masing kontainer.
 
 Perintah docker inspect <friendly-name|container-id> memberikan rincian IP address kontainer yang berjalan.

 Perintah docker logs <friendly-name|container-id> akan menampilkan pesan yang telah ditulis oleh kontainer ke kesalahan standar 
 
 3. MENGAKSES REDIS
 Dalam kasus tersebut, redis berjalan namun tidak dapat diakses karena setiap kontainer dikosongkan. Jika suatu layanan ingin diakses oleh 
 proses yang tidak berjalan dalam suatu kontainer, maka port perlu dibuka melalui host.
 Setelah port dibuka maka seolah-olah akan berjalan pada OS host itu sendiri.
 Secara default, redis berjalan pada port 6379
 port akan terikat ketika kontainer mulai menggunakan   -p <host-port>:<container-port> option
 selain itu nama juga perlu didefinisikan terlebih dahulu untuk memulai kontainer. 
 untuk menjalankan redis di latar belakang adalah dengan mengguakan redisHostPort di port 6379
 perintahnya adalah seperti di bawah ini :
 docker run -d --name redisHostPort -p 6379:6379 redis:latest
  ![4](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/1/4.PNG)
  
 4. MENGAKSES REDIS
 MasalahMasalah dengan menjalankan proses pada port tetap adalah kita hanya dapat menjalankan satu instance saja. Untuk dapat menjalankan beberapan 
 instansi redis . Caranya adalah menggunakan opsi -p 6379 memungkinkannya untuk mengekspos Redis tetapi pada port yang tersedia secara acak.
 perintahnya adalah :
 docker run -d --name redisDynamic -p 6379 redis:latest
 ![5](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/1/5.PNG)
 
 untuk mengetahui port yang telah ditetapkan adalah dengan menggunakan perintah :
 docker port redisDynamic 6379
 ![6](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/1/6.PNG)
 
untuk melihat kontainer yang sedang berjalan :
 ![7](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/1/7.PNG)
 
 5. PERSISTING DATA
 permasalahan : data yang sudah disimpan terhapus ketika menciptakan kontainer kembali.
 unttuk mengikat direktori (juga dikenal sebagai volume) dilakukan dengan menggunakan opsi -v <host-dir>: <container-dir>
 
 Dengan menggunakan dokumentasi Docker Hub untuk Redis,  image resmi Redis menyimpan log dan data ke direktori / data.
 Setiap data yang perlu disimpan di Host Docker, dan tidak di dalam kontainer, harus disimpan di / opt / docker / data / redis.
 ![8](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/1/8.PNG)
 
 6. Running A Container In The Foreground
 perintah docker run ubuntu ps untuk meluncurkan kontainer Ubuntu dan mengeksekusi perintah ps untuk melihat semua proses yang berjalan dalam wadah.
  ![9](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/1/9.PNG)
  
  Menggunakan docker run -it ubuntu bash untuk  untuk mendapatkan akses ke bash shell di dalam sebuah wadah.
   ![10](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/1/10.PNG)
   
   # cara membuat dan menjalankan Image Docker untuk menjalankan situs web HTML statis menggunakan Nginx
   1. MEMBUAT DOCKER FILE
   Image Docker dimulai dari Image dasar. Image dasar harus mencakup dependensi platform yang diperlukan oleh aplikasi Anda, 
   misalnya, dapat digunakan untuk  menginstal JVM atau CLR.
   Image dasar ini didefinisikan sebagai instruksi dalam Dockerfile. image Docker dibangun berdasarkan konten Dockerfile. 
   Dockerfile adalah daftar instruksi yang menjelaskan cara menggunakan aplikasi Anda.
   
   image dasar yang digunakan  adalah versi Alpine dari Nginx. 
   cara membuat Dockerfile untuk membangun image :
    code :
	FROM nginx:alpine
	//untuk mendefinisikan image dasar 
    COPY . /usr/share/nginx/html
	//untuk menyalin konten direktori saat ini ke lokasi tertentu di dalam kontainer.
	
	![1.1](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/2/1.1.PNG)
	
   2. MEMBANGUN IMAGE DOCKER
   Perintah build menggunakan beberapa parameter yang berbeda. Formatnya adalah docker build -t <build-directory>. 
   Parameter -t digunakan untuk menentukan nama untuk gambar dan tag, yang biasa digunakan sebagai nomor versi. 
   Fungsinya untuk melacak image buatan dan melihat tentang versi mana yang sedang dimulai.
   
   perintah untuk membangun image statis HTML  :
   docker build -t webserver-image:v1 .
   ![1.2](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/2/1.2.PNG)

   perintah untuk melihat daftar semua image di host:
   docker images
   ![1.3](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/2/1.3.PNG)
   Image yang dibangun akan memiliki nama webserver-image dengan tag v1.
   
   3. MENJALANKAN IMAGE DOCKER
   untuk membuka dan mengikat port jaringan pada host  adala menggunakan parameter -p <host-port>: <container-port>.
   
   Jalankan image  yang baru dibangun dengan memberikan nama dan tag yang ramah. 
   Karena ini adalah server web, bind port 80 ke host  menggunakan parameter -p.
   perintahnya adalah seperti di bawah ini :
   docker run -d -p 80:80 webserver-image:v1
   ![1.4](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/2/1.4.PNG)
   
   Setelah dimulai, kita dapat mengakses hasil port 80 
   curl docker
   ![1.5](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/2/1.5.PNG)
   Untuk merender permintaan di browser gunakan link berikut
   https://2886795308-80-cykoria04.environments.katacoda.com/

   situs web HTML statis berhasil dibuat dan dilayani oleh Nginx.

   # MEMBANGUN IMAGE KONTAINER
   tujuan : mengetahui cara membuat image berdasarkan kebutuhan.
   1. IMAGE DASAR   
   Semua Image Docker dimulai dari image dasar. image dasar adalah image yang sama dari Docker Registry 
   yang digunakan untuk memulai container.
   Image dasar ini digunakan sebagai dasar untuk perubahan tambahan  untuk menjalankan aplikasi.
   Dalam skenario, memerlukan NGINX untuk dikonfigurasikan dan berjalan pada sistem sebelum  dapat menggunakan file HTML statis . 
   Karena itu NGINX digunakan sebagai image dasar .
   Untuk mendefinisikan image dasar, gunakan instruksi FROM <image-name>: <tag>
   
   langkah pertama adalah membuat Membuat Dockerfile
   Baris pertama Dockerfile harus FROM nginx: 1.11-alpine
   ![2.1](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/3/2.1.PNG)
   
   
   2. MENJALANKAN PERINTAH 
   Dengan image dasar ditentukan, kita perlu menjalankan berbagai perintah untuk mengkonfigurasi image kita. 
   Perintah utamanya adalah COPY dan RUN.
   
   RUN <perintah> digunakan untuk mengeksekusi perintah apa pun seperti yang dilakukan pada prompt perintah,
   misalnya menginstal paket aplikasi yang berbeda atau menjalankan perintah build.
   
   COPY <src> <dest> digunakan untuk menyalin file dari direktori yang berisi Dockerfile ke image container. 
   
   Pada kasus file index.html baru telah dibuat untuk dijalankan dari kontainer. 
   Setelah perintah FROM, selanjutnya adalah menggunakan perintah COPY untuk menyalin index.html ke direktori 
   yang disebut / usr / share / nginx / html
   ![2.2](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/3/2.2.PNG)
   
   3. MENGEKSPOS PORT
   Dengan menggunakan perintah EXPOSE <port> Anda memberi tahu Docker port mana yang harus dibuka dan juga dapat diikat. 
   kita dapat menentukan beberapa port pada perintah tunggal, misalnya, EXPOSE 80 433 atau EXPOSE 7000-8000
   
   untuk dapat mengakses server web agar dapat diakses melalui port 80, tambahkan baris EXPOSE 80 pada Dockerfile.
   ![2.3](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/3/2.3.PNG)

   4. PERINTAH DEFAULT
   Jika perintah membutuhkan argumen maka disarankan untuk menggunakan array, misalnya ["cmd", "-a", "nilai arga", "-b", "argb-value"], 
   yang akan digabungkan bersama dan perintah cmd -a "arga value" -b argb-value akan dijalankan.   
   Perintah untuk menjalankan NGINX adalah nginx -g daemon off ;. Atur ini sebagai perintah default di Dockerfile.
   ![2.4](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/3/2.4.PNG)
   NGINX akan menjadi entrypoint dengan -g daemon off; perintah default.
   
   5. MEMBANGUN KONTAINER
   docker build -t my-nginx-image:latest 
   ![2.6](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/3/2.6.PNG)
   
   Setelah menulis Dockerfile maka perlu menggunakan docker build untuk mengubahnya menjadi image. 
   Perintah build mengambil dalam direktori yang berisi Dockerfile, menjalankan langkah-langkah dan 
   menyimpan image di Docker Engine lokal Anda. Jika salah satu gagal karena kesalahan maka build berhenti.
   
   docker image digunakan untuk melihat daftar image pada mesin lokal 
   ![2.5](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/3/2.5.PNG)
   
   6. Launching New Image
   NGINX dirancang untuk dijalankan sebagai layanan latar belakang sehingga  harus menyertakan opsi -d. 
   Untuk dapat mengakses server web dapat , ikat ke port 80 menggunakan p 80:80
   ![2.7](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/3/2.7.PNG)

   Anda dapat mengakses server web yang diluncurkan melalui hostname docker. Setelah meluncurkan kontainer, 
   perintah curl -i http://docker  akan mengembalikan file indeks  melalui NGINX dan image yang dibuat.
   ![2.8](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/3/2.8.PNG)
   
   kontainer yang sedang berjalan
   ![2.9](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/3/2.9.PNG)
   
   
   #  cara menggunakan aplikasi Node.js dalam sebuah wadah.
   1. IMAGE DASAR
   Image untuk Node 10.0 adalah node: 10-alpine. 
   selain image dasar, kita perlu membuat direktoori dasar tempat aplikasi berjalan. 
   perintah RUN <command>  untuk menjalankan perintah
   mkdir  untuk membuat direktory tempat aplikasi dijalankan.
   direktori yang ideal adalah / src / app
   
   Kita dapat mendefinisikan direktori  menggunakan WORKDIR <directory> untuk memastikan bahwa semua perintah selanjutnya dapat
   dijalankan dari direktori relatif ke aplikasi kita.\
   
   Menentukan Base Environment

   Atur FROM <image>: <tag>, RUN <command> dan WORKDIR <directory> pada baris terpisah untuk mengonfigurasi lingkungan 
   dasar untuk menggunakan aplikasi Anda.
   ![3.1](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/4/3.1.PNG)
   
   2. NPM INSTALL
   Cara menjalankan instalasi npm dengan dockerfile :
   ![3.2](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/4/3.2.PNG)
   
   Jika  tidak ingin menggunakan cache sebagai bagian dari build maka 
   atur opsi --no-cache = true sebagai bagian dari perintah build docker.
   
   3. Mengkonfigurasi Aplikasi
   ![3.3](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/4/3.3.PNG)
   
   4. MEMBANGUN DAN MENJALANKAN KONTAINER
   Perintah untuk membangun image:
      docker build -t my-nodejs-app 
	![3.4](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/4/3.4.PNG)
		
   Perintah untuk meluncurkan image yang dibangun adalah
   docker run -d --name my-running-app -p 3000:3000 my-nodejs-app
   ![3.5](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/4/3.5.PNG)
   
   uji kontainer menggunakan curl apakah aplikasi merespon atau tidak.
   ![3.6](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/4/3.6.PNG)
   
   5. ENVIRONMENT VARIABLES
   Image Docker harus dirancang agar dapat ditransfer dari satu lingkungan ke 
   lingkungan lainnya tanpa membuat perubahan apa pun atau harus dibangun kembali.
   Dengan aplikasi Node.js, kita harus mendefinisikan variabel lingkungan untuk NODE_ENV saat berjalan dalam produksi.
   ![3.7](https://github.com/ayuwidyainggit/docker-ayu/blob/master/images/4/3.7.PNG)
   
   
		
   