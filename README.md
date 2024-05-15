docker compose untuk service mariadb 10.11.7



jalankan perintah "docker-compose up -d" untuk mengaktifkan service db



pertama kali dijalankan akan menginstal beberapa source yang diperlukan




```
aplikasi laravel yang masih development lokal menggunakan service database ini
dan juga menggunakan container docker dan dijalankan dengan compose,
set networknya yang sejajar dengan `services:`
tambahkan ini pada docker-compose.yml
```

```
version:"version..."
    name:
        other:
    network:
        - app
    volume:
        ....

// ini yang perlu ditambah
networks:
    app:
        driver: bridge
        // ini yang penting untuk komunikasi antar container
        name: db_service
        external: true
```

update file config/database.php bagian koneksi mysql-nya seperti pada kode berikut

```
'mysql' => [
    'driver' => 'mysql',
    'url' => env('DATABASE_URL'),
    'host' => env('DB_HOST', '127.0.0.1'),
    'port' => env('DB_PORT', '3306'),
    'database' => env('DB_DATABASE', 'forge'),
    'username' => env('DB_USERNAME', 'forge'),
    'password' => env('DB_PASSWORD', ''),
    'unix_socket' => env('DB_SOCKET', ''),
    'charset' => 'utf8mb4',
    'collation' => 'utf8mb4_unicode_ci',
    'prefix' => '',
    'prefix_indexes' => true,
    'strict' => true,
    'engine' => null,
    'options' => extension_loaded('pdo_mysql') ? array_filter([
        PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
    ]) : [],
],
```

dan .env bagian DB_*** nya

```
DB_CONNECTION=mysql
// host "db" ini diambil dari service di compose jadi bukan localhost atau 127.0.0.1
DB_HOST=db
DB_PORT=3306
DB_DATABASE=nama_database
DB_USERNAME=root
DB_PASSWORD=mypass
```

koneksi ke navicat atau phpmyadmin

```
host        = localhost
post        = 3307
username    = root
password    = mypass
```

koneksi ke php native (sample)

```
$db_host        = "db";
$db_user        = "root";
$db_pass        = "mypass";
$db_database    = "nama_database"; 
    
$dblink = mysqli_connect($db_host,$db_user,$db_pass,$db_database);
```