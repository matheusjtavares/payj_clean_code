# Pay-J

Sistema de gestão de agenda e atendimentos para profissionais autônomos (cabeleireiros, personal trainers, psicólogos, consultores, terapeutas, etc.), permitindo o controle de disponibilidade, agendamento de atendimentos e acompanhamento financeiro básico.

Projeto desenvolvido para fins acadêmicos, aplicando princípios de **Clean Code** e **Padrões de Projeto (Design Patterns)**.

---

## 1. Sobre o Projeto

O Pay-J resolve o problema de profissionais autônomos que controlam sua agenda manualmente, causando conflitos de horário, esquecimentos e falta de organização financeira dos atendimentos.

Principais funcionalidades da primeira versão:
- Cadastro de profissional, serviços e disponibilidade.
- Cadastro de clientes.
- Agendamento, confirmação, remarcação e cancelamento de atendimentos.
- Validação de conflitos de horário.
- Controle de status do atendimento (agendado, confirmado, realizado, cancelado, faltou).
- Registro do valor cobrado por atendimento.

---

## 2. Arquitetura

Monolito Java com Spring Boot, organizado em camadas seguindo os princípios de **Clean Architecture / Arquitetura em Camadas**:

```
src/main/java/com/payj
├── domain/              # Entidades e regras de negócio puras (sem dependência de framework)
│   ├── model/           # Profissional, Cliente, Servico, Atendimento, Disponibilidade
│   ├── enums/           # StatusAtendimento, etc.
│   └── exception/       # Exceções de domínio
│
├── application/         # Casos de uso (orquestram as regras de negócio)
│   ├── usecase/         # AgendarAtendimentoUseCase, CancelarAtendimentoUseCase...
│   ├── strategy/        # Políticas de cancelamento, cálculo de disponibilidade
│   └── service/         # Serviços de aplicação
│
├── infrastructure/      # Implementações técnicas
│   ├── persistence/     # Repositórios JPA, entidades de banco
│   ├── notification/    # Adapters de e-mail/SMS (mock inicialmente)
│   └── payment/         # Adapter de gateway de pagamento (mock inicialmente)
│
└── api/                 # Camada de exposição
    ├── controller/      # Controllers REST
    ├── dto/             # Request/Response DTOs
    └── mapper/          # Conversão entre DTO e Domínio
```

**Regra de dependência:** `api` → `application` → `domain`. A camada `domain` não depende de nada do Spring. `infrastructure` implementa interfaces (ports) definidas em `domain`/`application`.

---

## 3. Padrões de Projeto Aplicados

---

## 4. Tecnologias

- **Java 21**
- **Spring Boot 3.x** (Web, Data JPA, Validation)
- **Banco de dados:** H2 (desenvolvimento/testes) e PostgreSQL (produção, futuro)
- **Build:** Maven
- **Testes:** JUnit 5, Mockito
- **Documentação da API:** springdoc-openapi (Swagger)
- **Lombok** (opcional, para reduzir boilerplate)

---

---

## 5. Estrutura de Branches e Fluxo de Trabalho

- `main`: código estável.
- `develop`: integração das funcionalidades.
- `feature/<nome-da-feature>`: desenvolvimento de novas funcionalidades (ex: `feature/agendar-atendimento`).

Fluxo sugerido:
1. Criar branch a partir de `develop`.
2. Desenvolver com commits pequenos e descritivos (padrão [Conventional Commits](https://www.conventionalcommits.org/)).
3. Abrir Pull Request para `develop`.
4. Revisar (auto ou pares) antes do merge.

---

## 6. Convenções de Código

- Seguir **Clean Code**: nomes significativos, métodos pequenos e com responsabilidade única, evitar comentários desnecessários (código autoexplicativo).
- Testes unitários obrigatórios para regras de negócio no `domain` e `application`.
- Sem lógica de negócio em `controller` ou `infrastructure`.
- Utilizar DTOs na camada `api`, nunca expor entidades de domínio diretamente.

---

## 7. Licença

Projeto acadêmico, sem fins comerciais.
