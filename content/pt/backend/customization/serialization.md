---
title: Serialização
weight: 196
description: Nesta seção, você encontra como funciona a serialização no Beagle. 
---

---

Como a maioria dos frameworks integram com Jackson e oferecem propriedades para customizar serialização, o time do Beagle optou por não adicionar as próprias propriedades. Mas foi disponibilizado a compatibilidade com seu framework escolhido.

### Customização

Dentro da pasta `src/main/resources` procure por um arquivo chamado\(se não existir pode criar\) `application.properties`. Caso a chave não esteja listada no seu arquivo, significa que a configuração padrão será automaticamente aplicada.

Nos links abaixo você confere as propriedades disponíveis para serialização de acordo com framework:

- [**Micronaut**](https://docs.micronaut.io/latest/guide/index.html#_jackson_configuration)
- [**Spring**](https://docs.spring.io/spring-boot/pt/current/reference/html/appendix-application-properties.html#json-properties)**​**

{{% alert color="info" %}}
No caso do Spring, deve-se utilizar apenas as chaves `spring.jackson`
{{% /alert %}}
