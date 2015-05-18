# php-image-upload
php를 이용하여 image를 업로드 합니다.

# 필수 조건 (Requirement)
- PHP (>=5.3)
- composer (>=1.0.0)

# 사용법 (Usage)
- 일반적으로는 외부 Library는 포함하여 commit하지는 않지만, 편의를 위해 포함하였습니다.
- PHP에서 외부 Library는 composer을 이용합니다.
- composer를 이용하여 필요한 외부 Library는 다음과 같이 추가합니다.
```
> composer.json을 수정하여 추가할 Library를 명세합니다.
// composer.json
"require" : {
    "firebase/php-jwt": ">=1.0.0",
    "eventviva/php-image-resize": "1.4.*"
} 

> composer를 이용하여 Library를 추가합니다.
./composer install
```


- !! eventviva/php-image-resize/ImageResize.php는 임의로 수정되었습니다. 만약 이 repository에 있는 파일을 사용하지 않는다면
```php
$success = $image->getSuccess();
```
에서 오류가 발생할 것입니다.

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


# 이미지 경로 (Image path)
- upload.ini에서 명시된 directory는 upload.php가 존재하는 경로를 기준으로 한 상대경로 입니다.
- upload.ini에서 명시된 directory는 실제로 존재할 것을 권장합니다.

# 이미지 이름 (Image name)
- upload.ini에 Authorization이 명시되어 있으면 fk_kids의 이름으로 저장합니다. (확장자는 유지됩니다.)
- 토큰이 fk_parents의 경우 http request에서 body의 fk_kids의 값을 이름으로 저장합니다. (자식 등록여부는 미구현)
- 토큰이 fk_admin의 경우도 위 경우와 같지만 자식 등록여부는 검사하지 않습니다.
- upload.ini에 Authorization이 명시되지 않은 경우, 업로드한 파일 이름을 그대로 저장합니다.

# HTTP Request body
- HTTP Request는 POST방식으로 전달되어야 합니다.
- 파일의 key값은 항상 "filename"으로 명시되어야 합니다. 
- "directory"는 항상 명시되어야 합니다.
- "width, height, crop"은 명시될 수 있지만, upload.ini의 설정값이 최우선 입니다. (body의 값은 무시됩니다.)
- "crop"의 경우 배열로 "width"와 "height"가 명시되어야 합니다.

# Response
- 성공시 response code 200과 json형식으로 {"filename" : 저장된 파일 경로와 이름}을 리턴합니다.
- 실패시 response code 500과 json형식으로 {"Error" : Error_message}을 리턴합니다.
