# php_jwt
PHP签发Token和验证Token是否被篡改

## 实例


```php
use Lcobucci\JWT\Builder;
use Lcobucci\JWT\Signer\Hmac\Sha256;
use Lcobucci\JWT\Parser;


/**
 * 单例 一次请求中所以出现使用jwt的地方都是一个用户
 */
class JwtAuth
{
    private $token;
    private $uid;
    private $secrect = 'bgOEPw3BimF4re7x9OiFXhBUW4A3WDObj3dw';
    private static $instance;
    public static function getInstance(){
        if(is_null(self::$instance)){
            self::$instance = new self();
        }
        return self::$instance;
    }
    /**
     * 私有化构造函数
     * JwtAuth __construct
     */
    public function __construct()
    {
        
    }
    /**
     * 私有化__clone函数
     */
    public function __clone()
    {
       
    }
    /**
     * 编码jwt token
     */
    public function encode($uid){
        $time = time();
        $token = (new Builder())->setHeader('alg','HS256')
        ->setIssuer('http://example.com') // Configures the issuer (iss claim)
        ->setAudience('http://example.org') // Configures the audience (aud claim)
        ->setIssuedAt($time) // Configures the time that the token was issued (iat claim)
        ->setExpiration(time() + 3600) // Configures the expiration time of the token (exp claim)
        ->set('uid', $uid) // Configures a new claim, called "uid"
        ->sign(new Sha256(),$this->secrect)
        ->getToken(); // Retrieves the generated token
        return $token;
            }
            /**
             * 解析成token对象
             */
    public function decode($token){
        if(!$token){
            $token = (new Parser())->parse((string) $token); // Parses from a string
            return $token;
        }
       
    }
    /**
     * 校验token是否被篡改
     * @result bool
     */
    public function verify($token){
            $token = (new Parser())->parse((string) $token); // Parses from a string
            $result = $token->verify(new Sha256(),$this->secrect);
            return $result;
    }
}
```
