# Descrição das Stacks para ZEP

## Arquivos de Stack

- **01-01-zep-db.yaml**: Stack do PostgreSQL exclusivo para o ZEP.
- Recomendação: 2 CPU E 4 GB MEMÓRIA
- **02-02-zep-app.yaml**: Stack do Serviço e do Aplicativo do ZEP.

## Segurança

Para garantir a segurança, esta stack deve ser utilizada apenas na rede local do Swarm, sem acesso público.

- **Autenticação**:
  - O ZEP não possui autenticação nativa.
  - Foi criada uma autenticação básica (Basic Auth) para bloquear acessos externos.
  - A autenticação externa é protegida com Basic Auth.
  - A API e a interface gráfica estão bloqueadas externamente, mas estão abertas internamente na rede.


# Instruções para Configuração de Autenticação no ZEP

Você precisará do `apache2-utils` para gerar a senha. Siga as instruções abaixo:

## Passo 1: Instalar apache2-utils

No servidor, execute o seguinte comando para instalar o `apache2-utils` no seu manager:

```bashdddddddddd
apt-get install -y apache2-utils
``` 

## Passo 2: Definir Usuário e Senha
No terminal, execute o seguinte comando para gerar a senha:
```bash
echo $(htpasswd -nb user password) | sed -e s/\$/\$\$/g
```
Substitua USER e PASSWORD pelo usuário e senha desejados.
OBS. SENHA: Não use caracteres especiais. Use mauisculo, minusculo e números
```bash
echo $(htpasswd -nb SEUNOME SUASENHA) | sed -e s/\$/\$\$/g
``` 
## Passo 3: Verificar a Senha Gerada
Dica de boa prática: Continue executando o comando até que não haja nada no final da senha gerada como "/" ou "caracter especial".

## Passo 4: Configurar a Autenticação Basic Auth no ZEP
Cole a senha gerada na linha de configuração de autenticação basic-auth para o ZEP dentro da stack "02-zep-app.yaml":

```bash
environment:
  # Configura a autenticação basic-auth para o ZEP
  - ZEP_BASIC_AUTH=SEUUSER:$$apr1$$/pZiybma$$fteaUddGkjadW61q9H5Xs
```

Seguindo essas instruções, você conseguirá configurar a autenticação básica para proteger o acesso ao ZEP.

## Passo 5: Faça o deploy da sua stack zep no Swarm
## Passo 6: Configure seu DNS
## Passo 7: Exemlos de Acessos: 
- Públicos
  - zep.seudominio.com.br 
    - é normal que o conteúdo desta página seja "404 - Oops, something went wrong."
    - Esta página é importante apenas para o lets Cripting poder criar o certificado 

- Protegidos pelo Basic Auth 
  - use seu usuário e senha
    - zep.seudominio.com.br/admin
    - zep.seudominio.com.br/api




# Conexão com Flowise

Para conectar o Flowise ao ZEP, siga as instruções abaixo:

1. **Mesma Rede**:
   - O Flowise deve estar dentro da mesma rede do seu ZEP.

2. **Credenciais Fake**:
   - Crie uma credencial fake dentro do Flowise:
     - Nome: `zeplocal`
     - API Key: deixe em branco

3. **Base URL para Fluxos**:
   - Use a seguinte URL base nos seus fluxos:
     ```
     http://zep:8000
     ```

---

A configuração acima garante que sua aplicação ZEP esteja segura e que o Flowise possa se conectar corretamente.
