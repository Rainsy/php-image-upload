# php-image-upload
php를 이용하여 image를 업로드 합니다.

# 필수 조건 (Requirement)
- PHP (>=5.3)
- composer (>=1.0.0)

# 사용법 (Usage)
- 일반적으로는 외부 Library는 포함하여 commit하지는 않지만, 편의를 위해 포함하였습니다.
- PHP에서 외부 Library는 composer을 이용합니다.
- composer를 이용하여 필요한 외부 Library는 다음과 같이 추가합니다.

 - composer.json을 수정하여 추가할 Library를 명세합니다.
```
// composer.json
"require" : {
    "firebase/php-jwt": ">=1.0.0",
    "eventviva/php-image-resize": "1.4.*"
}                  
 - composer를 이용하여 Library를 추가합니다.
```command
./composer install
```


!! eventviva/php-image-resize/ImageResize.php는 임의로 수정되었습니다.
!! 만약 이 repository에 있는 파일을 사용하지 않는다면
```php
$success = $image->getSuccess();
```
!! 에서 오류가 발생할 것입니다.

# 설정 방법 (Setting)
- upload.php의 설정은 upload.ini 파일을 이용합니다.
```json
{
        "version" : "1.0.1",
        "writer" : "jeon",
        "directory" : {
                "image/kids" : {
                        "Authorization" : ["kids", "parents", "admin"],
                        "width" : 200,
                        "height" : 200,
                        "crop" : {
                                "width" : 100,
                                "height" : 100
                        }
                },
                "image/parents" : {
                        "width" : 200,
                        "height" : 200
                }
        }
}
```
- "directory" => 파일을 저장할 경로입니다. 경로마다 설정을 다르게 할 수 있습니다.
- -  "image/kids" => 실제 저장될 경로입니다. http request에서 body의 "directory"의 값과 일치해야 합니다.
- - - "Authorization" => 저장할 사용자의 인증 여부입니다. 배열로 선언하며, 선언된 누구나 저장할 수 있습니다.
- - - "width" "height" => 저장될 이미지의 크기입니다. http request에서 body의 "width"와 "height"를 무시합니다.
- - - "crop" => 이미지를 자르기합니다. 선언된 경우 http request에서 body의 "crop"을 무시합니다.
- - - - "width" "height" => 자르기할 크기입니다. 
