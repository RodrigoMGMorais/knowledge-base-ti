# 🌐 Guia de Referência: APIs (Conceitos, Manuseio & Troubleshooting)

As APIs (Application Programming Interfaces) são a espinha dorsal da integração em sistemas de missão crítica. Este guia cobre desde a compreensão dos métodos até o troubleshooting de integrações.

---

## 🚀 1. Conceitos Fundamentais
API é a "ponte" que permite que sistemas distintos conversem entre si. 
* **Endpoints:** O endereço (URL) onde o recurso da API reside.
* **Verbos HTTP:** As ações que realizamos:
    * `GET`: Solicita dados (Leitura).
    * `POST`: Envia novos dados (Criação).
    * `PUT/PATCH`: Atualiza dados existentes.
    * `DELETE`: Remove recursos.

---

## 🛠️ 2. Como Manusear e Gerenciar (Boas Práticas)

Para um analista de sistemas, o manuseio envolve garantir que a comunicação seja estável e segura:

1. **Autenticação:** Nunca exponha tokens em logs ou repositórios. Sempre utilize variáveis de ambiente ou *Secret Managers*.
2. **Rate Limiting:** Entenda se a API possui limites de requisições para evitar o bloqueio do seu IP (o famoso erro `429 Too Many Requests`).
3. **Idempotência:** Garanta que, ao enviar a mesma requisição duas vezes (por falha de rede), o sistema não processe o mesmo dado em duplicidade.

---

## 🔍 3. Troubleshooting de APIs (O "Pulo do Gato")

Quando uma integração falha, o erro HTTP é o seu melhor aliado para o diagnóstico:

| Status Code | Categoria | Significado e Ação |
| :--- | :--- | :--- |
| **200-201** | Sucesso | Requisição processada com êxito. |
| **400** | Bad Request | Erro no formato do seu JSON/XML. Revise a documentação. |
| **401/403** | Autenticação | Token expirado ou sem permissão. Verifique suas credenciais. |
| **404** | Not Found | Endpoint inexistente ou URL digitada incorretamente. |
| **500** | Erro Interno | Falha no servidor do fornecedor. Aguarde ou contate o suporte. |
| **503** | Service Unavailable | Servidor sobrecarregado ou em manutenção. |

---

## 💡 Dica de Ouro para Analistas de Sustentação
Sempre utilize ferramentas como **Postman** ou **Insomnia** para isolar o problema. Se o erro ocorre via código, mas funciona no Postman, o problema está na sua implementação (código/configuração). Se falha no Postman, o problema está na rede ou no serviço de destino.

*Dica de diagnóstico:* Use `curl -v <url>` no terminal Linux para ver o *handshake* (negociação) da conexão antes mesmo de chegar na camada de aplicação.

-----------------------------------
# Guia Pragmático: Como Compreender e Configurar APIs Eficientes

Este repositório contém um passo a passo direto e sem enrolação para entender, desenhar e testar APIs utilizando o padrão REST e o formato JSON. O objetivo é fornecer uma base sólida e 100% eficiente para o desenvolvimento de integrações modernas.

---

## 1. O que é uma API na Prática?

Uma API (Application Programming Interface) é um **contrato** entre dois sistemas. Pense nela como um garçom: você (o cliente) faz um pedido (Requisição), o garçom leva até a cozinha (Servidor) e traz o seu prato pronto (Resposta).

### Os 4 Elementos de uma Requisição
1. **URL (Endpoint):** O endereço do recurso (Ex: `https://api.seuapp.com/v1/usuarios`).
2. **Método HTTP (O Verbo):** O que você quer fazer.
3. **Headers (Cabeçalhos):** Metadados da requisição (Ex: Tokens de autenticação, tipo de dados).
4. **Body (Corpo):** Os dados que você está enviando (geralmente em JSON).

---

## 2. Os Verbos HTTP que Você Deve Dominar

Para criar uma API eficiente, utilize os métodos HTTP corretos para cada ação (padrão CRUD):

| Método HTTP | Ação CRUD | Descrição | Exemplo de Endpoint |
| :--- | :--- | :--- | :--- |
| **GET** | Read (Ler) | Recupera dados do servidor. | `/v1/produtos` |
| **POST** | Create (Criar) | Envia novos dados para o servidor. | `/v1/produtos` |
| **PUT** | Update (Atualizar)| Atualiza **todo** o objeto existente. | `/v1/produtos/42` |
| **PATCH** | Update (Atualizar)| Atualiza **parcialmente** o objeto. | `/v1/produtos/42` |
| **DELETE** | Delete (Deletar)| Remove um recurso do servidor. | `/v1/produtos/42` |


### 🛠️ Comando GET via prompt 💡
curl -X GET https://jsonplaceholder.typicode.com/todos/1

Para dominar o teste de APIs via terminal (o que chamamos de CLI Testing), o curl é a sua ferramenta principal. Como você já aprendeu a consultar (GET), agora vamos explorar os outros verbos que permitem criar, alterar e deletar dados.

Use estes exemplos no seu arquivo APIs.md para criar uma seção de "Comandos Avançados de Teste":

🚀 Comandos Avançados com curl
Para todos os comandos abaixo, vamos utilizar o JSONPlaceholder como alvo.

1. Criando um novo dado (POST)
O POST é usado para enviar informações. Note o uso do parâmetro -H para definir o tipo de conteúdo como JSON.

curl -X POST https://jsonplaceholder.typicode.com/posts \
     -H "Content-Type: application/json" \
     -d '{"title": "Teste de API", "body": "Aprendendo a usar curl", "userId": 1}'

--------------------------------------------------     
O que faz: Envia um novo registro. O servidor responderá com o ID do objeto criado.

2. Atualizando um dado existente (PUT)
O PUT substitui um recurso completo.

curl -X PUT https://jsonplaceholder.typicode.com/posts/1 \
     -H "Content-Type: application/json" \
     -d '{"id": 1, "title": "Título Alterado", "body": "Conteúdo modificado", "userId": 1}'

--------------------------------------------------
3. Atualizando apenas um campo (PATCH)
Diferente do PUT, o PATCH altera apenas o que você especificar.

curl -X PATCH https://jsonplaceholder.typicode.com/posts/1 \
     -H "Content-Type: application/json" \
     -d '{"title": "Apenas o Título mudou"}'

--------------------------------------------------
4. Removendo um registro (DELETE)
O teste clássico para garantir que sua API consegue remover recursos.

curl -X DELETE https://jsonplaceholder.typicode.com/posts/1

--------------------------------------------------
💡 Pulo do Gato: Analisando o "Header" (Cabeçalho)
Se você quer ser um analista sênior de sustentação, você não olha apenas o resultado (o JSON), você olha o cabeçalho da resposta para saber se o servidor está saudável.

Adicione o parâmetro -i (ou -v para verboso) em qualquer comando:

curl -i -X GET https://jsonplaceholder.typicode.com/todos/1


💡 Por que isso ajuda? Ao usar o -i, o terminal exibirá HTTP/1.1 200 OK (ou outros códigos como 404, 500) antes do conteúdo. Isso é o que você usará para provar para um desenvolvedor ou para o time de infraestrutura que o problema é no servidor deles e não na sua requisição.


--------------------------------------------------
---

## 3. Estruturando o JSON (O Contrato)

O JSON (JavaScript Object Notation) é o padrão ouro para troca de dados. Ele precisa ser limpo, tipado corretamente e previsível.

### Exemplo de Payload Eficiente (POST `/v1/usuarios`)

```json
{
  "nome": "Rodrigo Morais",
  "email": "rodrigo@email.com",
  "ativo": true,
  "papeis": ["admin", "usuario"],
  "metadados": {
    "empresa_id": 1024,
    "criado_em": "2026-06-15T10:00:00Z"
  }
}
