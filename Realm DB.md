적은 코드로 DB를 작성할 수 있다(SQL 문법을 몰라도 사용 가능)

## Realm 사용 준비
1. 프로젝트 수준의 build.gradle 파일 dependencies 항목에 (classpath "io.realm:realm-gradle-plugin:5.12.0") 추가   
2. 모듈 수준의 build.gradle 파일 상단에 (apply plugin: 'kotlin-kapt'), (apply plugin: 'realm-android') 추가   
3. sync now   
4. 테이블 정보를 다룰 모델 클래스를 새로 작성(Realm 객체로 만드는 방법 참고)

## Realm 객체로 만드는 방법
Realm에서 테이블로 사용하려면 모델 클래스 앞에 open을 붙이고 RealmObject 클래스를 상속 받는다
```
open class Dog(
  @PrimaryKey val id: Long = 0,
  var name: String = "",
  var age: Int = 0
) : RealmObject() {
  // ...
}
```
