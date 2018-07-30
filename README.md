# DOCKER SWARM

Docker memperkenalkan swarm mode yang memiliki kemampuan untuk men-deploy banyak container ke banyak host, menggunakan overlay network untuk service discovery dengan built-in load balancer untuk menskalakan layanan (services)

Docker swarm memperkenalkan 3 konsep baru, antara lain:
* Node : adalah bagian dari docker engine yang terhubung dengan Swarm. Node adalah manager atau worker, dimana manager menjadwalkan container akan berjalan dimana, worker akan mengeksekusi task. Secara default, maneger adalah worker.
* Services : adalah konsep yang berhubungan dengan sekumpulan tasks untuk dieksekusi oleh worker. Contohnya adalah service pada HTTP server yang berjalan sebagai docker container pada tiga node.
* Load Balancing: docker menyertakan load balancer untuk memproses request dari semua kontainer dalam service.

## Create Swarm Mode Cluster

Untuk menginisialisasi swarm mode digunakan perintah init

` docker swarm init `

setelah menjalankan command, docker engine akan tahu bagaimana bekerja dengan cluster dan menjadi manager. Hasil inisialisasi adalah token yang digunakan untuk menambahkan node dengan aman.

gambar 1

## Join Cluster

Dengan berjalannya swarm mode, maka dapat ditambahkan node tambahan dan menjalankan perintah dari semua node tersebut. Apabila node hilang, contohnya ketika terjadi crash, maka container yang berjalan dalam host-host tersebut akan otomatis me-rescheduled ke node lain. Proses rescheduling memastikan kita tidak kehilangan kapasitas dan memiliki availability yang tinggi.

pada tiap node yang ingin kita tambahkan ke cluster, gunakan docker CLI untuk menggabunggannya ke grup yang sudah ada. Penggabungan dilakukan dengan menunjukkan host lain ke manajer dalam cluster, dalam hal ini adalah host pertama.

Hal pertama yang dilakukan setelahnya adalah mendapatkan token yang dibutuhkan untuk menambahkan worker ke dalam cluster.

` token=$(docker -H 172.17.0.12:2345 swarm join-token -q worker) && echo $token `

gambar 2

pada host kedua, gabungkan ke cluster dengan me-request akses melalui manager. Token disediakan sebagai parameter tambahan

` docker swarm join 172.17.0.12:2377 --token $token `

gambar 3

Secara default, manager akan secara otomatis menerima nodes baru yang dimasukkan dalam cluster. Untuk melihat node pada cluster digunakan perintah `docker node ls`

gambar 4
