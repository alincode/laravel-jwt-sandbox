# Laravel with JWT

### 安裝

```
composer create-project --prefer-dist laravel/laravel laravel-jwt-sandbox
composer require tymon/jwt-auth
```

### 配置

```
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\JWTAuthServiceProvider"
php artisan jwt:generate
```