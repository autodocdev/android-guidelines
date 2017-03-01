# 1. Project guidelines

## 1.1 Project structure

Novos projetos devem seguir a estrutura Gradle Android que é definida no [Android Gradle plugin user guide](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Project-Structure).

## 1.2 File naming

### 1.2.1 Class files
Nome de classes são escritos em [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase).

Para classes que extendem um componente do Android, o nome da classe deve terminar com o nome do componente; por exemplo: `SignInActivity`, `SignInFragment`, `ImageUploaderService`, `ChangePasswordDialog`.

### 1.2.2 Resources files

Resources file names são escritos em __lowercase_underscore__.

#### 1.2.2.1 Drawable files

Conversões de nomenclatura para drawables:


| Asset Type   | Prefix            |		Example               |
|--------------| ------------------|-----------------------------|
| Action bar   | `ab_`             | `ab_stacked.9.png`          |
| Button       | `btn_`	            | `btn_send_pressed.9.png`    |
| Dialog       | `dialog_`         | `dialog_top.9.png`          |
| Divider      | `divider_`        | `divider_horizontal.9.png`  |
| Icon         | `ic_`	            | `ic_star.png`               |
| Menu         | `menu_	`           | `menu_submenu_bg.9.png`     |
| Notification | `notification_`	| `notification_bg.9.png`     |
| Tabs         | `tab_`            | `tab_pressed.9.png`         |

Conversões de nomenclatura para icons (tirado de [Android iconography guidelines](http://developer.android.com/design/style/iconography.html)):

| Asset Type                      | Prefix             | Example                      |
| --------------------------------| ----------------   | ---------------------------- |
| Icons                           | `ic_`              | `ic_star.png`                |
| Launcher icons                  | `ic_launcher`      | `ic_launcher_calendar.png`   |
| Menu icons and Action Bar icons | `ic_menu`          | `ic_menu_archive.png`        |
| Status bar icons                | `ic_stat_notify`   | `ic_stat_notify_msg.png`     |
| Tab icons                       | `ic_tab`           | `ic_tab_recent.png`          |
| Dialog icons                    | `ic_dialog`        | `ic_dialog_info.png`         |

Convensões de nomenclatura para selector states:

| State	       | Suffix          | Example                     |
|--------------|-----------------|-----------------------------|
| Normal       | `_normal`       | `btn_order_normal.9.png`    |
| Pressed      | `_pressed`      | `btn_order_pressed.9.png`   |
| Focused      | `_focused`      | `btn_order_focused.9.png`   |
| Disabled     | `_disabled`     | `btn_order_disabled.9.png`  |
| Selected     | `_selected`     | `btn_order_selected.9.png`  |


#### 1.2.2.2 Layout files

Os arquivos de layout devem corresponder ao nome dos componentes do Android para os quais eles se destinam, mas movendo o nome do componente para o ínicio. Por exemplo, se estivermos criando um layout para o `SignInActivity`, o nome do arquivo de layout deve ser` activity_sign_in.xml`.

| Component        | Class Name             | Layout Name                   |
| ---------------- | ---------------------- | ----------------------------- |
| Activity         | `UserProfileActivity`  | `activity_user_profile.xml`   |
| Fragment         | `SignUpFragment`       | `fragment_sign_up.xml`        |
| Dialog           | `ChangePasswordDialog` | `dialog_change_password.xml`  |
| AdapterView item | ---                    | `item_person.xml`             |
| Partial layout   | ---                    | `partial_stats_bar.xml`       |

Um caso ligeiramente diferente é quando estamos criando um layout que vai ser inflado por um `Adapter`, por exemplo, para preencher um` ListView`. Neste caso, o nome do layout deve começar com `item_`.

Observe que há casos em que essas regras não serão possíveis de aplicar. Por exemplo, ao criar arquivos de layout que se destinam a fazer parte de outros layouts. Neste caso, você deve usar o prefixo `partial_`.

#### 1.2.2.3 Menu files

Semelhante aos arquivos de layout, os arquivos de menu devem corresponder ao nome do componente. Por exemplo, se estivermos definindo um arquivo de menu que vai ser usado no `UserActivity`, então o nome do arquivo deve ser` activity_user.xml`.

Uma boa prática é não incluir a palavra `menu` como parte do nome porque esses arquivos já estão localizados no diretório` menu`.

#### 1.2.2.4 Values files

Resource files da pasta values devem ser __plural__, e.g. `strings.xml`, `styles.xml`, `colors.xml`, `dimens.xml`, `attrs.xml`

# 2 Code guidelines

## 2.1 Java language rules

### 2.1.1 Não ignore exceptions

Você nunca deve fazer o seguinte:

```java
void setServerPort(String value) {
    try {
        serverPort = Integer.parseInt(value);
    } catch (NumberFormatException e) { }
}
```

_Você pode pensar que seu código nunca irá encontrar esta condição de erro ou que não é importante para lidar com isso, ignorando exceções como acima cria "minas/armadilhas" em seu código para alguém tropeçar em algum dia. Você deve lidar com todas as exceções no seu código de alguma maneira com princípios. A manipulação específica varia de acordo com o caso._ - ([Android code style guidelines](https://source.android.com/source/code-style.html))

Veja alternativas [aqui](https://source.android.com/source/code-style.html#dont-ignore-exceptions).

### 2.1.2 Não utilize a Exception generica

Você não deve fazer isso:

```java
try {
    someComplicatedIOFunction();        // may throw IOException
    someComplicatedParsingFunction();   // may throw ParsingException
    someComplicatedSecurityFunction();  // may throw SecurityException
    // phew, made it all the way
} catch (Exception e) {                 // I'll just catch all exceptions
    handleError();                      // with one generic handler!
}
```

Veja o porque e algumas alternativas [aqui](https://source.android.com/source/code-style.html#dont-catch-generic-exception)

### 2.1.3 Don't use finalizers

_Não usamos finalizers. Não há garantias quanto a quando um finalizer será chamado, ou mesmo que ele será chamado em tudo. Na maioria dos casos, você pode fazer o que você precisa de um finalizer com uma boa manipulação de exception. Se você absolutamente precisar dele, defina um método `close ()` (ou similar) e documente exatamente quando esse método precisa ser chamado. Veja `InputStream` para um exemplo. Nesse caso, é apropriado, mas não é necessário imprimir uma mensagem de log curto do finalizador, desde que não se espere que inunde os logs._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#dont-use-finalizers))


### 2.1.4 Fully qualify imports

Isso é ruim: `import foo.*;`

Isso é bom: `import foo.Bar;`

Veja mais informações [aqui](https://source.android.com/source/code-style.html#fully-qualify-imports)

## 2.2 Java style rules

### 2.2.1 Definição de campos e nomenclatura

Campos devem ser definidos no __começo do arquivo__ e eles devem ser as regras de nomenclatura a seguir.

* Nome de campos: Private, non-static começam com __m__.
* Nome de campos: Private, static field começam com __s__.
* Outros campos começam com letras lower case.
* Campos Static final (constants) são ALL_CAPS_WITH_UNDERSCORES.

Example:

```java
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    public int publicField;
    private static MyClass sSingleton;
    int mPackagePrivate;
    private int mPrivate;
    protected int mProtected;
}
```

### 2.2.3 Trate acrônimos como palavras

| Bom           | Ruim            |
| -------------- | -------------- |
| `XmlHttpRequest` | `XMLHTTPRequest` |
| `getCustomerId`  | `getCustomerID`  |
| `String url`     | `String URL`     |
| `long id`        | `long ID`        |

### 2.2.4 Use espaço para identação

Use __4 espaços__ para identação de blocos:

```java
if (x == 1) {
    x++;
}
```

Use __8 space__ para identação de linhas quebradas:

```java
Instrument i =
        someLongExpression(that, wouldNotFit, on, one, line);
```

### 2.2.5 Use o padrão de abertura de chaves

As aberturas de chaves seguem a linha do código atual.

```java
class MyClass {
    int func() {
        if (something) {
            // ...
        } else if (somethingElse) {
            // ...
        } else {
            // ...
        }
    }
}
```

Aberturas de chaves são requiridas a menos que a condição e o corpo do código caibam em uma linha.

Se a condição e o corpo cabem em uma linha e a linhas é menor que o tamanho máximo da linhas, então a chave não é requerida, e.g.

```java
if (condition) body();
```

Isso é __ruim__:

```java
if (condition)
    body();  // bad!
```

### 2.2.6 Annotations

#### 2.2.6.1 Praticas de Annotations

De acordo com o Android code style code, as praticas padões para algumas annotations predefinidas em java são:

* `@Override`: A annotation @Override __deve ser usada__ sempre que um metodo overrides a declaração ou implementação da super-class. 

* `@SuppressWarnings`: A @SuppressWarnings annotation deve ser usada somente sobre circunstancia onde é impossível eliminar o warning.

Quando a annotation `@SuppressWarnings` é necessária o código deve ser anotado com um comentário TODO com a explicação do porque foi impossível eliminar o `@SuppressWarnings`, e.g.

```java
// TODO: The third-party class com.third.useful.Utility.rotate() needs generics
@SuppressWarnings("generic-cast")
List<String> blix = Utility.rotate(blax);
```

Mais informações sobre annotation guidelines podem ser encontradas [aqui](http://source.android.com/source/code-style.html#use-standard-java-annotations).

#### 2.2.6.2 Annotations style

__Classes, Metodos e Construtores__

Quando annotations são aplicata em uma classe, metodo, ou construtor, eles são listados após o bloco de documentação e devem aparecer como __uma annotation por linha__ .

```java
/* This is the documentation block about the class */
@AnnotationA
@AnnotationB
public class MyAnnotatedClass { }
```

__Campos__

Annotations aplicadas em campos devem ser listadas __na mesma linha__, a menos que a linha atinga o limite máximo de tamanho.

```java
@Nullable @Mock DataManager mDataManager;
```

### 2.2.7  Limitar o escopo da variável

_O escopo das variáveis locais deve ser mantido ao mínimo (Item Java Eficaz 29). Ao fazer isso, você aumenta a legibilidade e a capacidade de manutenção do seu código e reduz a probabilidade de erro._

_As variáveis locais devem ser declaradas no ponto em que são usadas pela primeira vez. Quase todas as declarações de variáveis locais devem conter um inicializador. Se ainda não tiver informações suficientes para inicializar uma variável de forma sensata, deverá adiar a declaração._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#limit-variable-scope))

### 2.2.8 Order import statements

Se você estiver usando uma IDE como o Android Studio, não precisa se preocupar com isso porque sua IDE já está obedecendo a essas regras. Se não, dê uma olhada abaixo.

A ordem das declarações de importação é:

1. Android imports
2. Importação de terceiros (com, junit, net, org)
3. java and javax
4. Importações do mesmo projeto

Para corresponder exatamente às configurações IDE, as importações devem ser:

* Ordem alfabética dentro de cada agrupamento, com letras maiúsculas antes de letras minúsculas (por exemplo, Z antes de a).
* Deve haver uma linha em branco entre cada grande agrupamento (android, com, junit, net, org, java, javax).

Mais informações [here](https://source.android.com/source/code-style.html#limit-variable-scope)

### 2.2.9 Logging guidelines

Use os métodos de log fornecidos pela classe `Log` para imprimir mensagens de erro ou outras informações que podem ser úteis para os desenvolvedores identificarem problemas:

* `Log.v(String tag, String msg)` (verbose)
* `Log.d(String tag, String msg)` (debug)
* `Log.i(String tag, String msg)` (information)
* `Log.w(String tag, String msg)` (warning)
* `Log.e(String tag, String msg)` (error)

Como regra geral, usamos o nome da classe como tag e o definimos como um campo `static final` na parte superior do arquivo. Por exemplo:

```java
public class MyClass {
    private static final String TAG = MyClass.class.getSimpleName();

    public myMethod() {
        Log.e(TAG, "My error message");
    }
}
```

Logs VERBOSE e DEBUG devem ser desabilitados no build releas. Também é recomendado desabilitar os logs de INFORMATION, WARNING e ERROR, mas você pode manter isso habilitado se você acha que pode ser útil para identificar problemas no build release. Se você decidir deixar eles habilitados, você tem que ter certeza que eles não estão vazando informações privadas, como endereço de email, id de usuário, etc.

Para mostrar logs somente em build debug:

```java
if (BuildConfig.DEBUG) Log.d(TAG, "The value of x is " + x);
```

### 2.2.10 Ordenação de membros da classe

Não existe uma única solução para isso, mas usando uma ordenação __lógica__ e __consistente__ irá melhorar a capacidade de aprendizado e legibilidade do código. É recomendável usar a seguinte ordem:

1. Constants
2. Fields
3. Constructors
4. Override methods and callbacks (public or private)
5. Public methods
6. Private methods
7. Inner classes or interfaces

Example:

```java
public class MainActivity extends Activity {

	private String mTitle;
    private TextView mTextViewTitle;

    public void setTitle(String title) {
    	mTitle = title;
    }

    @Override
    public void onCreate() {
        ...
    }

    private void setUpView() {
        ...
    }

    static class AnInnerClass {

    }

}
```

Se sua classe extends de algum __componente do Android__ como Activity ou Fragment, é boa pratica ordenar os métodos override equivalente ao __ciclo de vida do componente__. Por exemplo, se você tem uma Activity que implementa `onCreate()`, `onDestroy()`, `onPause()` and `onResume()`, então a ordenação correta é:

```java
public class MainActivity extends Activity {

	//Order matches Activity lifecycle
    @Override
    public void onCreate() {}

    @Override
    public void onResume() {}

    @Override
    public void onPause() {}

    @Override
    public void onDestroy() {}

}
```

### 2.2.11 Ordenação de parâmetro de métodos 

Programando em Android, é comum definir métodos que levam o `Context` como parâmetro. Se você esta escrevendo um método como esse, então o __Context__ deve ser o primeiro parâmetro.

A exceção é na utilização de interfaces __callback__ que devem sempre ser o __último__ parâmetro

Examplos:

```java
// Context sempre é o primeiro
public User loadUser(Context context, int userId);

// Callbacks sempre é o último
public void loadUserAsync(Context context, int userId, UserCallback callback);
```

### 2.2.13 String constants, naming, and values

Muitos elementos do SDK do Android como `SharedPreferences`, `Bundle`, ou `Intent` usam par de chave-valor, por isso é muito provável que mesmo para um pequeno aplicativo que você acaba escrevendo um monte de constantes String. 

Ao usar um desses componentes, você deve definir as chaves como campos `static final` e eles devem ser prefixados conforme indicado abaixo.

| Elemento            | Prefix do nome de campo |
| -----------------  | ------------------- |
| SharedPreferences  | `PREF_`             |
| Bundle             | `BUNDLE_`           |
| Fragment Arguments | `ARGUMENT_`         |
| Intent Extra       | `EXTRA_`            |
| Intent Action      | `ACTION_`           |
| Bundle State       | `STATE_`            |

Note que os argumentos de um Fragment - `Fragment.getArguments()` - também são um Bundle. Entretando, como é muito comum o uso de Bundles, nós definimos um prefixo diferente para eles.

Examplo:

```java
// Observe que o valor do campo é igual ao nome para evitar problemas de duplicação
static final String PREF_EMAIL = "PREF_EMAIL";
static final String BUNDLE_AGE = "BUNDLE_AGE";
static final String STATE_IMAGE = "STATE_IMAGE";
static final String ARGUMENT_USER_ID = "ARGUMENT_USER_ID";

// Itens relacionados à intent usam o nome do pacote completo como valor
static final String EXTRA_SURNAME = "com.myapp.extras.EXTRA_SURNAME";
static final String ACTION_OPEN_USER = "com.myapp.action.ACTION_OPEN_USER";
```

### 2.2.14 Argumentos em Fragments e Activities

Quando um dado é passado para uma `Activity` ou `Fragment` via `Intent` ou um `Bundle`, as chaves para os diferentes valores devem seguir as seguintes regras descritas abaixo.

Quando a `Activity` ou `Fragment` espera argumentos, deve ser provido um método `public static` para facilitar a criação da `Intent` ou `Fragment`.

No caso de Activities o método é geralmente chamado `getStartIntent()`
In the case of Activities the method is usually called `getStartIntent()`:

```java
public static Intent getStartIntent(Context context, User user) {
	Intent intent = new Intent(context, ThisActivity.class);
	intent.putParcelableExtra(EXTRA_USER, user);
	return intent;
}
```

Para Fragments é nomeado `newInstance()` e controlam a criação do Fragment com os argumentos corretos:

```java
public static UserFragment newInstance(User user) {
	UserFragment fragment = new UserFragment;
	Bundle args = new Bundle();
	args.putParcelable(ARGUMENT_USER, user);
	fragment.setArguments(args)
	return fragment;
}
```

__Note 1__: Esses métodos devem ficar no topo da classe antes do `onCreate()`.

__Note 2__: Se fornecemos os métodos descritos acima, as chaves para extras e argumentos devem ser `private` porque não há necessidade de serem expostos fora da classe.

### 2.2.15 Limite de tamanho de linha

Linhas de código não devem exceder __100 caracteres___. Se a linha é maior que o limite, existem duas opções para redução de seu tamanho:

* Extrair uma variável ou método local (preferível).
* Aplicar a quebra de linha para transformar em multiplas linhas.

Há duas __exceções__ onde é possível ter linhas com mais de 100:

* Linhas que não são possível dividir, e.g. URLs longas nos comentários
* Declarações de `package` e `import`.

#### 2.2.15.1 Line-wrapping strategies

Não existe uma fórmula exata que explique como quebrar linhas e muitas vezes soluções diferentes são válidas. No entanto, existem algumas regras que podem ser aplicadas a casos comuns.

__Quebra nos operadores__

Quando a linha é quebrada em um operador, a quebra vem __antes___ do operador. Por exemplo:

```java
int longName = anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne
        + theFinalOne;
```

__Exceção do Operador de Atribuição__

Uma exceção para `quebra de linha nos operadores` é quando utilizamos o operador `=`, neste caso a linha deve quebrar __depois__ do operador.

```java
int longName =
        anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne + theFinalOne;
```

__Cadeia de métodos__

Quando multiplos métodos são encadeados na mesma linha - por exemplo usando Builders - cada chamado para método deve ficar em sua própia linha, quebrando a linha antes do `.`

```java
Picasso.with(context).load("http://ribot.co.uk/images/sexyjoe.jpg").into(imageView);
```

```java
Picasso.with(context)
        .load("http://ribot.co.uk/images/sexyjoe.jpg")
        .into(imageView);
```

__Parâmetros longos__

Quando um método tem muitos parâmetros ou seus parâmetros são muito longos, neste caso devemos quebrar a linha a cada vírgula `,`

```java
loadPicture(context, "http://ribot.co.uk/images/sexyjoe.jpg", mImageViewProfilePicture, clickListener, "Title of the picture");
```

```java
loadPicture(context,
        "http://ribot.co.uk/images/sexyjoe.jpg",
        mImageViewProfilePicture,
        clickListener,
        "Title of the picture");
```

### 2.2.16 Cadeia de operadores RxJava

Encadeamento de operadores Rx requerem quebra de linha. Todo operador deve ficar em uma linha nova e deve ser quebrada antes do `.`

```java
public Observable<Location> syncLocations() {
    return mDatabaseHelper.getAllLocations()
            .concatMap(new Func1<Location, Observable<? extends Location>>() {
                @Override
                 public Observable<? extends Location> call(Location location) {
                     return mRetrofitService.getLocation(location.id);
                 }
            })
            .retry(new Func2<Integer, Throwable, Boolean>() {
                 @Override
                 public Boolean call(Integer numRetries, Throwable throwable) {
                     return throwable instanceof RetrofitError;
                 }
            });
}
```

## 2.3 Regras de estilos XML

### 2.3.1 Use a tag de fechamento automático

Quando um elemente não tem conteúdo, você __deve__ utilizar a tag de fechamento automático.

Isso é bom:

```xml
<TextView
	android:id="@+id/text_view_profile"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content" />
```

Isso é __ruim__ :

```xml
<!-- Não faça isso! -->
<TextView
    android:id="@+id/text_view_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
</TextView>
```


### 2.3.2 Nomenado Resources

Nome e IDs de Resources são escritos em __lowercase_underscore__.

#### 2.3.2.1 Nomeação de ID

IDs devem ser pré fixados com o nome do elemento em lowercase underscore. Por exemplo:


| Element            | Prefix            |
| -----------------  | ----------------- |
| `TextView`           | `text_`             |
| `ImageView`          | `image_`            |
| `Button`             | `button_`           |
| `Menu`               | `menu_`             |

Exemplo de Image view:

```xml
<ImageView
    android:id="@+id/image_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

Exemplo de Menu:

```xml
<menu>
	<item
        android:id="@+id/menu_done"
        android:title="Done" />
</menu>
```

#### 2.3.2.2 Strings

Nomes de String começam com um prefixo que identificam a seção que elas pertencem. Por exemplo `registration_email_hint` ou `registration_name_hint`. Se a string __não pertence__ a nenhuma seção, então você deve seguir as seguintes regras:


| Prefix             | Description                           |
| -----------------  | --------------------------------------|
| `error_`             | An error message                      |
| `msg_`               | A regular information message         |
| `title_`             | A title, i.e. a dialog title          |
| `action_`            | An action such as "Save" or "Create"  |



#### 2.3.2.3 Styles e Themes

Nomes de estilos são escritos em __UpperCamelCase__.

### 2.3.3 Ordenação de Atributos

Como regra geral você deve tentar agrupar atributos similares juntos. Uma boa maneira de ordenar os atributos mais comuns é:

1. View Id
2. Style
3. Layout width e layout height
4. Outros atributos de layout, ordenados alfabeticamente
5. Atributos restantes, ordenados alfabeticamente

## 2.4 Regras de Testes

### 2.4.1 Unit tests

Os nomes das classes de testes devem equivaler aos nomes das classes que serão testadas, seguindo pela palavra `Test`. Por exemplo, se criarmos uma classes de testes para testar a classe `DatabaseHelper`, nós devemos nomear para `DatabaseHelperTest`.

Os métodos de teste são anotados com `@Test` e geralmente devem começar com o nome do método que está sendo testado, seguido por uma pré-condição e / ou comportamento esperado.

* Template: `@Test void methodNamePreconditionExpectedBehaviour()`
* Example: `@Test void signInWithEmptyEmailFails()`

A condição prévia e / ou o comportamento esperado podem nem sempre ser necessários se o teste for suficientemente claro sem eles.

Às vezes, uma classe pode conter uma grande quantidade de métodos, que ao mesmo tempo exigem vários testes para cada método. Neste caso, é recomendável dividir a classe de teste em vários. Por exemplo, se o `DataManager` contiver muitos métodos, poderemos dividi-lo em` DataManagerSignInTest`, `DataManagerLoadUsersTest`, etc. Geralmente, você será capaz de ver quais testes pertencem juntos porque eles têm comum [test fixtures](https://en.wikipedia.org/wiki/Test_fixture).

### 2.4.2 Espresso tests

Cada classe de teste do Espresso normalmente tem como alvo uma activity, portanto o nome deve corresponder ao nome da atividade segmentada seguida por `Test`, e.g. `SignInActivityTest`

Ao usar o Espresso API é uma prática comum colocar métodos encadeados em novas linhas.

```java
onView(withId(R.id.view))
        .perform(scrollTo())
        .check(matches(isDisplayed()))
```

# License

```
Copyright 2017 Autodoc Ltd.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
