---
layout: post
title:  "Lookup de serviços injetáveis no Angular"
date:   2017-09-20
categories: javascript typescript angular frontend
author: marcos
---
No episódio 16 do [Hipsters.tech](https://hipsters.tech/single-page-applications-hipsters-16/) ouvi a seguinte expressão "Angular é coisa de Javeiro". De fato, qualquer desenvolvedor Java vai se sentir em casa com o Angular 2+, fazendo uso de classes, orientação a objeto e injeção de dependências.

Quem já usou o CDI do Java EE, sabe que é possível solicitar a injeção de dependências em atributos de subclasses e superclasses, construtores e até como parâmetro de métodos específicos. Inclusive é possível pedir o lookup de Java Beans em objetos não gerenciados pelo CDI. Por outro lado, o Angular só oferece a injeção no construtor dos componentes e serviços, não sendo permitido utilizar essa funcionalidade em superclasses (creio eu que seja devido a limitações do JavaScript). O código abaixo ilustra essa deficiência que eu mencionei:

{% highlight typescript %}
export class CrudService {
    // Injeção não disponível, a subclasse é quem deve prover essa referencia
    constructor(private restangular: Restangular) {}

    public save() {
        // implementacao
    }
}

@Injectable()
export class ProdutoService extends CrudService {

    constructor(restangular: Restangular) {
        super(restangular); // Subclasse passando a referência para Superclasse
    }
}
{% endhighlight %}

No Angular 1.x (ou AngularJS para os íntimos), podemos fazer este lookup através da seguinte mandinga:

{% highlight typescript %}
var injector = angular.element(document).injector(); // Forma genérica para obter o $injector
var ServicoDesejado = injector.get('ServicoDesejado');
{% endhighlight %}

No Angular 2+, devido ao redesign total do framework e sua API, não conseguimos acessar o Injector dessa forma. Felizmente, é possível implementarmos uma classe/serviço que faça o lookup de uma maneira bem simples.

Vamos criar um serviço injetável chamado “InjectorUtils” conforme o código abaixo. Note que nele possuímos um método não-estático para atribuirmos a referência o Injector do Angular e outro método estático que usaremos para fazer o lookup da nossa dependência.

{% highlight typescript %}
import { Injectable, Injector, Type, InjectionToken } from '@angular/core';

@Injectable()
export class InjectorUtils {

    private static instance: Injector;

    public static lookup<T>(token: Type<T> | InjectionToken<T>, notFoundValue?: T): T {
        if (!InjectorUtils.instance) {
            throw new Error('You must set an Injector instance before try lookup a dependency!');
        }
        return InjectorUtils.instance.get(token, notFoundValue);
    }

    public setInstance(instance: Injector): void {
        if (!instance) {
            throw new Error('You cannot set a null instance of Injector!');
        }

        if (!!InjectorUtils.instance) {
            throw new Error('Another instance of Injector is already registred!');
        }

        InjectorUtils.instance = instance;
    }
}
{% endhighlight %}

Após isto, precisamos registrar ele no módulo raiz/principal da aplicação e também fornecer ao nosso serviço uma instancia de Injector:

{% highlight typescript %}
import { NgModule, Injector } from '@angular/core';

import { AppComponent } from './app.component';
import { InjectorUtils } from './utils/injector-utils';

@NgModule({
    declarations: [
        AppComponent
    ],
    providers: [
        InjectorUtils
    ],
    bootstrap: [AppComponent]
})
export class AppModule {

    constructor(injectorUtils: InjectorUtils, injector: Injector) {
        injectorUtils.setInstance(injector);
    }
}
{% endhighlight %}

Agora a classe “CrudService” que citei no início do post, pode fazer o lookup dos serviços sem depender da subclasse. E mais: o construtor dele fica livre para receber parâmetros que customizem seu funcionamento.

{% highlight typescript %}
import { InjectorUtils } from '../utils/injector-utils';
import { Restangular } from 'ngx-restangular';

export class CrudService {

    private _endpoint: Restangular;

    constructor(subResource: string) {
        const restangular = InjectorUtils.lookup(Restangular);
        this._endpoint = restangular.all(subResource);
    }

    // Os demais métodos foram omitidos para simplificar a classe
}
{% endhighlight %}

{% highlight typescript %}
import { Injectable } from '@angular/core';
import { CrudService } from './crud.service';

@Injectable()
export class ProdutoService extends CrudService {

    constructor() {
        super('produto'); // Nome do subpath do resource a ser consumido
    }
}
{% endhighlight %}

Se você quiser baixar o código-fonte deste exemplo, disponibilizei um projeto baseado no Angular CLI no meu [GitHub](https://github.com/salvadormarcos/pogamando-exemplos/tree/master/angular-lookup-servicos-injetaveis).