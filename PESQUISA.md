# Avaliação - Automação com GitHub Actions

---

## Equipe

- Ítalo Aurélio
- Roberta Ferreira Antunes
- Phablo Ribeiro de Oliveira

## Tema

O que é o conceito de "Fail Fast" e por que isso economiza dinheiro para as empresas?

---

## Fail Fast - E como isso pode salvar seu emprego

O conceito de *Fail Fast* (literalmente falhar rápido) consiste em uma metodologia que procura não reproduzir um código incompleto, e parar a execução assim que é encontrado um erro, para evitar que por conta do mesmo, venham a aparecer outros, de modo a dificultar o desenvolvimento e teste do sistema em questão.

Exemplo:

```php
$users = DB::connection('system')->table('users')
    ->where('data_nasc', ' >=', '2005-01-01')
    ->get();

DB::conection('system')->table('matricula')
    ->insert($users);
```

Nesse caso, não há verificação de se a consulta vai ser feita ou não, e existem usuários cujo a data de nascimento é maior que 2005, por isso, na hora de dar insert, ele pode retornar sucesso, mesmo não tendo inserido nada, e nesse caso do PHP, não há como dar um catch de forma nativa para pegar a quantidade de linhas inseridas.

Usando a estratégia que estamos analisando, podemos previnir esse tipo de evento da seguinte maneira:

```php
$users = DB::connection('system')->table('users')
    ->where('data_nasc', ' >=', '2005-01-01')
    ->get();

// Bloco de implementação do Fail Fast

if (!isset($users)) {
    $this->info("Não foi encontrado nenhum usuário com essas condições");
    return 0;
}

// Fim da implementação

DB::conection('system')->table('matricula')
    ->insert($users);
```

Esse novo bloco vai fazer com que o código seja parado ao encontrar o erro, e não vai proliferar, rodando o resto do controller errado.