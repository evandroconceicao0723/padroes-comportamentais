# Padrão Command

O padrão Command encapsula uma solicitação como um objeto, permitindo parametrizar clientes com diferentes solicitações, enfileirar ou registrar solicitações e suportar operações reversíveis.

## Demonstração de Código

<details>
<summary><b>Ver Demonstração de Código ⚡</b></summary>

**Classe: Command**  
```php
<?php

namespace Alura\DesignPattern;

interface Command
{
    public function execute();
}
```

**Classe: GerarPedido**  
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

    /**
     * @return float
     */
    public function getValorOrcamento(): float
    {
        return $this->valorOrcamento;
    }

    /**
     * @return int
     */
    public function getNumeroItens(): int
    {
        return $this->numeroItens;
    }

    /**
     * @return string
     */
    public function getNomeCliente(): string
    {
        return $this->nomeCliente;
    }
}
```

**Uso no projeto:**
```php
// Exemplo de uso do Command
$gerarPedido = new GerarPedido(100, 2, 'João');
// Implementar execute se necessário
```

</details>

## Exemplo Prático do Commit

Veja como o padrão Command foi implementado no projeto.

**Arquivos modificados:**
- `src/Command.php` (novo)
- `src/GerarPedido.php` (novo)

Este commit demonstra o uso do Command para encapsular solicitações.