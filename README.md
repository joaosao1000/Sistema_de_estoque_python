Esse foi um projeto que eu fiz no meu estágio de verão em Lean Manufacturing na Baker Hughes.
O sistema não está totalmente otimizado, pois tive que recorrer a IA em algumas partes, especialmente na parte gráfica.

Eu adicionei um fluxograma feito no Lucidchart pra ficar melhor explicado o funcionamento do programa.

A empresa passava por dificuldades no setor de estoque, e o sistema foi feito a fim de manter um maior controle do que entra/sai no estoque, além de automatizar os emails com
as solicitações de compra caso se chegue em um estoque mínimo.

# Explicando o programa

O programa funciona dessa maneira: temos 2 planilhas, em excel, no sharepoint da empresa. A estrutura delas é parecida:
Nas ferramentas, temos uma aba chamada "prateleira principal" onde se encontra os itens de mão, outra chamada "prateleira secundária" onde temos ferramentas especiais.
Ambas as abas contém nome, quantidade, estoque mínimo, máximo e etc. Por fim uma chamada "registros" onde temos os registros de saída e entrada, e outra chamada "emails" 
onde temos uma coluna com os destinatários e outra com os emails em cópia.
Já nos consumíveis, temos uma aba chamada "estoque", contendo todos os consumíveis, com nome, código de manual, quantidade, estoque mínimo, máximo e etc. 
E também possuímos os registros e emails.

O programa começa em uma tela de login, onde faz-se o login com as credencias da empresa e a biblioteca e API do office365 faz a autenticação no sharepoint, caso de erro,
é emitido um erro "credenciais incorretas". Depois temos a tela onde se escolhe o inventário.
OS CONTROLES DE ACESSO SÃO FEITOS PELO SHAREPOINT (Quem pode editar e quem pode ver cara inventário).
Após escolhido o inventário, trazemos os dados da planilha, como nome da ferramenta/consumível, localização, quantidade e situação(em estoque ou enviada para compras).
Temos as funções de adicionar item, remover item, adicionar novo item (que não há na planilha), e sair.
Cada função, quando utilizada, é registrada na aba de registros da respectiva planilha, e caso fiquemos abaixo do estoque mínimo, é enviado um email contendo detalhes do item.
Em cada função, temos os respectivos tratamentos de erro, ex: Não dá pra remover mais itens do que há em estoque, no caso das ferramentas da pratileira principal, que possuem PN,
em caso de PN errado, nada é adicionado, e temos o erro "PN não encontrado".


