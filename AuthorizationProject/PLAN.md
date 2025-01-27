# Структура объектов

* Создать объект **Пользователь**
```kotlin
data class User(
    val id: Int,
    val login: String,
    val pass: String,
    val salt: String,

)
```

* Создать перечисление **Ролей**
```kotlin
enum class Roles() {
    READ,
    WRITE,
    EXECUTE
}
```

* Создать объект **РольРесурс**
```kotlin
// {user resourse role}
class RoleResourses {
    TODO ...
}
```

* Создать объект **Аргумент**
```kotlin
data class Arguments(
    val login: String?,
    val password: String?,
    val role: String?,
    val resourse: String?,
    val ds: String?,
    val de: String?,
    val vol: Int?
)
```

* Создать объект **КодСтатус**
```kotlin
enum class CodeExecute(val statusCode: Int){
    TRUE(0),
    HELP(1),
    NOT_FORMAT_LOGIN(2),
    NOT_LOGIN(3),
    NOT_PASSWORD(4),
    UNKNOWN_ROLES(5),
    FORBIDDEN(6),
    INCORRECT_ACTIVITY(7)
}
```

* Создать объект **БазаДанныхПровайдер**
```kotlin
import SourseData.*

class DataBaseProvider(val sourseData: SourseData) {
    
}
```

* Создать объект **ИсточникДанных**
```kotlin
package SourseData

class SourseData {
    public val roleResourses: List<RoleResourses> = listOf(...)
    public val users: List<User> = listOf(...)
}
```

* Создать объект **АутентификацииПровайдер**
```kotlin
// Для аутентификации (логин + pass)
class AuthenticationProvider {

    companion object {

    }

    TODO ...
}
```

* Создать объект **АвторизацияПровайдер**
```kotlin
// Для авторизации (user + resourse)
class AuthorizeProvider {
    TODO ...
}
```



* Создать объект **Утилит**
```kotlin
// Для хеширования паролей
import java.security.MessageDigest
import kotlin.random.Random
import kotlinx.cli.*

class Utils {
    companion object {

    }
}
```


# Методы объектов
## Объект: **База Данных Провайдер** 

* Метод: Возвращающий объект пользователя по логину
```kotlin
fun getUserByLogin(login: String): User? {
    return sourseData.users.find { it.login == login }
}
```

* Метод: Возвращающий пароль по логину
```kotlin
fun getPasswordByLogin(login: String): String {
    return getUserByLogin(login)!!.pass
}
```

* Метод: Возвращающий соль по логину
```kotlin
fun getSaltByLogin(login: String): String {  
    return getUserByLogin(login)!!.salt
}
```

* Метод: Проверяющий наличие такого логина в БД
```kotlin
fun hasLogin(login: String): Boolean {
    return getUserByLogin(login) != null
}
```

## Объект: **АутентификацииПровайдер** 

* Метод: Начать Аутентификацию
```kotlin
fun authenticate(login: String, password: String): User {
    if (!loginValidate(login)){
        print("...")
        System.exit(CodeExecute.NOT_FORMAT_LOGIN.statusCode)
    }
    if (!DataBaseProvider.hasLogin(login)){
        print("...")
        System.exit(CodeExecute.NOT_LOGIN.statusCode)
    }
    if (!passwordValidate(login, password)){
        print("...")
        System.exit(CodeExecute.NOT_PASSWORD.statusCode)
    }

    val user: User = DataBaseProvider.getUserByLogin(login)

    return user

}
```

* Метод: Проверяющий валидный ли логин
```kotlin
fun loginValidate(login: String): Boolean {
    login
    // TODO ... какое-нибудь условие
}
```

* Метод: Проверяющий совпадение паролей
```kotlin
fun passwordValidate(login: String, password: String): Boolean {
    val salt = DataBaseProvider.getSaltByLogin(login)
    val resultPassword = DataBaseProvider.getPasswordByLogin(login)

    return Utils.encode(argPass, salt) == resultPassword
}
```

* Метод: TODO ...
```kotlin

```

## Объект: **АвторизацияПровайдер** 

* Метод: TODO ...
```kotlin

```

## Объект: **КодСтатус** 

* Метод: TODO ...
```kotlin

```

## Объект: **Утилит** 

* Метод: Генерирует соль случайно
```kotlin
fun genearateSalt(): String = Random.nextBytes(64).joinToString("") { "%02x".format(it) }
```

* Метод: Шифрует пароль
```kotlin
fun encode(password: String, salt: String): String = hashCode(hashCode(password) + salt)
```

* Метод: Парсит строку и возвращает объект Аргументов
```kotlin
fun parseArguments(args: Array<String>): Arguments {

    val parser = ArgParser("Authorization project")

    val login by parser.option(ArgType.String, shortName = "login", description = "Input login")
    val password by parser.option(ArgType.String, shortName = "password", description = "Input password")
    val role by parser.option(ArgType.String, shortName = "role", description = "Input role")
    val resourse by parser.option(ArgType.String, shortName = "resourse", description = "Input resource")
    val ds by parser.option(ArgType.String, shortName = "ds", description = "Input date start: YYYY-m-d")
    val de by parser.option(ArgType.String, shortName = "de", description = "Input date finish: YYYY-m-d")
    val vol by parser.option(ArgType.String, shortName = "vol", description = "Input number")

    parser.parse(args)

    return Arguments(login, password, role, resourse, ds, de, vol)

}
```


# Тестовые данные

## Список: **Пользователей**
```kotlin

1. admin(login = admin, password = 0000)
2. user(login = user, password = 1111)

```

## Список: **Ресурсов**
```kotlin

1. admin READ A
2. admin READ B
3. admin

```


# Тестовые сценарии
```kotlin

TODO ...

```


# Точка входа в приложение
```kotlin

fun main(args: Array<String>){
    val arguments: Arguments = Urils.parseArguments(args)

    val user: User = AuthenticationProvider.authenticate(arguments.login, arguments.password)

    // TODO ...

}


```



