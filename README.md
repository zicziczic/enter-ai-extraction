# PDF Information Retrieval

## TECHNICAL CHALLENGE - AI FELLOWSHIP
Este repositório contém os dados utilizados no desafio técnico do processo seletivo para a AI Fellowship da Enter.

## Sobre o Desafio Técnico
O desafio técnico consiste em focar na redução de custo de tokens em modelos LLMs, visando extrair informações de PDFs de forma barata e eficiente.

## Dificuldades Encontradas
Devido a problemas recentes, foi necessário com que o desafio fosse totalmente desenvolvido no google colab, isso complica no momento em que tive que transformá-lo em um script python, porém o código que funciona, está no formato notebook.

Durante o desenvolvimento do desafio, encontrei certas dificuldades devido a falta de contato com este tipo de problema, o que fez com que me inspirasse em soluções que situam-se no meu campo de interesse principal.
- Então, o primeiro desafio foi entender como poderia relacionar a pergunta que está sendo feita, com o texto do PDF, para isso foi utilizado um Bi-Encoder (BERTimbau) para gerar embeddings.
- Para evitar todas as vezes que aparecesse um documento, recalcular todos os elementos da pagina, foi feita uma cache de embeddings, para evitar com que isso seja feita todas as vezes.
- Depois deste passo, percebi que havia uma dificuldade em identificar a resposta correta, portanto usufrui de metadados (posição espacial do texto no PDF) para enriquecer a busca.
- Por fim, utilizei o GPT 5 Mini para resolver a questão, adicionando o contexto da chave da pergunta e o seu enrriquecimento (especificação do que esta chave representa) e comparando com o texto extraído do PDF e sua localização espacial.
- Verificando que muitas vezes a mesma "resposta" era perguntada diversas vezes, foi implementado um cache de perguntas e respostas, para evitar o custo de tokens desnecessários, eliminando a palavra que já havia sido enviada ao modelo.

## Como Executar o Código
1. Clone este repositório para sua máquina local.
2. '''bash
    python -m venv env

2. '''bash
    source ./env/bin/activate  # No Windows use `env\Scripts\activate`

3.  Instale as dependências necessárias:
   '''bash
    pip install -r requirements.txt

4. Parâmetros:
   - --input: Caminho para a pasta contendo os arquivos JSON com as perguntas e PDFs.
   - --output: Nome do arquivo de saída onde o resultado será salvo.
   - --neighbors: Número de vizinhos a serem considerados na busca por similaridade.

5. Execute o script principal:
   '''bash
    python pdf_smart_extraction.py --input ["pasta_com_jsons"] --output ["arquivo_saida.json"] --neighbors 5

6. Para adicionar uma chave da API, crie um arquivo chamado `API_KEY.env` e coloque sua chave.

## Estrutura da saída (output.json)

O processamento gera um arquivo JSON com a seguinte estrutura:

```json
{
  "label_do_documento": {
    "nome_do_arquivo": {
      "Pergunta 1": "Resposta encontrada",
      "Pergunta 2": null,
      "Pergunta 3": "Outra resposta"
    }
  }
}