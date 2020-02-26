적은 코드로 DB를 작성할 수 있다(SQL 문법을 몰라도 사용 가능)

## Realm 사용 준비
1. 프로젝트 수준의 build.gradle 파일 dependencies 항목에 (classpath "io.realm:realm-gradle-plugin:5.12.0") 추가   
2. 모듈 수준의 build.gradle 파일 상단에 (apply plugin: 'kotlin-kapt'), (apply plugin: 'realm-android') 추가   
3. sync now   
4. 테이블 정보를 다룰 모델 클래스를 새로 작성(Realm 객체로 만드는 방법 참고)   

## Realm 객체로 만드는 방법
Realm에서 테이블로 사용하려면 모델 클래스 앞에 open을 붙이고 RealmObject 클래스를 상속 받는다
```
open class Todo(
    @PrimaryKey val id: Long = 0,
    var title: String = "",
    var date: Long = 0
) : RealmObject() {

}
```

## Realm 초기화
앱이 실행될 때 제일 먼저 Realm을 초기화해서 다른 액티비티가 사용하도록 할 수 있다. 앱을 실행하면 가장 먼저 실행되는 애플리케이션 객체를 상속하여 Realm을 초기화 한다.   
File - New - Kotlin File/Class 에서 새로운 클래스 생성
```
class MyApplication : Application() {  // 가정 먼저 실행되는 Application 클래스 상속 받는다
    override fun onCreate() {  // 메서드가 생성되기 전에 호출
        super.onCreate()
        Realm.init(this)  Realm.init() 메서드를 사용하여 초기화
    }
}
```

매니패스트 파일을 열고 application 태그 안에 name 속성 추가. 앱에서 사용하는 전체 액티비티에서 공통 사용하는 객체를 초기화할 때 이러한 방법 사용
```
<application
  android:name=".MyApplication(생성한 클래스 명)"
  ...
</application>
```

## 액티비티에서 Realm 인스턴스 객체 얻기
사용할 화면의 Kotlin클래스에 Realm을 사용하기 위해 인스턴스를 얻는다
```
val realm = Realm.getDefaultInstance()  // 인스턴스 얻기

override fun onDestroy() {
        super.onDestroy()
        realm.close()  // 인스턴스 해제
    }
```

## 작업 단위
beginTransaction() 메서드와 commitTransaction() 메서드 사이에 작성한 코드들은 전체가 하나의 작업
```
realm.beginTransaction()  // 트랜잭션 시작

// ...                    // 작업(추가, 삭제, 업데이트)

realm.commitTransaction()  // 트랜잭션 종료
```

## 어댑터 작성
Realm을 사용할 때는 Realm에서 제공하는 RealmBaseAdapter 클래스 상속받는다

모듈수준의 gradle파일에 추가
```
dependencies {
    implementation 'io.realm:android-adapters:3.1.0'
}
```

File - New - Kotlin File/Class로 새로운 클래스 생성후 RealmBaseAdapter상속
```
class TodoListAdapter(realmResult: OrderedRealmCollection<Todo>)
    : RealmBaseAdapter<Todo>(realmResult) {
}
```
RealmBaseAdapter는 미구현 클래스이므로 (Alt+Enter)fh Implement members를 클릭하여 미구현 메서드 구현한다   
getView() 메서드 선택 (getView() 메서드 오버라이드)

















