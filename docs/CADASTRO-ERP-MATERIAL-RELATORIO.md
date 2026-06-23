# Página de Cadastro — RGA ERP Material de Construção

- **Data:** 2026-06-23
- **Repositório:** `rgatechsolutions.com.br` (site estático, sem build system)
- **Branch:** `main` (commit local — sem push/deploy)

## 1. Página criada
`cadastro/erp-material/index.html` — página pública de **pré-cadastro/onboarding** do ERP Material,
autocontida (HTML + CSS + JS embutidos), no padrão visual do site (mesmas fontes Sora/Manrope/JetBrains
Mono, paleta navy/verde, logo RGA, botões e cards). Mobile-first.

## 2. URL da página
Pelo roteamento por pastas do site (cada pasta abre seu `index.html`):

```
https://rgatechsolutions.com.br/cadastro/erp-material
```

## 3. Campos implementados
- **Loja:** nome fantasia*, razão social, CNPJ* (com máscara), inscrição estadual, cidade*, UF*, endereço.
- **Responsável:** nome*, WhatsApp*, e-mail*.
- **Operação:** segmento, qtd. de produtos (faixas), caixas/PCs, leitor de código, impressora térmica,
  precisa importar planilha.
- **Fiscal/NFC-e futura:** deseja NFC-e, tem certificado A1, tem CSC/Token, tem contador, contato do contador.
- **Aceite obrigatório:** termo do piloto (comprovante não fiscal).

(*) obrigatório.

## 4. Validações
- Nome fantasia, responsável, cidade obrigatórios; UF obrigatória.
- **CNPJ** com máscara `00.000.000/0000-00` e validação de **14 dígitos** (limpeza de máscara).
- WhatsApp obrigatório; **e-mail** com validação de formato.
- **Aceite** obrigatório.
- Campos inválidos são destacados; o primeiro erro recebe foco e mensagem amigável.
- Campos opcionais vazios **não quebram** o envio.

## 5. Como o envio funciona
**Opção A (escolhida para esta fase):** o formulário **não envia para servidor**. Ao enviar:
- **"Enviar cadastro pelo WhatsApp"** → monta uma mensagem formatada com todos os dados e abre o
  WhatsApp da RGA (`wa.me/5579998358788`) com o texto pronto.
- **"Enviar por e-mail"** → abre o app de e-mail (`mailto:` para `gomes.renan549@gmail.com`) com os dados.
- **"Falar com a RGA"** → conversa direta no WhatsApp.

Sem backend, sem secrets, sem terceiros recebendo os dados automaticamente — o cliente confirma o
envio. **Provisório por design** (ver pendências). A senha do certificado, o CSC e tokens **não** são
coletados (apenas "tem? sim/não"); há aviso explícito na seção fiscal.

## 6. Por que não cria usuário/tenant automaticamente nesta fase
Decisão de segurança do piloto: a página é **onboarding/coleta**, não provisionamento. Não libera
conta MASTER, não cria tenant e não dá acesso. A RGA cria a loja/usuário **manualmente** no Admin
após receber os dados e envia o login + senha temporária por canal seguro. Isso evita criação
indevida de contas e mantém o controle do piloto.

## 7. Como o instalador 0.1.2 é apresentado
- Botão **"Baixar instalador piloto 0.1.2"** → link oficial:
  `https://api.rgatechsolutions.com.br/downloads/erp-material/rga-erp-material-setup-0.1.2.exe`
- Bloco de **verificação**: Versão **0.1.2**, Tamanho **81.947.416 bytes (~78,1 MB)**,
  **SHA-256 `D2979756315B51E852F25DCF81ED40EC7DD5ED569162D3C47D0E9851C010AD0C`**, com o comando
  PowerShell `Get-FileHash` para o cliente conferir.
- Aviso de **versão piloto / comprovante não fiscal** e o passo a passo do piloto.

## 8. Testes executados
Site estático sem `package.json`/build/lint/test (conforme `LEIA-ME.txt`). Validação possível:
- **Sintaxe do JS** embutido: `node --check` → **OK**.
- **Balanceamento de tags**: fieldset 4/4, div 48/48, form 1/1.
- Conferência de fatos críticos (URL do instalador, SHA-256, WhatsApp, tamanho) presentes.
- Recomendado abrir localmente: `python -m http.server` → `http://localhost:8000/cadastro/erp-material`.

## 9. Pendências futuras
- Integração com **API para salvar o lead** (endpoint próprio seguro) em vez de WhatsApp/mailto.
- **Criação automática de tenant** após aprovação no Admin.
- **Painel interno** para acompanhar cadastros recebidos.
- **Envio automático de e-mail** de confirmação ao cliente.
- **Assinatura digital** do instalador (reduz aviso SmartScreen).
- Etapa **fiscal Focus NFC-e** (certificado A1, CSC/Token, homologação).

## 10. Não feito (conforme regras)
Sem deploy, sem alterar DNS, sem publicar, sem secrets, sem cobrança/Focus real, sem criação
automática de conta. Aguardando validação para publicar.
