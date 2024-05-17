Nama: Muhammad Rafi Zia Ulhaq<br>
NPM: 2206814551<br>
Kelas: Pemrograman Lanjut B<br>

# Module 11 - Deployment on Kubernetes

### Hello Minikube

1. Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running. What do you see in the logs? Does the number of logs increase each time you open the app?

**Jawab**<br>

Sebelum aplikasi diekspos sebagai *service*, log menunjukkan hanya aktivitas internal aplikasi tanpa *request* eksternal. Setelah diekspos sebagai *service*, log menunjukkan peningkatan aktivitas setiap kali aplikasi diakses melalui *service*. Kemudian ketika kita mengakses aplikasi beberapa kali, maka terlihat terdapat peningkatan jumlah log. Hal ini karena setiap kali aplikasi dibuka melalui *service*, setiap permintaan baru akan dicatat dalam log aplikasi.

2. Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to `kube-system`. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?

**Jawab**<br>

Ketika menjalankan perintah `kubectl get` tanpa opsi `-n`, perintah tersebut secara *default* akan menunjukkan *resource* yang terdapat pada *namespace default*. Sedangkan, opsi `-n` digunakan untuk menentukan *namespace* dari mana data akan diambil. Misalnya, `-n kube-system` akan menunjukkan *resource* yang ada di *namespace* `kube-system`. Output dari `kubectl get pods -n kube-system` tidak menampilkan *pods* atau *services* yang dibuat di *namespace default* karena perintah tersebut hanya menampilkan *resource* yang berada di *namespace* `kube-system`.

### Rolling Update Deployment

1. What is the difference between Rolling Update and Recreate deployment strategy?

**Jawab**<br>

* **Rolling Update**: Mengganti *pod* lama dengan *pod* baru secara bertahap. Hal ini dilakukan untuk memastikan bahwa selalu terdapat sejumlah *pod* yang berjalan selama proses pembaruan untuk menjaga aplikasi tetap tersedia selama proses ini.
* **Recreate**: Mengganti semua *pod* lama dengan *pod* baru sekaligus. Semua *pod* yang menjalankan versi lama aplikasi akan dimatikan sebelum *pod* baru yang menjalankan versi baru aplikasi dibuat. Hal ini menyebabkan aplikasi tidak tersedia atau *downtime* selama proses ini.

2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt.

![alt text](https://github.com/rafizia/adpro-module-11-kubernetes/blob/master/image/Recreate.png?raw=true)

3. Prepare different manifest files for executing Recreate deployment strategy

**Jawab**<br>

Saya menggunakan manifest file yang saya buat dengan menggunakan deployment.yaml dan mengganti strategy dari RollingUpdate menjadi Recreate.
```
...
  strategy:
    type: Recreate
...
```

4. What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking `kubectl apply-f` command) to the cluster.

**Jawab**<br>

Mendeploy aplikasi secara manual melibatkan banyak langkah berulang dan rentan terhadap kesalahan. Sebaliknya, dengan menggunakan *manifest files* dan perintah `kubectl apply -f`, proses deployment menjadi lebih efisien. Manifest files memungkinkan kita mendefinisikan seluruh konfigurasi aplikasi dalam format deklaratif sehingga kita dapat dengan mudah memahami dan mereplikasi konfigurasi deployment yang sama.