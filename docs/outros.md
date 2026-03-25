# Outros Padrões

Aqui você pode adicionar outros padrões comportamentais.

## Exemplo: Command

O padrão Command encapsula uma solicitação como um objeto, permitindo parametrizar clientes com diferentes solicitações, enfileirar ou registrar solicitações e suportar operações reversíveis.

<details>
<summary><b>Ver Demonstração de Código ⚡</b></summary>

```php
<?php

namespace Alura\DesignPattern;

interface Command
{
    public function execute();
}
```

```php
<?php

namespace Alura\DesignPattern;

class GerarPedido
{
    private float $valorOrcamento;
    private int $numeroItens;
    private string $nomeCliente;

    public function __construct(
        float $valorOrcamento,
        int $numeroItens,
        string $nomeCliente
    ) {
        $this->valorOrcamento = $valorOrcamento;
        $this->numeroItens = $numeroItens;
        $this->nomeCliente = $nomeCliente;
    }

    public function getValorOrcamento(): float
    {
        return $this->valorOrcamento;
    }

    public function getNumeroItens(): int
    {
        return $this->numeroItens;
    }

    public function getNomeCliente(): string
    {
        return $this->nomeCliente;
    }
}
```

```php
<?php

namespace Alura\DesignPattern;

use Alura\DesignPattern\AcoesAoGerarPedido\AcaoAposGerarPedido;
use Alura\DesignPattern\AcoesAoGerarPedido\CriarPedidoNoBanco;
use Alura\DesignPattern\AcoesAoGerarPedido\EnviarPedidoPorEmail;
use Alura\DesignPattern\AcoesAoGerarPedido\LogGerarPedido;

class GerarPedidoHandler
{
    /** @var AcaoAposGerarPedido[] */
    private array $acoesAposGerarPedido = [];

    public function __construct(/* PedidoRepository, MailService */)
    {
    }

    public function adicionarAcaoAoGerarPedido(AcaoAposGerarPedido $acao)
    {
        $this->acoesAposGerarPedido[] = $acao;
    }

    public function execute(GerarPedido $gerarPedido)
    {
        $orcamento = new Orcamento();
        $orcamento->quantidadeItens = $gerarPedido->getNumeroItens();
        $orcamento->valor = $gerarPedido->getValorOrcamento();

        $pedido = new Pedido();
        $pedido->dataFinalizacao = new \DateTimeImmutable();
        $pedido->nomeCliente = $gerarPedido->getNomeCliente();
        $pedido->orcamento = $orcamento;

        foreach ($this->acoesAposGerarPedido as $acao) {
            $acao->executaAcao($pedido);
        }
    }
}
```

**Uso no projeto:**
```php
$gerarPedido = new GerarPedido(100, 2, 'João');
$gerarPedidoHandler = new GerarPedidoHandler();
$gerarPedidoHandler->adicionarAcaoAoGerarPedido(new CriarPedidoNoBanco());
$gerarPedidoHandler->adicionarAcaoAoGerarPedido(new EnviarPedidoPorEmail());
$gerarPedidoHandler->adicionarAcaoAoGerarPedido(new LogGerarPedido());
$gerarPedidoHandler->execute($gerarPedido);
```

</details>

## Exemplo Prático do Commit - Command

Veja como o padrão Command foi implementado no projeto, baseado no commit `5f8251f`:

**Arquivos modificados:**
- `src/Command.php` (novo)
- `src/GerarPedido.php` (novo)
- `src/GerarPedidoHandler.php` (novo)
- `src/Pedido.php` (novo)
- `gera-pedido.php` (modificado)

Este commit demonstra o uso do Command para encapsular a geração de pedidos como um comando executável.

## Padrão State

O padrão State permite que um objeto altere seu comportamento quando seu estado interno muda.

### Demonstração de Código

<details>
<summary><b>Ver Demonstração de Código ⚡</b></summary>

```php
<?php

namespace Alura\DesignPattern\EstadosOrcamento;

use Alura\DesignPattern\Orcamento;

abstract class EstadoOrcamento
{
    abstract public function calculaDescontoExtra(Orcamento $orcamento): float;

    public function aprova(Orcamento $orcamento)
    {
        throw new \DomainException('Este orçamento não pode ser aprovado');
    }

    public function reprova(Orcamento $orcamento)
    {
        throw new \DomainException('Este orçamento não pode ser reprovad');
    }

    public function finaliza(Orcamento $orcamento)
    {
        throw new \DomainException('Este orçamento não pode ser finalizado');
    }
}
```

```php
<?php

namespace Alura\DesignPattern\EstadosOrcamento;

use Alura\DesignPattern\Orcamento;

class EmAprovacao extends EstadoOrcamento
{
    public function calculaDescontoExtra(Orcamento $orcamento): float
    {
        return $orcamento->valor * 0.05;
    }

    public function aprova(Orcamento $orcamento)
    {
        $orcamento->estadoAtual = new Aprovado();
    }

    public function reprova(Orcamento $orcamento)
    {
        $orcamento->estadoAtual = new Reprovado();
    }
}
```

**Uso no projeto:**
```php
$orcamento = new Orcamento();
$orcamento->valor = 1000;
echo $orcamento->aplicaDescontoExtra(); // 50.0 (5%)

$orcamento->aprova();
// Agora está em Aprovado
```

</details>

### Exemplo Prático do Commit - State

Veja como o padrão State foi implementado no projeto, baseado no commit `a658eb2`:

**Arquivos modificados:**
- `src/EstadosOrcamento/Aprovado.php` (novo)
- `src/EstadosOrcamento/EmAprovacao.php` (novo)
- `src/EstadosOrcamento/EstadoOrcamento.php` (novo)
- `src/EstadosOrcamento/Finalizado.php` (novo)
- `src/EstadosOrcamento/Reprovado.php` (novo)
- `src/Orcamento.php` (modificado)

Este commit demonstra o uso do State para gerenciar os diferentes estados de um orçamento (Em Aprovação, Aprovado, Reprovado, Finalizado).

## Padrão Template Method

O padrão Template Method define o esqueleto de um algoritmo em uma operação, postergando alguns passos para as subclasses.

### Demonstração de Código

<details>
<summary><b>Ver Demonstração de Código ⚡</b></summary>

```php
<?php

namespace Alura\DesignPattern\Impostos;

use Alura\DesignPattern\Orcamento;

abstract class ImpostoCom2Aliquotas implements Imposto
{
    public function calculaImposto(Orcamento $orcamento): float
    {
        if ($this->deveAplicarTaxaMaxima($orcamento)) {
            return $this->calculaTaxaMaxima($orcamento);
        }

        return $this->calculaTaxaMinima($orcamento);
    }

    abstract protected function deveAplicarTaxaMaxima(Orcamento $orcamento): bool;
    abstract protected function calculaTaxaMaxima(Orcamento $orcamento): float;
    abstract protected function calculaTaxaMinima(Orcamento $orcamento): float;
}
```

```php
<?php

namespace Alura\DesignPattern\Impostos;

use Alura\DesignPattern\Orcamento;

class Icpp extends ImpostoCom2Aliquotas
{
    protected function deveAplicarTaxaMaxima(Orcamento $orcamento): bool
    {
        return $orcamento->valor > 500;
    }

    protected function calculaTaxaMaxima(Orcamento $orcamento): float
    {
        return $orcamento->valor * 0.03;
    }

    protected function calculaTaxaMinima(Orcamento $orcamento): float
    {
        return $orcamento->valor * 0.02;
    }
}
```

**Uso no projeto:**
```php
$calculadora = new CalculadoraDeImpostos();
$orcamento = new Orcamento();
$orcamento->valor = 600;

echo $calculadora->calcula($orcamento, new Icpp()); // 18.0 (3% para >500)
```

</details>

### Exemplo Prático do Commit - Template Method

Veja como o padrão Template Method foi implementado no projeto, baseado no commit `09a8b6b`:

**Arquivos modificados:**
- `src/Impostos/Icpp.php` (novo)
- `src/Impostos/Ikcv.php` (novo)
- `src/Impostos/ImpostoCom2Aliquotas.php` (novo)

Este commit demonstra o uso do Template Method para calcular impostos com duas alíquotas (ICPP, IKCV).

## Padrão Chain of Responsibility

O padrão Chain of Responsibility evita acoplar o remetente de uma solicitação ao seu receptor, dando a mais de um objeto a chance de tratar a solicitação.

### Demonstração de Código

<details>
<summary><b>Ver Demonstração de Código ⚡</b></summary>

```php
<?php

namespace Alura\DesignPattern\Descontos;

use Alura\DesignPattern\Orcamento;

abstract class Desconto
{
    protected ?Desconto $proximoDesconto;

    public function __construct(?Desconto $proximoDesconto)
    {
        $this->proximoDesconto = $proximoDesconto;
    }

    abstract public function calculaDesconto(Orcamento $orcamento): float;
}
```

```php
<?php

namespace Alura\DesignPattern;

use Alura\DesignPattern\Descontos\Desconto;
use Alura\DesignPattern\Descontos\DescontoMaisDe500Reais;
use Alura\DesignPattern\Descontos\DescontoMaisDe5Itens;
use Alura\DesignPattern\Descontos\SemDesconto;

class CalculadoraDeDescontos
{
    public function calculaDescontos(Orcamento $orcamento): float
    {
        $cadeiaDeDescontos = new DescontoMaisDe5Itens(
            new DescontoMaisDe500Reais(
                new SemDesconto()
            )
        );

        return $cadeiaDeDescontos->calculaDesconto($orcamento);
    }
}
```

```php
<?php

namespace Alura\DesignPattern\Descontos;

use Alura\DesignPattern\Orcamento;

class DescontoMaisDe5Itens extends Desconto
{
    public function calculaDesconto(Orcamento $orcamento): float
    {
        if ($orcamento->quantidadeItens > 5) {
            return $orcamento->valor * 0.1;
        }

        return $this->proximoDesconto->calculaDesconto($orcamento);
    }
}
```

**Uso no projeto:**
```php
$calculadora = new CalculadoraDeDescontos();
$orcamento = new Orcamento();
$orcamento->valor = 600;
$orcamento->quantidadeItens = 6;

echo $calculadora->calculaDescontos($orcamento); // 60.0 (10% para >5 itens)
```

</details>

### Exemplo Prático do Commit - Chain of Responsibility

Veja como o padrão Chain of Responsibility foi implementado no projeto, baseado no commit `a8a90f0`:

**Arquivos modificados:**
- `src/CalculadoraDeDescontos.php` (novo)
- `src/Descontos/Desconto.php` (novo)
- `src/Descontos/DescontoMaisDe500Reais.php` (novo)
- `src/Descontos/DescontoMaisDe5Itens.php` (novo)
- `src/Descontos/SemDesconto.php` (novo)
- `src/Orcamento.php` (modificado)
- `testes.php` (modificado)

Este commit demonstra o uso do Chain of Responsibility para calcular descontos em uma cadeia (mais de 5 itens, mais de 500 reais, sem desconto).