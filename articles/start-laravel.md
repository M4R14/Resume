# ติดตั้ง Laravel ด้วย Docker สำหรับ Dev

1. ติดตั้ง Laravel
    ```sh
    $ cd <workspace>
    $ docker run --rm --interactive --tty \\
        --volume $PWD:/app \\
        composer create-project --prefer-dist laravel/laravel laravel-blog
    ```
1. เข้า [phpdocker.io/generator](https://phpdocker.io/generator) ตั้งชื่อโปรเจค "laravel-blog"
    ```
    ****Global configuration
     - Project name : laravel-blog
     - Base port: 8080
    ****PHP configuration
     - PHP Version: 7.2.x
     - Extensions (PHP 7.2.x): MySQL, Bcmath, GD
    ****MySQL
     - Enable MySQL
     - Version: 5.7
         - Root Password: toor
         - Database Name: admin_laravel_blog
         - Database Username: admin_laravel_blog
         - Database Password: 123456
    ```
1. แตกไฟล์ที่ได้มาใส่เข้าไปใน Laravel
    ```sh
    $ unzip laravel-blog.zip
    $ cp laravel-blog/* ./<workspace>/laravel-blog
    ```
1. ติดตั้ง MyAdmin ด้วยการแก้ไข  `docker-compose.yml`
    ```yml
    ...
        myadmin:
          image: phpmyadmin/phpmyadmin
          environment:
              TZ: Asia/Bangkok
              PMA_HOST: mysql
          ports:
              - 8001:80
          links: 
              - mysql
    ```
1. แก้ไข .env 
    ```env
    DB_HOST:mysql
    DB_USERNAME:admin_laravel_blog
    DB_DATABASE:admin_laravel_blog
    DB_PASSWORD:123456
    ```
1. migrate database
    ```sh
    $ docker-compose up -d 
    $ docker-compose exec php-fpm php astisan migrate
    ```
1. เข้าสู่เว็บด้วย [http://127.0.0.1:8080](http://127.0.0.1:8080/)
