# Especificação do Modelo de Dados - Orders API

O modelo relacional é composto por duas entidades principais organizadas em um
relacionamento **1:N (Um-para-Muitos)** com dependência de ciclo de vida
(composição):

- **`orders` (Order)**: Representa o pedido realizado por um cliente na plataforma.
- **`items` (Item)**: Representa os itens/produtos associados a um pedido específico.

+-----------------------------------+ +-----------------------------------+
| orders | | items |
+-----------------------------------+ +-----------------------------------+
| PK id : String (UUID) |<------| PK id : String (UUID) |
| customer : String | 1:N | FK order_id : String (UUID) |
| status : String |-------| sku : String |
| created_at : DateTime (UTC) | | description : String |
+-----------------------------------+ | quantity : Integer |
+-----------------------------------+

## Tabela: `orders`

Armazena o cabeçalho do pedido, incluindo informações de cliente, status do
ciclo de vida e marca temporal de criação.

#### Colunas:

| Nome da Coluna | Tipo Python | Tipo do Banco / ORM       | Restrições / Modificadores | Valor Padrão (Default)       | Descrição                                                                                          |
| :------------- | :---------- | :------------------------ | :------------------------- | :--------------------------- | :------------------------------------------------------------------------------------------------- |
| `id`           | `str`       | `String`                  | `PRIMARY KEY`              | `str(uuid4())`               | Identificador único universal do pedido (UUID v4 em formato string).                               |
| `customer`     | `str`       | `String`                  | `NOT NULL`                 | _Nenhum_                     | Identificador ou nome do cliente solicitante do pedido.                                            |
| `status`       | `str`       | `String`                  | `NULLABLE`                 | `"open"`                     | Estado atual do pedido na máquina de estados (ex: `open`, `processing`, `completed`, `cancelled`). |
| `created_at`   | `datetime`  | `DateTime(timezone=True)` | `NULLABLE`                 | `datetime.now(timezone.utc)` | Data e hora exatas da criação do registro com informação explicita de fuso horário (UTC).          |

#### Relacionamentos e Regras:

- **`items`**: Relacionamento de lista (`list["Item"]`) com a entidade `Item`.
  - **`back_populates`**: `"order"` — Mantém a consistência bidirecional na sessão do SQLAlchemy.
  - **`cascade="all, delete-orphan"`**: Garante a integridade referencial ao nível do ORM. Se uma `Order` for removida ou se um `Item` for desassociado da lista `order.items`, o registro correspondente em `items` será automaticamente excluído do banco de dados.

---

## Tabela: `items`

Armazena as linhas de detalhe (itens) pertencentes a um pedido, especificando
produto (SKU), descrição legível e quantidade adquirida.

#### Colunas:

| Nome da Coluna | Tipo Python | Tipo do Banco / ORM | Restrições / Modificadores            | Valor Padrão (Default) | Descrição                                                                    |
| :------------- | :---------- | :------------------ | :------------------------------------ | :--------------------- | :--------------------------------------------------------------------------- |
| `id`           | `str`       | `String`            | `PRIMARY KEY`                         | `str(uuid4())`         | Identificador único universal do item do pedido (UUID v4 em formato string). |
| `order_id`     | `str`       | `String`            | `FOREIGN KEY (orders.id)`, `NOT NULL` | _Nenhum_               | Chave estrangeira que referencia a coluna `id` da tabela `orders`.           |
| `sku`          | `str`       | `String`            | `NOT NULL`                            | _Nenhum_               | Stock Keeping Unit (código identificador do produto no estoque).             |
| `description`  | `str`       | `String`            | `NOT NULL`                            | _Nenhum_               | Descrição detalhada do produto ou serviço negociado.                         |
| `quantity`     | `int`       | `Integer`           | `NOT NULL`                            | _Nenhum_               | Quantidade de unidades compradas do respectivo item.                         |

#### Relacionamentos e Regras:

- **`order`**: Relacionamento de referência única para o modelo `Order`.
  - **`back_populates`**: `"items"` — Permite acesso direto ao objeto pai via navegação `item.order`.

---
