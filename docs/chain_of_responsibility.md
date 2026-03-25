# Padrão Chain of Responsibility

O padrão Chain of Responsibility evita acoplar o remetente de uma solicitação ao seu receptor, dando a mais de um objeto a chance de tratar a solicitação.

## Demonstração de Código

<details>
<summary><b>Ver Demonstração de Código ⚡</b></summary>

**Classe: Desconto**  
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

**Classe: DescontoMaisDe500Reais**  
```php
<?php

namespace Alura\DesignPattern\Descontos;

use Alura\DesignPattern\Orcamento;

class DescontoMaisDe500Reais extends Desconto
{
    public function calculaDesconto(Orcamento $orcamento): float
    {
        if ($orcamento->valor > 500) {
            return $orcamento->valor * 0.05;
        }

        return $this->proximoDesconto->calculaDesconto($orcamento);
    }
}
```

**Uso no projeto:**
```php
$desconto1 = new DescontoMaisDe500Reais(null);
$orcamento = new Orcamento();
$orcamento->valor = 600;
echo $desconto1->calculaDesconto($orcamento); // 30
```

</details>

## Exemplo Prático do Commit

Veja como o padrão Chain of Responsibility foi implementado no projeto.

**Arquivos modificados:**
- `src/Descontos/Desconto.php` (novo)
- `src/Descontos/DescontoMaisDe500Reais.php` (novo)
- E outros descontos...

Este commit demonstra o uso do Chain of Responsibility para calcular descontos em cadeia.
