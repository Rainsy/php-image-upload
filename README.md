# php-image-upload
php를 이용하여 image를 업로드 합니다.

# 필수 조건 (Requirement)
- PHP (>5.3)
- composer

# 사용법 (Usage)
- composer.json을 수정하여 추가할 Library를 명세합니다.
```text
// composer.json
"require" : {
    "firebase/php-jwt": ">=1.0.0",
    "eventviva/php-image-resize": "1.4.*"
}                  
```
- composer를 이용하여 Library를 추가합니다.
```command
./composer install
