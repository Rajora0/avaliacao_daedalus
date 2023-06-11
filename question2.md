# Relatório de Reconhecimento Facial com Máscaras

## Introdução
Com a pandemia do Covid-19, muitos modelos de reconhecimento facial enfrentaram problemas devido ao uso de máscaras faciais. Neste projeto, treinamos um modelo de Aprendizado de Métrica (Metric Learning) utilizando uma base de dados de celebridades e criamos um banco de dados de vetores descritores. Em seguida, atualizamos esse banco de dados para incluir uma nova pessoa e testamos o desempenho do modelo com a imagem dessa pessoa usando uma máscara facial.

## 1 - Treinando o Modelo de Rede Neural
Primeiro, utilizamos o método de Aprendizado de Métrica para treinar nosso modelo numa base de dados de celebridades. Para isso, utilizamos o modelo pré-treinado ResNet18, que é apropriado para o aprendizado de métricas.

## 2 - Criando o Banco de Dados de Vetores Descritores
Após treinar nosso modelo, criamos um banco de dados com as imagens das celebridades e seus respectivos vetores descritores. Salvamos os vetores descritores e rótulos num arquivo `.pkl`, que será usado para a consulta e atualização dos dados.

## 3 - Inclusão de Nova Pessoa no Banco de Dados
Uma vez treinado o modelo e criado o banco de dados inicial, incluímos uma nova pessoa (Marcelinho) no banco de dados. Processamos a imagem dessa pessoa usando nossa rede neural e atualizamos o arquivo `.pkl` com o novo vetor descritor e rótulo.

## 4 - Implementação de Métricas e Teste com Máscara Facial
Para realizar a inferência, implementamos a métrica de Distância Euclidiana e a métrica de Probabilidade de Softmax com ajustes de temperatura e Gaussian Density. A imagem de teste usada foi a mesma pessoa incluída no banco de dados (Marcelinho), mas usando uma máscara facial.

O resultado obtido mostrou que nosso algoritmo conseguiu reconhecer corretamente a pessoa na imagem, mesmo com o uso da máscara facial. 

## Conclusão
Utilizando o método de Aprendizado de Métrica e a métrica de Softmax ajustada com temperatura e Gaussian Density, nosso modelo de reconhecimento facial mostrou bons resultados para lidar com pessoas usando máscaras faciais. Além disso, concluímos as etapas para incluir novas pessoas no banco de dados sem a necessidade de retratar todo o modelo, simplificando o processo de atualização do sistema. No entanto, vale ressaltar que o desempenho do modelo pode variar dependendo do tamanho e qualidade dos dados de treinamento e teste, além das variações nas características faciais de cada pessoa.
