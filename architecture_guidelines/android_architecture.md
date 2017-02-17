# Architecture Guidelines

A arquitetura de nossa aplicação Android é baseado no [MVP](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter) (Model View Presenter) pattern.

![](architecture_diagram.png)

* __Presentantion Layer__:
	- __Presenter__: Subscribe em observables providos pelo `UseCase` e processa os dados para ordem de chamada dos métodos da view.

	- __Activities, Fragments, ViewGroups (View)__:  Componentes padrão do Android que implementam métodos que a presenter podem chamar. Eles também controlam as interações do usuários como click do usuário, chamando os métodos correspondentes da presenter.
	
* __Domain Layer__:
	- __Domain Use Case__: Representa as regras de negócios, acionados pela presenter via subscribe para gravar e/ou recuperar/listar os dados com Observables do RxJava.
	
* __Data Layer__:
	- __Repository__: É reponsável por salvar, recuperar/listar dados de um banco de dados local ou banco na nuvem (Firebase). Também pode representar o consumo de uma API Restful.


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
