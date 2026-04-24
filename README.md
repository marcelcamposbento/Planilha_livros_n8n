# Planilha_livros_n8n
Integrando planilhas com o Slack

 INTEGRANDO PLANILHA E SLACK 

Documentação do Fluxo de Automação
1. Início do Fluxo (Trigger)
O processo começa com o nó Manual Trigger (When clicking 'Execute workflow').

Função: Inicia a execução do fluxo sob demanda do usuário. É a fase de ativação onde o n8n prepara o ambiente para processar os dados.

2. Extração de Dados (Google Sheets)
O nó Get row(s) in sheet conecta o fluxo a uma planilha específica.

Ação: Ele realiza a leitura de linhas de uma aba selecionada.

Resultado: No seu caso, o nó extraiu 8 itens, transformando os dados brutos da planilha em objetos JSON que o n8n consegue manipular individualmente.

3. Processamento em Paralelo e Filtragem
Aqui o fluxo se divide em duas frentes para tratar os dados recebidos:

Padronizando Dados (Edit Fields): Este nó (com o ícone de caneta) é usado para limpar ou formatar os campos. Por exemplo, converter textos para letras maiúsculas, ajustar formatos de data ou renomear colunas para que fiquem padronizadas antes de seguirem no fluxo.

Filtrando se tem Autor (Filter): Este nó atua como um funil de qualidade. Ele verifica se o campo "Autor" está preenchido.

Resultado: Dos 8 itens iniciais, apenas 5 itens passaram pelo filtro (foram mantidos), descartando entradas incompletas.

4. Lógica Condicional (If/Switch)
O nó Verificando e-mails Corporativos decide o destino de cada item com base em uma regra lógica (provavelmente verificando o domínio do e-mail).

Caminho True (Verdadeiro): 2 itens foram identificados como e-mails corporativos e seguiram para a etapa final.

Caminho False (Falso): 3 itens não atenderam ao critério e foram enviados para o nó No Operation (NoOp), que serve apenas para encerrar o caminho sem realizar nenhuma ação (descarta o dado de forma limpa).

5. Agregação de Dados
Os itens que passaram na validação chegam ao nó Aggregate.

Função: Este é um passo crucial. Em vez de enviar várias mensagens individuais para o Slack, o Aggregate agrupa os 2 itens em uma única lista ou bloco de texto. Isso evita "spam" no canal de destino e organiza a informação de forma concisa.

6. Entrega Final (Slack)
O nó Send a message conclui a automação.

Ação: Ele envia o conteúdo consolidado (o resultado do nó Aggregate) para um canal ou usuário no Slack.

Resultado: 1 mensagem final contendo o resumo dos dados processados e validados.
