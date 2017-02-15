# Architecture Guidelines

A arquitetura de nossa aplicação Android é baseado no [MVP](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter) (Model View Presenter) pattern.

* __View (UI layer)__: Este é o local onde Activities, Fragments e outros componentes Android vivem. Sua responsabilidade é mostrar dados fornecidos pela presenter para o usuário. A View também controla as interações do usuário, tais como click listeners, acionando a ação correta da presenter se necessário.

* __Presenter__: Responde as ações da UI controlando a interação entre a View e o Model, solicitando as regras de negócias para um ou mais UseCase/Interactor utilizando subscribe de RxJava para controle de qual método da view será acionado para exibição dos dados.

* __Model (Data Layer)__: É reponsável por salvar, recuperar/listar dados de um banco de dados local ou banco na nuvem (Firebase). Também pode representar o consumo de uma API Restful.

* __Model (Domain Layer)__: Representa as regras de negócios, acionados pela presenter via subscribe para gravar e/ou recuperar/listar os dados com Observables do RxJava

this is responsible for retrieving, saving, caching and massaging data. It can communicate with local databases and other data stores as well as with restful APIs or third party SDKs. It is divided in two parts: a group of helpers and a `DataManager`. The number of helpers vary between project and each of them has a very specific function, e.g. talking to an API or saving data in `SharedPreferences`. The `DataManager` combines and transforms the outputs from different helpers using Rx operators so it can: 1) provide meaningful data to the Presenter,  2) group actions that will always happen together. This layer also contains the actual model classes that define how the data structure is.

![](architecture_diagram.png)

Looking at the diagram from right to left:

* __Helpers (Model)__: A set of classes, each of them with a very specific responsibility. Their function can range from talking to APIs or a database to implementing some specific business logic. Every project will have different helpers but the most common ones are:
	- __DatabaseHelper__: It handles inserting, updating and retrieving data from a local SQLite database. Its methods return Rx Observables that emit plain java objects (models)
	- __PreferencesHelper__: It saves and gets data from `SharedPreferences`, it can return Observables or plain java objects directly.
	- __Retrofit services__ : [Retrofit](http://square.github.io/retrofit) interfaces that talk to Restful APIs, each different API will have its own Retrofit service. They return Rx Observables.

* __Data Manager (Model)__: It's a key part of the architecture. It keeps a reference to every helper class and uses them to satisfy the requests coming from the presenters. Its methods make extensive use of Rx operators to combine, transform or filter the output coming from the helpers in order to generate the desired output ready for the Presenters. It returns observables that emit data models.

* __Presenters__: Subscribe to observables provided by the `DataManager` and process the data in order to call the right method in the View.

* __Activities, Fragments, ViewGroups (View)__: Standard Android components that implement a set of methods that the Presenters can call. They also handle user interactions such as clicks and act accordingly by calling the appropriate method in the Presenter. These components also implement framework-related tasks such us managing the Android lifecycle, inflating views, etc.

* __Event Bus__: It allows the View components to be notified of certain types of events that happen in the Model. Generally the  `DataManager` posts events which can then be subscribed to by Activities and Fragments. The event bus is __only used for very specific actions__ that are not related to only one screen and have a broadcasting nature, e.g. the user has signed out.

# License

```
Copyright 2015 Ribot Ltd.

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
