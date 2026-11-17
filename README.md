# Sistema de Controle de Estoque

Aplicação desktop em Python para controle de inventário de ferramentas e
consumíveis, com registro de entradas e saídas, níveis de estoque mínimo e
máximo, e solicitação de compra disparada automaticamente por e-mail quando um
item atinge o ponto de reposição.

Desenvolvido durante meu estágio em Lean Manufacturing na Baker Hughes.

## O problema

A oficina não contabilizava as entradas e saídas do estoque de ferramentas, e
isso gerava dois prejuízos:

- **Tempo** — o mecânico parava a manutenção e gastava minutos procurando uma
  ferramenta, várias vezes ao dia
- **Ruptura** — itens acabavam sem ninguém perceber. Como o lead time de compra
  era longo, quando a falta era notada já era tarde, e em casos críticos
  chegava a interromper a produção

## Resultado

Medimos o antes e o depois com uma dinâmica cronometrada de busca:

| | Antes | Depois |
|---|---|---|
| Tempo para localizar uma ferramenta | 3min19s | 30s |

**Redução de 85%.** E a reposição deixou de depender de alguém notar que o item
tinha acabado.

## Como funciona

1. **Login** com as credenciais corporativas do usuário, autenticando no
   SharePoint via API do Office 365
2. **Escolha do inventário** — ferramentas ou consumíveis
3. **Visualização** da planilha correspondente em tabela, com busca por nome ou PN
4. **Entrada e saída** de itens, com validação (não é possível retirar mais do
   que há em estoque, nem movimentar um PN inexistente)
5. **Registro automático** de toda operação numa aba de log, com quem movimentou,
   o quê, quanto e quando
6. **Solicitação de compra** — ao cruzar o estoque mínimo, o sistema monta e
   envia um e-mail aos responsáveis com o item, a quantidade atual e a
   quantidade sugerida para repor, e marca o item como "Enviado para compras"
   para não duplicar o pedido

O fluxograma completo está em [`fluxograma.pdf`](link).

## Stack

`Python` · `pandas` · `openpyxl` · `tkinter` · `Office365-REST-Python-Client` ·
`pywin32` (Outlook)

## Decisões de projeto

**Por que Excel no SharePoint e não um banco de dados.** A equipe já trabalhava
em planilhas no SharePoint e tinha os acessos configurados. Usar o que já
existia significou zero infraestrutura nova, zero aprovação de TI, e nenhuma
mudança de hábito para quem ia usar. Num projeto de quatro meses com usuário
não-técnico, isso valeu mais do que a modelagem ideal.

**Controle de acesso delegado ao SharePoint.** O sistema não armazena
credenciais nem mantém usuários próprios — quem pode ver e editar cada
inventário é definido nas permissões do próprio SharePoint. Isso tirou toda a
superfície de gestão de identidade da aplicação.

**Preservação de formatação na escrita.** As planilhas eram usadas
manualmente também, então a gravação reaplica fontes, bordas, alinhamento e
larguras de coluna originais em vez de sobrescrever o arquivo.

**Construção apoiada em IA.** Era o maior projeto em Python que eu já tinha
feito e o prazo era de quatro meses. Usar IA em todo o desenvolvimento foi o
que tornou o escopo viável.

## O que eu faria diferente hoje

- Trocar a camada de persistência por um banco, mantendo a planilha apenas
  como exportação
- Extrair a lógica de negócio da camada de interface — hoje elas estão
  acopladas no mesmo módulo
- Substituir a automação do Outlook via COM por envio server-side, para o
  sistema não depender de uma máquina Windows com Outlook aberto
- Adicionar testes para as regras de estoque mínimo e ponto de reposição
