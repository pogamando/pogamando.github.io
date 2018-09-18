---
layout: post
title:  "Validação condicional com o Hibernate Validator"
date:   2016-11-06
categories: java java-ee bean-validator hibernate-validator
author: marcos
---
O Bean Validator ([JSR 303](http://jcp.org/en/jsr/detail?id=303)) simplifica o trabalho de validar dados na aplicação. Basta anotar o Java Bean com alguma constraint e o Validator se encarrega de fazer o "trabalho sujo".

Porém, em certas situações, precisamos que algumas constraints sejam aplicadas condicionalmente. Imagine a seguinte contexto: "O sistema possui um cadastro de clientes que aceita clientes do tipo Pessoa Física (CPF) e Pessoa Jurídica (CNPJ). A aplicação deve executar a validação correspondente para cada tipo de cliente". Dentro deste cenário, nossa classe de entidade poderia ser modelada da seguinte forma:

{% highlight java %}
// Cliente.java
package br.com.pogamando.validation.entity;

import javax.validation.constraints.NotNull;

import org.hibernate.validator.constraints.br.CNPJ;
import org.hibernate.validator.constraints.br.CPF;

public class Cliente {

  @NotNull
  private String nome;

  @NotNull
  private String tipo;

  @CPF // Ao inserir um CNPJ, esta constraint será violada...
  @CNPJ // Entretanto, inserindo um CPF, esta outra constaint será violada!
  private String cpfCnpj;

  // Getters e Setter omitidos
}
{% endhighlight %}

Note que esbarramos em um problema para usar o Bean Validator, pois as constraints @CPF e @CNPJ se contradizem. A API do Hibernate Validator (implementação de referência do Bean Validator) permite selecionarmos qual contraint deve ser aplicada, conforme a situação do nosso Java Bean.

Primeiramente, vamos criar duas interfaces para identificarmos os grupos de validação:

{% highlight java %}
// PessoaFisica.java
package br.com.pogamando.validation.groups;

public interface PessoaFisica {
}
{% endhighlight %}

{% highlight java %}
// PessoaJuridica.java
package br.com.pogamando.validation.groups;

public interface PessoaJuridica {
}
{% endhighlight %}

O próximo passo é criar uma classe que implemente DefaultGroupSequenceProvider (API do Hibernate Validator). Nesta classe, descreveremos a lógica que aplicará os grupos de validação ao nosso objeto Cliente.

{% highlight java %}
// ClienteGroupSequenceProvider.java
package br.com.pogamando.validation.groups.sequence;

import java.util.ArrayList;
import java.util.List;

import org.hibernate.validator.spi.group.DefaultGroupSequenceProvider;

import br.com.pogamando.validation.entity.Cliente;
import br.com.pogamando.validation.groups.PessoaFisica;
import br.com.pogamando.validation.groups.PessoaJuridica;

public class ClienteGroupSequenceProvider implements DefaultGroupSequenceProvider<Cliente> {

  public List<Class<?>> getValidationGroups(Cliente entity) {
    List<Class<?>> groups = new ArrayList<>();
    
    /*
     * Informamos ao HibernateValidator para usar as validações default
     * definidas na classe Cliente.
     */
    groups.add(Cliente.class);
    
    if (entity != null) {
      /*
       * Aqui nós implementamos a lógica que determina o grupo de
       * validação a ser aplicado ao bean.
       */
      if ("PF".equalsIgnoreCase(entity.getTipo())) {
        groups.add(PessoaFisica.class);
      } else if ("PJ".equalsIgnoreCase(entity.getTipo())) {
        groups.add(PessoaJuridica.class);
      }
    }
    
    return groups;
  }

}
{% endhighlight %}

Por último, vamos alterar a nossa classe Cliente usar estes grupos de validação e indicar qual Group Sequence Provider deve ser usado:

{% highlight java %}
// Cliente.java
package br.com.pogamando.validation.entity;

import javax.validation.constraints.NotNull;

import org.hibernate.validator.constraints.br.CNPJ;
import org.hibernate.validator.constraints.br.CPF;
import org.hibernate.validator.group.GroupSequenceProvider;

import br.com.pogamando.validation.groups.PessoaFisica;
import br.com.pogamando.validation.groups.PessoaJuridica;
import br.com.pogamando.validation.groups.sequence.ClienteGroupSequenceProvider;

@GroupSequenceProvider(value = ClienteGroupSequenceProvider.class)
public class Cliente {

  @NotNull
  private String nome;

  @NotNull
  private String tipo;

  @CPF(groups = PessoaFisica.class)
  @CNPJ(groups = PessoaJuridica.class)
  private String cpfCnpj;

  // Getters e setters omitidos
}
{% endhighlight %}

Se você quiser baixar o código-fonte deste exemplo, disponibilizei o mesmo no ([GitHub](https://github.com/pogamando/exemplos/tree/master/conditional-validation)).