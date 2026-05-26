# Sistema de Monitoramento de Risco de Lixo Espacial com Base no Dataset SpaceX

### Este projeto foi desenvolvido como requisito para a disciplina Generative AI For Engineering. A aplicação utiliza técnicas de Machine Learning para a catalogação, análise e previsão do risco de colisão de objetos e cargas úteis na órbita baixa da Terra, utilizando dados oficiais extraídos da API da SpaceX.

## Contexto do Problema

O aumento acelerado de satélites comerciais, constelações de comunicação e fragmentos de foguetes antigos em órbita gerou uma crise de 
tráfego espacial. O acúmulo de detritos (lixo espacial) na Órbita Baixa da Terra (LEO) eleva exponencialmente o risco de colisões.

Como os impactos no espaço ocorrem em velocidades hipersônicas, uma colisão pode desencadear o efeito conhecido como Síndrome de Kessler. 
Esse fenômeno caracteriza-se por uma reação em cadeia onde uma única colisão gera milhares de novos fragmentos que, por sua vez, destroem 
outros satélites ativos. As consequências incluem a inviabilização de órbitas inteiras e a interrupção de serviços terrestres essenciais, 
tais como sistemas de posicionamento global (GPS), telecomunicações, previsão meteorológica e internet.

## Fonte dos Dados

Os dados utilizados neste projeto são reais e de origem pública, obtidos por meio de requisições à API Oficial da SpaceX através do endpoint de cargas úteis:

## Link do Endpoint: (https://docs.spacexdata.com/)

O conjunto de dados consolida informações físicas, estruturais e orbitais de componentes lançados ao espaço. O pipeline conta com um sistema
de contingência baseado nas propriedades estatísticas e físicas do catálogo da SpaceX para garantir a estabilidade e a reprodutibilidade dos 
testes mesmo em cenários de instabilidade na rede externa.

## Metodologia e Pipeline de Machine Learning

### O projeto foi construído sobre um pipeline completo e automatizado de Ciência de Dados estruturado em Python:

Obtenção e Limpeza de Dados: Requisição HTTP dos payloads da SpaceX com tratamento de valores ausentes e filtragem de objetos sem registros de telemetria.

Engenharia de Atributos (Feature Engineering): Criação de novas variáveis físicas baseadas nas leis da mecânica orbital:

Razão de Órbita: Proporção de achatamento calculada entre o Periastro e o Apoastro.

Velocidade Média Estimada: Deduzida matematicamente através do raio médio (Eixo Semimaior) e do período orbital em minutos.

Pré-processamento: Aplicação de padronização estatística padrão (StandardScaler) para alinhar a escala de variáveis de ordens de grandeza 
distintas (ex: Massa em kg vs. Excentricidade decimal).

Definição da Variável Alvo (Target): Criação da classe preditiva (0 ou 1) que mapeia objetos abandonados (vida útil zerada) situados na zona 
orbital mais congestionada da Terra (entre 400 km e 1000 km de altitude).

Modelos Testados e Resultados

Foram aplicadas e comparadas duas técnicas diferentes de Classificação estudadas ao longo do período letivo:

Random Forest Classifier

Abordagem: Modelo baseado em comitê (Ensemble por Bagging), gerando múltiplas árvores de decisão em paralelo.

Características: Alta robustez a outliers e menor propensão ao sobreajuste (overfitting).

XGBoost Classifier (Extreme Gradient Boosting)

Abordagem: Modelo baseado em Boosting sequencial, onde cada nova árvore é construída com o objetivo de corrigir os erros residuais das estruturas anteriores.

Características: Alta eficiência computacional e precisão em dados tabulares estruturados.

## Comparação de Desempenho
Os modelos foram validados utilizando uma base de dados de teste isolada (20% do volume total). O pipeline extrai as métricas de Precisão (Precision),
Sensibilidade (Recall) e F1-Score para ambas as técnicas. O modelo XGBoost foi selecionado para a produção devido à sua capacidade superior de ajustar 
as fronteiras de decisão do tráfego orbital.

## Interpretabilidade do Modelo (SHAP)

Para mitigar o efeito de caixa-preta comumente associado a modelos de aprendizado profundo e boosting, utilizou-se a biblioteca SHAP 
(SHapley Additive exPlanations), fundamentada na teoria dos jogos cooperativos.

O gráfico de importância global (summary_plot) foi customizado para exibir os rótulos e explicações técnicas totalmente em língua portuguesa. 
Ele demonstra de forma quantitativa o impacto de cada variável na decisão final da inteligência artificial:

Atributos como Vida Útil e Periastro (Altitude Mínima) situam-se no topo do gráfico, indicando que a ausência de controle ativo e a presença em
altitudes congestionadas são os principais fatores de risco detectados pelo modelo.

A barra de intensidade lateral (Baixo a Alto) expõe a correlação direta entre o aumento ou diminuição de cada métrica física e a probabilidade de colisão estimada.

## INSTRUÇÕES PARA EXECUÇÃO DO PROJETO

Para reproduzir o projeto localmente ou em ambientes de computação em nuvem, execute as seguintes etapas:

### Clonar o Repositório
git clone https://github.com/SEU_USUARIO/SEU_REPOSITORIO.git
cd SEU_REPOSITORIO

### Instalar as Dependências
Certifique-se de possuir o ambiente Python 3.8 ou superior configurado e execute a instalação dos pacotes necessários:
pip install pandas numpy requests scikit-learn xgboost shap matplotlib gradio

### Executar o Script Principal
Rode o arquivo Python para iniciar a coleta, processamento, treinamento, exibição das métricas e renderização do gráfico do SHAP:
python seu_codigo.py

## IMPLANTAÇÃO E DISPONIBILIZAÇÃO (DEPLOY)

A interface do usuário foi desenvolvida utilizando a biblioteca Gradio, estabelecendo uma aplicação web para a simulação de cenários em tempo real. A plataforma permite que engenheiros aeroespaciais insiram novas métricas orbitais e obtenham o diagnóstico preditivo imediato emitido pelo modelo.

## Link de Acesso: https://cc8a6de59562c69a57.gradio.live/
