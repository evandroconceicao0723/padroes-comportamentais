# Padrão Observer

O padrão Observer define uma dependência um-para-muitos entre objetos, de modo que quando um objeto muda de estado, todos os seus dependentes são notificados e atualizados automaticamente.

## Demonstração de Código

<details>
<summary><b>Ver Demonstração de Código ⚡</b></summary>

**Classe: AcaoAposGerarPedido**  
```php
<?php

namespace Alura\DesignPattern\AcoesAoGerarPedido;

use Alura\DesignPattern\Pedido;

interface AcaoAposGerarPedido
{
    public function executaAcao(Pedido $pedido): void;
}
```

**Classe: CriarPedidoNoBanco**  
```php
<?php

namespace Alura\DesignPattern\AcoesAoGerarPedido;

use Alura\DesignPattern\Pedido;

class CriarPedidoNoBanco implements AcaoAposGerarPedido
{
    public function executaAcao(Pedido $pedido): void
    {
        echo "Salvando pedido no banco de dados";
    }
}
```

**Classe: EnviarPedidoPorEmail**  
```php
<?php

namespace Alura\DesignPattern\AcoesAoGerarPedido;

use Alura\DesignPattern\Pedido;

class EnviarPedidoPorEmail implements AcaoAposGerarPedido
{
    public function executaAcao(Pedido $pedido): void
    {
        echo "Enviando e-mail de pedido gerado";
    }
}
```

**Classe: LogGerarPedido**  
```php
<?php

namespace Alura\DesignPattern\AcoesAoGerarPedido;

use Alura\DesignPattern\Pedido;

class LogGerarPedido implements AcaoAposGerarPedido
{
    public function executaAcao(Pedido $pedido): void
    {
        echo "Gerando log de geração de pedido";
    }
}
```

**Classe: GerarPedidoHandler**  
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

**Classe: Pedido**  
```php
<?php

namespace Alura\DesignPattern;

class Pedido
{
    public string $nomeCliente;
    public \DateTimeInterface $dataFinalizacao;
    public Orcamento $orcamento;
}
```

**Uso no projeto:**
```php
$gerarPedido = new GerarPedidoHandler();
$gerarPedido->adicionarAcaoAoGerarPedido(new CriarPedidoNoBanco());
$gerarPedido->adicionarAcaoAoGerarPedido(new EnviarPedidoPorEmail());
$gerarPedido->adicionarAcaoAoGerarPedido(new LogGerarPedido());

$gerarPedido->execute(new GerarPedido(100, 2, 'João'));
```

</details>

## Exemplo Prático do Commit

Veja como o padrão Observer foi implementado no projeto, baseado no commit `7e3edc5`:

**Arquivos modificados:**
- `src/AcoesAoGerarPedido/AcaoAposGerarPedido.php` (novo)
- `src/AcoesAoGerarPedido/CriarPedidoNoBanco.php` (novo)
- `src/AcoesAoGerarPedido/EnviarPedidoPorEmail.php` (novo)
- `src/AcoesAoGerarPedido/LogGerarPedido.php` (novo)
- `src/GerarPedidoHandler.php` (modificado)
- `gera-pedido.php` (modificado)

Este commit demonstra o uso do Observer para executar múltiplas ações após gerar um pedido (salvar no banco, enviar email, logar).