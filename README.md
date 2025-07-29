## Project Overview
- README: Multi-System Login with Laravel 12 + Sanctum (website-app & software-app)
1.  Requirements
    - PHP 8.2+
    - Composer
    - Laravel 12
    - MySQL (same DB for both apps)
    - Localhost with custom domains (website-app.test, software-app.test)

2. Installation Steps (For Both Projects)

    -   Step 1: Clone Both Repositories
        - git clone https://github.com/jalalkhan1011/website-app.git
        - git clone https://github.com/jalalkhan1011/software-app.git

    -   Step 2: Install Dependencies
        - cd website-app
        - composer install
        - cp .env.example .env

        - cd ../software-app
        - composer install
        - cp .env.example .env

    - Step 3: Configure .env
        - উভয় প্রজেক্টের .env ফাইলে একই DB ব্যবহার করুন

        - APP_URL=http://website-app.test # website-app এর জন্য
        - APP_URL=http://software-app.test # software-app এর জন্য

        - DB_DATABASE=multi_system_db
        - DB_USERNAME=root
        - DB_PASSWORD=

    -   Step 4: Sanctum Install (দুই প্রজেক্টে)
        - composer require laravel/sanctum
        - php artisan migrate

    -   Step 5: Update User Model
        - app/Models/User.php এ:

        - use Laravel\Sanctum\HasApiTokens;

        class User extends Authenticatable
        {
        use HasApiTokens, Notifiable;
        }

    -   Step 6: Create config/cors.php (Manually)
        - config/cors.php তৈরি করুন:

       - <?php

       - return [
            'paths' => ['api/*', 'sanctum/csrf-cookie', '*'],

            'allowed_methods' => ['*'],

            'allowed_origins' => [
                'http://website-app.test',
                'http://software-app.test',
            ],

            'allowed_headers' => ['*'],
            'supports_credentials' => true,
        ];

    - Step 7: Add Middleware in bootstrap/app.php

        - ->withMiddleware(function (Middleware $middleware): void {
            $middleware->web([
                \Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
                \Illuminate\Http\Middleware\HandleCors::class,
            ]);
        })

    - Step 9: Clear Cache
        - php artisan config:clear
        - php artisan cache:clear

3. Localhost Domain Setup
    - C:\Windows\System32\drivers\etc\hosts ফাইলে যোগ করুন:
    - 127.0.0.1 website-app.test
    - 127.0.0.1 software-app.test

4. Run Both Projects
    - cd website-app
    - php artisan serve --host=website-app.test --port=8000
    - cd ../software-app
    - php artisan serve --host=software-app.test --port=8001

5. Testing SSO Flow
    - software-app এ ইউজার auto login হবে এবং /dashboard এ যাবে।
    - logout করলে দুই সিস্টেম থেকেই লগআউট হবে।

6. Features Implemented
    - Single Sign-On (SSO) via Laravel Sanctum
    - Auto Login to software-app after website-app login
    - Shared Database for Users
    - Logout Sync across both apps
    - CORS configured for cross-domain requests

7. Login credential
    - User-> test@example.com
    - password-> 123456789

///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>

<p align="center">
<a href="https://github.com/laravel/framework/actions"><img src="https://github.com/laravel/framework/workflows/tests/badge.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Learning Laravel

Laravel has the most extensive and thorough [documentation](https://laravel.com/docs) and video tutorial library of all modern web application frameworks, making it a breeze to get started with the framework.

You may also try the [Laravel Bootcamp](https://bootcamp.laravel.com), where you will be guided through building a modern Laravel application from scratch.

If you don't feel like reading, [Laracasts](https://laracasts.com) can help. Laracasts contains thousands of video tutorials on a range of topics including Laravel, modern PHP, unit testing, and JavaScript. Boost your skills by digging into our comprehensive video library.

## Laravel Sponsors

We would like to extend our thanks to the following sponsors for funding Laravel development. If you are interested in becoming a sponsor, please visit the [Laravel Partners program](https://partners.laravel.com).

### Premium Partners

- **[Vehikl](https://vehikl.com)**
- **[Tighten Co.](https://tighten.co)**
- **[Kirschbaum Development Group](https://kirschbaumdevelopment.com)**
- **[64 Robots](https://64robots.com)**
- **[Curotec](https://www.curotec.com/services/technologies/laravel)**
- **[DevSquad](https://devsquad.com/hire-laravel-developers)**
- **[Redberry](https://redberry.international/laravel-development)**
- **[Active Logic](https://activelogic.com)**

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
