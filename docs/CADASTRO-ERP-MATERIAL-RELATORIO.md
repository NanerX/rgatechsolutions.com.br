# Pagina de Cadastro - RGA ERP Material

- Data: 2026-06-23
- Repositorio: `rgatechsolutions.com.br`
- Arquivo: `cadastro/erp-material/index.html`

## Fluxo antigo
A pagina mostrava botao/link direto para o instalador, permitia envio principal por WhatsApp/mailto e exibia e-mail pessoal.

## Fluxo novo
1. Cliente preenche o formulario.
2. O site envia POST para `https://api.rgatechsolutions.com.br/api/v1/public/merchant-applications`.
3. A RGA analisa no Admin.
4. Apos aprovacao, o link oficial do instalador e enviado por e-mail.
5. WhatsApp permanece apenas como contato secundario.

## Seguranca implementada
- Link direto do instalador removido da pagina.
- `mailto:` pessoal removido.
- Honeypot oculto `website`.
- Validacoes client-side para nome fantasia, responsavel, cidade, UF, CNPJ com 14 digitos, WhatsApp, e-mail e aceite.
- Pagina informa versao, tamanho e SHA-256 apenas como metadados de verificacao.

## PII
O formulario envia dados de loja/responsavel para a API da RGA. Nao coleta senha de certificado, CSC, tokens fiscais ou secrets.

## Por que nao cria tenant automaticamente
O site publico gera lead pendente. Tenant e usuario continuam sob aprovacao MASTER para evitar acesso indevido ao piloto.

## Instalador
O instalador nao aparece mais como link publico. A pagina explica que o link oficial chega por e-mail apos aprovacao.

## SMTP e token
O site nao gera token e nao envia e-mail. A API gera token HMAC e usa e-mail simulado por padrao. SMTP real depende de variaveis de ambiente no backend.

## Pendencias para producao
- Publicar a pagina apos validacao.
- Garantir CORS da API para `rgatechsolutions.com.br`.
- Configurar SMTP real e chave `ErpDownloads__DownloadTokenKey` no backend.
- Mover o `.exe` para area privada ou `X-Accel-Redirect`.

## Verificacao esperada
Os comandos `findstr` para `downloads/erp-material`, `rga-erp-material-setup`, `api.rgatechsolutions.com.br/downloads` e `mailto:gomes.renan549@gmail.com` nao devem encontrar link direto nem e-mail pessoal.
