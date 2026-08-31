## Checkpoint-sprint-1-solucao-energias-renovaveis
# Dataset 1 — Appliances Energy Prediction (UCI)
Isolando apenas o consumo elevado (acima de 70% do máximo), **22 registros (1.11% da amostra)** foram selecionados. Ao exigir também que a temperatura da cozinha (T1) estivesse acima da média, o conjunto caiu para **8 registros (0.41%)** — menos da metade.

Isso indica que boa parte dos picos de consumo de eletrodomésticos **não** ocorre necessariamente em momentos de temperatura interna elevada: o padrão de uso dos aparelhos (horário, quantidade de equipamentos ligados) parece pesar mais do que a temperatura ambiente da cozinha nesses picos específicos. A temperatura funciona aqui como um segundo filtro que reduz o conjunto, evidenciando que "alto consumo" e "ambiente mais quente" só coincidem em parte dos casos.

# Dataset 2 — Steel Industry Energy Consumption (UCI)
Do total de **65 registros de alto consumo (1.86% da amostra)**, **32 (quase metade)** já ocorrem em carga classificada como *Maximum_Load* — o que é esperado, já que consumo elevado tende a coincidir com a categoria de carga máxima definida pela própria indústria.

Ao cruzar consumo elevado com fator de potência atrasado abaixo de 90, o conjunto cai para apenas **22 registros (0.63% da amostra)**. Esse subconjunto merece atenção especial da equipe de energia porque reúne simultaneamente **alta demanda** e **baixa eficiência elétrica**: fator de potência baixo nesses momentos de pico aumenta a corrente reativa na rede, eleva perdas e pode gerar penalidades tarifárias — é justamente a combinação mais custosa para a operação, e não apenas o consumo alto isoladamente.

# Dataset 3 — Power Consumption of Tetouan City (UCI)
A **Zona 1** apresenta o maior pico de consumo entre as três zonas. Considerando apenas o consumo acima de 70% do seu próprio máximo, **2.388 registros (30.37% da amostra)** foram selecionados — uma parcela relevante, o que sugere que a Zona 1 passa uma fração considerável do tempo em regime de demanda elevada.

Ao adicionar a condição de temperatura acima da média, o conjunto caiu para **1.597 registros (20.31% da amostra)**. A redução (de ~30% para ~20%) mostra que existe uma associação real, mas não total, entre consumo elevado e temperaturas mais altas — parte dos picos de consumo na Zona 1 ocorre também em dias mais amenos, provavelmente ligados a outros fatores (horário comercial, uso simultâneo de equipamentos, dias específicos da semana) além da temperatura.

# Dataset 4 — Solar Power Generation Data (Kaggle)
Apenas **8.61% da amostra (1.185 registros)** corresponde a momentos de alta geração (acima de 70% da potência CA máxima) — coerente com o fato de a geração solar se concentrar em poucas horas do dia, com muitos registros noturnos em zero.

O inversor **`adLQvlD726eNBSB`** aparece com maior frequência nesse recorte (68 ocorrências), seguido de perto por outros dois ou três inversores com contagens próximas. Isso permite observar que a geração elevada não está concentrada em um único equipamento, mas distribuída de forma relativamente equilibrada entre os inversores da planta. **Não é possível concluir**, apenas com essa contagem, se algum inversor tem desempenho superior ou inferior — para isso seria necessário normalizar pela capacidade nominal de cada inversor e pelo tempo em que cada um esteve ativo, o que foge do escopo desta atividade.

# Dataset 5 — Wind & Solar Energy Production (Kaggle)
**6. Comparação entre os grupos**

O Grupo B (Inverno) apresenta uma frequência de alta geração muito maior em relação ao seu próprio máximo (**7.21%**) do que o Grupo A, Verão (**0.75%**) — quase 10 vezes mais. Isso mostra que, dentro dessa base de dados, os episódios de produção próxima do pico se concentram proporcionalmente mais no inverno.

**7. Por que não usar o mesmo valor numérico como limite para os dois grupos**

O máximo do Grupo A é 20.359 e o do Grupo B é 22.043 — próximos, mas os grupos têm distribuições diferentes. Se comparássemos por escalas totalmente distintas (como aconteceria de fato entre geração solar e eólica, cujas potências nominais e perfis de variação são muito diferentes), usar um único valor absoluto como limiar penalizaria a fonte de menor escala e nunca filtraria adequadamente a de maior escala. Calcular o limiar como percentual do próprio máximo de cada série garante que a comparação seja proporcional e justa entre fontes com magnitudes distintas — é exatamente por isso que o exercício pede que cada fonte seja avaliada em relação ao seu próprio valor máximo, e não a um valor fixo comum.

# Dataset 6 — Individual Household Electric Power Consumption (UCI)
Os dois DataFrames têm exatamente o **mesmo tamanho: 46 registros (0.22% da amostra)**. Ou seja, a condição de corrente acima da média não removeu nenhum registro do conjunto de alta potência ativa.

Isso faz sentido fisicamente: potência ativa e corrente estão diretamente relacionadas (P = V × I, com a tensão variando pouco em torno de ~240V). Assim, praticamente todo registro com potência ativa muito alta já apresenta corrente elevada — as duas variáveis carregam, em grande parte, a mesma informação sobre o nível de demanda da residência. Diferente do que ocorreu nos Datasets 1 e 3 (onde temperatura era uma condição relativamente independente do consumo), aqui a segunda condição é redundante com a primeira porque decorre da própria física do circuito elétrico.
