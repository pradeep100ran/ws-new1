# HeroKu WebSocket Project

Simple websocket server using heroku free nodejs cloud.

## Getting Started

Buat akun Heroku di url:

```
https://signup.heroku.com/
```

Download dan install HeroKu cli di url:

```
https://devcenter.heroku.com/articles/heroku-cli#download-and-install
```

Download dan install git di url:

```
https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
```

Setelah semuanya selesai, lakukan langkah berikut.
1. login :
* buka cmd lalu ketik "heroku login", lakukan login pada browser yang telah terbuka.
2. clone project from github :
* arahkan ke directory yang diinginkan "cd D:\heroku-project"
* untuk contoh project dapat di fork dari repo ini. 
* clone project yang sudah dibuat dari github, contoh : "git clone https://github.com/user/nodejs-heroku"
* arahkan ke directory project tersebut "cd nodejs-heroku"
* lakukan penyesuaian (edit code, delete or something else)
3. upload to heroku :
* dari cmd ketikkan "heroku create"
* lalu "git push heroku master"
4. untuk start aplikasi yang sudah diupload :
* ketikkan "heroku ps:scale web=1 --app nama-aplikasi-nya"
5. untuk membuka aplikasi : 
* "heroku open"
6. untuk melihat log debug :
* "heroku logs --tail"

Untuk dokumentasi lengkap bisa diakses melalui url:

```
https://devcenter.heroku.com/articles/getting-started-with-nodejs
```

## Clone heroku project to local and update
1. login :
* buka cmd lalu ketik "heroku login", lakukan login pada browser yang telah terbuka.
2. clone : 
* arahkan ke directory yang diinginkan "cd D:\heroku-project"
* clone heroku project "heroku git:clone -a myproject-some-53694"
* arahkan directory "cd myproject-some-53694"
3. edit your project
4. update heroku project :
* "git add ."
* "git commit -am 'catetan perubahan'"
* "git push heroku master"

Untuk dokumentasi lengkap bisa diakses melalui url:

```
https://dashboard.heroku.com/apps/myproject-some-53694/deploy/heroku-git
```

## Update existing heroku project from local
1. buka CMD, arahkan ke directory project. contoh "cd D:\heroku\my-project"
2. login ke heroku. "heroku login"
3. arahkan ke spesifik project. "heroku git:remote -a my-project"
4. lakukan perubahan, edit delete or somethings else with your project.
5. update and push :
* "git add ."
* "git commit -am 'catetan perubahan'"
* "git push heroku master" 

## Notes
1. Project github yang bisa diupload ke server heroku hanya project yang berisi git.
2. Untuk struktur package.json bisa mengikuti [contoh](https://github.com/bangjii/heroku-ws/blob/master/package.json) atau buka [dokumentasi](https://devcenter.heroku.com/articles/getting-started-with-nodejs#declare-app-dependencies) untuk informasi lengkapnya.
3. Jika ini adalah awal memakai git, disaat melakukan "git commit" nanti akan ditolak pada saat pertama, lakukan git config untuk bisa melakukan git commit, contoh "git config --global user.email 'alitopan666@gmail.com'" dan "git config --global user.name 'ali topan'".
4. Nama project akan di generate oleh heroku server, nama itu akan muncul pada saat kita melakukan "git push heroku master" dan dapat dibuka ketika kita melakukan "heroku open".
5. Untuk contoh didalam readme ini adalah "myproject-some-53694" dan dapat diakses aplikasinya melalui "myproject-some-53694.herokuapp.com"
6. Nama project untuk contoh ini (myproject-some-53694 | myproject-some-53694.herokuapp.com) hanya dummy dan tidak dapat diakses.
4. Untuk melihat dashboard dapat diakses melalui [Heroku Dashboard](https://dashboard.heroku.com/apps).
5. Untuk delete aplikasi dapat diakses melalui

```
	https://dashboard.heroku.com/apps/myproject-some-53694/settings
```

6. Yang paling terpenting untuk deploy nodejs didalam heroku harus menggunakan express (web apps).
7. Karena saya sudah mencoba hanya menggunakan project murni websocket hasilnya tidak dapat jalan secara normal di heroku (atau memang saya salah confignya yah :D).
8. Untuk project websocket di heroku disaat idle (selama bbrp detik) client akan otomatis close connection.
9. Berdasarkan explorasi semalaman tentang issue ini, ternyata memang seperti itu, tidak ada kesalah config ataupun coding disisi client ataupun server websocket. Untuk mensiasati hal ini tidak disarankan untuk melakukan reconnecting (dan memang dokumentasi websocket tidak tersedia function ini) secara manual, yang disarakan adalah melakukan "ping-pong" antara client dan server agar koneksi "keep-alive".
10. Error: Multiple apps in git remote.

```
D:\bangjii\nodejs\heroku\jamblang-thepurple>heroku ps:scale web=0
 ??   Error: Multiple apps in git remotes
 ??      Usage: --remote cempedak-thejackfruit
 ??         or: --app mysterious-dust-12345
 ??      Your local git repository has more than 1 app referenced in git
 ??   remotes.
 ??      Because of this, we can't determine which app you want to run this
 ??   command against.
 ??      Specify the app you want with --app or --remote.
 ??      Heroku remotes in repo:
 ??      cempedak-thejackfruit (heroku)
 ??   mysterious--dust-12345 (doyou-knowapp)
 ??
 ??      https://devcenter.heroku.com/articles/multiple-environments
``` 

Itu karena telah melakukan perubahan nama pada git heroku dan project lama telah dihapus. Jadi git heroku mendeteksi adanya lebih dari satu repository, dan jika kita mau melakukan remote harus menggunakan --app nama-aplikasi dibelakang command yang akan dilakukan.
Untuk menghapus total git heroku itu ketik "git remote rm nama-aplikasi". Dan command "heroku ps:scale web=0" pun dapat dijalankan kembali (tanpa --app nama-aplikasi).

11. Untuk debug log dari aplikasi yang kita buat dapat diakses melalui cmd / terminal
```
heroku logs --tail
```

12. Untuk restart aplikasi
```
heroku restart
```

13. PENTING! websocket didalam heroku idling time nya 55 detik. Jika dalam waktu tersebut tidak ada aktifitas komunikasi antara server dan client maka websocket server akan closed. Untuk mensiasatinya adalah dengan menggunakan method ping-pong yaitu send websocket (server dan client) secara berkala untuk menjaga koneksi keepalive.
```
setInterval(() => {
	if (ws.readyState === WebSocket.OPEN) {
	   ws.send(JSON.stringify({keepalive:"PING!"})); //anything yg pntg kirim apaan kek
	}
}, 55000);
```

## Sumber Data

* [Heroku](https://www.heroku.com/) - Official site Heroku.
* [Rename Project Heroku](https://devcenter.heroku.com/articles/renaming-apps) - Renaming Apps & Delete git from the CLI.
* [WebSocket Issues](https://github.com/websockets/ws/issues/767) - Official github WebSocket issues Reconnection and KeepAlive.

## Authors

* **Hudzaifah Al Jihad** - *Initial work* - [bangjii](https://github.com/bangjii)
