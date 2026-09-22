Aqui tens apenas a estrutura funcional em código limpo, dividida entre as classes Java e o script SQL.

---

### 1. Classes Java (`domain`)

```java
package com.seuprojeto.domain;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
@EqualsAndHashCode(onlyExplicitlyIncluded = true)
public class Cliente {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @EqualsAndHashCode.Include
    private Integer id;
    
    private String nome;
    private String email;
    private String telefone;
}

```

```java
package com.seuprojeto.domain;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;
import java.math.BigDecimal;

@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
@EqualsAndHashCode(onlyExplicitlyIncluded = true)
public class Pedido {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @EqualsAndHashCode.Include
    private Integer id;
    
    private LocalDateTime data;
    private String status;
    private BigDecimal valorTotal;

    @ManyToOne
    @JoinColumn(name = "cliente_id")
    private Cliente cliente;

    @OneToOne(mappedBy = "pedido", cascade = CascadeType.ALL)
    private Pagamento pagamento;
}

```

```java
package com.seuprojeto.domain;

import jakarta.persistence.*;
import lombok.*;
import java.math.BigDecimal;

@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
@EqualsAndHashCode(onlyExplicitlyIncluded = true)
public class ItemPedido {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @EqualsAndHashCode.Include
    private Integer id;
    
    private Integer quantidade;
    private BigDecimal valorUnitario;

    @ManyToOne
    @JoinColumn(name = "pedido_id")
    private Pedido pedido;

    @ManyToOne
    @JoinColumn(name = "produto_id")
    private Produto produto;
}

```

```java
package com.seuprojeto.domain;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;
import java.math.BigDecimal;

@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
@EqualsAndHashCode(onlyExplicitlyIncluded = true)
public class Pagamento {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @EqualsAndHashCode.Include
    private Integer id;
    
    private BigDecimal valor;
    private LocalDateTime data;
    private String status;
    private String tipo;

    @OneToOne
    @JoinColumn(name = "pedido_id")
    private Pedido pedido;
}

```

---

### 2. Script SQL (`import.sql`)

```sql
INSERT INTO categoria (nome) VALUES ('Eletrônicos');
INSERT INTO categoria (nome) VALUES ('Livros');
INSERT INTO categoria (nome) VALUES ('Móveis');
INSERT INTO categoria (nome) VALUES ('Vestuário');
INSERT INTO categoria (nome) VALUES ('Alimentos');

INSERT INTO produto (nome, preco, categoria_id) VALUES ('Notebook Dell', 4500.00, 1);
INSERT INTO produto (nome, preco, categoria_id) VALUES ('Livro Clean Code', 95.00, 2);
INSERT INTO produto (nome, preco, categoria_id) VALUES ('Mesa de Escritório', 600.00, 3);
INSERT INTO produto (nome, preco, categoria_id) VALUES ('Camiseta Básica', 50.00, 4);
INSERT INTO produto (nome, preco, categoria_id) VALUES ('Arroz 5kg', 25.00, 5);

INSERT INTO cliente (nome, email, telefone) VALUES ('João Silva', 'joao@email.com', '11999991111');
INSERT INTO cliente (nome, email, telefone) VALUES ('Maria Souza', 'maria@email.com', '11999992222');
INSERT INTO cliente (nome, email, telefone) VALUES ('Carlos Oliveira', 'carlos@email.com', '11999993333');
INSERT INTO cliente (nome, email, telefone) VALUES ('Ana Clara', 'ana@email.com', '11999994444');
INSERT INTO cliente (nome, email, telefone) VALUES ('Pedro Santos', 'pedro@email.com', '11999995555');

INSERT INTO pedido (data, status, valor_total, cliente_id) VALUES ('2023-10-01T10:00:00', 'CONCLUIDO', 4500.00, 1);
INSERT INTO pedido (data, status, valor_total, cliente_id) VALUES ('2023-10-02T11:30:00', 'CONCLUIDO', 95.00, 2);
INSERT INTO pedido (data, status, valor_total, cliente_id) VALUES ('2023-10-03T14:15:00', 'AGUARDANDO_PAGAMENTO', 600.00, 3);
INSERT INTO pedido (data, status, valor_total, cliente_id) VALUES ('2023-10-04T09:20:00', 'CONCLUIDO', 100.00, 4);
INSERT INTO pedido (data, status, valor_total, cliente_id) VALUES ('2023-10-05T16:45:00', 'CONCLUIDO', 25.00, 5);

INSERT INTO item_pedido (quantidade, valor_unitario, pedido_id, produto_id) VALUES (1, 4500.00, 1, 1);
INSERT INTO item_pedido (quantidade, valor_unitario, pedido_id, produto_id) VALUES (1, 95.00, 2, 2);
INSERT INTO item_pedido (quantidade, valor_unitario, pedido_id, produto_id) VALUES (1, 600.00, 3, 3);
INSERT INTO item_pedido (quantidade, valor_unitario, pedido_id, produto_id) VALUES (2, 50.00, 4, 4);
INSERT INTO item_pedido (quantidade, valor_unitario, pedido_id, produto_id) VALUES (1, 25.00, 5, 5);

INSERT INTO pagamento (valor, data, status, tipo, pedido_id) VALUES (4500.00, '2023-10-01T10:05:00', 'APROVADO', 'PIX', 1);
INSERT INTO pagamento (valor, data, status, tipo, pedido_id) VALUES (95.00, '2023-10-02T11:35:00', 'APROVADO', 'CARTAO_CREDITO', 2);
INSERT INTO pagamento (valor, data, status, tipo, pedido_id) VALUES (600.00, '2023-10-03T14:15:00', 'PENDENTE', 'BOLETO', 3);
INSERT INTO pagamento (valor, data, status, tipo, pedido_id) VALUES (100.00, '2023-10-04T09:25:00', 'APROVADO', 'PIX', 4);
INSERT INTO pagamento (valor, data, status, tipo, pedido_id) VALUES (25.00, '2023-10-05T16:50:00', 'APROVADO', 'CARTAO_DEBITO', 5);

```
