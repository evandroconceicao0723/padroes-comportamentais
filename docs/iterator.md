# Padrão Iterator

O padrão Iterator fornece uma maneira de acessar sequencialmente os elementos de um objeto agregado sem expor sua representação subjacente.

## Demonstração de Código

<details>
<summary><b>Ver Demonstração de Código ⚡</b></summary>

**Classe: ListaDeOrcamentos**  
```php
<?php

namespace Alura\DesignPattern;

class ListaDeOrcamentos implements \IteratorAggregate
{
    /** @var Orcamento[] */
    private array $orcamentos;

    public function __construct()
    {
        $this->orcamentos = [];
    }

    public function addOrcamento(Orcamento $orcamento)
    {
        $this->orcamentos[] = $orcamento;
    }

    public function getIterator()
    {
        return new \ArrayIterator($this->orcamentos);
    }
}
```

**Uso no projeto:**
```php
$orcamento1 = new Orcamento();
$orcamento1->quantidadeItens = 7;
$orcamento1->aprova();
$orcamento1->valor = 1500.75;

$orcamento2 = new Orcamento();
$orcamento2->quantidadeItens = 3;
$orcamento2->reprova();
$orcamento2->valor = 150;

$orcamento3 = new Orcamento();
$orcamento3->quantidadeItens = 5;
$orcamento3->aprova();
$orcamento3->finaliza();
$orcamento3->valor = 1350;

$listaOrcamentos = new ListaDeOrcamentos();
$listaOrcamentos->addOrcamento($orcamento1);
$listaOrcamentos->addOrcamento($orcamento2);
$listaOrcamentos->addOrcamento($orcamento3);

foreach ($listaOrcamentos as $orcamento) {
    echo "Valor: " . $orcamento->valor . PHP_EOL;
    echo "Estado: " . get_class($orcamento->estadoAtual) . PHP_EOL;
    echo "Qtd. Itens: " . $orcamento->quantidadeItens . PHP_EOL;
    echo PHP_EOL;
}
```

</details>

## Exemplo Prático do Commit

Veja como o padrão Iterator foi implementado no projeto, baseado no commit `2a5900e`:

**Arquivos modificados:**
- `src/ListaDeOrcamentos.php` (novo)
- `lista-orcamentos.php` (novo)

Este commit demonstra o uso do Iterator para percorrer uma lista de orçamentos sem expor a estrutura interna.