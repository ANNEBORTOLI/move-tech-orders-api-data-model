# move-tech-orders-api-data-model

Ponto de partida do **Lab H1 — Documentar o modelo de dados**.

Parte do curso **Move Tech** — Magalu × Prósper Digital Skills
Formação em Cloud Computing para iniciantes

---

## Contexto

O código desta API já está integrado com o banco de dados via SQLAlchemy. As entidades estão definidas em `app/models.py`.

Seu trabalho neste lab é **documentar o modelo de dados** antes de provisionar o banco.

---

## O que você vai fazer

- [ ] Inspecionar `app/models.py` e identificar as tabelas e colunas
- [ ] Criar a pasta `docs/` no repositório
- [ ] Criar `docs/data-model.md` com a estrutura das tabelas e o relacionamento
- [ ] Commitar e enviar para o repositório remoto

---

## Como rodar localmente

**Pré-requisito:** Docker Desktop instalado.

```bash
docker compose up --build
```

Acesse: http://localhost:8000/docs

---

## Estrutura do projeto

```
app/
  main.py        # rotas da API
  models.py      # definição das tabelas (SQLAlchemy)
  database.py    # engine e sessão do banco
tests/           # testes automatizados
k8s/             # manifests Kubernetes
.github/
  workflows/
    deploy.yml   # pipeline de deploy
```

---

## Próximo lab

Após concluir este lab, avance para o **Lab H2 — Provisionar e conectar o banco**.
