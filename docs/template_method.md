# Padrão Template Method

O padrão Template Method define o esqueleto de um algoritmo em uma operação, postergando alguns passos para as subclasses.

## Demonstração de Código

<details>
<summary><b>Ver Demonstração de Código ⚡</b></summary>

**Classe: ImpostoCom2Aliquotas**  
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

**Uso no projeto:**
```php
// Exemplo de subclasse implementando o template
class Icpp extends ImpostoCom2Aliquotas
{
    protected function deveAplicarTaxaMaxima(Orcamento $orcamento): bool
    {
        return $orcamento->valor > 500;
    }

    protected function calculaTaxaMaxima(Orcamento $orcamento): float
    {
        return $orcamento->valor * 0.07;
    }

    protected function calculaTaxaMinima(Orcamento $orcamento): float
    {
        return $orcamento->valor * 0.05;
    }
}
```

</details>

## Exemplo Prático do Commit

Veja como o padrão Template Method foi implementado no projeto.

**Arquivos modificados:**
- `src/Impostos/ImpostoCom2Aliquotas.php` (novo)

Este commit demonstra o uso do Template Method para impostos com duas alíquotas.
