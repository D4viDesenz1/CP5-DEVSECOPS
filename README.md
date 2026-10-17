# FIAP - DevSecOps (projeto unificado)

Projeto simples feito com a base do **CP04 - Containers** e do repositório **devsecops-fiap**.
Contém HTML puro (sem CSS/JS), Dockerfile e GitHub Actions com 3 jobs.

## Colocar no GitHub

1. Extraia o ZIP e envie **o conteúdo da pasta**, não o ZIP, para a raiz do repositório existente.
2. Mantenha a pasta oculta `.github/workflows/` e os arquivos `.gitleaks.toml` e `.semgrep.yml`.
3. **Apague o antigo `config.js` no GitHub**: ele contém a chave fictícia usada no exercício anterior; não é necessário no site.
4. Abra **Actions > Security Pipeline**. A execução limpa deverá ter os jobs Gitleaks, Semgrep e Trivy verdes, desde que não haja achados críticos ou segredos em histórico examinado.

> Observação: excluir `config.js` agora não apaga seu conteúdo de commits antigos. Se o Gitleaks ainda apontar esse histórico, é preciso tratar o commit antigo; se algum segredo fosse real, também seria necessário revogá-lo. Para Gitleaks Action em repositórios de **organização**, pode ser necessária a licença; em repositórios pessoais, não.

## Site com Docker (opcional)

```bash
docker build -t site-fiap .
docker run --rm -p 8080:80 site-fiap
```

Acesse `http://localhost:8080`.

## 3 testes para tirar prints do Actions

Faça **cada sabotagem em uma branch separada criada a partir da `main`**. Não misture os três testes e não faça merge dessas branches.

### 1. Gitleaks: segredo fictício

No Git Bash, dentro da branch `teste-gitleaks`:

```bash
printf 'FIAP_TEST_SECRET_%s\n' "$(openssl rand -hex 8)" > segredo-teste.txt
git add segredo-teste.txt
git commit -m "teste: segredo ficticio"
git push -u origin teste-gitleaks
```

**Print:** job `Gitleaks - Segredos` vermelho, mostrando que a chave fictícia foi detectada. O prefixo de teste é `FIAP_TEST_SECRET_`, seguido de 16 caracteres alfanuméricos.

### 2. Semgrep: `eval()` vulnerável

Na branch `teste-semgrep`, crie `exemplo-vulneravel.py` com este conteúdo:

```python
resultado = eval(input("Digite algo: "))
```

Faça commit e push. **Print:** job `Semgrep - Codigo vulneravel` vermelho, com a regra `fiap-python-eval`.

### 3. Trivy: imagem com CVEs críticos

Na branch `teste-trivy`, troque **apenas a primeira linha** do Dockerfile:

```dockerfile
FROM nginx:1.17-alpine
```

Faça commit e push. **Print:** job `Trivy - CVEs criticos` vermelho se forem encontradas vulnerabilidades críticas no sistema da imagem antiga.

Depois dos prints, mantenha a branch `main` com `FROM nginx:alpine`, sem os arquivos de sabotagem.

## Conteúdo para o PDF

Capture: tela da Actions limpa; falha do Gitleaks; falha do Semgrep; falha do Trivy. Explique em uma frase o que cada ferramenta detectou. Os resultados reais dependem da execução do GitHub Actions.

## Integrantes

- Fabrício Gutierrez Saavedra - RM97631
- Márcio Gastaldi - RM98811
- Arthur Bessa Pian - RM99215
- Davi Desenzi - RM550849
- João Victor - RM551410
# CP5-DEVSECOPS
