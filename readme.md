# Laravel with JWT

* [Installation · tymondesigns/jwt-auth Wiki · GitHub](https://github.com/tymondesigns/jwt-auth/wiki/Installation)

### 安裝

```
composer create-project --prefer-dist laravel/laravel laravel-jwt-sandbox
composer require tymon/jwt-auth
```

### 配置

**config/app.php**

```
// aliases

'JWTFactory' => Tymon\JWTAuth\Facades\JWTFactory::class,
```

**app/HTTP/Kernel.php**

```
//  routeMiddleware

'jwt.auth' => \Tymon\JWTAuth\Middleware\GetUserFromToken::class,
'jwt.refresh' => \Tymon\JWTAuth\Middleware\RefreshToken::class,
```

```
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\JWTAuthServiceProvider"
php artisan jwt:generate
```

### 實作

**AuthenticateController**

```php
public function authenticate(Request $request)
{
        // grab credentials from the request
    $credentials = $request->only('email', 'password');

    try {
        // attempt to verify the credentials and create a token for the user
        if (! $token = JWTAuth::attempt($credentials)) {
            return response()->json(['error' => 'invalid_credentials'], 401);
        }
    } catch (JWTException $e) {
        // something went wrong whilst attempting to encode the token
        return response()->json(['error' => 'could_not_create_token'], 500);
    }

    $user = User::where('email', $request->email)->first();
    $token = JWTAuth::fromUser($user);

    // return dd($token);

    // all good so return the token
    return response()->json([
        'token' => $token
    ]);
}
```

**route/api.php**

```php
Route::post('/login', 'AuthenticateController@authenticate');

Route::middleware('jwt.auth')->post('/jwt/check', function (Request $request) {
    return 'token is correct';
});
```

**Http Header**

```
Authorization: Bearer TOKEN_STRING
```

