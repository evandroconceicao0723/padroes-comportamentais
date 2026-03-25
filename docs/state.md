# Padrão State

O padrão State permite que um objeto altere seu comportamento quando seu estado interno muda, parecendo que mudou sua classe.

## Demonstração de Código

<details>
<summary><b>Ver Demonstração de Código ⚡</b></summary>

**Classe: EstadoOrcamento**  
```php
<?php

namespace Alura\DesignPattern\EstadosOrcamento;

use Alura\DesignPattern\Orcamento;

abstract class EstadoOrcamento
{
    /**
     * @throws \DomainException
     */
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

**Classe: EmAprovacao**  
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
$orcamento->aplicaDescontoExtra(); // Aplica desconto baseado no estado
```

</details>

## Exemplo Prático do Commit

Veja como o padrão State foi implementado no projeto.

**Arquivos modificados:**
- `src/EstadosOrcamento/EstadoOrcamento.php` (novo)
- `src/EstadosOrcamento/EmAprovacao.php` (novo)
- E outros estados...

Este commit demonstra o uso do State para gerenciar estados de orçamento.
